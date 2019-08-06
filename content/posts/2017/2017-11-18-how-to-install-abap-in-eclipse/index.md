---
title: How to install ABAP in Eclipse
description: This tutorial will explain how to install ABAP in Eclipse to explore or modify ABAP Development objects.
date: 2017-11-18T21:20:00+01:00
lastmod: 2017-11-18T21:20:00+01:00
tags: [sap]
author: dalanzg
image:  
  url: "img/open-abap-object-2.png"
  width: 1280
  height: 777
comments: true
---

This tutorial will explain how to install ABAP in Eclipse to explore or modify ABAP Development objects.

## Requirements

You will need the following:

- [SAP NetWeaver Server and SAPGUI]({{< ref "/posts/2017/2017-11-09-how-to-install-sap-netweaver-751-sp02-as-abap-developer-edition-in-opensuse" >}})
- Eclipse (Oxygen)

## Steps

Run Eclipse and install a new software from the menu.

{{< bootstrap-figure src="img/install-new-software.png" title="Install new software" width="500">}}

Add the SAP HANA tools repository:

- [https://tools.hana.ondemand.com/oxygen](https://tools.hana.ondemand.com/oxygen)**

Install the following components:

- **ABAP Development Tools for SAP NetWeaver**
- **SAP Cloud Platform Tools**
- **UI Development Toolkit for HTML5**

{{< bootstrap-figure src="img/tools-hana-oxygen-1.png" title="Install Eclipse HANA Tools" width="500">}}

{{< bootstrap-figure src="img/tools-hana-oxygen-2.png" title="Install Eclipse HANA Tools" width="500">}}

{{< bootstrap-figure src="img/tools-hana-oxygen-3.png" title="Install Eclipse HANA Tools" width="500">}}

Click on the top right-hand corner to open ABAP perspective.

{{< bootstrap-figure src="img/open-abap-perspective.png" title="Open ABAP perspective" width="500">}}

Create a new ABAP Project (File > New > ABAP Project), and select your system connection created by SAPGUI.

{{< bootstrap-figure src="img/system-connection-1.png" title="SAP System connection" width="500">}}

{{< bootstrap-figure src="img/system-connection-2.png" title="Connection settings" width="500">}}

{{< bootstrap-figure src="img/system-connection-3.png" title="Client, User and Password" width="500">}}

{{< bootstrap-figure src="img/system-connection-4.png" title="Project name" width="500">}}

You can browse all the System Library, and open any ABAP object.

{{< bootstrap-figure src="img/system-library.png" title="System Library" width="400">}}

To open a specific object click on Open ABAP Development Object.

{{< bootstrap-figure src="img/open-abap-object-1.png" title="Open ABAP object" width="500">}}

{{< bootstrap-figure src="img/open-abap-object-2.png" title="Open ABAP object" width="500">}}

Besides, you can run SAPGUI from Eclipse.

{{< bootstrap-figure src="img/run-sapgui.png" title="System connection" width="500">}}
