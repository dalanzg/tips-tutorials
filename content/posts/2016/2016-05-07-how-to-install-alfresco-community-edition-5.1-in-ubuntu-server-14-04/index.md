---
title: How to install Alfresco Community Edition 5.1 in Ubuntu Server 14.04
description: Alfresco Community Edition 5.1 is installed in Ubuntu Server virtual machine.
date: 2016-05-07T14:00:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [ubuntu, alfresco]
author: dalanzg
image: 
  url: "img/alfresco-repository.png"
  width: 530
  height: 460
comments: true
---

This tutorial will explain how to install Alfresco Community Edition 5.1 in Ubuntu Server 14.04. In this example, Ubuntu Server is a virtual machine made by VirtualBox.

## Requirements

- Ubuntu Server virtual machine with:
  - Two network adapters:
    - NAT adapter (eth0)
    - Host-only adapter (eth1) with a static IP address -> **192.168.56.101**
  - Base memory: 4608 Mb
  - 2 CPUs
- [Alfresco Community Edition binary file](https://www.alfresco.com/products/community/download) -> **alfresco-community-installer-201604-linux-x64.bin**

Check out [how to install Ubuntu Server on VirtualBox]({{< ref "/posts/2016/2016-04-15-install-ubuntu-server-on-virtualbox" >}}) to get ready a virtual machine to install Alfresco Community Edition.

## Make directories for installation and application

Make two directories:

- **/opt/alfsource** -> Installation directory
- **/opt/alfresco** -> Application directory

```terminal
sudo mkdir /opt/alfsource
sudo mkdir /opt/alfresco
```

Change permissions for the directories.

```terminal
sudo chmod 777 /opt/alfsource
sudo chmod 777 /opt/alfresco
```

Upload, for instance by sFTP, the binary file **alfresco-community-installer-201604-linux-x64.bin** to **/opt/alfsource** directory.

{{< bootstrap-figure src="img/upload-sftp-alfresco-binary.png" title="Upload Alfresco binary file" width="500">}}

And change permissions for the binary file.

```terminal
$ chmod 777 /opt/alfsource/alfresco-community-installer-201604-linux-x64.bin
```

## Libraries for LibreOffice

If you run the installer, you will get a warning about libraries needed for LibreOffice.

In order to make work LibreOffice you need some libraries installed on your system:

- fontconfig
- libSM
- libICE
- libXrender
- libXext
- libcups

Install the libraries running the following command:

```terminal
sudo apt-get install libfontconfig1 libsm6 libice6 libxrender1 libxt6 libcups2
```

## Alfresco installation

Run the installation. Hence, go to the installation folder and run the binary file.

```terminal
cd /opt/alfsource
sudo ./alfresco-community-installer-201604-linux-x64.bin
```

You will still get the warning when running the installation. Ignore it and continue.

{{< bootstrap-figure src="img/libraries-libreoffice-alfresco.png" title="Libraries needed for LibreOffice" width="500">}}

You can install in 2 ways:

- [1] Easy – everything done on defaults
- [2] Advanced- make your own choices on which options you want

Use [2] to change some installation options. In this tutorial, we will set the installation path and Web Server domain:

- Installation path -> **/opt/alfresco**
- Web Server domain -> **192.168.56.101**

This is the installation configuration:

- Language - English
- Installation type – Advanced
- Java – yes
- PostgreSQL – yes
- LibreOffice – yes
- Alfresco Community Edition - yes
- Solr1 – no
- Solr4 - yes
- Alfresco Office Services - yes
- Web Quick Start – yes
- Google Docs Integration – yes

And then,

- Installation Folder - /opt/alfresco
- Database Server Parameters – port 5432
- Tomcat Port Configuration – Web Server domain = 192.168.56.101
– Tomcat Server Port = 8080
- Tomcat Shutdown Port = 8005
- Tomcat SSL Port = 8443
- Tomcat AJP Port = 8009
- LibreOffice Server Port = 8100
- Alfresco FTP Port = 2121
- Admin Password = ******

{{< bootstrap-figure src="img/installation-type-and-components.png" title="Installation type and Components" width="500">}}

{{< bootstrap-figure src="img/installation-folder-database-server-parameters-and-tomcat-port-configuration.png" title="Installation type and Components" width="500">}}

{{< bootstrap-figure src="img/libreoffice-and-alfresco-ports.png" title="LibreOffice and Alfresco Ports" width="500">}}

The installation will have already started, and you will see a progress bar.

{{< bootstrap-figure src="img/installation-progress-bar.png" title="Installation progress bar" width="500">}}

When the installation is completed, you can launch Alfresco Community Edition.

{{< bootstrap-figure src="img/launch-alfresco.png" title="Launch Alfresco" width="500">}}

Alfresco will be launched in Tomcat. So, go to your Web Server domain and check the installation.

Go to Tomcat index page for Alfresco -> http://192.168.56.101:8080

{{< bootstrap-figure src="img/tomcat-index-page.png" title="Tomcat index page for Alfresco" width="500">}}

{{< bootstrap-figure src="img/alfresco-repository.png" title="Alfresco Repository" width="500">}}

Congratulations!! Alfresco Community Edition is installed in Ubuntu Server.

Let's check if Alfresco is running properly.

```terminal
cd /opt/alfresco
.alfresco.sh status
```

If either Tomcat or PostgreSQL is not running, start the services with **alfresco.sh** script.

```terminal
cd /opt/alfresco
.alfresco.sh start
```

Let's log in Alfresco. Go to share page (http://192.168.56.101:8080/share) with:

- User: Admin
- Password: (your Admin password)

{{< bootstrap-figure src="img/alfresco-community-edition-share.png" title="Alfresco Community Edition" width="500">}}

You are in! Enjoy Alfresco 😃
