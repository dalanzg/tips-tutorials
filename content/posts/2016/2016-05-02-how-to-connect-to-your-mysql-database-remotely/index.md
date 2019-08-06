---
title: How to connect to your MySQL database remotely
description: A MySQL client is connected remotely to the database installed in Ubuntu Server virtual machine.
date: 2016-05-02T12:00:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [ubuntu, mysql]
author: dalanzg
image:
  url: "img/mybb-database-toad.png"
  width: 1150
  height: 787
comments: true
---

This tutorial will explain how to connect remotely to MySQL database. In this example, MySQL server is installed in Ubuntu Server virtual machine with VirtualBox.

## Requirements

- Ubuntu Server virtual machine with MySQL:
  - Two network adapters:
    - NAT adapter (eth0)
    - Host-only adapter (eth1) with a static IP address -> **192.168.56.101**.
  - Database name: mybb

- MySQL client:
  - [Toad World](https://www.toadworld.com/)

## Expose MySQL server

To expose MySQL to anything other than localhost you will have to expose the IP address. Find **bind-address** in **/etc/mysql/my.cnf** and assigned to your computers IP address.

```terminal
sudo vim /etc/mysql/my.cnf
```

Place your IP in **/etc/mysql/my.cnf** file.

```vim
bind-address = 192.168.56.101
```

Restart MySQL to apply changes.

```terminal
sudo service mysql restart
```

## Granting access

For a remote user to connect with the correct privileges, you need to have that user created in both the localhost and '%' as in.

Login to your MySQL server.

```terminal
mysql -u root -p
```

Make sure the service user for your application is both localhost and '%'.

```mysql
CREATE USER 'mybbuser'@'localhost' IDENTIFIED BY 'password';
CREATE USER 'mybbuser'@'%' IDENTIFIED BY 'password';
```

then, grant privileges on the MySQL database (database name: mybb).

```mysql
GRANT ALL ON mybb.* TO 'mybbuser'@'localhost';
GRANT ALL ON mybb.* TO 'mybbuser'@'%';
```

and finally,

```mysql
FLUSH PRIVILEGES;
EXIT;
```

## MySQL client

Open Toad application, and filled up your database parameters.

Test your connection and manage your database with your MySQL client.

{{< bootstrap-figure src="img/test-mysql-connection.png" title="Test connection" width="500">}}

{{< bootstrap-figure src="img/mybb-database-toad.png" title="MySQL database with Toad" width="500">}}
