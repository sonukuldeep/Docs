---
title: Meta tags
author: Internet
datetime: 2023-05-02
slug: "metatags"
featured: false
draft: false
tags:
  - seo
  - meta-tags
ogImage: ""
description: Collection of meta tags for perfect seo score
---

# SEO optimization

## Sample

```html
<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <!-- seo stuff -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <meta name="description" content="Checkout my 10$ websites!" />
  <meta name="keywords" content="cheep webiste, websites, links, portfolio" />
  <meta name="author" content="my name" />
  <!-- tags that show when sharing link -->
  <meta property="og:title" content="10$ Website collection" />
  <meta
    property="og:description"
    content="Check out my 10$ websites! Order now"
  />
  <meta property="og:image" content="/og_image.png" />
  <meta property="og:url" content="mywebsite.com" />
  <meta property="og:type" content="website" />
  <meta property="og:site_name" content="10$_websites" />
  <meta property="og:locale" content="en_US" />
  <!-- tags for ios devices -->
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-title" content="My 10$ Website collection" />
  <meta name="apple-touch-icon" content="/apple-touch-icon.png" />
  <!-- twitter tags -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:site" content="@myhandle" />
  <meta name="twitter:title" content="10$ Website collection" />
  <meta
    name="twitter:description"
    content="Check out my 10$ websites! Order now"
  />
  <meta name="twitter:image" content="/og_image.png" />
  <!-- website crowllars -->
  <meta name="robots" content="index, follow" />
  <link rel="robots" href="https://www.example.com/robots.txt" />
  <!-- general stuff -->
  <link rel="shortcut icon" type="image/png" href="/favicon.png" />
  <link rel="stylesheet" href="home/css/style.css" />
  <script src="home/js/script.js" defer></script>
  <title>Website gallery</title>
</head>
```

## robot.txt sample

```txt
User-agent: *
Disallow: /login/
Disallow: /admin/
Disallow: /error/
Disallow: /duplicate-page/
Sitemap: https://www.example.com/sitemap.xml
```

Note that the Disallow directive specifies the pages or directories that you want to exclude from search engine indexing. The Sitemap directive specifies the location of your sitemap file.

## Generate sitemap

Generating a sitemap.xml file is important for search engine optimization and can be done using various tools or scripts. Here is a simple way to generate a sitemap.xml file using a Node.js script:

Install the sitemap package using NPM:

```js
npm install sitemap
```

Create a new file called sitemap.js and add the following code:

javascript

```js
const Sitemap = require("sitemap");
const fs = require("fs");
const path = require("path");

// define URLs of your website
const urls = [
  { url: "/page1", changefreq: "weekly", priority: 0.7 },
  { url: "/page2", changefreq: "monthly", priority: 0.5 },
  { url: "/page3", changefreq: "monthly", priority: 0.5 },
  { url: "/page4", changefreq: "monthly", priority: 0.5 },
  { url: "/page5", changefreq: "monthly", priority: 0.5 },
  { url: "/page6", changefreq: "monthly", priority: 0.5 },
  { url: "/page7", changefreq: "monthly", priority: 0.5 },
  { url: "/page8", changefreq: "monthly", priority: 0.5 },
  { url: "/page9", changefreq: "monthly", priority: 0.5 },
  { url: "/page10", changefreq: "monthly", priority: 0.5 },
  { url: "/", changefreq: "daily", priority: 1.0 },
  { url: "/about", changefreq: "monthly", priority: 0.5 },
  { url: "/contact", changefreq: "monthly", priority: 0.5 },
];

// create sitemap
const sitemap = Sitemap.createSitemap({
  hostname: "https://www.yourwebsite.com",
  urls: urls,
});

// write sitemap file
fs.writeFileSync(path.join(__dirname, "sitemap.xml"), sitemap.toString());
```

This code defines an array of URLs that should be included in the sitemap.xml file, sets the hostname of the website, creates the sitemap using the Sitemap module, and writes the sitemap to a file called sitemap.xml in the same directory as the script.

Run the script using Node.js:

```js
node sitemap.js
```

This will generate the sitemap.xml file in the same directory as the script, with the URLs and settings specified in the urls array. You can then upload this file to the root directory of your website to help search engines crawl your site.
