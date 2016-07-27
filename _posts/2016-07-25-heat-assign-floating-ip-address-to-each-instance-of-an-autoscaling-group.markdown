---
layout: post
title:  "Heat ASGs with Floating IP per Instance"
date:   2016-07-25 00:00:00 +0200
tags: [openstack, code]
description: How to assign a floating IP address to each instance of an autoscaling group in OpenStack Heat

imagefeature: 2016-07-25-heat-assign-floating-ip-address-to-each-instance-of-an-autoscaling-group/clouds-295695_1280.jpg

comments: true
share: true

---


OpenStack [Heat](http://docs.openstack.org/developer/heat/) is the orchestration project of OpenStack, which allows to describe and provision OpenStack resources based on Heat _templates_, similar to [Amazon CloudFormation](https://aws.amazon.com/cloudformation/).
For a research project, I recently had the requirement to assign a _floating_ IP address to every instance of a Heat _autoscaling group_ (ASG).
Given the fact that such setup is contradictory to the common practice having a load balancer in front of an ASG, this costed me some time to figure out.

## Creating a Single Instance Using Heat

Setting up a single instance using a Heat template (HOT) is pretty easy, straightforward and also covered in the [Heat templates repository](https://github.com/openstack/heat-templates/blob/497cf075f70e3b7051fcf52301cd8e356260a37c/hot/servers_in_existing_neutron_net.yaml):

{% highlight yaml %}
# single_server_with_floating_ip.yaml
# (abbreviated example based on hot/servers_in_existing_neutron_net.yaml)

parameters:
  # ~~~snip~~~
  network:
    type: string
    description: The network for the VM
    constraints:
      - {custom_constraint: neutron.network}
  public_net:
    type: string
    description: ID of public network for which floating IP addresses will be allocated
    constraints:
      - {custom_constraint: neutron.network}

resources:
  server1:
    type: OS::Nova::Server
    properties:
      name: Server1
      image: { get_param: image }
      networks:
        - port: { get_resource: server1_port }
      
  server1_port:
    type: OS::Neutron::Port
    properties:
      network_id: { get_param: network }
      security_groups: [{ get_resource: server_security_group }]

  server1_floating_ip:
    type: OS::Neutron::FloatingIP
    properties:
      floating_network_id: { get_param: public_net }
      port_id: { get_resource: server1_port }
{% endhighlight %}

As we can see, the `server1_floating_ip` (of type [`OS::Neutron::FloatingIP`](http://docs.openstack.org/developer/heat/template_guide/openstack.html#OS::Neutron::FloatingIP)) is NAT'ed to the `server1_port` which in turn is assigned to the server instance (type [`OS::Nova::Server`](http://docs.openstack.org/developer/heat/template_guide/openstack.html#OS::Nova::Server)) that we instantiate. 

## Creating an Autoscaling Group

In order to create an ASG of multiple instances, each only with a private IP, we also find help in the [Heat templates repository](https://github.com/openstack/heat-templates/blob/master/hot/asg_of_servers.yaml):

{% highlight yaml %}
# abbreviated example based on hot/asg_of_servers.yaml

parameters:
  # ~~~snip~~~
  image:
    type: string
    description: Name or ID of the image to use for the instances.
  network:
    type: string
    description: The network for the VM
    default: private

resources:
  asg:
    type: OS::Heat::AutoScalingGroup
    properties:
      resource:
        type: OS::Nova::Server
        properties:
            key_name: { get_param: key_name }
            image: { get_param: image }
            flavor: { get_param: flavor }
            networks: [{network: {get_param: network} }]
      min_size: 1
      desired_capacity: 3
      max_size: 10
{% endhighlight %}

We see the [`AutoScalingGroup`](http://docs.openstack.org/developer/heat/template_guide/openstack.html#OS::Heat::AutoScalingGroup) defined with the Nova server instance being scaled up and down (between 1 and 10 instances).
Using the [scale up and down](https://github.com/openstack/heat-templates/blob/497cf075f70e3b7051fcf52301cd8e356260a37c/hot/asg_of_servers.yaml#L61-L75) URLs provided, one can test the functionality of auto scaling.

Recalling our goal, namely to assign a floating IP address to every instance of the ASG (and also to the ones created during scale-out), it looks intuitively right to extend the `resource` block of the previous template. However, only one `resource` can be given to the [`AutoScalingGroup`](http://docs.openstack.org/developer/heat/template_guide/openstack.html#OS::Heat::AutoScalingGroup). For our use case, we also need the floating IP resources to be added and destroyed on demand.

## Scaling a Complete Stack

The solution to my problem was finally hidden in one of the other examples, the [ASG of stacks](https://github.com/openstack/heat-templates/blob/497cf075f70e3b7051fcf52301cd8e356260a37c/hot/asg_of_stacks.yaml#L47-L51).
Essentially, Heat can also scale a complete stack (defined by its own template file).
So what we now do is essentially to treat the server with its floating IP as defined in the first listing (`single_server_with_floating_ip.yaml`) as entity of scale and pass through all parameters: 

{% highlight yaml %}
# asg_of_server_with_floating.yaml
# (abbreviated example based on hot/asg_of_stacks.yaml)
parameters:
  # ~~~snip~~~
  image:
    type: string
    description: Name or ID of the image to use for the instances.
  network:
    type: string
    description: The network for the VM
    constraints:
      - {custom_constraint: neutron.network}
  public_net:
    type: string
    description: ID of public network for which floating IP addresses will be allocated
    constraints:
      - {custom_constraint: neutron.network}
resources:
  asg:
    type: OS::Heat::AutoScalingGroup
    properties:
      resource:
        # this refers to the file previously described
        type: server_with_floating.yaml
        properties:
            image: { get_param: image }
            network: { get_param: network }
            public_net: { get_param: public_net}
      min_size: 1
      desired_capacity: 3
      max_size: 10
{% endhighlight %}

As the `type` of of resource, we can also supply a file name. That's.. let me think.. _not_ obvious?
After long search, was able to find this behavior documented under [Template composition](http://docs.openstack.org/developer/heat/template_guide/composition.html#use-the-template-filename-as-type). 
 
Mind the `public_net` parameter defining the public network ID, from which floating IPs are assigned.
Once we trigger a scale-out, we can see antoehr instance including a floating IP being added after some seconds:

![screenshot horizon](/images/2016-07-25-heat-assign-floating-ip-address-to-each-instance-of-an-autoscaling-group/screenshot.png)
  
I am not aware of a way to launch such stack using the Horizon dashboard, except when the inner template is referenced via an HTTP(S) URL.
Instead, one can launch it using the command line client:

{% highlight bash %}
$ openstack stack create my-test-stack --template asg_of_server_with_floating.yaml -e environment.yaml
{% endhighlight %}
 
All definition of the required parameters, i.e., image and network IDs, are happening in the [`environment.yaml`](https://gist.github.com/StephenKing/13987089075af2cb72d43fee4a0c1ef4#file-environment-yaml) file. 

While my use case was a little academic (why in the world should something scale inside an OpenStack cloud and be _directly_ reachable from the outside world), I think this (to me still unknown) concept of [template composition](http://docs.openstack.org/developer/heat/template_guide/composition.html#use-the-template-filename-as-type) helps solving many more problems.
 
The complete code can be found in [this gist](https://gist.github.com/StephenKing/13987089075af2cb72d43fee4a0c1ef4).<br/>
<small>(cover image by [sipa on pixabay.com](https://pixabay.com/en/clouds-nature-clouds-form-295695/))</small>