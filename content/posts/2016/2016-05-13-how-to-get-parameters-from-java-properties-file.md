---
title: How to get parameters from Java Properties File
description: Parameters are read from Java properties file and printed on console.
date: 2016-05-13T21:00:00+02:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [java]
author: dalanzg
comments: true
---

Parameters are read from Java properties file and printed on console.

## Java Properties File

The content of Java Properties File is the following:

```properties
name = Daniel
lastName = Lanza
age = 29
```

And the file is located in the desktop:

- **/Users/dalanz/Desktop/test.properties**

## User Java class

```java
package com.dalanz.file;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class ReadProperties {

    private String fileSource;

    public ReadProperties (String fileSource) {
        this.fileSource = fileSource;
    }

    public String getProperty(String key) {

        Properties prop = new Properties();
        InputStream input = null;

        try {
            input = new FileInputStream(this.fileSource);

            // Load a properties file
            prop.load(input);

            // Get the property value
            return prop.getProperty(key);
        } catch (IOException ex) {
            ex.printStackTrace();
            return null;
        } finally {
            if (input != null) {
                try {
                    input.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## Main Class

```java
package test.com.dalanz;

import com.dalanz.file.*;

public class test {

    public static void main(String[] args) {

        String sourceFile = "/Users/dalanz/Desktop/test.properties";

        ReadProperties prop = new ReadProperties(sourceFile);

        System.out.println("Name: " + prop.getProperty("name"));
        System.out.println("Last name: " + prop.getProperty("lastName"));
        System.out.println("Age: " + prop.getProperty("age"));

    }
}
```

## Output

```terminal
Name: Daniel
Last Name: Lanza
Age: 29
```
