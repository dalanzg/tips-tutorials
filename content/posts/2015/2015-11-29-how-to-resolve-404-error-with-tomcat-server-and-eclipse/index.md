---
title: How to resolve 404 error with Tomcat Server and Eclipse
description: If you start Tomcat from Eclipse and get a 404-Error code when you go to http://localhost:8080, there is a easy way to fix the issue.
date: 2015-11-29T18:00:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [java]
author: dalanzg
image:
  url: "img/tomcat-index.png"
  width: 600
  height: 172
comments: true
---

If you start Tomcat from Eclipse and get a 404-Error code when you go to [http://localhost:8080](http://localhost:8080), there is a easy way to fix the issue.

Just go to Properties for Tomcat (right click on Tomcat - Servers Tab)

{{< bootstrap-figure src="img/tomcat-error-code-404-switch-location.png" title="Switch location Tomcat Properties" width="500">}}

Switch location from **[workspace metadata]** to **[/Servers/Tomcat v...]**

Restart Tomcat from Eclipse and go back to [http://localhost:8080](http://localhost:8080). You will see Tomcat configuration page.

{{< bootstrap-figure src="img/tomcat-index.png" title="Tomcat configuration page" width="500">}}
