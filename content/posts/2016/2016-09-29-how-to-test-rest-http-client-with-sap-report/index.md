---
title: How to test REST Http Client with SAP report
description: Before consuming external or internal REST Web Services, we need to check if a HTTP Client can be set. There is a SAP report in which REST requests can be sent to check out the response.
date: 2016-09-29T20:05:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [sap]
author: dalanzg
image: 
  url: "img/http-client-response.png"
  width: 1077
  height: 465
comments: true
---

This tutorial will explain how to check out if our SAP System can consume external or internal Web Services with a SAP report.

## Requirements

- External or internal REST service to consume within SAP

## Exercise

Firstly, find out what is your REST service to test. In this exercise we are going to consume an internal REST Web Service from a SAP Report.

GET -> http://hostname:port/zrest?myVar=50

Response:

```json
{"message": You have entered myVar=50 as query parameter."}
```

Check the following post if you want to implement -> [How to publish REST Web Service in SAP]({{< ref "/posts/2016/2016-09-25-how-to-publish-rest-webservice-in-sap" >}})

Go to **SE38** and find the following SAP report -> **RSHTTP01**.

Execute and fill up the parameters indicated in the following picture:

{{< bootstrap-figure src="img/http-client-request.png" title="HTTP Client request" width="500">}}

You will get the response.

{{< bootstrap-figure src="img/http-client-response.png" title="HTTP Client response" width="500">}}

This is a esay way to test external/internal REST Web Services connection without ABAP coding. If you get a 404 error code, ask the developer in charge of the REST Service or your SAP Basis Admin to fix connection problems.
