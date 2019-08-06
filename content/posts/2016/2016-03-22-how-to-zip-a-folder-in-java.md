---
title: How to zip a folder and unzip a Zip file in Java
description: A folder with subfolders and files will be zipped. Besides, the zip file will be unzipped. All of this by Java.
date: 2016-03-22T16:00:20+01:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [java]
author: dalanzg
comments: true
---

A folder with subfolders and files will be zipped. Besides, the zip file will be unzipped.

## Zip directory Java class

```java
package com.dalanz.file;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.ArrayList;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;
import java.util.zip.ZipOutputStream;

public class ZipDirectory {

    private String folderPath;
    private String zipPath;

    private static ArrayList<String> fileList = null;
    private static String sourceFolder = null;

    public String getFolderPath() {
        return folderPath;
    }

    public void setFolderPath(String folderPath) {
        this.folderPath = folderPath;
    }

    public String getZipPath() {
        return zipPath;
    }

    public void setZipPath(String zipPath) {
        this.zipPath = zipPath;
    }

    public ZipDirectory(){}

    public ZipDirectory(String folderPath, String zipPath){
        this.folderPath = folderPath;
        this.zipPath = zipPath;
    }

    public ZipDirectory(String zipPath){
        this.zipPath = zipPath;
    }

    public boolean zip() throws IOException{

        byte[] buffer = new byte[1024];

        FileInputStream fis = null;
        ZipOutputStream zos = null;

        fileList = new ArrayList<String>();

        try{
            zos = new ZipOutputStream(new FileOutputStream(this.zipPath));
            updateSourceFolder(new File(this.folderPath));

            if (sourceFolder == null) {
                zos.close();
                return false;
            }

            generateFileAndFolderList(new File(this.folderPath));

            for (String unzippedFile: fileList) {

                System.out.println(sourceFolder + unzippedFile);

                ZipEntry entry = new ZipEntry(unzippedFile);
                zos.putNextEntry(entry);

                if ((unzippedFile.substring(unzippedFile.length()-1)).equals(File.separator))
                    continue;

                try{
                    fis = new FileInputStream(sourceFolder + unzippedFile);

                    int len=0;
                    while ((len = fis.read(buffer))>0) {
                        zos.write(buffer,0,len);
                    }
                }catch(IOException e){

                    e.printStackTrace();
                    return false;

                }finally{

                    if (fis!=null)
                        fis.close();
                }
            }

            zos.closeEntry();

        }catch(IOException e){

            e.printStackTrace();
            return false;

        }finally{

            zos.close();
            fileList = null;
            sourceFolder = null;
        }

        return true;
    }

    public boolean unzip() throws IOException{

        byte[] buffer = new byte[1024];

        ZipInputStream zis = null;
        ZipEntry entry = null;
        FileOutputStream fos = null;

        fileList = new ArrayList<String>();

        try {

            File file = new File(this.zipPath);
            zis = new ZipInputStream(new FileInputStream(file));

            while((entry = zis.getNextEntry())!= null){

                String dir = entry.getName();

                try{

                    String fileSeparator = dir.substring(dir.length()-1);

                    boolean isFolder=(fileSeparator.equals("/") || fileSeparator.equals("\\"));

                    if (isFolder){

                        dir = dir.substring(0,dir.lastIndexOf(fileSeparator)+1);
                        dir = (file.getParent() == null? "":file.getParent()+ File.separator) + dir;

                        (new File(dir)).mkdirs();

                        continue;

                    }

                }catch(Exception e){}finally{}

                System.out.println("dir:" + dir);

                fos = new FileOutputStream((file.getParent() == null? "":file.getParent()+ File.separator) + entry.getName());

                int len=0;
                while ((len = zis.read(buffer))>0) {
                    fos.write(buffer,0,len);
                }
                fos.close();
            }

            zis.close();

            return true;

        }catch (IOException ioe){

            ioe.printStackTrace();
            return false;

        }finally{

            fileList = null;
            sourceFolder = null;
        }
    }

    //Fills filesList with file name and their paths
    private void generateFileAndFolderList(File node){

        // Add file only
        if (node.isFile()){

            fileList.add(generateZipEntry(node.getAbsoluteFile().toString()));
        }

        if (node.isDirectory()){

            String dir = node.getAbsoluteFile().toString();
            fileList.add(dir.substring(sourceFolder.length(), dir.length()) + File.separator);

            String[] subNode = node.list();
            for (String fileOrFolderName : subNode){

                generateFileAndFolderList(new File(node, fileOrFolderName));
            }
        }
    }

    //Generate file name based on source folder
    private String generateZipEntry(String file){

        return file.substring(sourceFolder.length(), file.length());
    }

    //Updates source folder based on source type: File or Folder
    private void updateSourceFolder(File node){

        if (node.isFile() || node.isDirectory()){

            String sf = node.getAbsoluteFile().toString();

            sourceFolder = sf.substring(0,(sf.lastIndexOf("/")>0?sf.lastIndexOf("/"):sf.lastIndexOf("\\")));
            sourceFolder += File.separator;

        } else
            sourceFolder=null;//the file does not exists
    }
}

```

## Main class to zip a folder

```java
package test.com.dalanz;

import java.io.IOException;

import com.dalanz.file.*;

public class test {

    public static void main(String[] args) {

        String fileDirectory = "/Users/dlanza/Desktop/Test";
        String zipDirectory = "/Users/dlanza/Desktop/Test.zip";

        ZipDirectory zip = new ZipDirectory (fileDirectory, zipDirectory);

        try {
            zip.zip();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

## Output

```terminal
/Users/dlanza/Desktop/Test/
/Users/dlanza/Desktop/Test/.DS_Store
/Users/dlanza/Desktop/Test/Folder 1/
/Users/dlanza/Desktop/Test/Folder 1/.DS_Store
/Users/dlanza/Desktop/Test/Folder 1/Folder 1.1/
/Users/dlanza/Desktop/Test/Folder 1/Folder 1.1/.DS_Store
/Users/dlanza/Desktop/Test/Folder 1/Folder 1.1/Test.rtf
/Users/dlanza/Desktop/Test/Folder 2/
/Users/dlanza/Desktop/Test/Folder 2/.DS_Store
/Users/dlanza/Desktop/Test/Folder 2/Folder 2.1/
/Users/dlanza/Desktop/Test/Folder 2/Folder 2.1/.DS_Store
/Users/dlanza/Desktop/Test/Folder 2/Folder 2.1/Test.rtf
/Users/dlanza/Desktop/Test/Folder 2/Folder 2.2/
/Users/dlanza/Desktop/Test/Folder 2/Folder 2.2/.DS_Store
/Users/dlanza/Desktop/Test/Folder 2/Folder 2.2/Test.rtf
/Users/dlanza/Desktop/Test/Test.rtf
```

## Main class to unzip a zip file

```java
package test.com.dalanz;

import java.io.IOException;

import com.dalanz.file.*;

public class test {

    public static void main(String[] args) {

        String zipDirectory = "/Users/dlanza/Desktop/Test.zip";

        ZipDirectory zip = new ZipDirectory (zipDirectory);

        try {
            zip.unzip();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

## Output

```terminal
dir:Test/.DS_Store
dir:Test/Folder 1/.DS_Store
dir:Test/Folder 1/Folder 1.1/.DS_Store
dir:Test/Folder 1/Folder 1.1/Test.rtf
dir:Test/Folder 2/.DS_Store
dir:Test/Folder 2/Folder 2.1/.DS_Store
dir:Test/Folder 2/Folder 2.1/Test.rtf
dir:Test/Folder 2/Folder 2.2/.DS_Store
dir:Test/Folder 2/Folder 2.2/Test.rtf
dir:Test/Test.rtf
```
