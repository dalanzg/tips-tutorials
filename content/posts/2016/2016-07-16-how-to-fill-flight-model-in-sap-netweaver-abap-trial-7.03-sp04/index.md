---
title: How to fill flight model in SAP Netweaver ABAP Trial 7.03 SP04
description: There is a report to clean and restore data flight model in SAP ABAP Netweaver. This report is very useful to refresh all data flight model.
date: 2016-07-16T9:00:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [sap]
author: dalanzg
image: 
  url: img/run-report.png
  width: 872
  height: 613
comments: true
---

This tutorial will explain how to fill flight model in SAP Netweaver.

{{< bootstrap-figure src="img/flight-model.png" title="Flight model" width="500">}}

{{< bootstrap-table "table table-bordered-striped" >}}
| Table     | Description      | Keys                                                                                   |
|-----------|------------------|----------------------------------------------------------------------------------------|
| T000      | Client           | client                                                                                 |
| SCURX     | Currencies       | currency                                                                               |
| SBUSPART  | Business partner | client, partner number                                                                 |
| STRAVELAG | Travel agencies  | client, travel agency number                                                           |
| SCUSTOM   | Customers        | client, customer number                                                                |
| SCARR     | Carriers         | client, carrier ID                                                                     |
| SCOUNTER  | Sales counters   | client, carrier ID, sales counter number                                               |
| SPFLI     | Flight schedule  | client, carrier ID, connection number                                                  |
| SFLIGHT   | Flights          | client, carrier ID, connection number, date of flight                                  |
| SBOOK     | Flight bookings  | client, carrier ID, connection number, date of flight, booking number, customer number |
{{</ bootstrap-table >}}

Run the following report in **SE38** transaction:

- **SAPBC_DATA_GENERATOR**

{{< bootstrap-figure src="img/SAPBC_DATA_GENERATOR-report.png" title="Flight model" width="500">}}

{{< bootstrap-figure src="img/run-report.png" title="Run report" width="500">}}

{{< bootstrap-figure src="img/log-report.png" title="Log report" width="300">}}
