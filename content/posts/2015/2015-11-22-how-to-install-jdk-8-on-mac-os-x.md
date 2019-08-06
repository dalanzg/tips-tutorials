---
title: How to install JDK 8 on Mac OS X
description: If you are planing to develop Java Apps on your Mac, you have to install the JDK package. You only have to install the binary files provided by Oracle.
date: 2015-11-22T18:00:00+02:00
lastmod: 2018-03-04T16:41:00+01:00
tags: [mac, tomcat]
author: dalanzg
comments: true
---

If you are planing to develop Java Apps on your Mac, you have to install the JDK package. You only have to install the binary files provided by Oracle.

- Download JDK 8 from [Oracle WebSite](http://www.oracle.com/technetwork/java/javase/downloads/index.html) [jdk-8u65-macosx-x64.dmg for Mac]
- Double click on **jdk-8u65-macosx-x64.dmg** and follow the screen instructions
- JDK package will have been installed in **/Library/Java/JavaVirtualMachines**. In my computer, I have both 1.6 and 1.8 JDK package

```terminal
$ ls -l /Library/Java/JavaVirtualMachines
drwxr-xr-x  3 root  wheel  102 14 jul 23:52 1.6.0.jdk
drwxr-xr-x  3 root  wheel  102 20 nov 20:33 jdk1.8.0_65.jdk
```

- Check that your current JDK version is 1.8.

```terminal
$ java -version
java version "1.8.0_65"
Java(TM) SE Runtime Environment (build 1.8.0_65-b17)
Java HotSpot(TM) 64-Bit Server VM (build 25.65-b01, mixed mode)
```

If you are planning to uninstall a JDK version, just remove the folder. In this case, the oldest version.

```terminal
sudo rm -rf /Library/Java/JavaVirtualMachines/1.6.0.jdk
```
