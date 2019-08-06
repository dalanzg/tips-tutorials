---
title: Tomcat load balancing with mod_proxy and mod_proxy_balancer
description: This tutorial will explain how to load balancing two Tomcat instances with Apache HTTPD server with mod_proxy and mod_proxy_balancer modules.
date: 2018-11-01T13:00:00+01:00
lastmod: 2019-08-06T14:30:00+02:00
tags: [opensuse, httpd]
author: dalanzg
image:
  url: "img/balancer-manager.png"
  width: 718
  height: 482
comments: true
---

This tutorial will explain how to load balancing two Tomcat instances with Apache HTTPD server with **mod_proxy** and **mod_proxy_balancer** modules.

The environment will be simulated with three virtual machines and VirtualBox:

{{< bootstrap-table "table table-bordered-striped" >}}
| # | Server FQDN                | IP             |                            |
|:-:|----------------------------|----------------|----------------------------|
| 1 | apache-server.dalanzg.com  | 192.168.56.101 | Apache HTTPD - Port 80     |
| 2 | tomcat-server1.dalanzg.com | 192.168.56.102 | Apache Tomcat - Port 8080  |
| 3 | tomcat-server2.dalanzg.com | 192.168.56.103 | Apache Tomcat - Port 8080  |
{{</ bootstrap-table >}}

Check the following post to configure [openSUSE with Internet and statick IP address by using YaST on VirtualBox]({{< ref "/posts/2018/2018-10-20-opensuse-with-internet-and-static-ip-address-by-using-yast-on-virtualbox" >}})

## Steps

- [Install Apache HTTPD](#install-apache-httpd)
- [Install Apache Tomcat](#install-apache-tomcat)
- [Load apache modules](#load-apache-modules)
- [Example 1 - Reverse Proxying a Single Backend Server](#example-1-reverse-proxying-a-single-backend-server)
- [Example 2 - Load Balancing Across Multiple Backend Servers](#example-2-load-balancing-across-multiple-backend-servers)

### Install Apache HTTPD

Install Apache HTTP in server #1

```terminal
dlanza@apache-server:~> sudo zypper install apache2 apache2-example-pages
dlanza@apache-server:~> sudo systemctl enable apache2
Created symlink /etc/systemd/system/httpd.service → /usr/lib/systemd/system/apache2.service.
Created symlink /etc/systemd/system/apache.service → /usr/lib/systemd/system/apache2.service.
Created symlink /etc/systemd/system/multi-user.target.wants/apache2.service → /usr/lib/systemd/system/apache2.service.
dlanza@apache-server:~> sudo systemctl start apache2
```
Check Apache HTTPD works in server #1 from your host machine.

{{< bootstrap-figure src="img/apache-httpd-works.png" title="Apache HTTPD works" width="400">}}

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

### Load apache modules

First, load the following modules by editing **loadmodule.conf** file:

- **mod_proxy** -> Redirect connections. It allows Apache to act as a gateway to the underlying application servers.
- **mod_proxy_http** -> Add support for proxying HTTP connections
- **mod_proxy_balancer** -> Add load balancing features for multiple backend servers
- **mod_lbmethod_byrequests** -> Parameter to specify balance by requests
- **mod_slotmem_shm** -> Requirement for mod_proxy_balancer

```terminal
dlanza@apache-server:~> cd /etc/apache2/
dlanza@apache-server:/etc/apache2> sudo vim loadmodule.conf
```

Add the modules at the end:

```conf
LoadModule proxy_module                   /usr/lib64/apache2-prefork/mod_proxy.so
LoadModule proxy_http_module              /usr/lib64/apache2-prefork/mod_proxy_http.so
LoadModule lbmethod_byrequests_module     /usr/lib64/apache2-prefork/mod_lbmethod_byrequests.so
LoadModule proxy_balancer_module          /usr/lib64/apache2-prefork/mod_proxy_balancer.so
LoadModule slotmem_shm_module             /usr/lib64/apache2-prefork/mod_slotmem_shm.so
```

Restart and check if Apache modules are loaded.

```terminal
dlanza@apache-server:/etc/apache2> sudo service apache2 restart
dlanza@apache-server:/etc/apache2> sudo httpd -M
Loaded Modules:
 core_module (static)
 so_module (static)
 http_module (static)
 mpm_prefork_module (static)
 unixd_module (static)
 systemd_module (static)
 actions_module (shared)
 alias_module (shared)
 auth_basic_module (shared)
 authn_file_module (shared)
 authz_host_module (shared)
 authz_groupfile_module (shared)
 authz_user_module (shared)
 autoindex_module (shared)
 cgi_module (shared)
 dir_module (shared)
 env_module (shared)
 expires_module (shared)
 include_module (shared)
 log_config_module (shared)
 mime_module (shared)
 negotiation_module (shared)
 setenvif_module (shared)
 ssl_module (shared)
 socache_shmcb_module (shared)
 userdir_module (shared)
 reqtimeout_module (shared)
 authn_core_module (shared)
 authz_core_module (shared)
 proxy_module (shared)
 proxy_http_module (shared)
 lbmethod_byrequests_module (shared)
 proxy_balancer_module (shared)
 slotmem_shm_module (shared)
```

### Example 1 - Reverse Proxying a Single Backend Server

All the Apache requests http://192.168.56.101/dalanzg will be redirected to server #2 (http://192.168.56.102:8080/dalanzg)

First, enable the modules **mod_proxy** and **mod_proxy_http** with **a2enmod**, and restart apache.

```terminal
dlanza@apache-server:/etc/apache2> sudo a2enmod proxy
dlanza@apache-server:/etc/apache2> sudo a2enmod proxy_http
dlanza@apache-server:/etc/apache2> sudo service apache2 restart
```

Create the a configuration file in **/etc/apache2/vhosts.d**

```terminal
dlanza@apache-server:/etc/apache2/vhosts.d> sudo vim singlebackend.conf
```

```conf
<VirtualHost *:80>
        ProxyPreserveHost On

        ProxyPass /dalanzg http://192.168.56.102:8080/dalanzg
        ProxyPassReverse /dalanzg http://192.168.56.102:8080/dalanzg
</VirtualHost>
```

- **ProxyPreserveHost** -> Makes Apache pass the original Host header to the backend server.
- **ProxyPass** -> The rule to redirect. If we only write /, all the request from Apache will be redirected
- **ProxyPassReverse** -> It tells Apache to modify the response headers from backend server. This makes sure that if the backend server returns a location redirect header, the client's browser will be redirected to the proxy address and not the backend server address, which would not work as intended.

Let's check the following URL -> http://192.168.56.101/dalanzg

{{< bootstrap-figure src="img/reverse-proxying-single-backend.png" title="Reverse Proxying a Single Backend Server" width="450">}}

### Example 2 - Load Balancing Across Multiple Backend Servers

All the Apache request will be balanced from http://192.168.56.101/dalanzg to:

- http://192.168.56.102:8080/dalanzg
- http://192.168.56.103:8080/dalanzg

Rename the Example 1 configuration file to **singlebackend.conf.bak** to disable the rule and restart apache.

```terminal
dlanza@apache-server:/etc/apache2/vhosts.d> sudo mv singlebackend.conf singlebackend.conf.bak
dlanza@apache-server:/etc/apache2/vhosts.d> sudo service apache2 restart
```

Activate the 5 modules and restart apache.

```terminal
dlanza@apache-server:/etc/apache2/vhosts.d> sudo a2enmod proxy
"proxy" already present
dlanza@apache-server:/etc/apache2/vhosts.d> sudo a2enmod proxy_http
"proxy_http" already present
dlanza@apache-server:/etc/apache2/vhosts.d> sudo a2enmod lbmethod_byrequests
dlanza@apache-server:/etc/apache2/vhosts.d> sudo a2enmod proxy_balancer
dlanza@apache-server:/etc/apache2/vhosts.d> sudo a2enmod slotmem_shm
dlanza@apache-server:/etc/apache2/vhosts.d> sudo service apache2 restart
```

Create a new conf file.

```terminal
dlanza@apache-server:/etc/apache2/vhosts.d> sudo vim balancer.conf
```

```conf
<VirtualHost *:80>
        ProxyRequests off
        ProxyPreserveHost On

        <Proxy balancer://mycluster>
                BalancerMember http://192.168.56.102:8080 route=worker1
                BalancerMember http://192.168.56.103:8080 route=worker2
                ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPass /dalanzg balancer://mycluster/dalanzg
        ProxyPassReverse /dalanzg balancer://mycluster/dalanzg

        <Location /balancer-manager>
                SetHandler balancer-manager
        </Location>

        ProxyPass /balancer-manager !
</VirtualHost>
```

- **ProxyRequests off** -> Turn off the standard proxy feature of mod_proxy.so
- **BalancerMember** -> The Worker URLs to balance
- **ProxySet lbmethod=byrequests** -> Balance by requests
- **SetHandler balancer-manager** -> Enable the balancer-manager page

And check how the page http://192.168.56.101/dalanzg is balanced from server #2 and #3.

{{< bootstrap-figure src="img/balancer1.png" title="Balanced to Tomcat 1" width="450">}}

{{< bootstrap-figure src="img/balancer2.png" title="Balanced to Tomcat 2" width="450">}}

Check the balancer-manager status -> http://192.168.56.101/balancer-manager

{{< bootstrap-figure src="img/balancer-manager.png" title="Balancer manager" width="500">}}

If Tomcat 2 (server #3) were down, the URL http://192.168.56.101/dalanzg would be still working, since the requests would go always to Tomcat 1 (server #2). Let's try it.

Shutdown Tomcat 2 in server #3.

```terminal
dlanza@tomcat-server2:~> /opt/apache-tomcat-8.5.34/bin/shutdown.sh
Using CATALINA_BASE:   /opt/apache-tomcat-8.5.34
Using CATALINA_HOME:   /opt/apache-tomcat-8.5.34
Using CATALINA_TMPDIR: /opt/apache-tomcat-8.5.34/temp
Using JRE_HOME:        /usr/lib64/jvm/jre
Using CLASSPATH:       /opt/apache-tomcat-8.5.34/bin/bootstrap.jar:/opt/apache-tomcat-8.5.34/bin/tomcat-juli.jar
```

Go several times to the URL http://192.168.56.101/dalanzg, and you will only see Tomcat 1 page.

{{< bootstrap-figure src="img/balancer1.png" title="Balanced to Tomcat 1" width="450">}}

Errors to balance Worker URL 2 will be gotten.

{{< bootstrap-figure src="img/errors-worker2.png" title="Balancer manager. Errors in worker2" width="500">}}
