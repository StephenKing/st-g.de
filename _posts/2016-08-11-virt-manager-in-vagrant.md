---
layout: post
title:  "virt-manager on MacOS"
date:   2016-08-11 08:00:00 +0200
tags: [code, vagrant, virtualization]
description: A comfortable way to run virt-manager virtualized in Vagrant.

imagefeature: 2016-08-11-virt-manager-in-vagrant/virt-manager.jpg

comments: true
share: true

---


[_Virt-Manager_](https://virt-manager.org/) is a graphical user interface for _libvirt_, a popular toolset for managing virtual machines provided by KVM.
Unfortunately, it does not work well on MacOS.

The [MacOS port](https://github.com/jeffreywildman/homebrew-virt-manager) did not work for me, as I was not able to connect to the socket on the remote host even after changing adding the `socket=/var/run/libvirt/libvirt-sock` parameter. The same holds for its command-line pendant `virsh`.

So the idea to let it natively run on Linux inside a Vagrant VM came up.

## Running Virt-Manager in Vagrant

On that path, I had to overcome the following two challenges:

- to run a window manager inside the VirtualBox GUI
- copying over my SSH private key into the VM

#
After digging into Vagrant's configuration options, I found solutions for both of these issues.

{% highlight ruby %}
config.ssh.forward_agent = true
config.ssh.forward_x11 = true
{% endhighlight %}

- SSH agent forwarding allows to use the private key located on my Mac
- X11 forwarding allows to run graphical applications inside the VM and display them using [XQuartz](https://www.xquartz.org/) as a normal program window on my Mac.

## Ready-to-Go Vagrantfile

I've put the complete `Vagrantfile` on Github: [StephenKing/vagrant-virt-manager](https://github.com/StephenKing/vagrant-virt-manager).

After cloning the repository, the VM can be created using

{% highlight shell %}
$ vagrant up
{% endhighlight %}

and `virt-manager` can be started using
{% highlight shell %}
$ vagrant ssh
ubuntu@ubuntu-xenial:~$ virt-manager
{% endhighlight %}


## SSH Host Key Verification

The only issue that remains for me is the acknowledgement of the KVM host's SSH fingerprint. The solution instead was to once connect to the server from the command line:

{% highlight shell %}
$ vagrant ssh -- "ssh-keyscan server.example.com >> ~/.ssh/known_hosts"
{% endhighlight %}
