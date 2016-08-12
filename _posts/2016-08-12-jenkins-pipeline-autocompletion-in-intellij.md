---
layout: post
title:  "Jenkins Pipeline Autocompletion in IntelliJ"
date:   2016-08-12 22:00:00 +0200
tags: [jenkins, code]
description: How to make Jetbrains IntelliJ aware of the Jenkinsfile Groovy DSL 

imagefeature: 2016-08-12-jenkins-pipeline-autocompletion-in-intellij/artwork.jpg

comments: true
share: true

---

I am a big fan of the new [Jenkins Pipeline](https://jenkins.io/pipeline/getting-started-pipelines/) suite and enjoy defining my pipelines as code.
Now, I noticed that there is even a way to make IntelliJ IDEA aware ofthe pipeline DSL syntax, which supports the developer with autocompletion and documentation.  

For looking up the DSL syntax, I am a frequent visitor of the [Pipeline Steps Reference](https://jenkins.io/doc/pipeline/steps/) on [jenkins.io](https://jenkins.tio).
As I am only seldom visiting the Jenkins web interface to use the _Pipeline Syntax Snippet Generator_, I was a bit puzzled when I stumbled over a mysterious _IntelliJ IDEA GDSL_ link (available for logged-in users).

![Jenkins with link to the IntelliJ IDEA GDSL file](/images/2016-08-12-jenkins-pipeline-autocompletion-in-intellij/GDSL-link.png)

After reading these words and curiously following the link, my confidence that this would finally help me (still a Groovy novice) to improve my experience of coding with the _roovy DSL used by the pipeline plugin. I already used the _Groovy_ plugin in IntelliJ IDEA, but, of course, all syntax of the Pipeline DSL were unknown until this point.

The content of the text file shown after following the link started with the following lines:
{% highlight groovy %}
//The global script scope
def ctx = context(scope: scriptScope())
contributor(ctx) {
method(name: 'build', type: 'Object', ...
method(name: 'build', type: 'Object', ...
method(name: 'echo', type: 'Object', params: [message:'java.lang.String'], doc: 'Print Message')
method(name: 'error', type: 'Object', params: [message:'java.lang.String'], doc: 'Error signal')
method(name: 'input', type: 'Object', params: [message:'java.lang.String'], doc: 'Wait for interactive input')
{% endhighlight %}

Yes, the known pipeline steps `build`, `echo`, `error`, etc. That's what I need to make IntelliJ aware of. But I had no clue, how to get that into the IDE.


Following the [documentation "Scripting IDE for DSL awareness"](https://confluence.jetbrains.com/display/GRVY/Scripting+IDE+for+DSL+awareness) and [this Gist](https://gist.github.com/gclayburg/0107bdeb38349c6fc87a), I figured out that I have to place this _"somewhere in the classpath"_. Nowhere within the project worked out, however.
[My project](https://github.com/TYPO3-infrastructure/jenkins-pipeline-global-library-chefci/), is a [Pipeline Global Library](https://github.com/jenkinsci/workflow-cps-global-lib-plugin), which allows to define functionality used in `Jenkinsfile`s of multiple projects. The same, however, should apply to projects that only make use of the `Jenkinsfile`.

After putting the contents of the Jenkins output into a [`pipeline.gdsl`](https://github.com/TYPO3-infrastructure/jenkins-pipeline-global-library-chefci/blob/7c4fbc063cd033e36fdc4457d0cac4938e102b70/src/pipeline.gdsl) file in the `src/` folder and importing my project as a new Groovy project into IntelliJ (_File > New > Project from Existing Sources..._), a message popped up: _DSL descriptor file has been change and isn't currently executed_.

![message that DSL descriptor file is found](/images/2016-08-12-jenkins-pipeline-autocompletion-in-intellij/DSL-descriptor-file.png)

Since then, I can enjoy autocompletion as well as well as documentation of the Pipeline DSL.

![autocompletion](/images/2016-08-12-jenkins-pipeline-autocompletion-in-intellij/autocompletion.png)
![documentation](/images/2016-08-12-jenkins-pipeline-autocompletion-in-intellij/documentation.png)


It is not completely clear to me, why I was not able to achieve that with my existing IntelliJ project. Adding the `src/pipeline.gdsl` file did not trigger any message. So probably, the Groovy setup that I had was not 100% correct. During import of the new project, IntelliJ found the two directories contained in my global library.

![message that DSL descriptor file is found](/images/2016-08-12-jenkins-pipeline-autocompletion-in-intellij/import.png)

I hope this helps, either by making you aware of this "feature" or by helping to solve the problem that the `.gdsl` file is not picked up. 