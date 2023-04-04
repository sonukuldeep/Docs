---
title: Express js
author: Internet
datetime: 2023-04-04
slug: "express"
featured: false
draft: false
tags:
  - express
  - node
ogImage: ""
description: Express js documemtation
---

# Express Js

![cover image](https://res.cloudinary.com/practicaldev/image/fetch/s--YbV36HLj--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/hpg6if7btrwilqkidqbe.png)

## Table of Contents

## Boilerplate

```js
//imports
const express = require("express");

const app = express();

//Middleware
app.use(express.urlencoded({ extended: false })); //body parser is depricated now
// http://expressjs.com/en/api.html#express.urlencoded
app.use(express.json());

// Routes
app.get("/", function (req, res) {
  res.status(200).sendFile(__dirname + "/index.html");
});

app.post("/", function (req, res) {
  const { num1, num2 } = req.body;
  res.status(200).send("The sum of these numbers is " + num1 + num2);
});

app.listen(5000, function () {
  console.log("Running on port 5000");
});
```
