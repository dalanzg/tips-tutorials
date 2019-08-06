---
title: Meta Tags for Blogger to share your posts on Facebook and Twitter
description: When it comes to sharing a post on Facebook and Twitter, you would like to customize their contents such as the title, image, summary and so on. This article will explain how to configure your blogger template to share your Blogger posts on Facebook and Twitter.
date: 2015-08-30T18:00:00+02:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [blogger]
author: dalanzg
comments: true
---

When it comes to sharing a post on Facebook and Twitter, you would like to customize their contents such as the title, image, summary and so on. This article will explain how to configure your blogger template to share your Blogger posts on Facebook and Twitter.

To check a preview, please place your post URL in the following websites:

- [Facebook Debugger](https://developers.facebook.com/tools/debug/)
- [Twitter Card Validator](https://cards-dev.twitter.com/validator)

I would recommend to check the preview before and after this change.

First of all, I would check if the first image is the one I want to share on Facebook and Twitter. If not so, I would read this post: [How to choose the first image on Blogger]({{< ref "/posts/2015/2015-08-16-how-to-choose-the-first-image-on-blogger" >}})). This would help you to set the image you want without being the first image of your post.

Secondly, I would verify that meta description is added for your blogger site (Settings > Search preferences > Meta Tags > Description) and for individual post (Post Settings > Search Description).

Thirdly, I would choose the [Twitter Card Type](https://dev.twitter.com/cards/types). In this example, I chose **summary_large_image** since I would like to draw the attention with a large image. Nevertheless you can select other kind of Twitter Card, for instance, **summary** for thumbnail images.

Let's edit your Blogger Template. Place the following code right before `</head>` tag.

```html
<!-- Meta for Facebook
     =========================   -->
<meta expr:content='data:blog.title' property='og:site_name'/>
<b:if cond='data:blog.pageType == &quot;item&quot;'>
<meta expr:content='data:blog.pageName' property='og:title'/>
<meta expr:content='data:blog.postImageUrl' property='og:image'/>
<b:else/>
<meta expr:content='data:blog.pageTitle' property='og:title'/>
</b:if>
<meta expr:content='data:blog.metaDescription' property='og:description'/>
<meta content='article' property='og:type'/>
<meta content='https://www.facebook.com/{username_facebook}' name='article:author'/>
<meta content='en_US' property='og:locale'/>
<meta expr:content='data:blog.canonicalUrl' property='og:url'/>

<!-- Meta for Twitter
     =========================   -->
<meta expr:content='data:blog.title' name='twitter:domain'/>
<b:if cond='data:blog.pageType == &quot;item&quot;'>
<meta expr:content='data:blog.pageName' name='twitter:title'/>
<meta expr:content='data:blog.postImageUrl' name='twitter:image:src'/>
<b:else/>
<meta expr:content='data:blog.pageTitle' name='twitter:title'/>
</b:if>
<meta expr:content='data:blog.metaDescription' name='twitter:description'/>
<meta content='summary_large_image' name='twitter:card'/>
<meta content='@{username_twitter}' name='twitter:site'/>
<meta content='@{username_twitter}' name='twitter:creator'/>
<meta expr:content='data:blog.canonicalUrl' name='twitter:url'/>
```

Only replace **{username_facebook}** and **{username_twitter}** by your Facebook and Twitter username.

Save your HTML Template and check again your post URL to see changes.
