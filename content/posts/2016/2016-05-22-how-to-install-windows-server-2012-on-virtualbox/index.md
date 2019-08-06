---
title: How to install Windows Server 2012 R2 on VirtualBox
description: Steps to install Windows Server 2012 R2 on your Windows/Mac with VirtualBox.
date: 2016-05-22T09:45:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [windows, virtualbox]
author: dalanzg
image: 
  url: "img/windows-setup.png"
  width: 1024
  height: 796
comments: true
---

This tutorial will explain how to install Windows Server 2012 R2 in a virtual machine.

First, you need to have Windows Server 2012 R2 ISO file and VirtualBox installed.

Create a new virtual machine on VirtualBox by clicking on **New**. Select type and version of the virtual machine. In this case, Windows 2012 (64-bits).

{{< bootstrap-figure src="img/create-new-windows-2012-virtual-machine.png" title="Create new virtual machine" width="500">}}

Set the amount of memory (RAM). I selected 4096 MB (4 GB).

{{< bootstrap-figure src="img/set-4gb-memory-ram.png" title="Set the amount of memory" width="500">}}

Create the virtual hard disk and set its size. I selected 40 GB.

{{< bootstrap-figure src="img/create-virtual-hard-disk.png" title="Create virtual hard disk" width="500">}}

{{< bootstrap-figure src="img/virtual-hard-disk-size.png" title="Set virtual hard disk size" width="500">}}

When starting the virtual machine, you will be asked to select the ISO file. Select the Windows Server 2012 R2 ISO file.

{{< bootstrap-figure src="img/select-iso-file-when-starting.png" title="Select the virtual optical disk file" width="500">}}

The installation wizard will have been started. Select your language, time and keyboard input. Then, start the installation.

{{< bootstrap-figure src="img/windows-setup.png" title="Windows setup" width="500">}}

{{< bootstrap-figure src="img/install-windows-2012.png" title="Install Windows Server 2012 R2" width="500">}}

Select type of installation. In this case, **Custom: Install Windows only (advanced)**. Hence, choose the partition to install windows and wait to finish the installation.

{{< bootstrap-figure src="img/type-of-installation.png" title="Type of installation" width="500">}}

{{< bootstrap-figure src="img/select-disk-partition.png" title="Partition for Windows Server" width="500">}}

{{< bootstrap-figure src="img/installing-windows.png" title="Installing Windows Server 2012 R2" width="500">}}

When finishing the installation, the virtual machine will be restarted. Then, type the administrator password and Windows Server 2012 R2 will be ready.

{{< bootstrap-figure src="img/administrator-password.png" title="Administrator password" width="500">}}

{{< bootstrap-figure src="img/windows-server-2012-r2.png" title="Windows Server 2012 R2" width="500">}}

Enjoy your new virtual machine! 😃
