---
title: Setting Postfix for outgoing mail in openSUSE
description: A tutorial to set Postfix to send mails in openSUSE.
date: 2017-04-14T12:45:00+02:00
lastmod: 2017-07-30T20:30:00+02:00
tags: [opensuse, postfix]
author: dalanzg
image:  
  url: "img/mail-sent-by-telnet.png"
  width: 603
  height: 274
comments: true
---

A tutorial to set Postfix to send mails in openSUSE.

## Requirements

You will need the following:

- An openSUSE virtual machine with VirtualBox ([Check this link]({{< ref "/posts/2017/2017-02-12-how-to-install-opensuse-leap-42.2-in-virtualbox" >}})
- Hostname and domain -> [Setting Hostname and domain in openSUSE]({{< ref "/posts/2017/2017-04-13-setting-hostname-and-domain-in-opensuse" >}})

In this tutorial, the machine has the following:

- FQDN hostname: **mail.dalanzg.com**
- Ethernet IP address: 10.0.2.15

## Steps

- [Install Postfix](#install-postfix)
- [Change main.cf file](#change-main.cf-file)
- [Send mail with telnet](#send-mail-with-telnet)

### Install Postfix

By openSUSE installation default, Postfix can be installed. If not, install it with YaST or zypper.

```terminal
dalanz@mail:~> sudo zypper install postfix
```

Check that postfix service is running.

```terminal
dalanz@mail:~> sudo service postfix status
* postfix.service - Postfix Mail Transport Agent
   Loaded: loaded (/usr/lib/systemd/system/postfix.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2017-04-14 11:02:21 CEST; 2min 27s ago
  Process: 1152 ExecStartPost=/etc/postfix/system/cond_slp register (code=exited, status=0/SUCCESS)
  Process: 1143 ExecStartPost=/etc/postfix/system/wait_qmgr 60 (code=exited, status=0/SUCCESS)
  Process: 1035 ExecStart=/usr/sbin/postfix start (code=exited, status=0/SUCCESS)
  Process: 1029 ExecStartPre=/etc/postfix/system/update_postmaps (code=exited, status=0/SUCCESS)
  Process: 1024 ExecStartPre=/etc/postfix/system/update_chroot (code=exited, status=0/SUCCESS)
  Process: 1017 ExecStartPre=/etc/postfix/system/config_postfix (code=exited, status=0/SUCCESS)
  Process: 1009 ExecStartPre=/bin/echo Starting mail service (Postfix) (code=exited, status=0/SUCCESS)
 Main PID: 1141 (master)
    Tasks: 3 (limit: 512)
   CGroup: /system.slice/postfix.service
           |-1141 /usr/lib/postfix/master -w
           |-1144 pickup -l -t fifo -u
           `-1145 qmgr -l -t fifo -u

Apr 14 11:02:19 mail systemd[1]: Starting Postfix Mail Transport Agent...
Apr 14 11:02:20 mail echo[1009]: Starting mail service (Postfix)
Apr 14 11:02:21 mail postfix/master[1141]: daemon started -- version 2.11.8, configuration /etc/postfix
Apr 14 11:02:21 mail systemd[1]: Started Postfix Mail Transport Agent.
```

### Change main.cf file

With root permissions, modify **/etc/postfix/main.cf** file.

```terminal
dalanz@mail:~> sudo vim /etc/postfix/main.cf
```

At the end of the file, modify or add the following parameters:

```vim
inet_interfaces = localhost, 10.0.2.15
inet_protocols = all
mydestination = $myhostname, localhost.$mydomain, $mydomain
myhostname = mail.dalanzg.com
```

Restart Postfix service.

```terminal
dalanz@mail:~> sudo service postfix restart
```

### Send mail with telnet

Send an email with Telnet. This is the procedure:

- Connect with telnet:

```terminal
dalanz@mail:~> telnet mail.dalanzg.com 25
```

- Type ehlo or helo followed by your domain (dalanzg.com)

```terminal
ehlo dalanzg.com
```

- Type sender mail. The user must be in your domain (dalanzg.com)

```terminal
mail from: admin@dalanzg.com
```

- Type recipients separated by comma

```terminal
rcpt to: user1@gmail.com, user2@yahoo.com
```

- Write the message. Type **data**, followed by your subject and message. End with **.** and **quit** to close the connection.

This is the complete process:

```terminal
dalanz@mail:~> telnet mail.dalanzg.com 25
Trying 10.0.2.15...
Connected to mail.dalanzg.com.
Escape character is '^]'.
220 mail.dalanzg.com ESMTP
ehlo dalanzg.com
250-mail.dalanzg.com
250-PIPELINING
250-SIZE
250-ETRN
250-ENHANCEDSTATUSCODES
250-8BITMIME
250 DSN
mail from: admin@dalanzg.com
250 2.1.0 Ok
rcpt to: {email}
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
Subject: My telnet test with Postfix

Hi,

This is an email sent by using telnet command. Mail service was set with Postfix.

Best regards,

Admin

.
250 2.0.0 Ok: queued as E57AA60775
quit
221 2.0.0 Bye
Connection closed by foreign host.
```

And check your inbox.

{{< bootstrap-figure src="img/mail-sent-by-telnet.png" title="Email sent by telnet" width="500">}}
