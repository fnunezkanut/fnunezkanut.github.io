---
published: true
layout: post
title: Installing Oracle Java8 on CentOS 7
---
## Installing Oracle Java8 on CentOS 7

You can download official Java downloads from [oracle site](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

Or on the command line (CentOS 7)

<!--more-->

    #download from oracle (accepting agreement)
    wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u121-b13/e9e7ea248e2c4826b92b3f075a80e441/jdk-8u121-linux-x64.rpm"

    #install from rpm
    rpm -ivh jdk-8u121-linux-x64.rpm

    #check java version
    java -version
    
    #configure paths
    export JAVA_HOME=/usr/java/jdk1.8.0_121
    export PATH=$PATH:/usr/java/jdk1.8.0_121



Thats it...
