---
title: How to install PostgreSQL in openSUSE
description: This tutorial will explain how to install postgreSQL server in openSUSE. A new role and database will be created with the client software.
date: 2017-07-31T18:15:00+02:00
lastmod: 2018-03-17T10:15:00+01:00
tags: [opensuse, postgresql]
author: dalanzg
comments: true
---

This tutorial will explain how to install postgreSQL server in openSUSE. A new role and database will be created with the client software.

## Requirements

You will need the following:

- openSUSE Leap 42.3

## Steps

- [Install PostgreSQL](#install-postgresql)
- [Post-installation checks](#post-installation-checks)
- [Change postgres role password](#change-postgres-role-password)
- [Create a new role](#create-a-new-role)
- [Create a new database](#create-a-new-database)

### Install PostgreSQL

Install PostgreSQL from YAST or zypper command:

- postgresql -> PostgreSQL client
- postgresql-server -> PostgreSQL server
- postgresql-contrib -> Contributed extensions and additions

```terminal
dalanz@linux-sgm1: ~> sudo zypper postgresql postgresql-server postgresql-contrib
```

### Post-installation checks

Use systemctl command to control **postgresql.service**.

```terminal
dalanz@linux-sgm1:~> sudo systemctl start postgresql.service
dalanz@linux-sgm1:~> sudo systemctl status postgresql.service
● postgresql.service - PostgreSQL database server
   Loaded: loaded (/usr/lib/systemd/system/postgresql.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-07-31 17:03:13 CEST; 17s ago
  Process: 5062 ExecStart=/usr/lib/postgresql-init start (code=exited, status=0/SUCCESS)
    Tasks: 7 (limit: 512)
   CGroup: /system.slice/postgresql.service
           ├─5089 /usr/lib/postgresql96/bin/postgres -D /var/lib/pgsql/data
           ├─5090 postgres: logger process
           ├─5092 postgres: checkpointer process
           ├─5093 postgres: writer process
           ├─5094 postgres: wal writer process
           ├─5095 postgres: autovacuum launcher process
           └─5096 postgres: stats collector process

Jul 31 17:03:05 linux-sgm1 systemd[1]: Starting PostgreSQL database server...
Jul 31 17:03:05 linux-sgm1 postgresql-init[5062]: Initializing PostgreSQL 9.6.3 at location /var/lib/pgsql/data
Jul 31 17:03:12 linux-sgm1 postgresql-init[5062]: 2017-07-31 17:03:12 CEST   LOG:  redirecting log output to logging collector process
Jul 31 17:03:12 linux-sgm1 postgresql-init[5062]: 2017-07-31 17:03:12 CEST   HINT:  Future log output will appear in directory "pg_log"
Jul 31 17:03:13 linux-sgm1 systemd[1]: Started PostgreSQL database server.
```

When first starting PostgreSQL server, the data will be created in **/var/lib/pgsql/data** folder.

PostgreSQL also needs to be enabled to start on reboot. Do that with this command:

```terminal
dalanz@linux-sgm1:~> sudo systemctl enable postgresql.service
Created symlink from /etc/systemd/system/multi-user.target.wants/postgresql.service to /usr/lib/systemd/system/postgresql.service.
```

After rebooting, PostgreSQL will run. You can control the services with the following commands:

```terminal
dalanz@linux-sgm1:~> sudo service postgresql status
dalanz@linux-sgm1:~> sudo service postgresql start
dalanz@linux-sgm1:~> sudo service postgresql stop
```

### Change postgres role password

Login with **postgres** user and connect to PostgreSQL console.

```terminal
dalanz@linux-sgm1:~> sudo su - postgres
postgres@linux-sgm1:~> psql
psql (9.6.3)
Type "help" for help.

postgres=#
```

Modify password by the following statement (remember quit PostgreSQL console with **\q**):

```terminal
postgres=# ALTER USER postgres WITH PASSWORD 'postgres';
ALTER ROLE
postgres=# \q
```

### Create a new role

By default, **postgres** role is created in the database. Create a new one with **createuser** command. You must be login with **postgres** user.

```terminal
dalanz@linux-sgm1:~> sudo su - postgres
postgres@linux-sgm1:~> createuser --interactive
Enter name of role to add: dalanz
Shall the new role be a superuser? (y/n) y
```

### Create a new database

Create a new database in which the role **dalanz** is the owner. Use the same database name to enable autologin in PostgreSQL console.

```terminal
dalanz@linux-sgm1:~> sudo su - postgres
postgres@linux-sgm1:~> createdb -O dalanz dalanz
```

Then, **dalanz** user can autologin only with **psql** command.

```terminal
dalanz@linux-sgm1:~> psql
psql (9.6.3)
Type "help" for help.

dalanz=#
```
