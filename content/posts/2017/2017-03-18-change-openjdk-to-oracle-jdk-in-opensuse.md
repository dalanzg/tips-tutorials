---
title: Change OpenJDK to Oracle JDK in openSUSE
description: When installing openSUSE, OpenJDK is installed by default. This tutorial will explain how to change to Oracle JDK.
date: 2017-03-18T14:14:00+01:00
lastmod: 2017-03-18T14:14:00+01:00
tags: [opensuse, java]
author: dalanzg
comments: true
---

When installing openSUSE, OpenJDK is installed by default. This tutorial will explain how to change to Oracle JDK.

```terminal
dalanz@linux-geij:~> java -version
openjdk version "1.8.0_121"
OpenJDK Runtime Environment (IcedTea 3.3.0) (suse-8.1-x86_64)
OpenJDK 64-Bit Server VM (build 25.121-b13, mixed mode)
```

## Requirements

You will need the following:

- [Oracle JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

## Steps

- Download [Oracle JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html). This case will be Linux x64 (jdk-8u121-linux-x64.tar.gz).

- Unzip *jdk-8u121-linux-x64.tar.gz* file and change the ownership

```terminal
dalanz@linux-geij:~> tar -xf jdk-8u121-linux-x64.tar.gz
dalanz@linux-geij:~> sudo chown -R root:root jdk1.8.0_121
```

- Create a folder in */usr/local/java* to place Oracle Java JDK. */usr/local* is a folder similar to */usr* and remain safe from system software upgrades.

```terminal
dalanz@linux-geij:~> sudo mkdir -p /usr/local/java
dalanz@linux-geij:~> sudo mv jdk1.8.0_121 /usr/local/java
```

- Update **JAVA_HOME**, **$JRE_HOME** and **$PATH** environmental variables in */etc/profile* file to set system wide environmental variables on users shells.

```terminal
dalanz@linux-geij:~> sudo vim /etc/profile
```

```terminal
JAVA_HOME=/usr/local/java/jdk1.8.0_121
JRE_HOME=$JAVA_HOME
PATH=$PATH:$JAVA_HOME/bin

export JAVA_HOME
export JRE_HOME
export PATH
```

- Update alternatives to */usr/bin/java* and */usr/bin/javac*. The system needs to know where the new Java version is placed.

```terminal
dalanz@linux-geij:~> sudo update-alternatives --install /usr/bin/java java /usr/local/java/jdk1.8.0_121/bin/java 1
dalanz@linux-geij:~> sudo update-alternatives --install /usr/bin/javac javac /usr/local/java/jdk1.8.0_121/bin/javac 1
```

```terminal
dalanz@linux-geij:~> sudo update-alternatives --config java
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                       Priority   Status
------------------------------------------------------------
* 0            /usr/lib64/jvm/jre-1.8.0-openjdk/bin/java   1805      auto mode
  1            /usr/lib64/jvm/jre-1.7.0-openjdk/bin/java   1705      manual mode
  2            /usr/lib64/jvm/jre-1.8.0-openjdk/bin/java   1805      manual mode
  3            /usr/local/java/jdk1.8.0_121/bin/java       1         manual mode

Press <enter> to keep the current choice[*], or type selection number: 3
update-alternatives: using /usr/local/java/jdk1.8.0_121/bin/java to provide /usr/bin/java (java) in manual mode
```

```terminal
dalanz@linux-geij:~> sudo update-alternatives --config javac
There is only one alternative in link group javac (providing /usr/bin/javac): /usr/local/java/jdk1.8.0_121
```

- Load the system **$PATH** variable.

```terminal
dalanz@linux-geij:~> source /etc/profile
```

- Reboot your system and check the new Java version.

```terminal
dalanz@linux-geij:~> java -version
java version "1.8.0_121"
Java(TM) SE Runtime Environment (build 1.8.0_121-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.121-b13, mixed mode)
```
