---
title: How to set JAVA_HOME on Mac OS X
description: If you are planing to develop Java Apps on your Mac, you may have to set $JAVA_HOME environment variable.
date: 2016-03-21T18:00:00+01:00
lastmod: 2018-03-04T16:41:00+01:00
tags: [mac, java]
author: dalanzg
comments: true
---

If you are planing to develop Java Apps on your Mac, you may have to set **$JAVA_HOME** environment variable.

## Install the latest Java Virtual Machine

Go to [How to install JDK on Mac OS X]({{< ref "/posts/2015/2015-11-22-how-to-install-jdk-8-on-mac-os-x" >}})) to install a new Java Virtual Machine. Download the latest Java JDK package from Oracle.

## Check Java Virtual Machines

List directories in the following root -> **/Library/Java/JavaVirtualMachines**

```terminal
$ ls -l /Library/Java/JavaVirtualMachines
drwxr-xr-x  3 root  wheel  102 23 dic 20:36 jdk1.8.0_66.jdk
drwxr-xr-x  3 root  wheel  102 21 mar 14:20 jdk1.8.0_74.jdk
```

And the version for **java** will be the latest.

```terminal
$ java -version
java version "1.8.0_74"
Java(TM) SE Runtime Environment (build 1.8.0_74-b02)
Java HotSpot(TM) 64-Bit Server VM (build 25.74-b02, mixed mode)
```

## Set JAVA_HOME environment variable

Open your terminal, and create **.bash_profile** file if it does not exist.

```terminal
vim .bash_profile
```

And write the following:

```vim
export JAVA_HOME=$(/usr/libexec/java_home)
```

Refresh the environment variables by running the following command, and check the **$JAVA_HOME** value:

```terminal
$ source .bash_profile
$ echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk1.8.0_74.jdk/Contents/Home
```
