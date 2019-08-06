---
title: How to get parameters from XML Properties File
description: Parameters are read from XML properties file and printed on console.
date: 2016-05-14T18:00:00+02:00
lastmod: 2016-12-27 10:51:00+01:00
tags: [java]
author: dalanzg
comments: true
---

Parameters are read from XML properties file and printed on console.

## XML Properties File

The content of XML Properties File is the following:

```xml
<?xml version="1.0"?>
<properties>
    <name>Daniel</name>
    <lastName>Lanza</lastName>
    <age>29</age>
</properties>
```

And the file is located in the desktop:

- **/Users/dalanz/Desktop/person.xml**

## User Java class

```java
package com.dalanz.file;

import java.io.File;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.w3c.dom.Document;
import org.w3c.dom.NodeList;

public class ReadPropertiesXML {

    private File fileSource;

    public ReadPropertiesXML (File fileSource) {
        this.fileSource = fileSource;
    }

    public String getProperty(String key) {

        try {
            DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();
            DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();
            Document doc = dBuilder.parse(this.fileSource);

            //Optional, but recommended
            //read this - http://stackoverflow.com/questions/13786607/normalization-in-dom-parsing-with-java-how-does-it-work
            doc.getDocumentElement().normalize();

            NodeList list = doc.getElementsByTagName(key);

            if (list.getLength() == 1)
                return list.item(0).getTextContent();
            else
                return null;

        } catch (Exception ex) {
            ex.printStackTrace();
            return null;
        }
    }
}
```

## Main Class

```java
package test.com.dalanz;

import java.io.File;

import com.dalanz.file.*;

public class test {

    public static void main(String[] args) {

        File sourceFile = new File("/Users/dalanz/Desktop/person.xml");

        ReadPropertiesXML xml = new ReadPropertiesXML(sourceFile);

        System.out.println("Name: " + xml.getProperty("name"));
        System.out.println("Last Name: " + xml.getProperty("lastName"));
        System.out.println("Age: " + xml.getProperty("age"));
    }
}
```

## Output

```terminal
Name: Daniel
Last Name: Lanza
Age: 29
```
