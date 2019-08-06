---
title: How to install SAP NetWeaver 7.51 as ABAP Developer Edition in openSUSE
description: This tutorial will explain how to install SAP NetWeaver as ABAP server in a virtual machine to develop your own custom ABAP objects
date: 2017-11-09T13:44:00+01:00
lastmod: 2017-11-09T13:44:00+01:00
tags: [opensuse, sap]
author: dalanzg
image:  
  url: "img/sap-gui-java-connection-3.png"
  width: 629
  height: 600
comments: true
---

This tutorial will explain how to install SAP NetWeaver as ABAP server in a virtual machine to develop your own custom ABAP objects.

## Requirements

You will need the following:

- An openSUSE virtual machine with VirtualBox ([Check this link]({{< ref "/posts/2017/2017-02-12-how-to-install-opensuse-leap-42.2-in-virtualbox" >}})
- Download SAP NetWeaver compress files from [SAP NetWeaver AS ABAP Developer Edition](https://tools.hana.ondemand.com/#abap)

## Steps

- [Set port forwarding](#set-port-forwarding)
- [Set hostname and hosts](#set-hostname-and-hosts)
- [Install uuidd](#install-uuidd)
- [Mount SAP NetWeaver files](#mount-sap-netweaver-files)
- [Install SAP NetWeaver](#install-sap-netweaver)
- [Install SAP GUI in your host machine](#install-sap-gui-in-your-host-machine)
- [SAP GUI for JAVA logon](#sap-gui-for-java-logon)
- [Check SAP NetWeaver license](#check-sap-netweaver-license)
- [Stop SAP NetWeaver](#stop-sap-netweaver)
- [Start SAP NetWeaver](#start-sap-netweaver)

### Set port forwarding

Forward from the Host IP and ports to the following Guest IP and ports in the virtual machine settings:

{{< bootstrap-table "table table-bordered-striped" >}}
| Name                | Protocol | Host IP   | Host Port | Guest IP  | Guest Port |
| ------------------- |--------- | --------- | --------- | --------- | ---------- |
| HTTP                | TCP      | 127.0.0.1 | 8000      | 10.0.2.15 | 8000       |
| HTTPS               | TCP      | 127.0.0.1 | 44300     | 10.0.2.15 | 44300      |
| SAP Cloud Connector | TCP      | 127.0.0.1 | 8443      | 10.0.2.15 | 8443       |
| SAP GUI             | TCP      | 127.0.0.1 | 3200      | 10.0.2.15 | 3200       |
| ABAP in Eclipse     | TCP      | 127.0.0.1 | 3300      | 10.0.2.15 | 3300       |
| SSH                 | TCP      | 127.0.0.1 | 22        | 10.0.2.15 | 22         |
{{</ bootstrap-table >}}

{{< bootstrap-figure src="img/port-forwarding.png" title="Port forwarding" width="500">}}

Restart **rcnetwork** service.

```terminal
dlanza@linux-fd61:~> sudo rcnetwork restart
```

### Set hostname and hosts

If the virtual machine only has network NAT adapter, the static **10.0.2.15** is assigned (eth0). Change hostname and hosts for this static IP address:

- Hostname: **vhcalnplci**
- Full-Qualified-Hostname: **vhcalnplci**

Change hostname in **/etc/hostname**.

```terminal
dlanza@linux-fd61:~> sudo vim /etc/hostname
```

```vim
vhcalnplci
```

Add the following line in **/etc/hosts**:

- 10.0.2.15       vhcalnplci      vhcalnplci.dummy.nodomain

```terminal
dlanza@linux-fd61:~> sudo vim /etc/hosts
```

```vim
#
# hosts         This file describes a number of hostname-to-address
#               mappings for the TCP/IP subsystem.  It is mostly
#               used at boot time, when no name servers are running.
#               On small systems, this file can be used instead of a
#               "named" name server.
# Syntax:
#
# IP-Address  Full-Qualified-Hostname  Short-Hostname
#

127.0.0.1       localhost
10.0.2.15       vhcalnplci      vhcalnplci.dummy.nodomain

# special IPv6 addresses
::1             localhost ipv6-localhost ipv6-loopback

fe00::0         ipv6-localnet

ff00::0         ipv6-mcastprefix
ff02::1         ipv6-allnodes
ff02::2         ipv6-allrouters
ff02::3         ipv6-allhosts
```

Restart **rcnetwork** service. Check values and connections:

```terminal
dlanza@vhcalnplci:~> sudo rcnetwork restart
dlanza@vhcalnplci:~> hostname
vhcalnplci
dlanza@vhcalnplci:~> hostname -f
vhcalnplci
dlanza@vhcalnplci:~> ping vhcalnplci
PING vhcalnplci (10.0.2.15) 56(84) bytes of data.
64 bytes from vhcalnplci (10.0.2.15): icmp_seq=1 ttl=64 time=0.028 ms
64 bytes from vhcalnplci (10.0.2.15): icmp_seq=2 ttl=64 time=0.052 ms
^C
--- vhcalnplci ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.028/0.040/0.052/0.012 ms
```

### Install uuidd

Install **uuidd** service with YaST or Zypper, and start the service.

```terminal
dlanza@vhcalnplci:~> sudo zypper install uuidd
dlanza@vhcalnplci:~> sudo service uuidd start
```

### Mount SAP NetWeaver files

Uncompress SAP NetWeaver files in a folder called **netweaver** in your host machine, and mount a transient folder.

{{< bootstrap-figure src="img/netweaver-virtualbox-transition-folder.png" title="NetWeaver transient folder" width="500">}}

```terminal
dlanza@vhcalnplci:~> mkdir netweaver
dlanza@vhcalnplci:~> sudo mount -t vboxsf netweaver netweaver
dlanza@vhcalnplci:~> ll netweaver/
total 64
drwxr-xr-x 1 root root   136 ago  3 15:16 client
drwxr-xr-x 1 root root   374 jul 27 15:50 img
-rw-r--r-- 1 root root 12751 sep  2  2016 install.sh
-rw-r--r-- 1 root root 11434 ago  3 12:02 readme.html
-rw-r--r-- 1 root root 36270 sep  9  2016 SAP_COMMUNITY_DEVELOPER_License
drwxr-xr-x 1 root root   102 jul 27 15:50 server
```

### Install SAP NetWeaver

Go to mounted folder, and change file permissions for **install.sh** to execute.

```terminal
dlanza@vhcalnplci:~> cd netweaver/
dlanza@vhcalnplci:~/netweaver> sudo chmod 755 install.sh
```

Execute **install.sh** script to start the installation. Accept License Agreement (press many times **SPACE** key), enter password for UNIX SAP users (**npladm**) and wait.

```terminal
dlanza@vhcalnplci:~/netweaver> sudo ./install.sh
Hostname vhcalnplci assumed to be SAP compliant
Hit enter to continue!



Do you agree to the above license terms? yes/no:
yes


Now we need the passwords for the OS users.
Please enter a password which will be used
for all operating system users.

Please enter a password:
Please re-enter password for verification:


Now we begin with the installation.
Be patient, this will take a while ...


extracting data archives...
extracting /home/dlanza/netweaver/server/TAR/x86_64/dbdata.tgz-*
sybase/NPL/sapdata_1/
sybase/NPL/sapdata_1/NPL_data_001.dat
```

After a long time...

```terminal
Checking syb Database
Database is running
-------------------------------------------
Starting Startup Agent sapstartsrv
OK
Instance Service on host vhcalnplci started
-------------------------------------------
starting SAP Instance ASCS01
Startup-Log is written to /home/npladm/startsap_ASCS01.log
-------------------------------------------
/usr/sap/NPL/ASCS01/exe/sapcontrol -prot NI_HTTP -nr 01 -function Start
Instance on host vhcalnplci started
Starting Startup Agent sapstartsrv
OK
Instance Service on host vhcalnplci started
-------------------------------------------
starting SAP Instance D00
Startup-Log is written to /home/npladm/startsap_D00.log
-------------------------------------------
/usr/sap/NPL/D00/exe/sapcontrol -prot NI_HTTP -nr 00 -function Start
Instance on host vhcalnplci started
Installation of NPL successful
```

### Install SAP GUI in your host machine

Go to client > JavaGUI (if you use UNIX), and execute the first jar file to install the client.

{{< bootstrap-figure src="img/sap-gui-installation-1.png" title="SAP GUI installation" width="500">}}

Click on next to finish the installation.

{{< bootstrap-figure src="img/sap-gui-installation-2.png" title="SAP GUI installation" width="500">}}

{{< bootstrap-figure src="img/sap-gui-installation-3.png" title="SAP GUI installation" width="500">}}

{{< bootstrap-figure src="img/sap-gui-installation-4.png" title="SAP GUI installation" width="500">}}

{{< bootstrap-figure src="img/sap-gui-installation-5.png" title="SAP GUI installation" width="500">}}

### SAP GUI for JAVA logon

When connecting with **127.0.0.1:3200** in Host machine, there is a port forwarding in VirtualBox to **10.0.2.15:3200** in Guest machine (SAP NetWeaver server). Therefore, open SAP GUI for JAVA, and add the following connection in expert mode:

- **conn=/H/127.0.0.1/S/3200**

{{< bootstrap-figure src="img/sap-gui-java-connection-1.png" title="SAP GUI Java connection" width="500">}}

These are the SAP users available:

{{< bootstrap-table "table table-bordered-striped" >}}
| Username  | Password  | Description          |
| --------- | --------- | -------------------- |
| DDIC      | Appl1ance | Data Dictionary User |
| DEVELOPER | Appl1ance | Developer User       |
| SAP*      | Appl1ance | SAP Administrator    |
{{</ bootstrap-table >}}

Logon with:

- User: **Developer**
- Pass: **Appl1ance**
- Language: **EN**

{{< bootstrap-figure src="img/sap-gui-java-connection-2.png" title="SAP GUI Java connection" width="500">}}

{{< bootstrap-figure src="img/sap-gui-java-connection-3.png" title="SAP GUI Java connection" width="500">}}

### Check SAP NetWeaver license

Go to **SLICENSE** transaction to check the license:

{{< bootstrap-figure src="img/slicense.png" title="SAP GUI Java connection" width="500">}}

Request a new one in [SAP License Keys for Preview, Evaluation and Developer Versions](https://go.support.sap.com/minisap/#/minisap). Select **NPL - SAP NetWeaver 7.x (Sybase ASE)** and fill up your data (Hardware Key is available in **SLICENSE** transaction).

{{< bootstrap-figure src="img/sap-netweaver-license.png" title="SAP NetWeaver License" width="500">}}

Download **NPL.txt** file and uploaded it in **SLICENSE** transaction.

{{< bootstrap-figure src="img/NPL-file.png" title="SAP NetWeaver License" width="500">}}

{{< bootstrap-figure src="img/install-license-1.png" title="SAP NetWeaver License" width="500">}}

{{< bootstrap-figure src="img/install-license-2.png" title="SAP NetWeaver License" width="500">}}

{{< bootstrap-figure src="img/install-license-3.png" title="SAP NetWeaver License" width="500">}}

### Stop SAP NetWeaver

Run **stopsap ALL** with **npladm** user to stop SAP NetWeaver.

```terminal
vhcalnplci:npladm 3> stopsap ALL
Checking syb Database
Database is running
-------------------------------------------
stopping the SAP instance D00
Shutdown-Log is written to /home/npladm/stopsap_D00.log
-------------------------------------------
/usr/sap/NPL/D00/exe/sapcontrol -prot NI_HTTP -nr 00 -function Stop
Instance on host vhcalnplci stopped
Waiting for cleanup of resources
................
stopping the SAP instance ASCS01
Shutdown-Log is written to /home/npladm/stopsap_ASCS01.log
-------------------------------------------
/usr/sap/NPL/ASCS01/exe/sapcontrol -prot NI_HTTP -nr 01 -function Stop
Instance on host vhcalnplci stopped
Waiting for cleanup of resources
..
stopping database NPL ...
stop database completed successfully
Checking syb Database
Database is not available via R3trans
-------------------------------------------
```

### Start SAP NetWeaver

Check the static IP address is **10.0.2.15**, and ping **vhcalnplci**.

```terminal
dlanza@vhcalnplci:~> sudo ifconfig
[sudo] password for root:
eth0      Link encap:Ethernet  HWaddr 08:00:27:D5:EC:A1
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fed5:eca1/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:237 errors:0 dropped:0 overruns:0 frame:0
          TX packets:219 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:28409 (27.7 Kb)  TX bytes:27910 (27.2 Kb)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:40 errors:0 dropped:0 overruns:0 frame:0
          TX packets:40 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:2204 (2.1 Kb)  TX bytes:2204 (2.1 Kb)

dlanza@vhcalnplci:~> ping vhcalnplci
PING vhcalnplci (10.0.2.15) 56(84) bytes of data.
64 bytes from vhcalnplci (10.0.2.15): icmp_seq=1 ttl=64 time=0.022 ms
64 bytes from vhcalnplci (10.0.2.15): icmp_seq=2 ttl=64 time=0.040 ms
^C
--- vhcalnplci ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 999ms
rtt min/avg/max/mdev = 0.022/0.031/0.040/0.009 ms
```

Run **startsap ALL** with **npladm** user to start SAP NetWeaver.

```terminal
dlanza@vhcalnplci:~> sudo su - npladm
vhcalnplci:npladm 2> startsap ALL
Checking syb Database
Database is not available via R3trans
-------------------------------------------
starting database NPL ...
Log file: /sybase/NPL/startdb.log
parse level 0: identified message 'Database 'master' is now online.'
parse level 1: identified message 'Database 'tempdb' is now online.'
parse level 2: identified message 'Database 'sybsystemprocs' is now online.'
parse level 3: identified message 'Recovery complete.'
Recovery Complete
startdb completed successfully
Starting Startup Agent sapstartsrv
OK
Instance Service on host vhcalnplci started
-------------------------------------------
starting SAP Instance ASCS01
Startup-Log is written to /home/npladm/startsap_ASCS01.log
-------------------------------------------
/usr/sap/NPL/ASCS01/exe/sapcontrol -prot NI_HTTP -nr 01 -function Start
Instance on host vhcalnplci started
Starting Startup Agent sapstartsrv
OK
Instance Service on host vhcalnplci started
-------------------------------------------
starting SAP Instance D00
Startup-Log is written to /home/npladm/startsap_D00.log
-------------------------------------------
/usr/sap/NPL/D00/exe/sapcontrol -prot NI_HTTP -nr 00 -function Start
Instance on host vhcalnplci started
```

And check all services are green.

```terminal
vhcalnplci:npladm 3> sapcontrol -nr 00 -function GetProcessList

09.11.2017 12:58:41
GetProcessList
OK
name, description, dispstatus, textstatus, starttime, elapsedtime, pid
igswd_mt, IGS Watchdog, GREEN, Running, 2017 11 09 12:57:47, 0:00:54, 23988
disp+work, Dispatcher, GREEN, Running, 2017 11 09 12:57:47, 0:00:54, 23987
gwrd, Gateway, GREEN, Running, 2017 11 09 12:57:52, 0:00:49, 24006
icman, ICM, GREEN, Running, 2017 11 09 12:57:52, 0:00:49, 24007
```
