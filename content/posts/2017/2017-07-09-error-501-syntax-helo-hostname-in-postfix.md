---
title: Error 501 Syntax HELO hostname in postfix
description: Certain SMTP servers require HELO servername in order to allow a request to proceed. Set a command filter to let it work without any arguments.
date: 2017-07-09T21:05:00+02:00
lastmod: 2019-08-26T17:30:00+02:00
tags: [opensuse, postfix]
author: dalanzg
comments: true
---

Certain SMTP servers require HELO **servername** in order to allow a request to proceed. Set a command filter to let it work without any arguments.

[Source](http://www.postfix.org/postconf.5.html#smtpd_command_filter)

## Requirements

You will need the following:

- An openSUSE virtual machine with VirtualBox ([Check this link]({{< ref "/posts/2017/2017-02-12-how-to-install-opensuse-leap-42.2-in-virtualbox/index.md" >}})
- Hostname and domain -> [Setting Hostname and domain in openSUSE]({{< ref "/posts/2017/2017-04-13-setting-hostname-and-domain-in-opensuse" >}})
- SMTP server with Postfix running -> [Setting Postfix for outgoing mail in openSUSE]({{< ref "/posts/2017/2017-04-14-setting-postfix-for-outgoing-mail-in-opensuse" >}})

In this tutorial, the machine has the following:

- FQDN hostname: **mail.dalanzg.com**
- Ethernet IP address: 10.0.2.15
- Postfix running

```terminal
dalanzg@mail:~> telnet localhost 25
Trying ::1...
Connected to localhost
Escape character is `^]`.
220 mail.dalanzg.com ESMTP
HELO
501 Syntax: HELO hostname
quit
221 2.0.0. Bye
Connection closed by foreign host.
```

## Steps

- [Create command filter](#create-command-filter)
- [Add command filter in main.cf file](#add-command-filter-in-main.cf-file)
- [Restart postfix service](#restart-postfix-service)
- [Test](#test)

### Create command filter

Create the following file: **/etc/postfix/command_filter**

```terminal
dalanzg@mail:~> sudo vim /etc/postfix/command_filter
```

```vim
# Work around clients that send malformed HELO commands.
/^HELO\s*$/ HELO domain.invalid
# Work around clients that send empty lines.
/^\s*$/     NOOP
# Work around clients that send RCPT TO:<'user@domain'>.
# WARNING: do not lose the parameters that follow the address.
/^(RCPT\s+TO:\s*<)'([^[:space:]]+)'(>.*)/     $1$2$3
# Append XVERP to MAIL FROM commands to request VERP-style delivery.
# See VERP_README for more information on how to use Postfix VERP.
/^(MAIL FROM:\s*<listname@example\.com>.*)/   $1 XVERP
# Bounce-never mail sink. Use notify_classes=bounce,resource,software
# to send bounced mail to the postmaster (with message body removed).
/^(RCPT\s+TO:\s*<.*>.*)\s+NOTIFY=\S+(.*)/     $1 NOTIFY=NEVER$2
/^(RCPT\s+TO:.*)/                             $1 NOTIFY=NEVER
```

### Add command filter in main.cf file

Add the following in main.cf

```terminal
dalanzg@mail:~> sudo vim /etc/postfix/main.cf
```

```vim
smtpd_command_filter = pcre:/etc/postfix/command_filter
```

### Restart postfix service

Restart Postfix service.

```terminal
dalanz@mail:~> sudo service postfix restart
```

### Test

Send an email with Telnet using only HELO or EHLO (without hostname argument)

```terminal
dalanz@mail:~> telnet mail.dalanzg.com 25
Trying 10.0.2.15...
Connected to mail.dalanzg.com.
Escape character is '^]'.
220 mail.dalanzg.com ESMTP
ehlo
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
