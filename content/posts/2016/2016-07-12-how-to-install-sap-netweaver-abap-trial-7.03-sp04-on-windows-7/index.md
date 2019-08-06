---
title: How to install SAP Netweaver ABAP Trial 7.03 SP04 on Windows 7
description: Steps to install SAP Netweaver ABAP Trial (MiniSAP) to develop your own ABAP programs. A license SAP Trial is applied and Web GUI for SAP Netweaver is enabled.
date: 2016-07-12T17:00:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [sap]
author: dalanzg
image: 
  url: "img/select-sap-gui-for-windows.png"
  width: 1024
  height: 796
comments: true
---

This tutorial will explain how to install SAP Netweaver ABAP Trial 7.03 in a virtual machine.

## Requirements

A virtual machine with:

- Windows 7
- At least 4 GB RAM
- 80 GB hard disk space temporary during installation - 36 GB permanent. The faster the better

Other things:

- [Java JRE 1.5 installer](http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase5-419410.html)
- [A SCN account](http://scn.sap.com/)
- [SAP Netweaver ABAP Trial 7.03 SP04 installer](https://store.sap.com/sap/cp/ui/resources/store/html/SolutionDetails.html?pid=0000000218)

## Steps

- [Add Microsoft Loopback Adapter](#add-microsoft-loopback-adapter)
- [Install Java JRE](#install-java-jre)
- [Install SAP Netweaver](#install-sap-netweaver)
- [Install a trial license key](#install-a-trial-license-key)
- [Enable Web GUI for SAP Netweaver](#enable-web-gui-for-sap-netweaver)

### Add Microsoft Loopback Adapter

A network adapter with static IP address is needed. So firstly, add the network adapter. Run **cmd.exe** as Administrator, and the wizard to add new hardware will be opened.

```terminal
hdwwiz.exe
```

Select Microsoft Loopback Adapter.

{{< bootstrap-figure src="img/add-hardware-microsoft-loopback-adapter-01.png" title="Run cmd as Administrator and add new hardware" width="500">}}

{{< bootstrap-figure src="img/add-hardware-microsoft-loopback-adapter-02.png" title="Select Network Adapter" width="500">}}

{{< bootstrap-figure src="img/add-hardware-microsoft-loopback-adapter-03.png" title="Select Microsoft Loopback Adapter" width="500">}}

Change properties for the Network Adapter to assign a static IP address. For instance:

- IP address: 10.10.0.10
- Subnet mask: 255.255.255.0

{{< bootstrap-figure src="img/ip-address-for-network-adapter-01.png" title="Properties for Microsoft Loopback Adapter" width="500">}}

{{< bootstrap-figure src="img/ip-address-for-network-adapter-02.png" title="IP address for Network Adapter" width="500">}}

The virtual machine needs to resolve the IP address with a hostname.

Open the host file (**C:\Windows\System32\drivers\etc\hosts**) with Notepad as Administrator, and add the following line:

```vim
10.10.0.10    NETWEAVER.com
```

{{< bootstrap-figure src="img/host-netweaver.png" title="Hostname for Netweaver" width="500">}}

### Install Java JRE

The virtual machine needs Java JRE to install SAP Netweaver.

{{< bootstrap-figure src="img/installing-java-jre.png" title="Installing Java JRE" width="500">}}

### Install SAP Netweaver

Check out that your computer name is less than 14 characters. My computer is called NETWEAVER.

{{< bootstrap-figure src="img/computer-name.png" title="Computer name: NETWEAVER" width="500">}}

Then, if you changed the computer name and rebooted the virtual machine, the SAP Netweaver installation can start. Run as administrator the file **sapinst.exe**.

Select Central System within SAP Application Server ABAP > MaxDB > Central System.

{{< bootstrap-figure src="img/maxdb-central-system.png" title="Install Database and Central instance" width="500">}}

You will be asked to log off. Do it, and the installer will continue when logging in.

{{< bootstrap-figure src="img/log-off-before-starting.png" title="Log off and continue" width="500">}}

Accept the license.

{{< bootstrap-figure src="img/accept-the-license.png" title="Accept the license" width="500">}}

Set the master password. Do not forget it!

{{< bootstrap-figure src="img/set-master-password.png" title="Set the master password" width="500">}}

Check the parameter summary and the installation will be started. It takes a lot of time 😃

{{< bootstrap-figure src="img/parameter-summary.png" title="Parameter summary" width="500">}}

{{< bootstrap-figure src="img/installing-sap-netweaver.png" title="Installing SAP Netweaver" width="500">}}

The execution will be finished.

{{< bootstrap-figure src="img/execution-finished.png" title="Execution finished" width="500">}}

Two more Windows users will be created (**nspadm** and **sapadm**). So, switch the user and log in with **sapadm** (the password is the one you typed before starting the installation)

{{< bootstrap-figure src="img/windows-users.png" title="Log in with sapadm" width="500">}}

Open SAP Management Console and start the SAP System with right-click. **sapadm** user needs to be authenticated again.

{{< bootstrap-figure src="img/start-sap-system.png" title="Start SAP System" width="500">}}

{{< bootstrap-figure src="img/authentication-to-start-sap-system.png" title="Authentication for sapadm" width="500">}}

Wait for the starting process ends. The icon for SAP System will be turned green.

{{< bootstrap-figure src="img/sap-system-started.png" title="SAP System started title" width="500">}}

Do not log off sapadm session. Switch to your user to install SAP GUI.

{{< bootstrap-figure src="img/install-sap-gui.png" title="Install SAP GUI" width="500">}}

Select SAP GUI for Windows 7.20, and click next to start the installation.

{{< bootstrap-figure src="img/select-sap-gui-for-windows.png" title="SAP System started title" width="500">}}

Open SAP Logon, and add the following entry:

- Description NSP Local
- Application server: localhost
- Instance number: 00
- System ID: NSP

{{< bootstrap-figure src="img/sap-gui-connection.png" title="SAP connections" width="500">}}

Log in with the following users:

- DDIC -> your master password
- BCUSER -> minisap

{{< bootstrap-figure src="img/log-in-ddic-user.png" title="Login" width="500">}}

Welcome to Minisap and start to develop!

{{< bootstrap-figure src="img/welcome-to-minisap.png" title="DDIC login" width="500">}}

### Install a trial license key

Go to **SLICENSE**. You will see that there is no license apply. Get the Active Hardware Key.

{{< bootstrap-figure src="img/active-hardware-key.png" title="Active Hardware Key" width="500">}}

Go to [http://www.sap.com/minisap](http://www.sap.com/minisap), and fill up the form.

{{< bootstrap-figure src="img/minisap-license-request.png" title="Active Hardware Key" width="500">}}

The request will have been submitted, and you will get the license key in your email.

Apply the license in **SLICENSE** transaction.

{{< bootstrap-figure src="img/apply-license.png" title="Apply license" width="500">}}

{{< bootstrap-figure src="img/license-applied.png" title="License applied" width="500">}}

### Enable Web GUI for SAP Netweaver

Assign a hostname and port for Netweaver. Go to **C:\usr\sap\NSP\SYS\profile\DEFAULT.FPL** file, and add the following properties:

```properties
icm/host_name_full = <machine_name>.com
icm/server_port_0 = PROT=HTTP,PORT=8000,TIMEOUT=3600,PROCTIMEOUT=3600
```

{{< bootstrap-figure src="img/icm-properties.png" title="ICM properties" width="500">}}

Restart the instance from SAP Management Console to make the icm parameters changes take effect.

{{< bootstrap-figure src="img/restart-to-make-icm-changes-take-effect.png" title="ICM properties" width="500">}}

Go to **SICF** transaction, and active the following services:

- /default_host/sap/bc/gui/sap/its/webgui
- /default_host/sap/public/bc/ur
- /default_host/sap/public/bc/its/mimes

Run these two reports (**SE38**) in the following order:

- **RS_TT_CLEANUP_SECSTORE** -> avoid RFC error when sending logon data
- **SIAC_PUBLISH_ALL_INTERNAL** -> active services

{{< bootstrap-figure src="img/siac-publish-all-internal-report.png" title="SIAC_PUBLISH_ALL_INTERNAL report" width="500">}}

Open browser and go to the following URL to log into SAP Netweaver for HTML:

- [http://netweaver.com:8000/sap/bc/gui/sap/its/webgui](http://netweaver.com:8000/sap/bc/gui/sap/its/webgui)

{{< bootstrap-figure src="img/web-gui-for-sap-netweaver.png" title="Web GUI for SAP Netweaver" width="500">}}
