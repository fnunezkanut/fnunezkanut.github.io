---
published: true
layout: post
title: 'Self contained jar to help with black box testing'
---
## Self contained jar to help with black box testing

After reading [this presentation on microservice testing](https://martinfowler.com/articles/microservice-testing/) from [Tobias Clemson](https://github.com/tobyclemson) I got thinking about a problem recently I encountered working on a project which has a microservice'ish architechture. The problem is how to isolate a  service which has a REST api and black box test it from end to end.

<!--more-->

So I spent a few hours looking around at existing solutions and surprisingly enough most I found lacking or abandoned or needlessly complicated or forcing you to edit the black box you want to test (crazy right?!).

To illustrate I want to black box test the inputs/outputs at both ends for the pink middleware service in this diagram below
![]({{site.baseurl}}/images/test1.png)

There are several levels of testing:

* unit and integration testing can help you be confident that a service (at a class/component level) does what it say would do (they would occur within the pink box above for example)

* system and acceptance testing are higher up from these and in context of microservices are usefull to check if a specification/contract is met.

![]({{site.baseurl}}/images/test2.png)

At the backend side of the service I want to test I have found [stubby4j](https://github.com/azagniotov/stubby4j) to be the perfect tool for the job.

However due to specific environment settings, 3rd party dependancies, and no gradle install available on server where the middleware service i want to blackbox test runs. I realized that I would need a selfcontained jar file which would contain all my black box acceptance tests. I looked around and [tespackage](https://github.com/testpackage/testpackage) seems to do the job but on closer examination I found this maven project to be needlessly complicated and not maintained.

So having to scratch an itch I fired up my IDE and threw together the beginnings of a springboot project which can generate a self contained jar that would run junit tests annotated with my custom @BBTest annotation

This project is called [groovy-springboot-bbtester](https://github.com/fnunezkanut/groovy-springboot-bbtester)
