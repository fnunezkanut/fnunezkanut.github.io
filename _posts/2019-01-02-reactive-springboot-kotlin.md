---
published: true
layout: post
title: 'Exploring Reactive Spring Boot (in Kotlin)'
---
## Exploring Reactive Spring Boot (in Kotlin)

I have been using Kotlin in Spring Boot applications for some time now, and have decided to write an article examining a bare minimal springboot 2 application written in Kotlin which makes use of new reactive and functional SpringBoot goodies.

<!--more-->

I therefore decided to create a very minimal example springboot application, whose purpose is to have one endpoint which returns a small bit of JSON containing current datetime and a randomly generated UUID. The plan is to use is this project in subsequent articles which will benchmark and load test various new Spring Boot features.

### Getting started
Starting any Spring Boot project involves spending a few minutes organizing boilerplate, however this is made easier by simply generating the boilerplate at https://start.spring.io/ (tip: or using the Spring Boot new project wizard in IntelliJ)

![]({{site.baseurl}}/images/2019-01-02_01.png)

I chose Kotlin + Gradle + Reactive Web dependency.

### The Gradle file
Added ```jcenter()``` repository as its quite good, also AssertJ which is useful during testing (more on this later), then bumped out Kotlin version as the Spring Initializer seems to be slightly out of date at this time.

I then added a gradle wrapper task, this is quite useful to generate gradle wrapper files, which makes the project more self contained and portable, not having to rely on exact version being installed on someone else's machine when working in a team.
```
wrapper {
    gradleVersion = "5.2.1"
}
```

Also added a jar task, this can be useful to modify the jar output
```
jar {
    manifest {
        attributes("Implementation-Title": "SpringBoot Kotlin Reactive Loadtarget")
    }
}
```

### Entrrypoint into our Spring Boot up
Lets take a quick look at the dozen lines that make up *App.kt* file, this is the entry point into the Spring Boot project
```kotlin
package com.github.fnunezkanut

import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@SpringBootApplication
class App

fun main(args: Array<String>) {
    runApplication<App>(*args)
}
```

That is it :D ain't Kotlin beautiful?! Compared to Java's verbosity Kotlin is a pleasure to write and read as it does away with a lot of cruft and boilerplate that Java is infamous for. ```@SpringBootApplication``` tells Spring Boot this is a special class, and the standalone main method starts the app. That's all!

### Route configuration
In the old Spring Boot style controllers would be heavily annotated, which is just another way of configuration in code which made Spring famous. However one of the ongoing additions to Spring Boot 2 is functional configuration/coding, here we can do things differently to the classic Spring Way. IMHO this makes code much more readable, but it could be jarring (ha!) at first, it seems Spring took heavy inspiration from other frameworks such as Vert.x for this. 
So in the file *RouteConfiguration.kt* we see a single configuration class with one Bean into which a handler is injected (more on that below), that handles GET requests on ```/uuid```, so far so good right?

### Handler
Now lets take a look a this UUID handler, this is the meat of the project where the required logic occurs, here we will finally see some reactive concepts.
```kotlin
@Component
class UuidHandler {
```

Notice how we do not annotate with @Controller or @GetController but with @Component, this just helps wire things up in Spring.

```kotlin
fun main(req: ServerRequest): Mono<ServerResponse> {
```
Here we have a method imaginatively called main, that takes ServerRequest and returns a Mono of a ServerResponse, aka it returns a promise of ServerResponse (More on reactive concept of Mono and Flux at some later time perhaps)

The next few lines we just create 2 objects, add these to UuidResponse (which you might have noticed is heavily annotated with jackson specific annotations), which we convert to a Mono (think of Mono as a Promise of one thing only)

Finally we respond with a 200 and json formatted uuid response object
```kotlin
return ok().body(uuidPromise, UuidResponse::class.java)
```

In short our handler doesnt return an Uuid object but a promise of one, this is most minimal example I can think of, where reactive comes into full bloom is using reactive libraries such as Reactive Cloud Stream, Redis and MongoDB, these can make the most of advanced reactive concepts such as backpressure. Unfortunately no reactive SQL libraries exist yet at this time.

### Application properties
In the resources directory you might have noticed two application.properties files, these allow us to have separate config between dev and normal application run. 

### Testing
Of course no good example is complete without tests (and one can not be certain it functions as expected without any tests!) so in the test directory you would find a single test which exercises this single endpoint.
```kotlin
@RunWith(SpringRunner::class)
@SpringBootTest
class AppTests {

    lateinit var client: WebTestClient

    @Autowired
    lateinit var routerFunction: RouterFunction<ServerResponse>

    @Before
    fun setUp() {
        client = WebTestClient.bindToRouterFunction(routerFunction).build()
    }

    @Test
    fun getUuid() {

        //given

        //when
        var result = client.get().uri("/uuid").exchange().expectStatus().isOk.returnResult<UuidResponse>()
        var uuidResponse = result.responseBody.blockFirst()

        //then
        assertThat(result.status.value()).isEqualTo(200)
        assertThat(uuidResponse).isExactlyInstanceOf(UuidResponse::class.java)
    }
}
```
Here we use assertj to assert our expectations match reality during a test

### Building and running
With the built in gradle wrapper, building this (and running tests) is straightforward, on the console
```bash
cd /path/to/project
./gradlew clean build
```
You will notice a spring boot jar pop into existence in the build folder. You can run this directly with ```java -jar output.jar``` command, or read on about how to Dockerize and run this project in a minimal Docker container

### Docker
I have added a minimal Dockerfile which creates a container on top of the excellent Alpine Linux project, sticks the resulting jar file into it and the adds and entrypoint, so far quite bog standard, notice several optimisations I often use in my Java containers ```"-XX:+UnlockExperimentalVMOptions","-XX:+UseCGroupMemoryLimitForHeap","-XX:MaxRAMFraction=2"``` These help the app make better use of memory in container.

In the Readme.md i added sample commands to build and run this container
```bash
#goto project root
cd ~/projects/springboot-kotlin-reactive-loadtarget/
#build image
docker build -t loadtarget .
#list images
docker images
#start a container
docker run -p 8080:8080 --memory 512M --memory-swap 512M -it loadtarget
```

### Show me the code!
The source code for all of this can be found on my github [springboot-kotlin-reactive-loadtarget](https://github.com/fnunezkanut/springboot-kotlin-reactive-loadtarget) [updated: 2019-02-10]

Hopefully this article and example helps others exploring Spring Boot and Kotlin :)
