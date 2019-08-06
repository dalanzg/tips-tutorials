---
title: How to enable SOAMANAGER in SAP Netweaver ABAP Trial 7.03 SP04
description: Steps to enable SOAMANAGER in Netweaver ABAP Trial 7.03 SP04. Web GUI for Netweaver needs to be available and license has to be applied.
date: 2016-07-14T19:00:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [sap]
author: dalanzg
image:
  url: "img/error-code-403-soamanager.png"
  width: 1280
  height: 796
comments: true
---

This tutorial will explain how to enable SOAMANAGER in SAP Netweaver ABAP Trial 7.03.

## Requirements

A virtual machine with:

- Windows 7 and SAP Netweaver ABAP Trial
- Web GUI for SAP Netweaver enabled

## Issue

When accessing **SOAMANAGER**, error code 403 turns up.

{{< bootstrap-figure src="img/error-code-403-soamanager.png" title="Error code 403 SOAMANAGER" width="500">}}

If we get error code 500, check Web GUI for Netweaver works properly. It is explained in [the installation post]({{< ref "/posts/2016/2016-07-12-how-to-install-sap-netweaver-abap-trial-7.03-sp04-on-windows-7" >}})

## Solution

Go to **SICF** transaction, and activate the following service:

- **default_host/sap/bc/webdynpro/appl_soap_manag ement**

{{< bootstrap-figure src="img/activate-soamanager-service.png" title="Activate SOAMANAGER service" width="500">}}

When it is activated, try again to access **SOAMANAGER** transaction.

{{< bootstrap-figure src="img/soamanager.png" title="SOAMANAGER" width="500">}}
