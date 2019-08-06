---
title: How to use Sass with Jekyll (Bootstrap and Font Awesome example)
description: This post will explain how to use Bootstrap and Font Awesome Sass files in a Jekyll project.
date: 2016-03-25T10:00:00+01:00
lastmod: 2016-12-27T10:51:00+01:00
tags: [jekyll]
author: dalanzg
comments: true
---

This post will explain how to use Bootstrap and Font Awesome Sass files in a Jekyll project.

## Download Sass packages

Download the Bootstrap and Font Awosome packages from their original website:

- [Bootstrap](http://getbootstrap.com/)
- [Font Awesome](https://fortawesome.github.io/Font-Awesome/)

## Jekyll project

The packages contain stylesheets, fonts and javascript files. Therefore, these files have to be placed into your Jekyll project.

The files will be placed in a Jekyll project like this one:

```terminal
jekyll-project/
├── _includes/
├── _layouts/
├── _posts/
├── _sass/
│   └── bootstrap/
│   └── font-awosome/
├── css/
│   └── main.scss
├── fonts
│   └── bootstrap/
│   └── font-awosome/
├── js
│   └── jquery/
│   └── bootstrap/
├── _config.yml
└── index.html
```

- **css/main.scss** is the main stylesheet to use in your jekyll project
- **_sass** directory contains all the scss files
- **fonts** and **js** folders are directories to use with Bootstrap and Font Awesome

## Sass files

Place Bootstrap and Font Awosome Sass files into **_sass** folder:

- **bootstrap-sass-3.3.6/assets/stylesheets/*** into **_sass/boostrap/**
- **font-awesome-4.5.0/scss/*** into **_sass/font-awesome/**

## Fonts

Bootstrap and Font Awosome Sass files request fonts files. Place them into fonts folder:

- **bootstrap-sass-3.3.6/assets/fonts/bootstrap/*** into **fonts/boostrap/**
- **font-awesome-4.5.0/fonts/*** into **fonts/font-awesome/**

## Javascripts

Place Javascript files in **js** folder:

- **bootstrap-sass-3.3.6/assets/javascripts/bootstrap.min.js** into **js/boostrap/**
- **jquery.min.js** into **js/jquery/**

## Set your main stylesheet and javascripts

The main stylesheet **css/main.scss** will have the commands to import all the scss files. The result will be a **css/main.css** file with all the stylesheets imported.

Include the main stylesheet in your jekyll **head**:

```html
<link rel="stylesheet" href="/css/main.css">
```

Bootstrap theme requires Jquery and Bootstrap javascripts. Include them right before your jekyll **/body**

```html
<!-- jQuery -->
<script src="{{ "/js/jquery/jquery.min.js" | prepend: site.baseurl }}"></script>

<!-- Bootstrap Core JavaScript -->
<script src="{{ "/js/bootstrap/bootstrap.min.js" | prepend: site.baseurl }}"></script>
```

## Sass imports

The command **@import** in the **css/main.scss** imports the scss file in the main stylesheets.

Bootstrap and Font Awosome use font folders, so we need to set the font directories before the **@import** command.

Therefore, the **css/main.scss** file can be something like this:

```scss
---
# Only the main Sass file needs front matter (the dashes are enough)
---
@charset "utf-8";

// Variables to modify font folders
$icon-font-path:      "../fonts/bootstrap";
$fa-font-path:        "../fonts/font-awesome";

// Import partials from `sass_dir` (defaults to `_sass`)
@import "bootstrap-sass/bootstrap";
@import "font-awesome/font-awesome";
```

Be in mind that a relative path is used in **@import** command. The files requested in the Sass command are the following:

- **_sass/bootstrap-sass/_bootstrap.scss**
- **_sass/font-awesome/_font-awesome.scss**

## Minification

Because of the fact you are importing Sass files, you can take advantage of it. You can minify your compiled css file by updating your **_config.yml**

```yaml
sass:
  style: compressed
```
