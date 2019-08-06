---
title: Install Ubuntu Server 14.04.4 on VirtualBox
description: Steps to install Ubuntu Server on your Windows/Mac with VirtualBox.
date: 2016-04-15T22:45:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [ubuntu, virtualbox]
author: dalanzg
image:
  url: "img/install-ubuntu-server.png"
  width: 642
  height: 523
comments: true
---

This tutorial will explain how to install Ubuntu Server on a virtual machine.

First, you need to have Ubuntu Server ISO file and VirtualBox installed.

- [VirtualBox](https://www.virtualbox.org/)
- [Ubuntu Server](http://www.ubuntu.com/download/server)

Create a new virtual machine on VirtualBox by clicking on **New**.

{{< bootstrap-figure src="img/create-new-virtual-machine.png" title="Create new virtual machine" width="500">}}

Select type and version of the virtual machine. In this case, Linux and Ubuntu (64-bits).

{{< bootstrap-figure src="img/select-type-and-version.png" title="Select type and version of the virtual machine" width="500">}}

Select the amount of memory (RAM). VirtualBox recommends 768 MB. I selected 1,536 MB to have enough memory.

{{< bootstrap-figure src="img/select-amount-of-memory-ram.png" title="Select the amount of memory (RAM)" width="500">}}

Select the hard disk size. 16 GB is enough.

{{< bootstrap-figure src="img/select-hard-disk-size.png" title="Select the hard disk size" width="500">}}

Click and start the new virtual machine.

{{< bootstrap-figure src="img/click-and-start-virtual-machine.png" title="Click and start the virtual machine" width="500">}}

At this point, you need to load the Ubuntu Server ISO file.

{{< bootstrap-figure src="img/load-ubuntu-server-iso.png" title="Load Ubuntu Server ISO" width="500">}}

Ubuntu Server installation has started. Select the language. In this example, English.

{{< bootstrap-figure src="img/english-language.png" title="English language" width="500">}}

Select **Install Ubuntu Server**.

{{< bootstrap-figure src="img/install-ubuntu-server.png" title="Install Ubuntu Server" width="500">}}

Choose your language, location and country of origin keyboard.

{{< bootstrap-figure src="img/language-ubuntu.png" title="Language Ubuntu Server" width="500">}}

{{< bootstrap-figure src="img/location-ubuntu.png" title="Location Ubuntu Server" width="500">}}

{{< bootstrap-figure src="img/layout-keyboard.png" title="Country of origin keyboard" width="500">}}

Type the Ubuntu Server hostname. This hostname will be used to access the server. I typed **ubuntuvmsrv**. **vm** means virtual machine on the network and **srv** server.

{{< bootstrap-figure src="img/hostname.png" title="Ubuntu Server hostname" width="500">}}

Type your real name, your username and your password.

{{< bootstrap-figure src="img/full-name.png" title="Full name" width="500">}}

{{< bootstrap-figure src="img/username.png" title="Username" width="500">}}

{{< bootstrap-figure src="img/user-password.png" title="User password" width="500">}}

Ubuntu can guide you regarding partitions. This is a virtual machine to test, so I selected **Guided - use entire disk**.

{{< bootstrap-figure src="img/partition-disk.png" title="Partition disk" width="500">}}

Configure updates as you want. I select manually updates.

{{< bootstrap-figure src="img/updates.png" title="Ubuntu updates" width="500">}}

You can choose software installation. I chose (with space bar):

- OpenSSH server
- LAMP server
- Samba file server

{{< bootstrap-figure src="img/software-installation.png" title="Software installation" width="500">}}

Set MySQL server root password.

{{< bootstrap-figure src="img/mysql-root-password.png" title="MySQL server root password" width="500">}}

Select install GRBU boot loader on hard disk.

{{< bootstrap-figure src="img/grbu-boot-loader.png" title="Create new virtual machine" width="500">}}

Congratulations! You have first installed Ubuntu Server on a virtual machine.

{{< bootstrap-figure src="img/installation-complete.png" title="Installation complete" width="500">}}

The virtual machine is restarted, and you will get a prompt to login. Use your username and password to login.

{{< bootstrap-figure src="img/login-ubuntu-server.png" title="Login Ubuntu Server" width="500">}}

{{< bootstrap-figure src="img/ubuntu-server.png" title="Ubuntu Server" width="500">}}

Update your Ubuntu Server with the following command:

```terminal
sudo apt-get upgrade
```

{{< bootstrap-figure src="img/upgrade-ubuntu.png" title="Upgrade Ubuntu Server" width="500">}}

The Ubuntu Server is ready to use. Although, a virtual machine network setting is needed.

We will need two network adapters:

- NAT (Adapter 1)
- Host-only network adapter (Adapter 2)

{{< bootstrap-figure src="img/nat-network-adapter.png" title="NAT network adapter" width="500">}}

{{< bootstrap-figure src="img/host-only-adapter.png" title="Host only adapter" width="500">}}

Check out your virtual machine recognizes the two adapters.

```terminal
$ ls /sys/class/net
eth0 eth1 lo
```

Add the second interface with a static IP address for the host-only adapter (eth1).

```terminal
sudo vim /etc/network/interface
```

Type the following text and save.

```vim
# This file describes the network interface available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface (NAT adapter)
auto eth0
iface eth0 inet dhcp

# The second network interface (Host-only adapter)
auto eth1
iface eth1 inet static
address 192.168.56.101
netmask 255.255.255.0
network 192.168.56.0
broadcast 192.168.56.255
```

Restart the virtual machine.

```terminal
sudo shutdown -h now
```

When Ubuntu Server is started, check out the static IP address for host-only adapter is available to show (eth1).

```terminal
ip addr show
```

{{< bootstrap-figure src="img/static-ip-address-show.png" title="Static IP address for host-only adapter" width="500">}}

Type the IP in your local machine, and you will see the Apache2 Ubuntu Default Page.

{{< bootstrap-figure src="img/apache2-ubuntu-default-page.png" title="Apache2 Ubuntu Default Page" width="500">}}

Enjoy your new virtual machine! 😃
