---
author: Kuldeep
datetime: 2022-12-18
title: PostCss-A marvel
slug: postcss
featured: true
draft: false
tags:
  - postcss
  - autoprefixer
  - open-props
  - postcss-jit-props
  - postcss-nested
ogImage: "/public/assets/postcss.svg"
description: A fabulous css implimentation.
---

![PostCss](/assets/postcss.svg)

# PostCSS: A tool for transforming CSS with JavaScript

## But why?

- Add vendor prefixes to CSS rules using values from Can I Use. Autoprefixer will use the data based on current browser popularity and property support to apply prefixes for you.
- PostCSS Preset Env, lets you convert modern CSS into something most browsers can understand, determining the polyfills you need based on your targeted browsers or runtime environments, using cssdb.
- CSS Modules means you never need to worry about your names being too generic, just use whatever makes the most sense.
- Enforce consistent conventions and avoid errors in your stylesheets with stylelint, a modern CSS linter. It supports the latest CSS syntax, as well as CSS-like syntaxes, such as SCSS.

### To install on Astro

```shell
npm i -D postcss autoprefixer open-props postcss-jit-props postcss-nested
```

include any other plugin you want to add.

Then add _postcss.config.js_ file in folder root with these settings

```shell
const postcssjitprops = require("postcss-jit-props");
const OpenProps = require("open-props");


module.exports = {
  plugins: [
    postcssjitprops(OpenProps),
    require('postcss-nested'),
    require('autoprefixer'),
  ]
};
```

Add these line to _package.json_ for autoprefixer

```shell
"browserslist": [
     "defaults"
 ]
```

---

### To install on Nextjs

As of now _postcss-jit-props_ doesn't work on Nextjs

```shell
npm i -D postcss open-props
```

Install any other plugin you want to use through npm

import indivisual package in global css file as required

```shell
@import "open-props/sizes";
@import "open-props/colors";
@import "open-props/fonts";
@import "open-props/gradients";
```

Check [Open-props website for more](https://open-props.style/)

---

### To install on Vite

```shell
npm i -D open-props postcss-jit-props
```

Include any other plugin you want to add.

Then add _postcss.config.js_ file in folder root with these settings

```shell
const postcssJitProps = require('postcss-jit-props');
const OpenProps = require('open-props');

module.exports = {
  plugins: [postcssJitProps(OpenProps)],
};
```
