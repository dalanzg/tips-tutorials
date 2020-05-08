---
title: How to renew the MiniSAP License
description: MiniSAP License expires in 3 months. This tutorial will show how to renew it.
date: 2016-10-01T16:35:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [sap]
author: dalanzg
image: 
  url: "img/expired-license-warning.png"
  width: 566
  height: 409
comments: true
---

This tutorial will explain how to renew your MiniSAP license. If you want to know how to install MiniSAP, check out this post:

- [How to install MiniSAP]({{< ref "/posts/2016/2016-07-12-how-to-install-sap-netweaver-abap-trial-7.03-sp04-on-windows-7/index.md" >}}).

When logging into MiniSAP, we may get two different pop-up windows:

- Warning about our license will expire soon
- Error about license expired

{{< bootstrap-figure src="img/expired-license-warning.png" title="Expired license warning." width="500">}}

Go to **SLICENSE** and get your Active Hardware Key.

{{< bootstrap-figure src="img/get-active-hardware-key-in-slicense-transaction.png" title="Active Hardware Key in SLICENSE transaction" width="500">}}

Go to [http://www.sap.com/minisap](http://www.sap.com/minisap) and fill up the form.

{{< bootstrap-figure src="img/minisap-form.png" title="Active Hardware Key in SLICENSE transaction" width="500">}}

You will get an email with a license file.

{{< bootstrap-figure src="img/license-file.png" title="License file" width="500">}}

Go back to **SLICENSE** transaction to proceed the license renewal.

If you have an active license, you need to remove it to apply the new one. If not, ignore this step and apply the license. Select Edit > Delete License.

{{< bootstrap-figure src="img/delete-license.png" title="Delete license" width="500">}}

And finally, apply the new license. Select Edit > Install License to upload the license file.

{{< bootstrap-figure src="img/install-license.png" title="Install license" width="500">}}

{{< bootstrap-figure src="img/license-applied-1.png" title="License applied" width="500">}}

{{< bootstrap-figure src="img/license-applied-2.png" title="License applied" width="500">}}

You got it! This license will expire in 3 months. To renew it, repeat again this tutorial.
