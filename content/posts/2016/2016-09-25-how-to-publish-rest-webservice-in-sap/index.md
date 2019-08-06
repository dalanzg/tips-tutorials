---
title: How to publish REST Web Service in SAP
description: When SAP Netweaver Gateway is not available, there is an easy way to public custom REST Web Services so other external applications can integrate with SAP.
date: 2016-09-25T23:05:00+02:00
lastmod: 2017-02-12T13:00:00+01:00
tags: [sap]
author: dalanzg
image: 
  url: "img/create-service-in-sicf.png"
  width: 968
  height: 636
comments: true
---

This tutorial will explain how to publish REST Web Service when SAP Netweaver Gateway is not available.

## Exercise

Go to http://hostname:port/zrest?myVar=50 and the server will respond with the following content:

```json
{"message": You have entered myVar=50 as query parameter."}
```

Be in mind that this content could be JSON or XML format. In this exercise, it is a basic JSON object.

### Create custom class

Firstly, create a class object with IF_HTTP_EXTENSION interface.

{{< bootstrap-figure src="img/create-class-with-if_http_extension-interface.png" title="Create custom class with IF_HTTP_EXTENSION interface" width="500">}}

Implement the method IF_HTTP_EXTENSION~HANDLE_REQUEST, where you will have the **server** parameter. There, the query parameter will be read from the request and the response will be set in JSON format.

```abap
method IF_HTTP_EXTENSION~HANDLE_REQUEST.

  DATA: lt_fields             TYPE tihttpnvp,
        lv_header_query       TYPE string,
        lv_html               TYPE string.

  FIELD-SYMBOLS: <fs_field>       LIKE LINE OF lt_fields.

*" get HEADER fields
  server->request->get_header_fields(
      CHANGING
        fields = lt_fields    " Header fields
    ).

  " Read the fields table and look for name "~query_string" -- this will contain the URL query
  READ TABLE lt_fields
    WITH KEY name = '~query_string'
    ASSIGNING <fs_field>.
  IF sy-subrc EQ 0.
    CONCATENATE '{"message": "You have entered'
                <fs_field>-value
                'as query parameter."}'
           INTO lv_html SEPARATED BY space.

*" Output to HTML
    server->response->set_cdata(
      EXPORTING
        data   = lv_html    " Character data
*        offset = 0    " Offset into character data
*        length = -1    " Length of character data
    ).
  ENDIF.

endmethod.
```

### Create service in SICF

We need to expose the handler as a service, so go to **SICF** transaction, and add a new service in **default_host**.

{{< bootstrap-figure src="img/create-service-in-sicf.png" title="Create a new service in SICF" width="500">}}

In this service, a communication user is needed. It is recommended that the user is a service user instead of a dialog user.

If a user is not specified, authentication dialog will request when performing the REST Web Service request.

{{< bootstrap-figure src="img/set-communication-user-for-service.png" title="Set communication user for service" width="500">}}

And finally, add the custom class as handler.

{{< bootstrap-figure src="img/set-handler-for-service.png" title="Set handler for service" width="500">}}

### Test your service

A new service with **ZREST** name was created in **default_host**, so the URL to test is the following:

- http://hostname:port/zrest?queryParameter

Firstly, right click on the service and select Test service. Your default browser will be launched with your SAP Client.

{{< bootstrap-figure src="img/test-service-from-sicf.png" title="Test service from SICF" width="500">}}

Change the query parameter and type anything. The response will be updated.

{{< bootstrap-figure src="img/test-service-with-other-query-parameter.png" title="Test service with other query parameter" width="500">}}
