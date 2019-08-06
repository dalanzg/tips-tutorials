---
title: How to create .bash_profile file on Mac OS X
description: By default, your system does not have the **.bash_profile** file for your user. Therefore you will need to create it to set environment variables for your current user when starting new session.
date: 2016-04-09 22:00:00+0200
lastmod: 2018-03-17 10:15:00+0100
tags: [mac]
author: dalanzg
comments: true
---

By default, your system does not have the **.bash_profile** file for your user. Therefore you will need to create it to set environment variables for your current user when starting new session.

In this tutorial, we are going to create a .bash_profile to set **$JAVA_HOME** environment variable.

- First, we are going to check out that .bash_profile does not exist.

```terminal
$ ls ~/.bash_profile
The file /Users/dalanz/.bash_profile does not exist.
```

- Go to user home and create the **.bash_profile** file.

```terminal
cd ~
vim .bash_profile
```

- Push **i** key to insert contents with VIM editor, and write the environment variable.

```vim
export JAVA_HOME=$(/usr/libexec/java_home)
```

- Push **esc** key and type **:wq** to save and close the file. Then, reload the .bash_profile file.

```terminal
source .bash_profile
```

- The .bash_profile has been executed, and **$JAVA_HOME** has been set.

```terminal
$ echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/jdk1.8.0_65.jdk/Contents/Home
```
