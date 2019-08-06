---
title: How to install Tomcat 7 on Mac OS X
description: Download the Tomcat binaries for MAC, and you will be able to start and stop your Application Server.
date: 2015-11-21T18:00:00+01:00
lastmod: 2019-08-06T13:00:00+02:00
tags: [mac, tomcat]
author: dalanzg
comments: true
---

Requirements: Java (at least 1.6).

First, check your java version by opening the terminal:

```terminal
$ java -version
java version "1.8.0_65"
Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.65-b01, mixed mode)
```

Follow these steps:

- Download the binary Core Distribution from the original website (apache-tomcat-7.0.65.tar.gz): [Apache Tomcat 7](https://tomcat.apache.org/download-70.cgi)
- Unzip the file downloaded
- Create a Tomcat folder in /Library. There we can leave several Tomcat versions to test

```terminal
sudo mkdir -p /Library/Tomcat
sudo mv ~/Downloads/apache-tomcat-7.0.65 /Library/Tomcat
```

- Make all scripts executable

```terminal
sudo chmod +x /Library/Tomcat/apache-tomcat-7.0.65/bin/*.sh
```

## Starting Tomcat

```terminal
sudo ./Library/Tomcat/apache-tomcat-7.0.65/bin/startup.sh
```

Go to [http://localhost:8080](http://localhost:8080).

## Stopping Tomcat

```terminal
sudo ./Library/Tomcat/apache-tomcat-7.0.65/bin/shutdown.sh
```
