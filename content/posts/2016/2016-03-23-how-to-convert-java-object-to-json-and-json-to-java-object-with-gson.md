---
title: How to convert Java object to JSON and JSON to Java object with Gson
description: A simple Java object will become to JSON by using Gson. Besides, this will become into Java object.
date: 2016-03-23T16:00:00+01:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [java]
author: dalanzg
comments: true
---

A simple Java object will become to JSON by using Gson. Besides, this will become into Java object.

## Maven dependencies

```xml
<dependencies>
    <dependency>
        <groupId>com.google.code.gson/groupId>
        <artifactId>gson</artifactId>
        <version>2.6.2</version>
    </dependency>
</dependencies>
```

## User Java class

```java
package com.dalanz.json;

import java.util.ArrayList;
import java.util.List;

public class User {

    private String firstName;
    private String lastName;
    private int age;
    private List<String> messages;

    public User(){}

    public User(String firstName, String lastName, int age, List<String> messages) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
        this.messages = messages;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public List<String> getMessages() {
        return messages;
    }

    public void setMessages(List<String> messages) {
        this.messages = messages;
    }

    public void userDefault(){
        this.firstName = "Daniel";
        this.lastName = "Lanza";
        this.age = 30;
        this.messages = new ArrayList<String>();
        this.messages.add("Message 1");
        this.messages.add("Message 2");
        this.messages.add("Message 3");
    }
}
```

## Gson Example class

```java
package com.dalanz.json;

import com.google.gson.Gson;

public class GsonExample {

    private String json;
    private User user;

    public String getJson() {
        return json;
    }
    public void setJson(String json) {
        this.json = json;
    }
    public User getUser() {
        return user;
    }
    public void setUser(User user) {
        this.user = user;
    }

    public GsonExample(){
        this.json="";
        this.user = null;
    }

    public String userToJson(){
        Gson gson = new Gson();
        return gson.toJson(this.user);
    }

    public User jsonToUser(){
        Gson gson = new Gson();
        return gson.fromJson(this.json, User.class);
    }
}
```

## Main Class

```java
package test.com.dalanz;

import com.dalanz.json.*;

public class test {

    public static void main(String[] args) {

        User user1 = new User();
        user1.userDefault();

        GsonExample gson1 = new GsonExample();
        GsonExample gson2 = new GsonExample();
        GsonExample gson3 = new GsonExample();

        // Gson1 -> JSON from Java Object
        gson1.setUser(user1);
        String jsonUser1 = gson1.userToJson();
        System.out.println("User 1 in JSON: " + jsonUser1);

        // Gson 2 -> Java Object from JSON (gson1)
        gson2.setJson(jsonUser1);
        User user2 = gson2.jsonToUser();
        System.out.println("User 2 calculated from JSON User 1");

        // Gson3 -> JSON from Java Object (gson2)
        gson3.setUser(user2);
        String jsonUser3 = gson1.userToJson();
        System.out.println("User 2 in JSON: " + jsonUser3);

        if (jsonUser1.equals(jsonUser3))
            System.out.println("OK");
        else
            System.out.println("Fail");
    }
}
```

## Output

```terminal
User 1 in JSON: {"firstName":"Daniel","lastName":"Lanza","age":30,"messages":["Message 1","Message 2","Message 3"]}
User 2 calculated from JSON User 1
User 2 in JSON: {"firstName":"Daniel","lastName":"Lanza","age":30,"messages":["Message 1","Message 2","Message 3"]}
OK
```
