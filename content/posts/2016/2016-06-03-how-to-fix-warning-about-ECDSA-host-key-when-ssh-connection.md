---
title: How to fix warning about ECDSA host key when SSH connection
description: Remove SSH cache to establish a SSH connection with same IP address.
date: 2016-06-03T18:00:00+02:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [linux]
author: dalanzg
comments: true
---

This tutorial will explain how to fix warning about ECDSA host key when SSH connection.

When establishing a new SSH connection, a fingerprint is cached. Hence, if you use the same IP address for several machines, a warning message can turn up.

```terminal
$ ssh dalanz@192.168.56.101
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:p4ZGs+YjsBAw26tn2a+HPkga1dPWWAWX+NEm4Cv4I9s.
Please contact your system administrator.
Add correct host key in /Users/dalanz/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /Users/dalanz/.ssh/known_hosts:9
ECDSA host key for 192.168.56.101 has changed and you have requested strict checking.
Host key verification failed.
```

Remove the cached key for the IP address on the local machine:

```terminal
$ ssh-keygen -R 192.168.56.101
# Host 192.168.56.101 found: line 9
/Users/dalanz/.ssh/known_hosts updated.
Original contents retained as /Users/dalanz/.ssh/known_hosts.old
```

And, try again the SSH connection:

```terminal
$ ssh dalanz@192.168.56.101
The authenticity of host '192.168.56.101 (192.168.56.101)' can't be established.
ECDSA key fingerprint is SHA256:p4ZGs+YjsBAw26tn2a+HPkga1dPWWAWX+NEm4Cv4I9s.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.56.101' (ECDSA) to the list of known hosts.
```
