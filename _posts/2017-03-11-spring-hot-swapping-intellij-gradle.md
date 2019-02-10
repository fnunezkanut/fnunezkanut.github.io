---
published: true
layout: post
title: How to enable hot swapping in Intellij IDEA for a Spring Boot project
---
## How to enable hot swapping in Intellij IDEA for a Spring Boot project

This quick how-to describes how to enable hot swapping in your Spring Boot Project both in Gradle and Maven, since having to constantly manually reload is highly annoying :).

<!--more-->

### Step 1 ###

If using Gradle, in your **build.gradle** file add this line in dependencies

	dependencies {

		compile 'org.springframework.boot:spring-boot-devtools'
		#other dependencies
	}

Otherwise of using Maven, in your **pom.xml** file add this block to dependencies

	<dependency>
	    <groupId>org.springframework.boot</groupId>
	    <artifactId>spring-boot-devtools</artifactId>
	    <optional>true</optional>
	</dependency>

### Step 2 ###

Then in Intellij goto:

File –> Setting –> Build, Execution, Deployment –> Compiler

check "Build project automatically" like so

![Build project automatically]({{site.baseurl}}/images/intellij1.jpg)

### Step 3 ###

- Press SHIFT+CTRL+A (Windows/Linux) or Command+CTRL+A (Mac) to open a pop-up window
- Type "registry"
- Make sure the option _compiler.automake.allow.when.app.running_ is checked

