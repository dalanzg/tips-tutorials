---
title: How to get MD5 checksum for a file in Java
description: MD5 checksum is a 128-bit hash value (32 digit hexadecimal number). This works as a fingerprint for a file which let us compare together other files to find out duplicate files. This post calculate MD5 checksum by Java.
date: 2016-03-22T11:00:00+01:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [java]
author: dalanzg
comments: true
---

MD5 checksum is a 128-bit hash value (32 digit hexadecimal number). This works as a fingerprint for a file which let us compare together other files to find out duplicate files.

These Java classes will let us calculate MD5 checksum for any single file.

## Maven dependencies

```xml
<dependencies>
    <dependency>
        <groupId>commons-codec</groupId>
        <artifactId>commons-codec</artifactId>
        <version>1.10</version>
    </dependency>
    <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.4</version>
    </dependency>
</dependencies>
```

## MD5 Java class

```java
package com.dalanz.file;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

import org.apache.commons.codec.digest.DigestUtils;
import org.apache.commons.io.IOUtils;

public class GetMD5ForFile {

    private File file;

    public GetMD5ForFile(String filePath) {
        this.file = new File(filePath);
    }

    public GetMD5ForFile(File file) {
        this.file = file;
    }

    public String getMD5() {
        String md5 = null;

        FileInputStream fileInputStream = null;

        try {
            fileInputStream = new FileInputStream(this.file);

            // md5Hex converts an array of bytes into an array of characters representing the hexadecimal values of each byte in order.
            // The returned array will be double the length of the passed array, as it takes two characters to represent any given byte.

            md5 = DigestUtils.md5Hex(IOUtils.toByteArray(fileInputStream));

            fileInputStream.close();

        } catch (IOException e) {
            e.printStackTrace();
        }

        return md5;
    }
}
```

## Main class

```java
package test.com.dalanz;

import com.dalanz.file.GetMD5ForFile;

public class test {

    public static void main(String[] args) {

        String filePath = "/Users/dlanza/Desktop/Test.txt";

        GetMD5ForFile file = new GetMD5ForFile(filePath);

        System.out.println("MD5: " + file.getMD5());
    }
}
```

## Output

```terminal
MD5: 6cbca9b36efc68e359df0d66338e0141
```
