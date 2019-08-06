---
title: How to install openSUSE Leap 42.2 in VirtualBox
description: Steps to install openSUSE Leap 42.2 with VirtualBox.
date: 2017-02-12T13:00:00+01:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [opensuse, virtualbox]
author: dalanzg
image:  
  url: "img/select-installation.png"
  width: 800
  height: 643
comments: true
---

This tutorial will explain how to install openSUSE 42.2 Leap in VirtualBox.

## Requirements

You will need the following:

- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [openSUSE Leap 42.2 ISO file](https://software.opensuse.org/422/en)

## Steps

- [Preparing the virtual machine](#preparing-the-virtual-machine)
- [Installing openSUSE](#installing-opensuse)
- [Post-installation tasks](#post-installation-tasks)

### Preparing the virtual machine

Open VirtualBox and create a new virtual machine. Select Linux type and openSUSE version.

{{< bootstrap-figure src="img/new-virtual-machine.png" title="New openSUSE virtual machine" width="500">}}

Select the amount of memory RAM. The recommendation is 4096 MB (4 GB).

{{< bootstrap-figure src="img/memory-size.png" title="4096-MB memory RAM" width="500">}}

Create a new virtual hard disk.

{{< bootstrap-figure src="img/new-hard-disk.png" title="New virtual hard disk" width="500">}}

Select VirtualBox Disk Image.

{{< bootstrap-figure src="img/vdi-hard-disk.png" title="VirtualBox Disk Image" width="500">}}

And dynamically allocated.

{{< bootstrap-figure src="img/dynamically-allocated.png" title="Dynamically allocated" width="500">}}

The amount of memory size will be 80 GB. As we selected dynamically allocated, the real size of hard disk will be growing until reaching 80 GB.

{{< bootstrap-figure src="img/set-80gb-size.png" title="80-GB memory size" width="500">}}

### Installing openSUSE

We have created a new empty virtual machine. We need to set more properties and load ISO file. Go to settings of the new virtual machine.

{{< bootstrap-figure src="img/settings-virtual-machine.png" title="Virtual machine settings" width="500">}}

Enable bidirectional shared clipboard and drag and drop functionality in General tab.

{{< bootstrap-figure src="img/shared-clipboard-and-drag-and-drop.png" title="Bidirectional shared clipboard and drag and drop" width="500">}}

Increase number of processors by 2 in System tab.

{{< bootstrap-figure src="img/increase-processors.png" title="2 processors" width="500">}}

And increase video memory by 128 MB in Display tab.

{{< bootstrap-figure src="img/set-128mb-video-memory.png" title="128-MB memory size" width="500">}}

Load openSUSE ISO file in Storage tab.

{{< bootstrap-figure src="img/opensuse-optical-drive-1.png" title="Load ISO openSUSE" width="500">}}

{{< bootstrap-figure src="img/opensuse-optical-drive-2.png" title="Load ISO openSUSE" width="500">}}

Let's start and first run the virtual machine.

{{< bootstrap-figure src="img/start-virtual-machine.png" title="First start the virtual machine" width="500">}}

The ISO file will be loaded when starting the virtual machine. Select installation.

{{< bootstrap-figure src="img/select-installation.png" title="Select installation" width="500">}}

Set language and keyboard.

{{< bootstrap-figure src="img/language-and-keyboard.png" title="Language and keyboard" width="500">}}

Continue the installation options.

{{< bootstrap-figure src="img/installation-options.png" title="Continue the installation" width="500">}}

Edit suggested partition. We will install using one partition and Ext4 file system for root partition.

{{< bootstrap-figure src="img/only-one-partition-1.png" title="Only one partition" width="500">}}

{{< bootstrap-figure src="img/only-one-partition-2.png" title="Only one partition" width="500">}}

{{< bootstrap-figure src="img/only-one-partition-3.png" title="Only one partition" width="500">}}

Set Clock and Time Zone.

{{< bootstrap-figure src="img/clock-and-time-zone.png" title="Clock and Time Zone" width="500">}}

And here the Desktop type. If you only want openSUSE as server, select server (text mode). In this case, we will use KDE Plasma Desktop.

{{< bootstrap-figure src="img/kde-plasma-desktop.png" title="KDE Plasma Desktop" width="500">}}

Local user to login.

{{< bootstrap-figure src="img/local-user.png" title="Local user" width="500">}}

Disable firewall and enable SSH service.

{{< bootstrap-figure src="img/firewall-and-ssh-1.png" title="Firewall and enable SSH" width="500">}}

{{< bootstrap-figure src="img/firewall-and-ssh-2.png" title="Firewall and enable SSH" width="500">}}

Confirm the installation and wait.

{{< bootstrap-figure src="img/confirm-installation.png" title="Install openSUSE" width="500">}}

{{< bootstrap-figure src="img/installing-opensuse.png" title="Install openSUSE" width="500">}}

When finished, openSUSE will be rebooted.

{{< bootstrap-figure src="img/opensuse-installed.png" title="Install openSUSE" width="500">}}

### Post-installation tasks

Shutdown the virtual machine, and remove the ISO file from virtual drive that was loaded previously.

{{< bootstrap-figure src="img/remove-disk-from-virtual-drive.png" title="Remove ISO file" width="500">}}

Next, start the virtual machine and update the system with the last software and libraries.

{{< bootstrap-figure src="img/check-software-updates-1.png" title="Update system" width="500">}}

{{< bootstrap-figure src="img/check-software-updates-2.png" title="Update system" width="500">}}

{{< bootstrap-figure src="img/check-software-updates-3.png" title="Update system" width="500">}}

{{< bootstrap-figure src="img/check-software-updates-4.png" title="Update system" width="500">}}

And finally, shutdown the virtual machine to make a snapshot. This snapshot will let you restore a fresh openSUSE installation.

{{< bootstrap-figure src="img/make-snapshot-1.png" title="Make a snapshot" width="500">}}

{{< bootstrap-figure src="img/make-snapshot-2.png" title="Make a snapshot" width="500">}}

{{< bootstrap-figure src="img/make-snapshot-3.png" title="Make a snapshot" width="500">}}
