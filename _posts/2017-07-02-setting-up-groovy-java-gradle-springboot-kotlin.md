---
published: true
layout: post
title: 'Quickly up and running with Groovy, Java8, Gradle, Spring Boot and Kotlin'
---

## Quickly up and running with Groovy, Java8, Gradle, Spring Boot and Kotlin

Assuming you have Cygwin installed on your Windows machine (with unzip and curl packages) then this straightforward post will help anyone quickly setup an up to data Javaish development environment up and running. For Linux users its even easier of course...

<!--more-->

Anyways in your cygwin command window type this to install SDKman which will do all the heavy work for you

    curl -s "https://get.sdkman.io" | bash

Then simply install each one of these

	sdk install java
	sdk install groovy
	sdk install gradle
	sdk install kotlin
	sdk install springboot

And that's it! you are ready to develop :)

![sdkman]({{site.baseurl}}/images/sdkman.png)
