---
title: How to install Apache HTTPD in openSUSE
description: This tutorial will explain how to install Apache HTTPD in openSUSE 15.
date: 2018-06-20T12:00:00+02:00
lastmod: 2019-08-06T14:30:00+02:00
tags: [opensuse, httpd]
author: dalanzg
image:
  url: "img/it-works-apache2.png"
  width: 597
  height: 263
comments: true
---

This tutorial will explain how to install Apache HTTPD in openSUSE 15.

The original documentation is found in [The Apache HTTP Server Project](https://httpd.apache.org/). And the official documentation for openSUSE is in [The Apache HTTP Server](https://doc.opensuse.org/documentation/leap/reference/html/book.opensuse.reference/cha.apache2.html).

## Requirements

You will need the following:

- openSUSE Leap 15

## Steps

- [Set hostname and domain](#set-hostname-and-domain)
- [Install Apache HTTPD](#install-apache-httpd)
- [Start and stop Apache](#start-and-stop-apache)
- [Start Apache automatically at boot time](#start-apache-automatically-at-boot-time)

### Set hostname and domain

Files **/etc/hostname** and **/etc/hosts** will be modified to resolve the following:

- **Hostname** -> apachesrv
- **Domain** -> dalanzg.com

More information in [Setting hostname and domain in openSUSE.]({{< ref "/posts/2017/2017-04-13-setting-hostname-and-domain-in-opensuse" >}})

```terminal
dalanz@apachesrv:~> hostname
apachesrv
dalanz@apachesrv:~> hostname --fqdn
apachesrv.dalanzg.com
```

### Install Apache HTTPD

Install Apache HTTPD with example pages in **YaST2** or **zipper** command.

```terminal
dalanz@apachesrv:~> sudo zypper install apache2 apache2-example-pages
```

### Start and stop Apache

Start the Apache service with the following command:

```terminal
dalanz@apachesrv:~> sudo systemctl start apache2
```

Check you can access the example page -> [http://localhost](http://localhost)

{{< bootstrap-figure src="img/it-works-apache2.png" title="It works" width="500">}}

Check the status:

```terminal
dalanz@apachesrv:~> sudo systemctl status apache2
● apache2.service - The Apache Webserver
   Loaded: loaded (/usr/lib/systemd/system/apache2.service; disabled; vendor preset: di>
   Active: active (running) since Mon 2018-08-20 11:35:59 CEST; 2min 56s ago
 Main PID: 4788 (httpd-prefork)
   Status: "Total requests: 0; Current requests/sec: 0; Current traffic:   0 B/sec"
    Tasks: 6
   CGroup: /system.slice/apache2.service
           ├─4788 /usr/sbin/httpd-prefork -DSYSCONFIG -C PidFile /var/run/httpd.pid -C >
           ├─4794 /usr/sbin/httpd-prefork -DSYSCONFIG -C PidFile /var/run/httpd.pid -C >
           ├─4795 /usr/sbin/httpd-prefork -DSYSCONFIG -C PidFile /var/run/httpd.pid -C >
           ├─4796 /usr/sbin/httpd-prefork -DSYSCONFIG -C PidFile /var/run/httpd.pid -C >
           ├─4797 /usr/sbin/httpd-prefork -DSYSCONFIG -C PidFile /var/run/httpd.pid -C >
           └─4799 /usr/sbin/httpd-prefork -DSYSCONFIG -C PidFile /var/run/httpd.pid -C >

Aug 20 11:35:59 apachesrv systemd[1]: Starting The Apache Webserver...
Aug 20 11:35:59 apachesrv systemd[1]: Started The Apache Webserver.
```

And stop the service:

```terminal
dalanz@apachesrv:~> sudo systemctl stop apache2
```

### Start Apache automatically at boot time

You can start Apache automatically at boot time.

```terminal
dalanz@apachesrv:~> sudo systemctl enable apache2
Created symlink /etc/systemd/system/httpd.service → /usr/lib/systemd/system/apache2.service.
Created symlink /etc/systemd/system/apache.service → /usr/lib/systemd/system/apache2.service.
Created symlink /etc/systemd/system/multi-user.target.wants/apache2.service → /usr/lib/systemd/system/apache2.service.
```
