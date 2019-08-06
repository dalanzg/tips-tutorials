---
title: How to convert File into Array of Bytes, and Array of Bytes to File in Java
description: Array of bytes will be calculated from a file, and then, that array of bytes will be used to create a file with different filename.
date: 2016-03-22T12:00:00+01:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [java]
author: dalanzg
comments: true
---

Array of bytes will be calculated from a file, and then, that array of bytes will be used to create a file with different filename.

## Array of bytes Java class

```java
package com.dalanz.file;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;

public class ArrayBytesFile {

    // Attributes

    private File file;
    private byte[] bFile;

    // Constructors

    public ArrayBytesFile() {
        this.file = null;
        this.bFile = null;
    }

    public ArrayBytesFile (File file) {
        this.file = file;
        fileToBFile();
    }

    public ArrayBytesFile (String filePath, byte[] bFile) {
        this.file = new File(filePath);
        this.bFile = bFile;
    }

    // Public methods

    public File getFile() {
        return this.file;
    }

    public byte[] getBFile() {
        return this.bFile;
    }

    public void setFile (File file) {
        this.file = file;
    }

    public void setBfile (byte[] bFile) {
        this.bFile = bFile;
    }

    public void writeFile() {

        FileOutputStream outputStream = null;

        try {
            outputStream = new FileOutputStream(this.file);
            outputStream.write(this.bFile);
            outputStream.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // Private methods

    private void fileToBFile() {
        this.bFile = new byte[(int) this.file.length()];

        FileInputStream inputStream = null;

        try {
            inputStream = new FileInputStream(this.file);
            inputStream.read(bFile);
            inputStream.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

## Main class

```java
package test.com.dalanz;

import java.io.File;

import com.dalanz.file.ArrayBytesFile;

public class test {

    public static void main(String[] args) {

        String filePath = "/Users/dlanza/Desktop/Test.txt";
        File fileInput = new File (filePath);

        String filePathOut = "/Users/dlanza/Desktop/Test_out.txt";

        ArrayBytesFile file = new ArrayBytesFile(fileInput);
        byte[] bFile = file.getBFile();

        System.out.println("File: " + filePath);
        System.out.println("Array of bites: " + bFile.toString());
        System.out.println("--------------------------");

        ArrayBytesFile fileOutput = new ArrayBytesFile(filePathOut, bFile);
        System.out.println("File: " + filePathOut);
        fileOutput.writeFile();
        System.out.println("File created");
    }
}

```

## Output

```terminal
File: /Users/dlanza/Desktop/Test.txt
Array of bites: [B@6d06d69c
--------------------------
File: /Users/dlanza/Desktop/Test_out.txt
File created
```
