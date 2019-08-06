---
title: Setting up a Virtual Web Server with VirtualBox, Apache, MySQL, FTP, Ubuntu, and Samba
description: Steps to set up a Ubuntu Server virtual machine. You will be able to public your HTML files, transfer files by FTP and network share.
date: 2016-04-17T15:00:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [ubuntu, virtualbox]
author: dalanzg
image: 
  url: "img/apache2-ubuntu-default-page.png"
  width: 1280
  height: 796
comments: true
---

This tutorial will explain how to set up a Ubuntu Server virtual machine with:

- Apache
- FTP
- Samba

First, check out [how to install Ubuntu Server on VirtualBox]({{< ref "/posts/2016/2016-04-15-install-ubuntu-server-on-virtualbox" >}}) to get ready a virtual machine to test.

Our virtual machine has two network adapters. The second one (eth1) is a host-only adapter with a static IP address -> **192.168.56.101**

## Apache Web Server

Apache Web Server needs to be running. The installation and configuration was indicated in the previous post. Type the IP address on your browser to see Apache2 default page.

{{< bootstrap-figure src="img/apache2-ubuntu-default-page.png" title="Apache2 Ubuntu Default Page" width="500">}}

## FTP

Install **proftd** on Ubuntu server:

```terminal
sudo apt-get install proftpd
```

Select the ProFTPD configuration. You can choose between:

- from inetd
- standalone

I selected **standalone**.

{{< bootstrap-figure src="img/proftpd-configuration.png" title="ProFTPD configuration" width="500">}}

Install on your local machine a FTP client. I recommend [FileZilla](https://filezilla-project.org/).

Now, we are going to verify our static IP Address for host-only network adapter (eth1).

```terminal
ifconfig
```

{{< bootstrap-figure src="img/server-for-ftp.png" title="Server for FTP connection" width="500">}}

Type server, user and password in your FTP client to connect.

{{< bootstrap-figure src="img/filezilla-connection.png" title="FileZilla connection" width="500">}}

## Samba

Edit Samba configuration file.

```terminal
sudo vi smb.conf
```

Add at the end the following:

{{< bootstrap-figure src="img/samba-configuration.png" title="Samba file configuration" width="500">}}

Type **:wq** to save and close. Then, restart Samba server.

```terminal
sudo restart smbd
```

Type your user login for network share.

{{< bootstrap-figure src="img/user-login-for-network-share.png" title="User login for network share" width="500">}}

You will be able to share files from your PC to the virtual machine by network.

{{< bootstrap-figure src="img/network-share.png" title="Network share" width="500">}}
