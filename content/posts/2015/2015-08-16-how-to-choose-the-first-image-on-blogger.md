---
title: How to choose the first image on Blogger
description: This article will explain how to customize your Index Page regarding images from posts. It is very common to use the following data tag data:post.firstImageUrl to retrieve an image url for the post, but the drawback is that it is the first image.
date: 2015-08-16T18:00:00+02:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [blogger]
author: dalanzg
comments: true
---

This article will explain how to customize your Index Page regarding images from posts. It is very common to use the following data tag **data:post.firstImageUrl** to retrieve an image url for the post, but the drawback is that it is the first image.

The HTML code for Index Page to get an image url of the post is the following:

```html
<img expr:alt='data:post.title' expr:src='data:post.firstImageUrl' expr:title='data:post.title' />
```

It is possible to select the image that you want for your index, and it is to trick the post entry.

At the beginning of the post entry, copy and paste the following HTML code:

```html
<img src="{image_url}" style="display:none">
```

where **{image_url}** is the image url that you want to show in Index. This way, Blogger understands that the first image is **{image_url}**, and because of the style, it will not be shown up in the post.
