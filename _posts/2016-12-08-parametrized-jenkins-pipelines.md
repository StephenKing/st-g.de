---
layout: post
title:  "Parametrized Jenkins Pipelines"
date:   2016-12-08 18:00:00 +0200
tags: [jenkins, code]
description: Jenkins pipelines can be parametrized in the same way as traditional feestyle jobs. This post describes how. 

comments: true
share: true

---

During and after the presentation about Jenkins pipelines that I gave yesterday at the [DevOps Meetup Stuttgart](https://www.meetup.com/de-DE/devops-stuttgart/events/235619665/), we had a couple of very interesting discussions.
One of them was a way to make a Jenkins pipeline job parametrized. Use cases for this that were discussed included different target environments, into which the pipeline should deploy to.
 
While it is not very obvious, there is one particular step that allows to configure a pipeline job's properties, including triggers, how to rotate logs, and said input parameters: The [`properties`](https://jenkins.io/doc/pipeline/steps/workflow-multibranch/#code-properties-code-set-job-properties) step contained in the _multibranch_ plugin.
Please don't expect too much from the referenced documentation, as it is pretty useless.

Of a lot greater help is the Snippet Editor, which is available in every Jenkins instance.


![Snippet Editor](/images/2016-12-08-parametrized-jenkins-pipelines/snippet-editor.png)

Selecting the `properties` step from the dropdown menu offers a whole lot of options to select, including _This project is parameterized_.
After checking this option, parameters of different kinds can be added, i.e., boolean, string, select parameters etc.

![Adding a string parameter](/images/2016-12-08-parametrized-jenkins-pipelines/string-parameter.png)

Hitting the _Generate Pipeline Script_ button emits the following code snippet (manually formatted by myself):

{% highlight groovy %}
properties([
  parameters([
    string(name: 'DEPLOY_ENV', defaultValue: 'TESTING', description: 'The target environment', )
   ])
])
{% endhighlight %}

Once this code is included at the top level of the pipeline script, any pipeline execution resets the job's parameters to the specified values.
From that point on, the _Build Now_ button has changed to a _Build with Parameter_ and every time the pipeline is launched, the user is asked to specify defined values.

![Build with parameters](/images/2016-12-08-parametrized-jenkins-pipelines/build-with-parameters.png)

P.S: The first run will most likely fail (_EDIT_: see last paragraph), as chances are good that you try to access the just added parameters ([JENKINS-40235](https://issues.jenkins-ci.org/browse/JENKINS-40235)).
Subsequent runs will work, if your pipeline script is correct. 

## Bonus: Selection from Dropdown

Using the _Choice Parameter_, the possible inputs can be restricted to a pre-defined list of choices.


![Adding a choice parameter](/images/2016-12-08-parametrized-jenkins-pipelines/choice-parameter.png)

Be warned that the emitted code including `choices: ['TESTING', 'STAGING', 'PRODUCTION']` fails with an exception

<blockquote>
java.lang.ClassCastException: hudson.model.ChoiceParameterDefinition.choices expects class java.lang.String but received class java.util.ArrayList
</blockquote>

Instead, the list of choices has to be supplied as String containing new line characters (`\n`): `choices: ['TESTING\nSTAGING\nPRODUCTION']` ([JENKINS-40358](https://issues.jenkins-ci.org/browse/JENKINS-40358)).

**EDIT January, 12th:** Previously, it was common to access these parameters using Groovy or environment variables:
  
{% highlight groovy %}
echo "Will deploy to ${DEPLOY_ENV}"
{% endhighlight %}

As of [`workflow-cps` version 2.18](https://wiki.jenkins-ci.org/display/JENKINS/Pipeline+Groovy+Plugin#PipelineGroovyPlugin-2.18%28Sep23%2C2016%29), a new `params` global variable provides sane access also on the first run (by returning specified default values).

{% highlight groovy %}
echo "Will deploy to ${params.DEPLOY_ENV}"
{% endhighlight %}
