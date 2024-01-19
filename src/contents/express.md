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

<hr>

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

## Separate js file

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

<hr>

## Set up to deploy in vercel

Vercel requires the folling files and folder to work<br>
Note these settings are for mern app

- app/routes.js
- index.js
- vercel.json
- public
- views

app/routes file contains the route which is then imported into index.js file.<br>
app/routes.js

```js
// imports
const express = require("express");
const router = express.Router();

// Define user routes
router.get("/test", function (req, res) {
  res.status(200).send("works");
});

router.get("/", function (req, res) {
  res.status(200).render("home");
});

router.get("/about", function (req, res) {
  res.status(200).render("about");
});

router.get("/contact", function (req, res) {
  res.status(200).render("contact");
});

router.post("/", function (req, res) {
  const { num1, num2 } = req.body;
  res.status(200).send("The sum of these numbers is " + num1 + num2);
});

// Export the router
module.exports = router;
```

index.js

```js
//Imports
require("dotenv").config();
const express = require("express");
const app = express();
const routes = require("./app/routes");
var path = require("path");

//Middleware
app.use(express.urlencoded({ extended: false }));
app.use(express.json());

// Set the view engine to EJS
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "ejs");

// Serve static files from a public folder
app.use(express.static(path.join(__dirname, "public")));

// Routes
app.use("/", routes);

// if port exist then run on localhost else deploy on vercel
if (process.env.PORT) {
  // Start the server
  app.listen(process.env.PORT, () => {
    console.log(`Server is running on port ${process.env.PORT}`);
  });
} else app.listen();
```

vercel.json

```json
{
  "version": 2,
  "builds": [
    {
      "src": "./index.js",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/"
    }
  ]
}
```

## Middleware

```js
const express = require("express");
const app = express();
const jwt = require("jsonwebtoken");

// middleware that runs on all request
app.use(loggingMiddleware);

app.get("/", (req, res) => {
  res.send("Home Page");
});

app.get("/users", authorizeUsersAccess, (req, res) => {
  res.send("Users Page");
});

function loggingMiddleware(req, res, next) {
  console.log(`${new Date().toISOString()}: ${req.originalUrl}`);
  next(); // always have return statement after this
  return;
}

// this middleware runs on /users route
function authorizeUsersAccess(req, res, next) {
  const authHeader = req.headers["authorization"];
  const token = authHeader && authHeader.split(" ")[1];

  if (!token) {
    return res.status(401).json({ message: "Unauthorized" });
  }

  jwt.verify(token, process.env.ACCESS_TOKEN_SECRET, (err, decoded) => {
    if (err) {
      return res.status(403).json({ message: "Forbidden" });
    }

    req.user = decoded;
    next(); // always have return statement after this
    return;
  });
}

app.listen(3000, () => console.log("Server Started"));
```

## Setup for react js

- React is served from express in this setup
- Express serves client/build or client/dist folder

### Server setup

```js
//imports
const express = require("express");
const app = express();
const path = require("path");

// Set the port based on environment
const PORT = 3000;

//Middleware
app.use(express.urlencoded({ extended: false }));
app.use(express.json());
app.use(logger);

// Serve static files from a public folder
app.use(express.static(path.join(__dirname, "../client/dist"))); // have a folder named public in root
app.use(express.static(path.join(__dirname, "public"))); // have a folder named public in root

// Routes
app.get("/api", (req, res) => {
  res.status(200).json({ message: "Hello from server!" });
});

app.get("/static", (req, res) => {
  res.sendFile(path.join(__dirname, "public", "index.html"));
});

// react app
app.get("*", (req, res) => {
  res.sendFile(path.resolve(__dirname, "../client/dist", "index.html"));
});

// logger
function logger(req, res, next) {
  console.log(`${new Date().toISOString()}: ${req.originalUrl}`);
  next(); // always have return statement after this
  return;
}

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT} in test mode`);
});
```

### React setup

- When using create-react-app<br/>
  Add `proxy": "http://localhost:3000` to _package.json_ for local development.
- When using vite<br/>
  Add this to `vite.config.js`

```js
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    proxy: {
      "/api": {
        target: "http://localhost:3000",
        changeOrigin: true,
      },
    },
  },
});
```
