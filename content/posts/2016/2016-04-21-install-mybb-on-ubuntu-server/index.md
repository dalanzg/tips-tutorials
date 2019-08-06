---
title: Install MyBB on Ubuntu Server
description: MyBB forum is installed in a Ubuntu Server virtual machine with VirtualBox.
date: 2016-04-21T15:00:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [ubuntu, mybb]
author: dalanzg
image:
  url: "img/mybb-forum.png"
  width: 1280
  height: 796
comments: true
---

This tutorial will explain how to install [MyBB](https://www.mybb.com/) in Ubuntu Server.

## Requirements

A Ubuntu Server virtual machine with:

- MySQL
- Apache
- FTP
- Samba

Check out these posts to get ready:

- [How to install Ubuntu Server on VirtualBox]({{< ref "/posts/2016/2016-04-15-install-ubuntu-server-on-virtualbox" >}})
- [Setting up a Virtual Web Server with VirtualBox, Apache, MySQL, FTP, Ubuntu, and Samba]({{< ref "/posts/2016/2016-04-17-setting-up-web-server-with-virtualbox-apache-mysql-ftp-ubuntu-and-samba" >}})

Our virtual machine has two network adapters. The second one (eth1) is a host-only adapter with a static IP address -> **192.168.56.101**

## Apache Web Server

Apache Web Server needs to be running. The installation and configuration was indicated in the previous post.

```terminal
sudo apt-get install apache2
```

## Install MySQL

MySQL will be the database management system.

We have installed MySQL. However, we are going to install other libraries and modules that let MyBB communicate easily with the database.

```terminal
sudo apt-get install mysql-server libapache2-mod-auth-mysql php5-mysql
```

## Install PHP

PHP is a requirement for MyBB.

```terminal
sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt
```

Restart Apache2.

```terminal
sudo service apache2 restart
```

Check out PHP is working.

By default, HTML and PHP files needs to be placed in **/var/www/html**. So, we are going to create a simple PHP file in that path.

```terminal
sudo nano /var/www/html/info.php
```

Type the following content and save the file.

```php
<?php phpinfo(); ?>
```

Check out you can display the PHP file on your local machine by typping [http://192.168.56.101/info.php](http://192.168.56.101/info.php).

{{< bootstrap-figure src="img/php-info-file.png" title="PHP info file" width="500">}}

## MySQL configuration

MySQL database needs to be initialized with the following command:

```terminal
sudo mysql_install_db
```

Then, our database needs to be secure. Run the following command:

```terminal
sudo mysql_secure_installation
```

And answer the following questions. Feel free to change your answers 😃

- Do you want to change MySQL root password? No
- Remove anonymous users? Yes
- Disallow root login remotely? Yes
- Remove test database and access to it? Yes
- Reload privilege tables now? Yes

## Apache configuration

PHP files needs to take preference over HTML files. Hence, we are going to change Apache file configuration.

```terminal
sudo vim /etc/apache2/mods-enabled/dir.conf
```

Make sure **index.php** is before **index.html**

```vim
<IfModule mod_dir.c>
      DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

Save and restart Apache.

```terminal
sudo service apache2 restart
```

## Database for MyBB

We need to create a database for our application. Log into MySQL as root user.

```terminal
mysql -u root -p
```

You will be in MySQL interface (**mysql >**).

Create an empty database.

```terminal
CREATE DATABSE mybb;
```

Create a user.

```terminal
CREATE USER 'mybbuser'@'localhost' IDENTIFIED BY 'password';
```

Grant permissions to the new user.

```terminal
GRANT ALL PRIVILEGES ON mybb.* TO 'mybbuser'@'localhost';
```

Reload all privileges with a flush command and exit the MySQL interface.

```terminal
FLUSH PRIVILEGES;
exit
```

## Install MyBB

We are going to download the MyBB package, so we are going to create a temp folder to place them.

```terminal
mkdir ~/temp
cd ~/temp
```

Download [MyBB package](https://www.mybb.com/download/), unzip and place **Upload** directory in **~/temp**. You can do it by FTP or Samba.

```terminal
sudo mv ~/temp/Upload /var/www/htm/forum
```

The installation wizard is available in the following address: [http://192.168.56.101/forum/install/](http://192.168.56.101/forum/install/)

{{< bootstrap-figure src="img/mybb-installation-wizard.png" title="Installation wizard" width="500">}}

{{< bootstrap-figure src="img/requirements-check.png" title="Requirements check" width="500">}}

As you can see, there are some errors because we need to meet MyBB requirements.

First, we place into **/var/www/htm/forum**

```terminal
cd /var/www/htm/forum
```

Rename **inc/config.default.php** to **inc/config.php**.

```terminal
sudo mv inc/config.default.php inc/config.php
```

Check again the MyBB requirements by the following url -> [http://192.168.56.101/forum/install/](http://192.168.56.101/forum/install/).

{{< bootstrap-figure src="img/requirements-check-2.png" title="Requirements check" width="500">}}

Give write permissions to **cache/**, **uploads/** and **uploads/avatars/**.

```terminal
sudo chmod 777 cache
sudo chmod 777 uploads
sudo chmod 777 uploads/avatars
```

Check out again the MyBB requirements, and now we meet them. So, we can follow the installation.

{{< bootstrap-figure src="img/requirements-check-ok.png" title="Requirements check" width="500">}}

Fill out the database configuration.

{{< bootstrap-figure src="img/database-configuration.png" title="Database configuration" width="500">}}

The installation wizard will create tables for MyBB application.

{{< bootstrap-figure src="img/table-creation.png" title="Table creation" width="500">}}

{{< bootstrap-figure src="img/table-population.png" title="Table population" width="500">}}

{{< bootstrap-figure src="img/theme-insertion.png" title="Theme insertion" width="500">}}

{{< bootstrap-figure src="img/board-configuration.png" title="Board configuration" width="500">}}

{{< bootstrap-figure src="img/create-administrator-account.png" title="Create administrator account" width="500">}}

{{< bootstrap-figure src="img/finish-setup.png" title="Finish setup" width="500">}}

Congratulations! You have successfully installed your MyBB. Go to your new MyBB forum ->  [http://192.168.56.101/forum/](http://192.168.56.101/forum/).

{{< bootstrap-figure src="img/remove-install-directory.png" title="Remove install directory" width="500">}}

Ups! Remove the install directory from your server to prevent other user runs the installation again.

```terminal
sudo rm -r /var/www/forum/install
```

Try again the forum url -> [http://192.168.56.101/forum/](http://192.168.56.101/forum/).

{{< bootstrap-figure src="img/mybb-forum.png" title="MyBB forum" width="500">}}

You got it! Enjoy your new MyBB forum on Ubuntu Server 😃
