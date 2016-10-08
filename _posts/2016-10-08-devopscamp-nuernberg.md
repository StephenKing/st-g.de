---
layout: post
title:  "DevOps Camp Compact 2016"
date:   2016-10-08 20:30:00 +0200
tags: [meta, event, jenkins]
description: A short recap of the one-day barcamp "DevOps Camp Compact" 

imagefeature: 2016-10-08-devopscamp-nuernberg/header.jpg

comments: true
share: true

---

Right now, I am sitting in the train on my way back from the [DevOps Camp Compact](openspacer.org/60-devops-community/127-devops-camp-compact-2016/) in Nürnberg. It was a very exhausting day with two talks that I gave and a lot of information and energy that I got from it.
I want to thank the organizers Tobias and Stefan, the location sponsor (my TYPO3 friends Netlogix), and the roughly 100 participants that made this such a great day. Further, I'm happy that I was finally able to join the 6th edition after I wasn't able to attend the previous four events.

During the session planning, I offered three talks about the following topics:
 
* HTTP/2, QUIC, and Multipath-TCP
* Jenkins pipelines
* SDN & NFV from the academic perspective

Based on the interest of the participants, I finally held sessions about the first two topics.

## Sessions Held by Me

### HTTP/2, QUIC, and Multipath-TCP

This talk brought in some of my network expertise that I gained from my $dayjob at the university. I started with the history of HTTP, the protocol, which powers the web. Regarding innovative protocols, I first covered HTTP/2, a protocol that should be used by everybody right now. The next one, QUIC (Quick UDP Internet Connections), is currently used by Google and they are using us as their testers (if we use Chrome and access Google services). In contrast to HTTP, it builds up on UDP instead of TCP. Finally, I covered Multipath TCP, which allows a multi-homed device to transfer data simultaneously via multiple connections. The unfortunate state is that the only public deployment is Apple Siri, which uses it only for reliability, but not for load balancing.
 
The slides are publicly available on [SlideShare](http://www.slideshare.net/StephenKing/cleaning-up-the-dirt-of-the-nineties-how-new-protocols-are-modernizing-the-web). To save some disk space, I didn't duplicate them, but refer to the identical slide deck that I used during TYPO3camp Vienna.

### Jenkins Pipelines

Very interesting was also my second talk -- for me, and I hope also for the rest of the audience. As there were a couple of people in the room, who have way more experience with Jenkins than I have, we had some very good discussions.
 
In short: The Jenkins pipeline plugins provide a modern way to describe delivery pipelines as code, much cleaner than previously possible with Jenkins.
 
The slides for this talk are available via [GitHub Pages](https://github.com/StephenKing/dvocc16-jenkins-pipeline).

The new things, which I learned include:

- The [Pipeline Milestone Step Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Pipeline+Milestone+Step+Plugin) allows to automatically cancel older pipeline instances, once a newer one reached a certain (`milestone`) step. This is particular useful, if a manual sign-off step is not acknowledged and it is (as usually) not desired that old pipeline executions remain running, or when (who knows why) a newer commit passes faster through a deployment pipeline than an old one, which could result in the old state being deployed over the newer. 
- The JobDSL plugin is still useful in sitations, where folks are not working with GitHub or Bitbucket and could be used to generate the pipeline jobs. (I never said that this plugin isn't useful, but I wouldn't have seen it together in relation with pipeline jobs.) 
- Pipeline versioning could be done using the new [`@Library`](https://github.com/jenkinsci/workflow-cps-global-lib-plugin/blob/master/README.md#) annotation on a per repo level within the `Jenkinsfile` by letting it point a specific branch of the pipeline repo.

## Other's Sessions

We had an interesting session during the "Docker Security" session. Some of the key points here are:

- Comtainers are not VMs. They do not offer the same way of isolation.
- Every user, who has access to the Docker socket (`/var/run/docker`), is root-equivalent. By mounting in directories and `sudo`-ing within the container, any file on the host can be edited.
- In some situations, Docker exposes more ports via `iptables` than the user might expect. Therefore, it can be helpful to start an additional (privileged) container within the same network namespace of the container to protect and block all traffic to ports, which are not explicitly white-listed.
- Cgroups should be used to limit resource usage by single containers.

During the "Where should we run those containers?" session, we discussed the cool kids Kubernetes, Mesos/DCOS, and Rancher. Nobody knows the answer, what to use ☺. While couple of people have been experimenting with some of them, none is using any of them in production. I definitely will experiment a bit more about Kubernetes, while I find Mesos/DCOS still extremely interesting.
 
## Summary

I really enjoyed being there. The people and the discussions with them were great and I hope that I will make it to the next edition somewhen in April/May 2017 again in Nürnberg. If you're interested, keep an eye on [devops-camp.de](http://www.devops-camp.de).