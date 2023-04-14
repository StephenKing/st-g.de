---
layout: pageminimal
permalink: /speaking/
title: Speaking
description: "From time time, I want to say something."
---

<h1 class="post-title text-center hyper lighter bordered-bottom entry-title">Selected Talks</h1>
<div class="cursive" style="color: #000; font-style:italic;">From time to time, I want to say something.</div>


## Hot Topics

Currently, I'd be particularly happy to speak about the following topics:

- AWS in general and
- AWS networking in particular
- Feature Management
- Jenkins Pipelines
- Monitoring

# Upcoming Talks

## Building an IoT SuperNetwork on top of the AWS Global Infrastructure

- Event: [AWS Summit Londo](https://aws.amazon.com/events/summits/london/)
- Date: June 7th, 2023

## When ‘allow all from 0.0.0.0’ didn’t help - troubleshooting network connectivity in AWS

- Event: [AWS User Group Nürnberg](https://www.meetup.com/de-DE/nurnberg-aws-user-group/)
- Date: May 8th, 2023

#  Past Talks

## AWS This is My Architecture (Feb 2023)

I was invited to present during an episode of the _This is My Architecture_ series by AWS.

- Video at [YouTube](https://www.youtube.com/watch?v=SC6n6J8Bi58)

## [German] Wenn selbst ‘erlaube allen Verkehr von 0.0.0.0/0’ nicht hilft - Verbindungsprobleme in AWS lösen (Oct 2022)

Was tun, wenn die Netzverbindung zwischen zwei AWS-Diensten will einfach nicht zu Stande kommen will - obwohl schon alle Security Groups weit offen wie Scheunentore sind?

- Together with my colleague Wolfgang Schäfer
- Slides at [SlideShare](https://www.slideshare.net/StephenKing/aws-community-day-network-troubleshooting/)
- Event: [AWS Community Day DACH 2022](https://aws-community-day.de/), Dresden

## Feature Management Platforms (June & Sept 2022)

The idea of Feature Toggles exists since more than a decade and is a corner stone of Continuous Delivery. However, few people seem to exploit the potential that this brings. Decoupling release from deployment brings greater flexibility. Gradual rollouts - percentage-based or for specific beta users and customers - increase safety as well as help to gather feedback early in the development cycle.

- Slides at [SlideShare](https://www.slideshare.net/StephenKing/feature-management-platforms)
- Event: [DevOps Camp 2022](https://devops-camp.de) and [CloudLand 2022](https://shop.doag.org/events/cloudland/2022/agenda/#eventDay.1656626400)

## Industrial Point of View on 5G and Beyond (July 2022)

Discussion session with academic and industrial researchers on the influence of 5G on IoT connectivity.

- Event: [WueWoWas2022](https://lsinfo3.github.io/WueWoWas2022/), Würzburg

## Three Main Challenges for Securely Scaling Connected Devices (Apr 2022)

IoT security starts with the device. If the initial device isn’t secure when it scales it will become a cybersecurity nightmare. 

_IoT for All_ webinar, with Jeff Stahlnecker

- Video at [YouTube](https://www.youtube.com/watch?v=H4Bmhs631eg)
- Event: [IoT for All webinar](https://www.iotforall.com/webinar/three-main-challenges-for-securely-scaling-connected-devices)

## Serverless Networking - How We Provide Cloud-Native Connectivity for IoT Devices (Nov 2020)

In serverless, the network is taken for granted. But what if the network is the product? Is there a routerless? Does it still have a CLI?

- Slides at [SlideShare](https://www.slideshare.net/StephenKing/serverless-networking-how-we-provide-cloudnative-connectivity-for-iot-devices)
- Video at [YouTube](https://www.youtube.com/watch?v=WAypNBSGQpw)
- Event: [AWS Community Day - Bay Area](https://awscommunityday.com/#speaker-drsteffengebert) (online)

## How our Cloudy Mindsets Approached Physical Routers (or: SNMP was not an option) (Nov 2020)

After the latest project, EMnify became a 99% only cloud company. To meet growing scalability and reliability requirements of the interconnection between our AWS-based deployments and multiple carriers, BGP peerings had to be moved out of AWS. Therefore, a pair of Juniper routers were put into place. For a company fully relying on cloud services so far, this alien technology resulted in several challenges.
We want to share, how we solved the integration puzzle of this physical equipment into our existing workflows and tools. 

- Slides at [SlideShare](https://www.slideshare.net/StephenKing/how-our-cloudy-mindsets-approached-physical-routers)
- Video at [YouTube](https://www.youtube.com/watch?v=R8GdDQDCVvY)
- Event: [DENOG12](https://www.denog.de/de/meetings/denog12/talks.html) (online)

## [German] Mobilfunknetz in der AWS-Cloud (Oct 2020)

How EMnify runs a fully virtualized mobile core network on AWS.

- Event: [Würzburg Web Week](https://timetable.wueww.de/veranstaltung/262), Würzburg

## Jenkins vs. CodePipeline (Sep 2019)

Who has not used Jenkins? Who does not have a love-hate relationship with it?<br>
At EMnify, we are heavy Jenkins users, but we re also always considering alternatives where hosted services could make our life easier.<br>
Therefore, we recently - once again - looked at AWS CodePipeline and its friends CodeCommit, CodeBuild, and CodeDeploy. In this talk, we will compare the current state of the two ecosystems regarding their simplicity and flexibility for implementing both trivial as well as complex pipelines. Further, we cover topics like: deployment, maintenance, security, costs, and usability.

- Slides at [SlideShare](https://www.slideshare.net/StephenKing/jenkins-vs-aws-codepipeline-170349976)
- Together with my colleague [Rafael Schleetz Benvenuti](https://twitter.com/rafaelbenvenuti)
- Event: [AWS Community Day Germany](https://www.aws-community-day.de), Hamburg

## Networking in Amazon Web Services  (Oct 2018)

- Event: [DevOps Meetup Würzburg](https://www.meetup.com/de-DE/DevOps-Wuerzburg-Mainfranken/events/254663322/)

## Monitoring Akka with Kamon 1.0 (Apr 2018)

Insights into the inner workings of an application become crucial latest when performance and scalability issues are encountered. This becomes especially challenging in distributed systems, like when using Akka cluster. A popular open-source solution for monitoring on the JVM in general, and Akka in particular, is Kamon. With its recently reached 1.0 milestone, it features means for both metrics collection and tracing of Akka applications, running both standalone or distributed. This talk gives an introduction to Kamon 1.0 with a focus on its metrics features. The basic setup using Prometheus and Grafana will be described, as well as an overview over the different modules and its APIs for implementing custom metrics. The resulting setup allows to record both, automatically exposed metrics about Akka’s actor systems, as well as metrics tailored to the monitored application’s domain and service level indicators. Finally, learnings from a first-time user experience of getting started with Kamon will be reported. The example of adding instrumentation to EMnify’s mobile core application will illustrate, how easy it is to get started and how to kill the Prometheus on a daily basis.

- Slides at [SlideShare](https://www.slideshare.net/StephenKing/monitoring-akka-with-kamon-10)
- Video at [YouTube](https://www.youtube.com/watch?v=7er2PMAppZo)
- Event: [ReactSphere](https://react.sphere.it/steffen-gebert), Krakow

## HTTP/2, QUIC, and Multipath-TCP (Mar 2017)

How new protocols are modernizing the web.

- Event: [adidas](http://www.adidas.de) Tech Talks, Herzogenaurach

## (Declarative) Jenkins Pipelines (Mar 2017)

An updated version of the Jenkins Pipelines talk below, this time with the introduction focussing on declarative pipelines.

- Slides at [SlideShare](https://www.slideshare.net/StephenKing/declarative-jenkins-pipelines)
- Event: [Java User Group Mannheim (MAJUG)](http://www.majug.de/2017/03/15/jenkins-pipelines/)


## An Open-Source Chef Cookbook CI/CD Implementation Using Jenkins Pipelines (Feb 2017)

Infrastructure is code and code should be tested. For Chef cookbooks, it is however up to the user to establish such testing and release workflows, at least when relying solely on non-commercial tooling. This talk introduces the - still far from perfect - implementation of a Chef cookbook CI/CD pipeline used in the TYPO3 open source project, publicly available at [chef-ci.typo3.org](https://chef-ci.typo3.org). The complete setup can be instantiated using a publicly available cookbook and is self-contained, i.e., the cookbook passes itself through its pipeline.

This implementation makes use of the novel Jenkins Pipeline plugins, which allow to define pipelines as code.

By pointing to a GitHub organization, any code changes in its cookbook repos trigger a pipeline execution. After passing successfully through the different test stages, including parallelized test-kitchen runs, the upload to the Chef Server terminates the pipeline.

- Slides at [SlideShare](https://www.slideshare.net/StephenKing/an-opensource-chef-cookbook-cicd-implementation-using-jenkins-pipelines)
- Event: [Config Management Camp (CfgMgmtCamp) Gent](http://cfgmgmtcamp.eu/schedule/chef/steffen-gebert.html)

## Build it, ship it, run it - here and there (Nov 2016)

Kubernetes as cloud infrastructure solution offering cross-platfrom container orchestration for small and big clusters.

- Event: [Datev](https://www.datev.de) TrendScout, Nuremberg

## Continuous Delivery (Nov 2016)

An introduction to continuous delivery.

- Slides at [SlideShare](http://www.slideshare.net/StephenKing/continuous-delivery-68335663)
- Event: [DevOps Meetup Würzburg](https://www.meetup.com/de-DE/DevOps-Wuerzburg-Mainfranken/events/234778486/)

## Jenkins Pipelines (Oct 2016)

Pipelines are a central element of Continuous Delivery and break the delivery process down into multiple stages. By visualizing the flow through the pipeline, the delivery team receives feedback about the status of a change made to the delivered software project.

The implementation of such pipelines using the popular Jenkins CI software has been pretty rough until the public release of a whole set of plugins earlier this year. These pipeline plugins are under very active development and now bring decent support for both pipeline visualization as well as configuration using an own domain specific language.

The talk will give an overview over Jenkins' new pipeline plugins, as well as different ways to define pipelines as code, i.e., manually, by scanning all repos of a Github organization, and via the shared library.
By describing a setup for automated testing and releasing of Chef cookbooks, some of the features offered by Jenkins pipelines will be demonstrated.

- Slides at [SlideShare](http://www.slideshare.net/StephenKing/jenkins-pipelines-67473540)
- Event: [DevOps Meetup Frankfurt](http://www.meetup.com/de-DE/DevOps-Frankfurt/events/234408955/)


## Let's go HTTPS-only! (May 2016)

We are really overdue with encrypting all the things! During this talk, I discussed that setting up HTTPS is more than just buying a
certificate - for the good and for the bad.
On the one hand, one doesn't even have to buy one, when [Let's Encrypt](https://letsencrypt.org) is an option. On the other hand,
there are bunch of options that should be undertaken to make the setup really secure. This includes HTTP Strict Transport Security headers,
correct ciphers, and many more.

- Slides at [SlideShare](http://www.slideshare.net/StephenKing/lets-go-httpsonly-more-than-buying-a-certificate)
- Event: [TYPO3camp Vienna 2016](http://t3cvienna.camp)


## Cleaning up the Dirt of the Ninetees (May 2016)

How new protocols (HTTP/2, QUIC, Multipath TCP) change the web and data transport.

- Slides at [SlideShare](http://www.slideshare.net/StephenKing/cleaning-up-the-dirt-of-the-nineties-how-new-protocols-are-modernizing-the-web)
- Event: [TYPO3camp Vienna 2016](http://t3cvienna.camp)

## [German] 7x7 = Feiner Sand - Architekturalternativen für moderne Anwendungen - Docker, Microservices & PaaS (May 2015)

 - Together with [Prof. Peter Sturm](https://www.uni-trier.de/index.php?id=17497)
 - Event: [Datev](https://www.datev.de) TrendScout, Nuremberg

## SDN Interfaces and Performance Analysis of SDN Components (Sep 2014)

- Sides at [SlideShare](http://www.slideshare.net/StephenKing/sdn-interfaces-and-performance-analysis-of-sdn-components)
- Event: [ZKI Herbsttagung](https://www.rhrk.uni-kl.de/aktuell/zki2014/tagungsprogramm/), Kaiserslautern


## [German] DevOps und Continuous Delivery (July 2014)

- Event: [Datev](https://www.datev.de) TrendScout, Nuremberg


## The Development Infrastructure of the TYPO3 Project (Feb. 2013)

_What tools are needed to run an open-source project?_
This talk gives the answer from a TYPO3 perspective and presents the toolchain.

- Video at [FOSDEM site](https://archive.fosdem.org/2013/schedule/event/the_development_infrastructure_of_the_typo3_project/)
- Event: [FOSDEM 2013](https://archive.fosdem.org/2013/schedule/event/the_development_infrastructure_of_the_typo3_project/) (lightning talk)


## Using Gerrit Code Review in an Open-Source Project (Feb. 2013)

How the TYPO3 project ensures code quality through peer-reviews using "Gerrit Code Review"
This talk presents Gerrit itself and the workflow and lessons learned in a big, community-driven open-source project: the TYPO3 project.
Gerrit is used here for all top-level projects (TYPO3 CMS, TYPO3 Flow, TYPO3 Neos) since two years to handle every single code change.
In addition to human reviews, automatic tests in Jenkins and TravisCI are executed. Gerrit brings transparency into our community-driven
project and allows everybody to participate by pushing patches and voting for/against any change.

- Video at [FOSDEM site](https://archive.fosdem.org/2013/schedule/event/using_gerrit_code_review_in_an_open_source_project/)
- Event: [FOSDEM 2013](https://archive.fosdem.org/2013/schedule/event/using_gerrit_code_review_in_an_open_source_project/) (lightning talk)


## Official typo3.org Infrastructure &  the TYPO3 Server Admin Team (Nov. 2012)

About the TYPO3 open source project's infrastructure and how we (the Server Admin Team) maintain it.

- Slides at [SlideShare](http://www.slideshare.net/StephenKing/die-offizielle-typo3org-infrastruktur-das-typo3-server-admin-team)
- Event: [TYPO3camp Rhein-Ruhr 2012](http://www.typo3camp-rheinruhr.de/)


## [German] Neuigkeiten aus dem TYPO3-Projekt (Oct. 2012)

News from the TYPO3 project. The current status of TYPO3 CMS 6.0, TYPO3 Neos and TYPO3 Flow.

- Slides at [SlideShare](http://www.slideshare.net/StephenKing/neuigkeiten-aus-dem-typo3projekt)
- Event: [1st CMS night, Nuremberg, 2012](http://cmsnue.de/1-joomla-typo3-wordpress-contao-drupal)


## Making of TYPO3 (Dec. 2011)

How TYPO3 is developed.

- Slides at [SlideShare](http://www.slideshare.net/StephenKing/making-of-typo3)
- Event: [TYPO3 User Group Austria (TUGA)](http://tuga.at/)
