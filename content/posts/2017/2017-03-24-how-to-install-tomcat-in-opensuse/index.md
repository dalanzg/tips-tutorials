---
title: How to install Tomcat in openSUSE
description: Steps to deploy and install Tomcat core in openSUSE.
date: 2017-03-24T22:50:00+01:00
lastmod: 2017-03-24T22:50:00+01:00
tags: [opensuse, tomcat]
author: dalanzg
image:  
  url: "img/tomcat-home-page.png"
  width: 1206
  height: 589
comments: true
---

## Requirements

You will need the following:

- [Tomcat 8](https://tomcat.apache.org/download-80.cgi)

Optional -> ([Change Java OpenJDK to Oracle JDK]({{< ref "/posts/2017/2017-03-18-change-openjdk-to-oracle-jdk-in-opensuse" >}}) to run Tomcat.

## Steps

- Download [Tomcat 8](https://tomcat.apache.org/download-80.cgi). This case will be Apache Tomcat 8.0.42 -> Core tar.gz (apache-tomcat-8.0.42.tar.gz).

- Unzip **apache-tomcat-8.0.42.tar.gz** file and place it in folder */usr/local/tomcat*. */usr/local* is a folder similar to */usr* and remain safe from system software upgrades.

```terminal
dalanz@linux-geij:~> tar -xf apache-tomcat-8.0.42.tar.gz
dalanz@linux-geij:~> sudo mkdir -p /usr/local/tomcat
dalanz@linux-geij:~> sudo mv apache-tomcat-8.0.42 /usr/local/tomcat
dalanz@linux-geij:~> cd /usr/local/tomcat/apache-tomcat-8.0.42/
dalanz@linux-geij:/usr/local/tomcat/apache-tomcat-8.0.42> ll
total 112
drwxr-xr-x 2 dalanz users  4096 Mar 24 22:39 bin
drwxr-xr-x 2 dalanz users  4096 Mar  8 20:59 conf
drwxr-xr-x 2 dalanz users  4096 Mar 24 22:39 lib
-rw-r--r-- 1 dalanz users 57011 Mar  8 20:59 LICENSE
drwxr-xr-x 2 dalanz users  4096 Mar  8 20:58 logs
-rw-r--r-- 1 dalanz users  1444 Mar  8 20:59 NOTICE
-rw-r--r-- 1 dalanz users  6741 Mar  8 20:59 RELEASE-NOTES
-rw-r--r-- 1 dalanz users 16195 Mar  8 20:59 RUNNING.txt
drwxr-xr-x 2 dalanz users  4096 Mar 24 22:39 temp
drwxr-xr-x 7 dalanz users  4096 Mar  8 20:58 webapps
drwxr-xr-x 3 dalanz users  4096 Mar 24 22:40 work
```

As you can see, user **dalanz** and group **users** can run Tomcat, since they can execute bin files and write files in *temp* and *logs* folders.

- Run the startup script.

```terminal
dalanz@linux-geij:~> /usr/local/tomcat/apache-tomcat-8.0.42/bin/startup.sh
Using CATALINA_BASE:   /usr/local/tomcat/apache-tomcat-8.0.42
Using CATALINA_HOME:   /usr/local/tomcat/apache-tomcat-8.0.42
Using CATALINA_TMPDIR: /usr/local/tomcat/apache-tomcat-8.0.42/temp
Using JRE_HOME:        /usr/local/java/jdk1.8.0_121
Using CLASSPATH:       /usr/local/tomcat/apache-tomcat-8.0.42/bin/bootstrap.jar:/usr/local/tomcat/apache-tomcat-8.0.42/bin/tomcat-juli.jar
Tomcat started.
```

- Check you can go to Tomcat home page -> [http://localhost:8080](http://localhost:8080)

{{< bootstrap-figure src="img/tomcat-home-page.png" title="Tomcat home page" width="500">}}
