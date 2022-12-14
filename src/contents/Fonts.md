---
author: Kuldeep
datetime: 2022-12-14
title: Fonts
slug: font
featured: true
draft: false
tags:
  - font
  - poppins
ogImage: ""
description: Poppins font initialization.
---

# Add fonts to your website

## How to add Own fonts

The CSS @font-face Rule

Web fonts allow Web designers to use fonts that are not installed on the user's computer.

When you have found/bought the font you wish to use, just include the font file on your web server, and it will be automatically downloaded to the user when needed.

Your "own" fonts are defined within the CSS @font-face rule.

```shell
@font-face {
  font-family: myFirstFont;
  src: url(sansation_light.woff);
  /*font-weight: bold;*/
}

div {
  font-family: myFirstFont;
}
```

## Poppins Font

Add Poppins font

> Add to index.html

```shell
<link href='https://fonts.googleapis.com/css?family=Poppins' rel='stylesheet'>
```

> Or link directly into css

```shell
@import url('https://fonts.googleapis.com/css?family=Poppins');
```

> Include in css

```shell
font-family: 'Poppins', sans-serif;
```
