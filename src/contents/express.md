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
var path = require("path");
const cors = require("cors");

//Middleware
app.use(express.urlencoded({ extended: false }));
app.use(express.json());

// cors all all
app.use(cors());

// Set the view engine to EJS
app.set("views", path.join(__dirname, "views")); // have a folder named views in root
app.set("view engine", "ejs");

// Serve static files from a public folder
app.use(express.static(path.join(__dirname, "public"))); // have a folder named public in root

// Routes
app.get("/", function (req, res) {
  res.status(200).render("index");
});

app.post("/", function (req, res) {
  const { num1, num2 } = req.body;
  res.status(200).send("The sum of these numbers is " + num1 + num2);
});

// Set the port based on environment
const PORT = process.env.NODE_ENV === "production" ? 80 : 3000;

// Start the server
app.listen(PORT, () => {
  console.log(
    `Server is running on port ${PORT} in ${process.env.NODE_ENV} mode`
  );
});
```

## Seperate file for routes

```js
// userRoutes.js

const express = require("express");
const router = express.Router();

// Define user routes
router.get("/", (req, res) => {
  // Handle user route logic
});

router.post("/", (req, res) => {
  // Handle user route logic
});

router.put("/:id", (req, res) => {
  // Handle user route logic
});

router.delete("/:id", (req, res) => {
  // Handle user route logic
});

// Export the router
module.exports = router;
```

In the main Express app file import userRouts

```js
// app.js

const express = require("express");
const userRoutes = require("./userRoutes");

const app = express();

// Register userRoutes middleware
app.use("/users", userRoutes);

// Define other routes and middleware

// Start the server
app.listen(3000, () => {
  console.log("Server is running on port 3000");
});
```

<hr>

## Import from another file

Another lib file

```js
module.exports = { odd: sumOfOdd, even: sumOfEven };

function sumOfOdd() {
  return 5 + 7;
}

function sumOfEven() {
  return 2 + 3;
}
```

In app.js file

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

<hr>

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
