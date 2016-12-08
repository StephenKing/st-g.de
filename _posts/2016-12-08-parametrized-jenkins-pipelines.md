---
layout: post
title:  "Parametrized Jenkins Piplines"
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

## Bonus: Selection from Dropdown

Using the _Choice Parameter_, the possible inputs can be restricted to a pre-defined list of choices.


![Adding a choice parameter](/images/2016-12-08-parametrized-jenkins-pipelines/choice-parameter.png)

Be warned that the emitted code including `choices: ['TESTING', 'STAGING', 'PRODUCTION']` fails with an exception

<quote>
java.lang.ClassCastException: hudson.model.ChoiceParameterDefinition.choices expects class java.lang.String but received class java.util.ArrayList
</quote>

Instead, the list of choices has to be supplied as String containing new line characters (`\n`): `choices: ['TESTING\nSTAGING\nPRODUCTION']` 
