---
title: Setting SSL or https in Tomcat with openSUSE
description: A tutorial to configure Tomcat 8 to support SSL or https connection.
date: 2017-04-07T20:53:00+02:00
lastmod: 2017-04-07T20:53:00+02:00
tags: [opensuse, tomcat]
author: dalanzg
image:  
  url: "img/tomcat-with-ssl.png"
  width: 1280
  height: 709
comments: true
---

A tutorial to configure Tomcat 8 to support SSL or https connection.

## Requirements

You will need the following:

- An openSUSE virtual machine with VirtualBox ([Check this link]({{< ref "/posts/2017/2017-02-12-how-to-install-opensuse-leap-42.2-in-virtualbox/index.md" >}})
- Java JDK to create Keystores -> [Change Java OpenJDK to Oracle JDK]({{< ref "/posts/2017/2017-03-18-change-openjdk-to-oracle-jdk-in-opensuse" >}})
- Tomcat -> [How to install tomcat in openSUSE]({{< ref "/posts/2017/2017-03-24-how-to-install-tomcat-in-opensuse" >}})

This tutorial was created with:

- openSUSE Leap 42.2
- Java Version -> jdk1.8.0_121
- Tomcat version -> apache-tomcat-8.0.42

## Steps

- [Create a keystore using Java JDK](#create-a-keystore-using-java-jdk)
- [Set keystore in Tomcat](#set-keystore-in-tomcat)
- [Check SSL or https connection](#check-ssl-or-https-connection)

### Create a keystore using Java JDK

Keystore file is a container for authorization certificates or public key certificates. They are identified by an alias from a trust chain.

A new keystore file with only 1 alias will be created with the following parameters:

- Keystore file -> keystore.jks
- Keystore password -> dalanzg
- Alias -> tomcat
- Alias password -> dalanzg

```terminal
dalanz@linux-geij:~> keytool -genkey -alias tomcat -keyalg RSA -keystore keystore.jks
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Daniel Lanza
What is the name of your organizational unit?
  [Unknown]:  dalanzg
What is the name of your organization?
  [Unknown]:  dalanzg
What is the name of your City or Locality?
  [Unknown]:  Santander
What is the name of your State or Province?
  [Unknown]:  Cantabria
What is the two-letter country code for this unit?
  [Unknown]:  ES
Is CN=Daniel Lanza, OU=dalanzg, O=dalanzg, L=Santander, ST=Cantabria, C=ES correct?
  [no]:  yes

Enter key password for <tomcat>
        (RETURN if same as keystore password):
Re-enter new password:
```

File **keystore.jks** was created. Check the contents referring to the keystore file (*keystore.jks*) or the keystore alias (*tomcat*).

```terminal
dalanz@linux-geij:~> keytool -list -keystore keystore.jks
Enter keystore password:

Keystore type: JKS
Keystore provider: SUN

Your keystore contains 1 entry

tomcat, Apr 7, 2017, PrivateKeyEntry,
Certificate fingerprint (SHA1): 48:36:A0:BA:49:25:D8:B4:0E:1B:DB:07:98:10:52:AC:FE:6A:7A:52
```

```terminal
dalanz@linux-geij:~> keytool -list -alias tomcat
Enter keystore password:
tomcat, Apr 7, 2017, PrivateKeyEntry,
Certificate fingerprint (SHA1): 48:36:A0:BA:49:25:D8:B4:0E:1B:DB:07:98:10:52:AC:FE:6A:7A:52
```

### Set keystore in Tomcat

In Tomcat directory, edit **server.xml** located in **conf** folder. Find the following expression and uncomment.

```vim
<--
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
    clientAuth="false" sslProtocol="TLS" />
-->
```

Add **keystoreFile** and **keystorePass** parameters.

```vim
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
    clientAuth="false" sslProtocol="TLS"
    keystoreFile="/home/dalanz/keystore.jks" keystorePass="dalanzg" />
```

### Check SSL or https connection

Go to [https://localhost:8443](https://localhost:8443) with your browser. The certificate generated was self-signed, so the connection is not secure. You need to trust in your own certificate to continue.

{{< bootstrap-figure src="img/connection-not-secure.png" title="Connection is not secure" width="500">}}

In this case, with FireFox, save the certificate in the browser. Now, browser and tomcat share the same certificate and you can access with a SSL or https connection.

{{< bootstrap-figure src="img/add-exception.png" title="Add exception" width="500">}}

{{< bootstrap-figure src="img/confirm-security-exception.png" title="Confirm security exception" width="500">}}

{{< bootstrap-figure src="img/tomcat-with-ssl.png" title="Tomcat with SSL or https connection" width="500">}}
