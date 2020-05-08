---
title: How to install Oracle 12c R2 in openSUSE
description: This tutorial will explain how to install Oracle Database 12c R2 in openSUSE Leap 42.3.
date: 2018-03-04T12:00:00+01:00
lastmod: 2018-03-04T12:00:00+01:00
tags: [opensuse, oracle]
author: dalanzg
image:  
  url: "img/progress-create-database.png"
  width: 800
  height: 604
comments: true
---

This tutorial will explain how to install Oracle Database 12c R2 in openSUSE Leap 42.3.

## Requirements

You will need the following:

- openSUSE Leap 42.3 -> [Install openSUSE virtual machine with VirtualBox]({{< ref "/posts/2017/2017-02-12-how-to-install-opensuse-leap-42.2-in-virtualbox/index.md" >}})
- Oracle 12c R2 database installation file ->  [linuxx64_12201_database.zip](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/index.html)

## Steps

- [Install libraries](#install-libraries)
- [Set hostname and domain](#set-hostname-and-domain)
- [Create groups and users](#create-groups-and-users)
- [Grant oracle user access to X server](#grant-oracle-user-access-to-x-server)
- [Configuring Kernel Parameters and Resource Limits](#configuring-kernel-parameters-and-resource-limits)
- [Create directory for Oracle Database](#create-directory-for-oracle-database)
- [Install Oracle Database](#install-oracle-database)
- [Configuration of Oracle Listener](#configuration-of-oracle-listener)
- [Configuration of Oracle Database](#configuration-of-oracle-database)
- [Create user for database](#create-user-for-database)
- [Create table for database](#create-table-for-database)
- [Connect to Oracle database with SQL Developer](#connect-to-oracle-database-with-sql-developer)
- [Start database when server is restarted](#start-database-when-server-is-restarted)

### Install libraries

According to [Supported SUSE Linux Enterprise Server 12 in Oracle documentation](https://docs.oracle.com/en/database/oracle/oracle-database/12.2/ladbi/supported-suse-linux-enterprise-server-12-distributions-for-x86-64.html#GUID-8BF62362-685D-4B7C-BD6F-094D53C76900), install the following libraries by using **YaST** or **zypper** command:

```raw
bc
binutils-2.24-2.165.x86_64
gcc-c++-32bit-4.8-6.189.x86_64
gcc-c++-4.8-6.189.x86_64
gcc48-c++-4.8.3+r212056-6.3.x86_64
gcc-32bit-4.8-6.189.x86_64
gcc-4.8-6.189.x86_64
gcc-info-4.8-6.189.x86_64
gcc-locale-4.8-6.189.x86_64
gcc48-32bit-4.8.3+r212056-6.3.x86_64
gcc48-4.8.3+r212056-6.3.x86_64
gcc48-info-4.8.3+r212056-6.3.noarch
gcc48-locale-4.8.3+r212056-6.3.x86_64
glibc-2.19-17.72.x86_64
glibc-devel-2.19-17.72.x86_64
libaio-devel-0.3.109-17.15.x86_64
libaio1-0.3.109-17.15.x86_64
libaio1-32bit-0.3.109-17.15.x86_64
libgfortran3-4.8.3+r212056-6.3.x86_64
libX11-6-1.6.2-4.12.x86_64
libX11-6-32bit-1.6.2-4.12.x86_64
libXau6-1.0.8-4.58.x86_64
libXau6-32bit-1.0.8-4.58.x86_64
libXtst6-1.2.2-3.60.x86_64
libXtst6-32bit-1.2.1-2.4.1.x86_64
libcap-ng-utils-0.7.3-4.125.x86_64
libcap-ng0-0.7.3-4.125.x86_64
libcap-ng0-32bit-0.7.3-4.125.x86_64
libcap-progs-2.22-11.709.x86_64
libcap1-1.10-59.61.x86_64
libcap1-32bit-1.10-59.61.x86_64
libcap2-2.22-11.709.x86_64
libcap2-32bit-2.22-11.709.x86_64
libgcc_s1-32bit-4.8.3+r212056-6.3.x86_64
libgcc_s1-4.8.3+r212056-6.3.x86_64
libpcap1-1.5.3-2.18.x86_64
libstdc++6-32bit-4.8.3+r212056-6.3.x86_64
libstdc++6-4.8.3+r212056-6.3.x86_64
make-4.0-2.107.x86_64
mksh-50-2.13.x86_64
net-tools-1.60-764.185.x86_64 (for Oracle RAC and Oracle Clusterware)
nfs-kernel-server-1.3.0-6.9.x86_64 (for Oracle ACFS)
smartmontools-6.2-4.33.x86_64
sysstat-8.1.5-7.32.1.x86_64
xorg-x11-libs-7.6-45.14
```

Additional software requirements:

```raw
unixODBC-2.2.12
unixODBC-devel-2.2.12
unixODBC-32bit-2.2.12 (32-bit)
```

### Set hostname and domain

Files **/etc/hostname** and **/etc/hosts** will be modified to resolve the following:

- **Hostname** -> oracle12cr2
- **Domain** -> dalanzg.com

More information in [Setting hostname and domain in openSUSE.]({{< ref "/posts/2017/2017-04-13-setting-hostname-and-domain-in-opensuse" >}})

```terminal
dlanza@oracle12cr2:~> hostname
oracle12cr2
dlanza@oracle12cr2:~> hostname --fqdn
oracle12cr2.dalanzg.com
```

### Create groups and users

Create **oinstall**, **dba** and **oper** groups.

```terminal
dlanza@oracle12cr2:~> sudo groupadd oinstall
dlanza@oracle12cr2:~> sudo groupadd dba
dlanza@oracle12cr2:~> sudo groupadd oper
```

Create **oracle** user, and add it to **oinstall** as primary group, and **dba** and **oper** groups.

```terminal
dlanza@oracle12cr2:~> sudo useradd -g oinstall -G dba,oper -d /home/oracle oracle
dlanza@oracle12cr2:~> sudo passwd oracle
New password:
dlanza@oracle12cr2:~> sudo mkhomedir_helper oracle
```

Modify .bash_profile for **oracle** user.

```terminal
dlanza@oracle12cr2:~> sudo su - oracle
oracle@oracle12cr2:~> vim .bash_profile
```

```vim
export TMP=/tmp

export ORACLE_HOSTNAME=oracle12cr2.dalanzg.com
export ORACLE_UNQNAME=orcl
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/12.2.0/db_1
export ORACLE_SID=orcl

export PATH=$PATH:$ORACLE_HOME/bin

export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib;
export CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib;

if [ $USER = "oracle" ]; then
  if [ $SHELL = "/bin/ksh" ]; then
    ulimit -p 16384
    ulimit -n 65536
  else
    ulimit -u 16384 -n 65536
  fi
fi
```

### Grant oracle user access to X server

Verify if access control for X server is enabled, and grant **oracle** user access.

```terminal
dlanza@oracle12cr2:~> xhost
access control enabled, only authorized clients can connect
dlanza@oracle12cr2:~> xhost +SI:localuser:oracle
localuser:oracle being added to access control list
dlanza@oracle12cr2:~> xhost
access control enabled, only authorized clients can connect
SI:localuser:oracle
```

Verify that oracle user can run X11 apps. In this case, use **xclock** app to check it out. Install it by using **YaST** or **zypper** command in case of not having it.

First, set **$DISPLAY** variable, and run **xclock** with **oracle** user. If the app was launched, **oracle** user can run X11 apps.

```terminal
dlanza@oracle12cr2:~> sudo su - oracle
oracle@oracle12cr2:~> DISPLAY=:0.0; export DISPLAY
oracle@oracle12cr2:~> xclock
```

{{< bootstrap-figure src="img/xclock-oracle-user.png" title="xclock app with oracle user" width="400">}}

### Configuring Kernel Parameters and Resource Limits

Add the following Kernel parameters in **/etc/sysctl.conf** file.

```terminal
oracle@oracle12cr2:~> sudo vim /etc/sysctl.conf
```

```vim
####
#
# /etc/sysctl.conf is meant for local sysctl settings
#
# sysctl reads settings from the following locations:
#   /boot/sysctl.conf-<kernelversion>
#   /lib/sysctl.d/*.conf
#   /usr/lib/sysctl.d/*.conf
#   /usr/local/lib/sysctl.d/*.conf
#   /etc/sysctl.d/*.conf
#   /run/sysctl.d/*.conf
#   /etc/sysctl.conf
#
# To disable or override a distribution provided file just place a
# file with the same name in /etc/sysctl.d/
#
# See sysctl.conf(5), sysctl.d(5) and sysctl(8) for more information
#
####

#Oracle
fs.suid_dumpable = 1
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 536870912
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 4194304
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048586
```

And load them by running the following command:

```terminal
oracle@oracle12cr2:~> sudo sysctl -p
fs.suid_dumpable = 1
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 536870912
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 4194304
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048586
```

Next, set the resource limits in **/etc/security/limits.conf** file.

```terminal
oracle@oracle12cr2:~> sudo vim /etc/security/limits.conf
```

```vim
#...
# harden against fork-bombs
*               hard    nproc           16384
*               soft    nproc           4096

# End of file

#Oracle
oracle  soft    nproc   2047
oracle  hard    nproc   16384
oracle  soft    nofile  1024
oracle  hard    nofile  65536
oracle  soft    stack   10240
```

And add the following in **/etc/pam.d/login** file.

```terminal
oracle@oracle12cr2:~> sudo vim /etc/pam.d/login
```

```vim
#%PAM-1.0
auth     requisite      pam_nologin.so
auth     include        common-auth
account  include        common-account
password include        common-password
session  required       pam_loginuid.so
session  include        common-session
#session  optional       pam_lastlog.so nowtmp showfailed
session  optional       pam_mail.so standard

#Oracle
session    required     pam_limits.so
session    required     /lib/security/pam_limits.so
```

##### Create directory for Oracle Database

Create the directory for Oracle Software and set permission for **oracle** user and **oinstall** group.

```terminal
dlanza@oracle12cr2:~> sudo mkdir -p /u01/app/oracle/product/12.2.0/db_1
dlanza@oracle12cr2:~> sudo chown -R oracle:oinstall /u01
```

### Install Oracle Database

Unzip **linuxx64_12201_database.zip** file. Then, set **DISPLAY** variable if it was not done before (DISPLAY=:0.0), and run the installer.

```terminal
oracle@oracle12cr2:~> unzip linuxx64_12201_database.zip
oracle@oracle12cr2:~> DISPLAY=:0.0; export DISPLAY
oracle@oracle12cr2:~/database> ./runInstaller
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 500 MB.   Actual 51372 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 2053 MB    Passed
Checking monitor: must be configured to display at least 256 colors.    Actual 16777216    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2018-03-02_08-30-43PM. Please wait ...
```

A GUI will be launched with a warning, since Oracle Database does not support openSUSE. Ignore it and continue.

{{< bootstrap-figure src="img/warning-not-supported-in-opensuse.png" title="Warning about Oracle Database does not support openSUSE" width="500">}}

Configure Security Updates if you have an oracle emails support. If not, leave it empty, and ignore the warning.

{{< bootstrap-figure src="img/configure-security-support.png" title="Configure Security Support" width="500">}}

{{< bootstrap-figure src="img/ignore-email-address-for-support.png" title="Ignore email address for support" width="500">}}

Install database software.

{{< bootstrap-figure src="img/install-database-software.png" title="Install database software" width="500">}}

Select single instance database.

{{< bootstrap-figure src="img/single-instance-database.png" title="Single instance database" width="500">}}

Select Enterprise Edition

{{< bootstrap-figure src="img/enterprise-edition.png" title="Select Enterprise Edition" width="500">}}

Oracle base and Oracle home is read from **ORACLE_BASE** and **ORACLE_HOME** variables. Click next.

{{< bootstrap-figure src="img/oracle-base-and-home-path.png" title="Oracle base and home path" width="500">}}

Inventory directory and group name by default.

{{< bootstrap-figure src="img/inventory-directory-and-group-name.png" title="Inventory directory and group name" width="500">}}

Administrator and operator group names by default.

{{< bootstrap-figure src="img/administrator-and-operator-groups.png" title="Administrator and operator group names" width="500">}}

Summary and click Install.

{{< bootstrap-figure src="img/summary-installation.png" title="Summary of installation" width="500">}}

The installation process will started.

{{< bootstrap-figure src="img/progress-installation.png" title="Progress of installation" width="500">}}

While the installation process, you will be asked to execute configuration scripts with root user. Login with root user and execute them.

{{< bootstrap-figure src="img/execute-configuration-scrits.png" title="Execute configuration scripts" width="500">}}

```terminal
dlanza@oracle12cr2:~> sudo su
[sudo] password for root:
oracle12cr2:/home/dlanza # /u01/app/oraInventory/orainstRoot.sh
Changing permissions of /u01/app/oraInventory.
Adding read,write permissions for group.
Removing read,write,execute permissions for world.

Changing groupname of /u01/app/oraInventory to oinstall.
The execution of the script is complete.
oracle12cr2:/home/dlanza # /u01/app/oracle/product/12.2.0/db_1/root.sh
Performing root user operation.

The following environment variables are set as:
    ORACLE_OWNER= oracle
    ORACLE_HOME=  /u01/app/oracle/product/12.2.0/db_1

Enter the full pathname of the local bin directory: [/usr/local/bin]:
   Copying dbhome to /usr/local/bin ...
   Copying oraenv to /usr/local/bin ...
   Copying coraenv to /usr/local/bin ...


Creating /etc/oratab file...
Entries will be added to the /etc/oratab file as needed by
Database Configuration Assistant when a database is created
Finished running generic part of root script.
Now product-specific root actions will be performed.
Do you want to setup Oracle Trace File Analyzer (TFA) now ? yes|[no] :
yes
Installing Oracle Trace File Analyzer (TFA).
Log File: /u01/app/oracle/product/12.2.0/db_1/install/root_oracle12cr2_2018-03-02_21-04-58-795554185.log
Finished installing Oracle Trace File Analyzer (TFA)
```

Then, press OK and continue.

The installation of Oracle Database was successful.

{{< bootstrap-figure src="img/finish-installation.png" title="Finish" width="500">}}

### Configuration of Oracle Listener

Run the command **netca** with **oracle** user to launch the configuration of Oracle Listener.

```terminal
oracle@oracle12cr2:~> DISPLAY=:0.0; export DISPLAY
oracle@oracle12cr2:~> netca

Oracle Net Services Configuration:
```

A GUI will be launched. Select Listener Configuration and click on Next.

{{< bootstrap-figure src="img/oracle-net-services.png" title="Oracle Net Services Configuration" width="500">}}

Add a new listener.

{{< bootstrap-figure src="img/add-listener.png" title="Add a new listener" width="500">}}

Listener names and protocols by default.

{{< bootstrap-figure src="img/listener-name.png" title="Listener name" width="500">}}

{{< bootstrap-figure src="img/listener-protocols.png" title="Listener protocols" width="500">}}

Listener port by default.

{{< bootstrap-figure src="img/listener-port.png" title="Listener port" width="500">}}

And select no more listeners.

{{< bootstrap-figure src="img/no-more-listeners.png" title="No more listeners" width="500">}}

Listener configuration complete.

{{< bootstrap-figure src="img/listener-configuration-complete.png" title="No more listeners" width="500">}}

```terminal
Configuring Listener:LISTENER
Listener configuration complete.
Oracle Net Listener Startup:
    Running Listener Control:
      /u01/app/oracle/product/12.2.0/db_1/bin/lsnrctl start LISTENER
    Listener Control complete.
    Listener started successfully.
```

Finish and exit.

{{< bootstrap-figure src="img/finish-listener.png" title="Finish listener" width="500">}}

### Configuration of Oracle Database

Run the command **dbca** with **oracle** user to launch the configuration of Oracle Database.

```terminal
oracle@oracle12cr2:~> DISPLAY=:0.0: export DISPLAY
oracle@oracle12cr2:~> dbca
```

Create database.

{{< bootstrap-figure src="img/create-database.png" title="Create database" width="500">}}

Typical configuration of database. Type the Global Database Name. Be in mind that it will be the **SID** followed by the domain (orcl.dalanzg.com)

{{< bootstrap-figure src="img/typical-configuration-database.png" title="Typical configuration of database" width="500">}}

Summary of creation database.

{{< bootstrap-figure src="img/summary-create-database.png" title="Summary of creation database" width="500">}}

Progress of creation database.

{{< bootstrap-figure src="img/progress-create-database.png" title="Progress of creation database" width="500">}}

Finish of creation database.

{{< bootstrap-figure src="img/finish-creation-database.png" title="Finish of creation database" width="500">}}

Change password of **SYSTEM** and **SYS** users in Password management.

{{< bootstrap-figure src="img/password-management.png" title="Password management" width="500">}}

Verify connection to the database with the client **sqlplus**.

```terminal
oracle@oracle12cr2:~> sqlplus

SQL*Plus: Release 12.2.0.1.0 Production on Sat Mar 3 16:25:56 2018

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Enter user-name: sys / as sysdba
Enter password:

Connected to:
Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production

SQL> exit
Disconnected from Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production
```

### Create user for database

Connect again with **sqlplus**. Create a user for the database and grant privileges to create session and to create tables.

```terminal
oracle@oracle12cr2:~> sqlplus

SQL*Plus: Release 12.2.0.1.0 Production on Sat Mar 3 16:25:56 2018

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Enter user-name: sys / as sysdba
Enter password:

Connected to:
Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production

SQL> CREATE USER dalanzg
  2  identified by dalanzg
  3  default tablespace users
  4  temporary tablespace temp
  5  quota 20m on users
  6  profile default;

User created.

SQL> GRANT CREATE SESSION TO dalanzg;

Grant succeeded.

SQL> GRANT RESOURCE TO dalanzg;

Grant succeeded.

SQL> conn dalanzg/dalanzg@orcl
Connected.

SQL> exit
Disconnected from Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production
```

### Create table for database

Create a table with the user created, and insert values.

```terminal
oracle@oracle12cr2:~> sqlplus
SQL*Plus: Release 12.2.0.1.0 Production on Sat Mar 3 16:25:56 2018

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Enter user-name: dalanzg
Enter password:

Connected to:
Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production

SQL> CREATE TABLE Test
  2  (
  3  ID_Test number(10) NOT NULL,
  4  DESC_Test varchar(32),
  5  CONSTRAINT Test_pk PRIMARY KEY (ID_Test)
  6  );

Table created.

SQL> INSERT INTO Test
  2  (ID_Test, DESC_Test)
  3  VALUES
  4  (1, 'This is a test');

1 row created.

SQL> SELECT * FROM Test;

   ID_TEST DESC_TEST
---------- --------------------------------
         1 This is a test

SQL> exit
Disconnected from Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production
```

### Connect to Oracle database with SQL Developer

Launch SQL Developer, type connection details and look for the table created before.

{{< bootstrap-figure src="img/sql-developer-connection.png" title="SQL Developer connection" width="500">}}

{{< bootstrap-figure src="img/sql-developer-data.png" title="Table data in SQL Developer" width="500">}}

### Start database when server is restarted

If the machine was restarted, the listener and the database need to be started.

Start the listener with the following command:

```terminal
oracle@oracle12cr2:~> lsnrctl start

LSNRCTL for Linux: Version 12.2.0.1.0 - Production on 04-MAR-2018 12:08:29

Copyright (c) 1991, 2016, Oracle.  All rights reserved.

Starting /u01/app/oracle/product/12.2.0/db_1/bin/tnslsnr: please wait...

TNSLSNR for Linux: Version 12.2.0.1.0 - Production
System parameter file is /u01/app/oracle/product/12.2.0/db_1/network/admin/listener.ora
Log messages written to /u01/app/oracle/diag/tnslsnr/oracle12cr2/listener/alert/log.xml
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=oracle12cr2.dalanzg.com)(PORT=1521)))
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=oracle12cr2.dalanzg.com)(PORT=1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 12.2.0.1.0 - Production
Start Date                04-MAR-2018 12:08:31
Uptime                    0 days 0 hr. 0 min. 1 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/12.2.0/db_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/oracle12cr2/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=oracle12cr2.dalanzg.com)(PORT=1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
The listener supports no services
The command completed successfully
```

And start the Database.

```terminal
oracle@oracle12cr2:~> sqlplus / as sysdba

SQL*Plus: Release 12.2.0.1.0 Production on Sun Mar 4 12:15:12 2018

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to an idle instance.

SQL> startup
ORACLE instance started.

Total System Global Area 1660944384 bytes
Fixed Size                  8621376 bytes
Variable Size            1056965312 bytes
Database Buffers          587202560 bytes
Redo Buffers                8155136 bytes
Database mounted.
Database opened.
SQL> exit
Disconnected from Oracle Database 12c Enterprise Edition Release 12.2.0.1.0 - 64bit Production
```
