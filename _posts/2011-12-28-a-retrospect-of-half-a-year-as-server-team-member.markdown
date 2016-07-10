---
layout: post
title:  "A retrospect of half a year as Server Team member"
date:   2011-12-28 00:00:00 +0200
tags: [typo3, meta]

comments: true
share: true
---

I am a member of the official TYPO3 Server Administration team since the Developer Days this year. With this article I not only want to look back, but also share some insights in what we are / I am doing in this team.

I have not planned this article at all, but there's the funny thing called Twitter and after [Olivier's tweet](http://twitter.com/#!/T3RevNeverEnd/status/151749320067133440) last night, I decided to shed some light on our work. The question was:

<quote>where can i find info about the actual team and its goals, roadmap and actions?</quote> 

So here is a short story of how I came into that team, why it is still a lot of fun, and what my plans for the next few months are.

## It all started with Git..

It happened [back then in Berlin](http://news.typo3.org/news/article/typo3-code-sprint-berlin-2011-t3csb11/), when we (=Core Team) just made the switch to Git and Gerrit as version control system. Sitting in the crappy AO Hostel's Teacher Room together with [Peter Niederlag](http://www.niekom.de/) and defining all the review processes by writing the [developer documentation](http://wiki.typo3.org/Git) somehow made me feel responsible for this topic. Improving procedures and sharing my knowledge by helping others while setting up Git - thus enabling them to contribute - took a lot of time.

Few weeks afterwards, we tried to set up Infrastructure Meetings. The aim was to improve the development infrastructure and processes of TYPO3 Core development. We all were no admins, thus had no possibility to change many things except documentation.

## The TYPO3 Developer Days

During the [TYPO3 Developer Days 2011](http://t3dd11.typo3.org/) in Sursee this year, Susanne Moog and me have been asked to join the Server Admin Team. We both accepted and participated in the first physical Server Team meeting that was ever held (which is in fact no problem, as we all know us personally quite well). It was also when the picture above was taken. Usually we're not so bad-tempered - it was just the "and now look serious" picture that got selected..

## Time went by.. and work started

The major things I achieved since then:

* TYPO3 (Association) uses an [OTRS](http://www.otrs.org/) Trouble Ticket system to distribute and document mail correspondence, like all the mails sent to/from {info,admin,documentation,..}@typo3.org etc. I was familiar with OTRS before and I really understood, why they all were very unhappy with it. OTRS is a lot like TYPO3. If you have a lazy admin, you will hate it. "lazy" in this case especially means "not familiar with disabling 80% of unneeded functionality or how to set better defaults".
* TYPO3 infrastructure uses OpenVZ for server [virtualization](http://wiki.openvz.org/Main_Page). OpenVZ is a very lightweight virtualization and you might now its commercial pendant Virtuzzo. I knew several other techniques, but this one was new for me. So getting familiar with it was one of my first actions. I must say that it is really the right tool for us.
* *"If it isn't monitored, it is not in production."* - some of you might now this speech. Although setting up a monitoring system was already planned (but not yet realized), I took over that job. It was just so essential for me to have it now. We are using [Zabbix](http://www.zabbix.org/) since then to monitor the whole TYPO3 server infrastructure.
* [Going live](https://buzz.typo3.org/teams/server-admin/article/inspiring-people-to-communicate/) with the BigBlueButton server.
* [Coordinating](https://buzz.typo3.org/teams/server-admin/article/inspiring-people-to-collaborate/) the setup of the iEtherpad server.
* In general, I tried to improve the visibility of the Server Team, by setting up this blog, creating a [twitter account](http://twitter.com/TYPO3server). In fact, this is why I'm writing this article: Make people know that we exist and that we are active - and that we try to help them.
* Having a look, whether [Chef](http://www.opscode.com/chef/) (or Puppet) fits us. Chef is for infrastructure automation and configuration management. It's a big project and we're just so far that we have a basic setup deployed to a handful of less-critical servers. But I try to set up new services based on Chef recipes (Zabbix and iEtherpad have been deployed with Chef already). Such a tool can enable you to easily configure your infrastructure - or to take it down with one wrong command. Thus it will still take some time during which we/I get used to it, before having 100% coverage (which we will probably never have).

These have been my big projects, for which I account more than a full day of work.

## Thousands of small things

You get never bored as an admin (maybe with Chef you do after a while?). Just a few examples, why the whole work is very diverse and involves a lot of communication with other teams and people:

* Helping teams while getting a new virtual server and setting it up (e.g. for Documentation Team's manual rendering, Release Team's Introduction Package template server, ..)
* Helping Christopher with taking over the job as Wiki admin, updating it and [integrating Single Sign-On](https://buzz.typo3.org/teams/server-admin/article/typo3-wiki-now-with-single-sign-on/).
* Exchanging an SSL certificate of the SVN server and see [Redmine](http://www.redmine.org/) ([forge](http://forge.typo3.org/)) crashing. Of course, it's an annoying job - but afterwards it's "Steffen vs. Redmine - 1:0" (or just a 1:4? ;-)).
* Blocking DoS attacks kind of every two weeks from the forge server. We're about to improve that..
* Setting it up and communicating with guys at [TU Eindhoven](http://www.tue.nl/), which sponsored us a new server.

Not all services are running on our infrastructure or maintained by us. There's e.g. DNS (operated by [Snowflake](http://www.snowflake.ch/)) or Mail plus a lot of other, mostly older things, which are operated by [punkt.de](http://punkt.de/) admins. I think we have a pretty good balance between "do it on our own" and "using it from someone (trusted)". It feels good, when you can just ask them and they fix it.

## Plans for the next 6+ months

Some parts of this are my personal agenda, some are (hopefully) plans of the whole team. As we have a very wide range of responsibilities (from system administration / virtualization up to PHP or even Ruby software administration) every one of us has a bit different working packages. This should just show that there would be a place maybe for you in this team, too. As in every TYPO3 team, there is enough work and enough space for new personalities bringing more spirit and ideas (and man/women-power).

So here are my major goals for the next months. If you are interested in helping or have knowledge in your head or your company, I / we would be happy to get supported!

* Bring back biweekly team meetings.
* Finish project "F". I just try to support and push Tim forward to finish that shiny thing that hopefully a lot will love. Something related to Mailing lists.
* Upgrade OTRS to upcoming version 3.1. We sticked to the old-fashioned 2.x series.
* Bring Chef forward to have a basic setup on every server. Deploy new services through Chef.
* Deploy more monitoring scripts for Zabbix through Chef. Until now we're limited to builtin checks.
* Set up a centralized rsyslog server.

From short this article got long.. so if you read this, it looks like you are really interested! Do you want to join our team? Then please get in touch with us (admin(at)typo3.org). You know that you can also just come up with service suggestions for the community that you maintain - running on infrastructure maintained by us?

Be warned: It's all unpaid work, except maybe getting a reward out of the [Server Team's budget](http://association.typo3.org/financial-statements/). If that's fine for you and you like team-working and new challenges, feel welcome!
