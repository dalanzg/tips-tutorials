---
title: Share transient folder in openSUSE with VirtualBox
description: Steps to share transient folder in openSUSE with VirtualBox without rebooting the virtual machine.
date: 2017-03-17T19:00:00+01:00
lastmod: 2017-03-18T14:14:00+01:00
tags: [opensuse, virtualbox]
author: dalanzg
image:  
  url: "img/transient-folder.png"
  width: 655
  height: 372
comments: true
---

This tutorial will explain how to share a host holder into your guest virtual machine.

## Requirements

You will need the following:

- An openSUSE virtual machine with VirtualBox ([Check this link]({{< ref "/posts/2017/2017-02-12-how-to-install-opensuse-leap-42.2-in-virtualbox/index.md" >}})

## Steps

- [Make transient folder](#make-transient-folder)
- [Mount folder](#mount-folder)
- [Copy and paste files](#copy-and-paste-files)

### Make transient folder

Go to share folder settings and create a new transient folder.

{{< bootstrap-figure src="img/create-transient-folder.png" title="Create a new transient folder" width="500">}}

{{< bootstrap-figure src="img/share-folder-name.png" title="Give the name to the transient folder" width="500">}}

{{< bootstrap-figure src="img/transient-folder.png" title="New transient folder created" width="500">}}

It is important to remember the transient folder name.

### Mount folder

Open terminal in guest machine and mount the transient folder.

```terminal
dalanz@linux-geij:~> mkdir transFolder
dalanz@linux-geij:~> sudo mount -t vboxsf share transFolder
dalanz@linux-geij:~> cd transFolder
dalanz@linux-geij:~/transFolder/> ll
```

{{< bootstrap-figure src="img/mount-transient-folder.png" title="Mount transient folder" width="500">}}

### Copy and paste files

Files from host machine are reachable. Only copy and paste file.
