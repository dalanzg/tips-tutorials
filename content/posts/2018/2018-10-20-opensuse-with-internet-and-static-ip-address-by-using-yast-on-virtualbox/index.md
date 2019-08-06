---
title: openSUSE with Internet and static IP address by using YaST on VirtualBox
description: An openSUSE virtual machine will be configured to have a static IP address and Internet connection on VirtualBox
date: 2018-10-20T19:00:00+02:00
lastmod: 2018-10-20T19:00:00+02:00
tags: [opensuse]
author: dalanzg
image:  
  url: "img/eth4-eth5-configured.png"
  width: 799
  height: 598
comments: true
---

An openSUSE virtual machine will be configured to have a static IP address and Internet connection on VirtualBox.

This scenario is really useful if you want to install a specific application in guest and you need to communicate from your host.

Therefore, the virtual machine will have two network adapters:

- NAT -> Internet connection
- Host-only-adapter -> Static IP address (192.168.56.200) and FQDN server1.dalanzg.com

## Steps

- [Settings for VirtualBox](#settings-for-virtualbox)
- [Virtual machine network settings](#virtual-machine-network-settings)
- [Settings for network devices](#settings-for-network-devices)

### Settings for VirtualBox

First, create a Host Network Manager in VirtualBox. In this case, **vboxnet0** with the following features:

{{< bootstrap-figure src="img/adapter-host-network-manager.png" title="Host network manager" width="500">}}

{{< bootstrap-figure src="img/dhcp-host-network-manager.png" title="Host network manager" width="500">}}

### Virtual machine network settings

Shutdown your virtual machine, and add two network adapters:

- NAT adapter for Internet connection
- Host-only-adapter with **vboxnet0** configuration

{{< bootstrap-figure src="img/nat-adapter.png" title="NAT adapter" width="500">}}

{{< bootstrap-figure src="img/host-only-adapter.png" title="Host-only-adapter" width="500">}}

### Settings for network devices

Start the virtual machine, and check the network devices available:

```terminal
dlanza@linux-klr1:~> ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:4a:28:1b brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth5
       valid_lft 86250sec preferred_lft 86250sec
    inet6 fe80::5fa5:3dc:b8c0:52f4/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: eth4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:b6:93:b5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.102/24 brd 192.168.56.255 scope global noprefixroute dynamic eth4
       valid_lft 1061sec preferred_lft 1061sec
    inet6 fe80::c12f:4627:19e4:79c5/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
````

The network devices are the following:

- eth5 -> NAT adapter (10.0.2.15)
- eth4 -> Host-only-adapter with **vboxnet0** configuration (dynamic IP - 192.168.56.102)

Now, let's going to set the following static IP address with YaST:

- eth5 -> NAT adapter (10.0.2.15)
- eth4 -> Host-only-adapter with **vboxnet0** configuration (static IP - 192.168.56.200)

```terminal
dlanza@linux-klr1:~> sudo yast
```

Go to System -> Network Settings.

{{< bootstrap-figure src="img/yast-network-settings.png" title="Network settings" width="500">}}

By default, the network setup method is Network manager service. You will get a warning, since YaSY is unable to configure some options.

{{< bootstrap-figure src="img/warning-network-manager.png" title="Warning Network manager" width="500">}}

Change Network setup method to Wicked service.

{{< bootstrap-figure src="img/network-manager-service.png" title="Network manager service" width="500">}}

{{< bootstrap-figure src="img/wicked-service.png" title="Wicked service" width="500">}}

The network devices for NAT and host-only adapters are not configured yet (**82540EM Gibabit Ethernet Controller**)

{{< bootstrap-figure src="img/nat-eth5.png" title="NAT network device" width="500">}}

Let's modify the network device **eth4**:

- Host-only-adapter with static IP address 192.168.56.200
- FQDN hostname server1.dalanzg.com

{{< bootstrap-figure src="img/host-only-adapter-eth4.png" title="Host-only-adapter network device" width="500">}}

{{< bootstrap-figure src="img/static-ip-address.png" title="Static IP address" width="500">}}

And, now the network device **eth5**:

- NAT adapter with DHCP

{{< bootstrap-figure src="img/nat-eth5-2.png" title="NAT network device" width="500">}}

{{< bootstrap-figure src="img/nat-dhcp-eth5.png" title="NAT network device" width="500">}}

Save changes and see that network devices are configured.

{{< bootstrap-figure src="img/eth4-eth5-configured.png" title="Network devices configured" width="500">}}

Now set hostname and DNS:

- Hostname -> server1
- Domain -> dalanzg.com
- DNS -> 8.8.4.4 (from Google)

{{< bootstrap-figure src="img/hostname-dns.png" title="Hostname and DNS" width="500">}}

And finally, save Network settings and reboot the virtual machine.

{{< bootstrap-figure src="img/save-network-settings.png" title="Save network settings" width="500">}}

After reboot, check again the network devices.

```terminal
dlanza@server1:~> ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:4a:28:1b brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global eth5
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe4a:281b/64 scope link 
       valid_lft forever preferred_lft forever
3: eth4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:b6:93:b5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.200/24 brd 192.168.56.255 scope global eth4
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:feb6:93b5/64 scope link 
       valid_lft forever preferred_lft forever
````

Next, check the FQDN hostname:

```terminal
dlanza@server1:~> hostname
server1
dlanza@server1:~> hostname -f
server1.dalanzg.com
dlanza@server1:~> ping server1.dalanzg.com
PING server1.dalanzg.com (192.168.56.200) 56(84) bytes of data.
64 bytes from server1.dalanzg.com (192.168.56.200): icmp_seq=1 ttl=64 time=0.160 ms
64 bytes from server1.dalanzg.com (192.168.56.200): icmp_seq=2 ttl=64 time=0.064 ms
^C
--- server1.dalanzg.com ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
rtt min/avg/max/mdev = 0.064/0.112/0.160/0.048 ms
```

And finally, the Internet connection:

```terminal
dlanza@server1:~> ping www.google.com
PING www.google.com (216.58.201.164) 56(84) bytes of data.
64 bytes from mad08s06-in-f4.1e100.net (216.58.201.164): icmp_seq=1 ttl=63 time=7.83 ms
64 bytes from mad08s06-in-f4.1e100.net (216.58.201.164): icmp_seq=2 ttl=63 time=6.73 ms
64 bytes from mad08s06-in-f4.1e100.net (216.58.201.164): icmp_seq=3 ttl=63 time=7.50 ms
^C
--- www.google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 6.739/7.360/7.835/0.469 ms
```
