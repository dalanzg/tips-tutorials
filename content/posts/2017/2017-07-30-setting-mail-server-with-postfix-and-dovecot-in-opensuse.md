---
title: Setting mail server with Postfix and Dovecot in openSUSE
description: A tutorial to set a mail server for inbound and outgoing mails.
date: 2017-07-30T22:15:00+02:00
lastmod: 2017-07-30T22:15:00+02:00
tags: [opensuse, postfix, dovecot]
author: dalanzg
comments: true
---

A tutorial to set a mail server for inbound and outgoing mails:

- Postfix for outgoing mails (SMTP)
- Dovecot for inbound mails (IMAP/POP3)

## Requirements

You will need the following:

- An openSUSE virtual machine with VirtualBox ([Check this link]({{< ref "/posts/2017/2017-02-12-how-to-install-opensuse-leap-42.2-in-virtualbox/index.md" >}})
- Hostname and domain -> [Setting Hostname and domain in openSUSE]({{< ref "/posts/2017/2017-04-13-setting-hostname-and-domain-in-opensuse" >}})
- SMTP server with Postfix running -> [Setting Postfix for outgoing mail in openSUSE]({{< ref "/posts/2017/2017-04-14-setting-postfix-for-outgoing-mail-in-opensuse" >}})

In this tutorial, the machine has the following:

- FQDN hostname: **mail.dalanzg.com**
- Ethernet IP address: **10.0.2.15**
- User for inbound emails: **dalanz@dalanzg.com**

## Steps

- [Review Postfix configuration](#review-postfix-configuration)
- [Install and setup Dovecot](#install-and-setup-dovecot)
- [Send mail with telnet](#send-mail-with-telnet)
- [Retrieve mail with Dovecot](#retrieve-mail-with-dovecot)

### Review Postfix configuration

Besides setting the initial Postfix configuration, modify **/etc/postfix/main.cf** file to store mails with Maildir format.

```terminal
dalanz@mail:~> vim /etc/postfix/main.cf
```

```vim
home_mailbox = Maildir/
inet_interfaces = localhost, 10.0.2.15
inet_protocols = all
mydestination = $myhostname, localhost.$mydomain, $mydomain
myhostname = mail.dalanzg.com
```

Restart Postfix service.

```terminal
dalanz@mail:~> sudo service postfix restart
```

### Install and setup Dovecot

Install Dovecot by zypper command.

```terminal
dalanz@mail:~> sudo zypper install dovecot
```

Edit Dovecot main configuration file to allow IMAP, POP3 and LMPT protocols. Edit file **/etc/dovecot/dovecot.conf**.

```vim
protocols = imap pop3 lmtp
```

Edit Mailbox location for mail users. Edit file **/etc/dovecot/conf.d/10-mail.conf**.

```vim
mail_location = maildir:~/Maildir
```

Edit authentication process. Edit file **/etc/dovecot/conf.d/10-auth.conf**

```vim
disable_plaintext_auth = yes

auth_mechanisms = plain login
```

Edit master process. Add an Postfix Unix Listener in service auth. Edit file **/etc/dovecot/conf.d/10-master.conf**

```vim
service auth {
  ...
  unix_listener /var/spool/postfix/private/auth {
    mode = 0660
    user = postfix
    group = postfix
  }
  ...
}
```

Start Dovecot service.

```terminal
dalanz@mail:~> sudo service dovecot start
```

### Send mail with telnet

Let's send an email to my current user, and check that it is stored in ~/Maildir folder for dalanz@dalanzg.com.

```terminal
dalanz@mail:~> telnet mail.dalanzg.com smtp
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
250-DSN
250 SMTPUTF8
mail from: test@dalanzg.com
250 2.1.0 Ok
rcpt to: dalanz@dalanzg.com
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
Subject: Dovecot test

This is a test with Dovecot.

Best regards,

Admin

250 2.0.0 Ok: queued as 9BC141B4
quit
221 2.0.0 Bye
Connection closed by foreign host.
```

Let's list new dalanz's mails.

```terminal
dalanz@mail:~> ll /home/dalanz/Maildir/new
total 4
-rw------- 1 dalanz users 474 Jul 30 21:10 1501441859.V34I9c2M448896.mail
```

Let's see mail content.

```terminal
dalanz@mail:~> cat /home/dalanz/Maildir/new/1501441859.V34I9c2M448896.mail
Return-Path: <test@dalanzg.com>
X-Original-To: dalanz@dalanzg.com
Delivered-To: dalanz@dalanzg.com
Received: from dalanzg.com (mail.dalanzg.com [10.0.2.15])
        by mail.dalanzg.com (Postfix) with ESMTP id 9BC141B4
        for <dalanz@dalanzg.com>; Sun, 30 Jul 2017 21:09:33 +0200 (CEST)
Subject: Dovecot test
Message-Id: <20170730190946.9BC141B4@mail.dalanzg.com>
Date: Sun, 30 Jul 2017 21:09:33 +0200 (CEST)
From: test@dalanzg.com

This is a test with Dovecot.

Best regards,

Admin
```

### Retrieve mail with Dovecot

Let's retrieve the same email with a POP3 account.

```terminal
dalanz@mail:~> telnet mail.dalanzg.com pop3
Trying 10.0.2.15...
Connected to mail.dalanzg.com.
Escape character is '^]'.
+OK Dovecot ready.
user dalanz
+OK
pass Okm1234,
+OK Logged in.
retr 1
+OK 490 octets
Return-Path: <test@dalanzg.com>
X-Original-To: dalanz@dalanzg.com
Delivered-To: dalanz@dalanzg.com
Received: from dalanzg.com (mail.dalanzg.com [10.0.2.15])
        by mail.dalanzg.com (Postfix) with ESMTP id 9BC141B4
        for <dalanz@dalanzg.com>; Sun, 30 Jul 2017 21:09:33 +0200 (CEST)
Subject: Dovecot test
Message-Id: <20170730190946.9BC141B4@mail.dalanzg.com>
Date: Sun, 30 Jul 2017 21:09:33 +0200 (CEST)
From: test@dalanzg.com

This is a test with Dovecot.

Best regards,

Admin
.
quit
+OK Logging out.
Connection closed by foreign host.
```

Dovecot works!!

And check email was moved from **~/Maildir/new** to **~/Maildir/cur** folder, since it was read by a POP3 account.

```terminal
dalanz@mail:~> ll /home/dalanz/Maildir/new
total 0
dalanz@mail:~> ll /home/dalanz/Maildir/cur
-rw------- 1 dalanz users 474 Jul 30 21:30 1501441859.V34I9c2M448896.mail
```
