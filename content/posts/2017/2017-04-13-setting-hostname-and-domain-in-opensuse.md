---
title: Setting hostname and domain in openSUSE
description: A tutorial to set hostname and domain in openSUSE.
date: 2017-04-13T18:00:00+02:00
lastmod: 2018-03-17T10:10:00+01:00
tags: [opensuse]
author: dalanzg
comments: true
---

A tutorial to set hostname and domain in openSUSE (server.dalanzg.com):

- **Hostname** -> srvopensuse
- **Domain** -> dalanzg.com

## Requirements

You will need the following:

- An openSUSE virtual machine with VirtualBox ([Check this link]({{< ref "/posts/2017/2017-02-12-how-to-install-opensuse-leap-42.2-in-virtualbox" >}})

## Steps

- [Modify hostname file](#modify-hostname-file)
- [Modify hosts file](#modify-hosts-file)
- [Verification](#verification)

### Modify hostname file

With root permissions, edit file **/etc/hostname**.

```terminal
dalanz@linux-geij:~> sudo vim /etc/hostname
```

Write the **hostname** for the server.

```vim
srvopensuse
```

### Modify hosts file

Check your network interfaces to find out the IP address for your Ethernet network (eth1).

```terminal
dalanz@linux-geij:~> sudo ifconfig
eth1      Link encap:Ethernet  HWaddr 08:00:27:91:22:4E
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe91:224e/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:234 errors:0 dropped:0 overruns:0 frame:0
          TX packets:266 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:22687 (22.1 Kb)  TX bytes:28332 (27.6 Kb)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:69 errors:0 dropped:0 overruns:0 frame:0
          TX packets:69 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1
          RX bytes:4630 (4.5 Kb)  TX bytes:4630 (4.5 Kb)
```

With root permissions, edit file **/etc/hosts**.

```terminal
dalanz@linux-geij:~> sudo vim /etc/hosts
```

Set your IP address for Ethernet network with your FQDN:

- 10.0.2.15       srvopensuse.dalanzg.com       srvopensuse

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
10.0.2.15       srvopensuse.dalanzg.com       srvopensuse

# special IPv6 addresses
::1             localhost ipv6-localhost ipv6-loopback

fe00::0         ipv6-localnet

ff00::0         ipv6-mcastprefix
ff02::1         ipv6-allnodes
ff02::2         ipv6-allrouters
ff02::3         ipv6-allhosts
```

Restart rcnetwork services.

```terminal
dalanz@linux-geij:~> sudo rcnetwork restart
```

### Verification

Check your parameters.

```terminal
dalanz@linux-geij:~> hostname --short
srvopensuse
dalanz@linux-geij:~> hostname --domain
dalanzg.com
dalanz@linux-geij:~> hostname --fqdn
srvopensuse.dalanzg.com
dalanz@linux-geij:~> hostname --ip-address
10.0.2.15
```
