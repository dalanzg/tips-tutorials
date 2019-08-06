---
title: Tomcat load balancing with HAProxy in openSUSE
description: This tutorial will explain how to load balancing two Tomcat instances with HAProxy in openSUSE.
date: 2018-11-03T21:30:00+01:00
lastmod: 2018-11-03T21:30:00+01:00
tags: [opensuse, haproxy]
author: dalanzg
image: 
  url: "img/haproxy-works.png"
  width: 1280
  height: 431
comments: true
---

This tutorial will explain how to load balancing two Tomcat instances with HAProxy in openSUSE.

The environment will be simulated with three virtual machines and VirtualBox:

{{< bootstrap-table "table table-bordered-striped" >}}
| # | Server FQDN                 | IP             |                            |
|:-:|-----------------------------|----------------|----------------------------|
| 1 | haproxy-server.dalanzg.com  | 192.168.56.101 | HAProxy - Port 80          |
| 2 | tomcat-server1.dalanzg.com  | 192.168.56.102 | Apache Tomcat - Port 8080  |
| 3 | tomcat-server2.dalanzg.com  | 192.168.56.103 | Apache Tomcat - Port 8080  |
{{< /bootstrap-table >}}

Check the following post to configure [openSUSE with Internet and statick IP address by using YaST on VirtualBox]({{< ref "/posts/2018/2018-10-20-opensuse-with-internet-and-static-ip-address-by-using-yast-on-virtualbox" >}})

## Steps

- [Install HAProxy](#install-haproxy)
- [Install Apache Tomcat](#install-apache-tomcat)
- [Load Balancing Across Multiple Backend Servers with HAProxy](#load-balancing-across-multiple-backend-servers-with-haproxy)

### Install HAProxy

Install HAProxy in server #1

```terminal
dlanza@haproxy-server:~> sudo zypper install haproxy
dlanza@haproxy-server:~> sudo systemctl enable haproxy.service 
Created symlink /etc/systemd/system/multi-user.target.wants/haproxy.service → /usr/lib/systemd/system/haproxy.service.
dlanza@haproxy-server:~> sudo systemctl start haproxy.service
```

Check HAProxy works in server #1 from your host machine.

{{< bootstrap-figure src="img/haproxy-works.png" title="HAProxy works" width="900">}}

### Install Apache Tomcat

Steps for servers #2 and #3:

- Download **apache-tomcat-8.5.34.tar.gz**
- Deploy Apache Tomcat and change permissions
- Start service
- Create a static html file

```terminal
dlanza@tomcat-server1:~> wget http://apache.uvigo.es/tomcat/tomcat-8/v8.5.34/bin/apache-tomcat-8.5.34.tar.gz
dlanza@tomcat-server1:~> sudo mv apache-tomcat-8.5.34.tar.gz /opt/
dlanza@tomcat-server1:~> cd /opt/
dlanza@tomcat-server1:/opt> sudo tar -xf apache-tomcat-8.5.34.tar.gz 
dlanza@tomcat-server1:/opt> sudo chown -R dlanza:users apache-tomcat-8.5.34
dlanza@tomcat-server1:/opt> sudo rm apache-tomcat-8.5.34.tar.gz
Using CATALINA_BASE:   /opt/apache-tomcat-8.5.34
Using CATALINA_HOME:   /opt/apache-tomcat-8.5.34
Using CATALINA_TMPDIR: /opt/apache-tomcat-8.5.34/temp
Using JRE_HOME:        /usr/lib64/jvm/jre
Using CLASSPATH:       /opt/apache-tomcat-8.5.34/bin/bootstrap.jar:/opt/apache-tomcat-8.5.34/bin/tomcat-juli.jar
Tomcat started.
```

And create a static html file.

```terminal
dlanza@tomcat-server1:/opt> mkdir /opt/apache-tomcat-8.5.34/webapps/dalanzg
dlanza@tomcat-server1:/opt> vim /opt/apache-tomcat-8.5.34/webapps/dalanzg/index.html
```

```html
<html>
  <body>
    <h3>Tomcat server #1</h3>
  </body> 
</html> 
```

Check you can reach the index page from servers #2 and #3:

- http://192.168.56.102:8080/dalanzg
- http://192.168.56.103:8080/dalanzg

{{< bootstrap-figure src="img/tomcat1.png" title="Tomcat 1" width="450">}}

{{< bootstrap-figure src="img/tomcat2.png" title="Tomcat 2" width="450">}}

### Load Balancing Across Multiple Backend Servers with HAProxy

The HAProxy configuration will work with the following features:

- All the Apache request will be balanced from http://192.168.56.101/dalanzg to:
  - http://192.168.56.102:8080/dalanzg
  - http://192.168.56.103:8080/dalanzg
- The stats will be accessed with:
  - http://192.168.56.101:9000/stats
  - User authentication:
    - User: admin
    - Password: haproxy

Edit **haproxy.cfg** configuration file.

```terminal
dlanza@haproxy-server:~> su -
haproxy-server:~ # vim /etc/haproxy/haproxy.cfg
```

```conf
global
  log /dev/log daemon
  maxconn 32768
  chroot /var/lib/haproxy
  user haproxy
  group haproxy
  daemon
  stats socket /var/lib/haproxy/stats user haproxy group haproxy mode 0640 level operator
  tune.bufsize 32768
  tune.ssl.default-dh-param 2048
  ssl-default-bind-ciphers ALL:!aNULL:!eNULL:!EXPORT:!DES:!3DES:!MD5:!PSK:!RC4:!ADH:!LOW@STRENGTH

defaults
  log     global
  mode    http
  option  log-health-checks
  option  log-separate-errors
  option  dontlog-normal
  option  dontlognull
  option  httplog
  option  socket-stats
  retries 3
  option  redispatch
  maxconn 10000
  timeout connect     5s
  timeout client     50s
  timeout server    450s

listen stats
  bind *:9000
  mode http
  stats enable
  stats hide-version   #Hide HAProxy version
  stats show-node
  stats uri /stats   #Stats URI
  stats auth admin:haproxy   #Authentication credentials
  stats refresh 5s
  
frontend tomcat-service
  bind *:80
  acl dalanzg_uri path_beg /dalanzg
  use_backend tomcat-server if dalanzg_uri  #Balance if URI
  default_backend tomcat-server   #Balance by default

backend tomcat-server
  balance roundrobin
  server tomcat1 192.168.56.102:8080 check
  server tomcat2 192.168.56.103:8080 check
```

Restart HAProxy.

```terminal
haproxy-server:~ # service haproxy restart
```

Check stats with authentication in http://192.168.56.101:9000

{{< bootstrap-figure src="img/stats-auth.png" title="Authentication for stats" width="600">}}

{{< bootstrap-figure src="img/stats.png" title="Stats page" width="800">}}

Go several times to http://192.168.56.101/dalanzg

{{< bootstrap-figure src="img/balancer1.png" title="Tomcat worker 1" width="500">}}

{{< bootstrap-figure src="img/balancer2.png" title="Tomcat worker 2" width="500">}}

And now, shutdown Tomcat 2 and check that http://192.168.56.101/dalanzg is still working in Tomcat 1.

```terminal
dlanza@tomcat-server2:~> /opt/apache-tomcat-8.5.34/bin/shutdown.sh 
Using CATALINA_BASE:   /opt/apache-tomcat-8.5.34
Using CATALINA_HOME:   /opt/apache-tomcat-8.5.34
Using CATALINA_TMPDIR: /opt/apache-tomcat-8.5.34/temp
Using JRE_HOME:        /usr/lib64/jvm/jre
Using CLASSPATH:       /opt/apache-tomcat-8.5.34/bin/bootstrap.jar:/opt/apache-tomcat-8.5.34/bin/tomcat-juli.jar
```

{{< bootstrap-figure src="img/balancer1.png" title="Tomcat worker 1" width="500">}}

And tomcat2 is down in stats page.

{{< bootstrap-figure src="img/stats-tomcat2-down.png" title="Tomcat 2 down" width="800">}}
