---
title: How to install SAP Cloud Connector in SAP NetWeaver Server
description: This tutorial will explain how to install SAP Cloud Connector in a NetWeaver Server.
date: 2017-12-06T14:00:00+01:00
lastmod: 2017-12-06T17:30:00+01:00
tags: [sap]
author: dalanzg
image:  
  url: "img/cloud_connector_login.png"
  width: 1278
  height: 722
comments: true
---

This tutorial will explain how to install SAP Cloud Connector in a NetWeaver Server.

## Requirements

You will need the following:

- [SAP NetWeaver Server and SAPGUI]({{< ref "/posts/2017/2017-11-09-how-to-install-sap-netweaver-751-sp02-as-abap-developer-edition-in-opensuse" >}})
- Download Cloud Connector from [SAP Development Tools](https://tools.hana.ondemand.com/#cloud)

## Steps

- [Verify SAP NetWeaver is running](#verify-sap-netweaver-is-running)
- [Unzip and install Cloud Connector](uncompress-and-install-cloud-connector)
- [Start and Stop Cloud Connector services](#start-and-stop-cloud-connector-service)
- [Login and change Administrator password in Cloud connector](#login-and-change-administrator-password-in-cloud-connector)

### Verify SAP NetWeaver is running

Login with **npladm** user and check SAP NetWeaver services are runnning.

```terminal
dlanza@vhcalnplci:~> sudo su - npladm
vhcalnplci:npladm 57> sapcontrol -nr 00 -function GetProcessList

06.12.2017 13:06:18
GetProcessList
OK
name, description, dispstatus, textstatus, starttime, elapsedtime, pid
igswd_mt, IGS Watchdog, GREEN, Running, 2017 12 06 13:04:10, 0:02:08, 27763
disp+work, Dispatcher, GREEN, Running, 2017 12 06 13:04:10, 0:02:08, 27762
gwrd, Gateway, GREEN, Running, 2017 12 06 13:04:15, 0:02:03, 27837
icman, ICM, GREEN, Running, 2017 12 06 13:04:15, 0:02:03, 27838
```

### Unzip and install Cloud Connector

```terminal
dlanza@vhcalnplci:~/netweaver> unzip sapcc-2.10.2-linux-x64.zip
dlanza@vhcalnplci:~/netweaver> sudo rpm -i com.sap.scc-ui-2.10.2-1.x86_64.rpm
Installing Cloud Connector
search for java installations ...
Could not find Java installation. Will check JAVA_HOME.
Could not find JAVA_HOME. Will check path.
Found /usr/bin/java and will use it during installation.
scc_Daemon installed.
You may now use /usr/local/sbin/rcscc_daemon to call this script.
search for keytool to use ...
Could not find keytool in Java installations. Will check JAVA_HOME.
Could not find JAVA_HOME. Will check path.
Found /usr/bin/keytool and will use it during installation.
setting password ...
Configure cloud connector to use the secure store
Using /usr/bin/keytool to set the Java Keystore password


Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore /opt/sap/scc/config/ks.store -destkeystore /opt/sap/scc/config/ks.store -deststoretype pkcs12".

Warning:
The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore /opt/sap/scc/config/ks.store -destkeystore /opt/sap/scc/config/ks.store -deststoretype pkcs12".
Prepare usage of different property source
Start cloud connector server daemon
Systemd is used. Reload classic daemon states
Failed to execute operation: Connection timed out
Job for scc_daemon.service failed because the control process exited with error code. See "systemctl status scc_daemon.service" and "journalctl -xe" for details.
Installation finished. Cloud Connector server has been started.
Cloud Connector server can be managed using service scc_daemon start|stop|restart.
```

### Start and Stop Cloud Connector service

Mange Cloud Connector service like this:

```terminal
dlanza@vhcalnplci:~/netweaver> sudo service scc_daemon start
dlanza@vhcalnplci:~/netweaver> sudo service scc_daemon stop
dlanza@vhcalnplci:~/netweaver> sudo service scc_daemon status
```

This is the result:

```terminal
dlanza@vhcalnplci:~/netweaver> sudo service scc_daemon status
Dec 06 13:18:06 vhcalnplci scc_daemon[30858]: Starting scc_Daemon
Dec 06 13:18:09 vhcalnplci scc_daemon[30858]: scc_Daemon start failed, see logfile: /opt/sap/scc/scc_daemon.log
Dec 06 13:18:09 vhcalnplci systemd[1]: scc_daemon.service: Control process exited, code=exited status=1
Dec 06 13:18:09 vhcalnplci systemd[1]: Failed to start LSB: LJS Daemon.
Dec 06 13:18:09 vhcalnplci systemd[1]: scc_daemon.service: Unit entered failed state.
Dec 06 13:18:09 vhcalnplci systemd[1]: scc_daemon.service: Failed with result 'exit-code'.
Dec 06 13:18:11 vhcalnplci su[30959]: (to sccadmin) root on none
Dec 06 13:18:11 vhcalnplci su[30959]: pam_unix(su:session): session opened for user sccadmin by (uid=0)
Dec 06 13:18:23 vhcalnplci scc_daemon[30858]: [1B blob data]
Dec 06 13:18:23 vhcalnplci scc_daemon[30858]: osgi>
dlanza@vhcalnplci:~/netweaver> sudo service scc_daemon start
dlanza@vhcalnplci:~/netweaver> sudo service scc_daemon status
* scc_daemon.service - LSB: LJS Daemon
   Loaded: loaded (/etc/init.d/scc_daemon; bad; vendor preset: disabled)
   Active: active (exited) since Wed 2017-12-06 13:23:12 CET; 6s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 31757 ExecStart=/etc/init.d/scc_daemon start (code=exited, status=0/SUCCESS)

Dec 06 13:23:09 vhcalnplci systemd[1]: Starting LSB: LJS Daemon...
Dec 06 13:23:12 vhcalnplci systemd[1]: Started LSB: LJS Daemon.
Dec 06 13:23:13 vhcalnplci scc_daemon[31757]: scc_Daemon is already running. Process ID: 31049
```

### Login and change Administrator password in Cloud connector

Access to **https://localhost:8443** to login in Cloud Connector. By default, use a self signed certificate. Hence, your browser will ask you to trust the server.

{{< bootstrap-figure src="img/untrusted_certificate.png" title="Untrusted certificate" width="500">}}

Write down the username and the password:

- Username: **Administrator**
- Password: **manage**

{{< bootstrap-figure src="img/cloud_connector_login.png" title="Login in Cloud Connector" width="500">}}

And change the default password.

{{< bootstrap-figure src="img/change_password_cloud_connector.png" title="Change Cloud Connector password" width="500">}}
