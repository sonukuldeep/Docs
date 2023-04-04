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

## Import from another file

Another js file

```js
module.exports = { odd: sumOfOdd, even: sumOfEven };

function sumOfOdd() {
  return 5 + 7;
}

function sumOfEven() {
  return 2 + 3;
}
```

//server.js

```js
const express = require("express");

const app = express();
const PORT = 5000;
const sum = require(__dirname + "/js/sum.js");

app.use(express.urlencoded({ extended: false }));
app.use(express.static("public"));

app.set("view engine", "ejs");

app.get("/", (req, res) => {
  const result = sum.odd() + sum.even();
  res.render("index", { title: "Newsletter Signup", sum: result });
});

app.post("/", function (req, res) {
  const { firstname, lastname, email } = req.body;
  res.status(200).sendFile(__dirname + "/success.html");
});

app.listen(PORT, function () {
  console.log("running on port " + PORT);
});
```

## Res.write and res.send

```js
const express = require("express");
const https = require("node:https");

const app = express();
const PORT = 5000;

const url = "https://official-joke-api.appspot.com/random_joke";

app.use(express.json());

app.get("/", function (req, res) {
  https.get(url, function (response) {
    response.on("data", function (data) {
      const decrypred = JSON.parse(data);
      const { setup, punchline, type } = decrypred;
      res.write("<p>The type is " + type + "</p>");
      res.write("<h1>" + setup + " " + punchline + "</h1>");
      res.status(200).send();
    });
  });
});

app.listen(PORT, function () {
  console.log("Runing on port " + PORT);
});
```
