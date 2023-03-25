---
title: Authentication
author: Internet
datetime: 2023-03-25
slug: "authentication"
featured: false
draft: false
tags:
  - authentication
  - passport
  - express
  - node
ogImage: ""
description: JavaScript React router v6
---

# Authentication

<img src="https://img.freepik.com/free-vector/data-protection-law-illustration-concept_114360-971.jpg?w=996&t=st=1679737546~exp=1679738146~hmac=c6bf42aef6f206bd15988f750aaaec6766fc6d55951b2f6565656504b9f95073" alt="cover image">

## Table of Contents

## Passport js

[Documentation](https://www.passportjs.org/)

Passport is authentication middleware for Node.js. Extremely flexible and modular, Passport can be unobtrusively dropped in to any Express-based web application.

Packages used with passport:

```js
npm install passport passport-local passport-local-mongoose express-session
```

Example code for implementation

Schema file

```jsx
require("dotenv").config();
const mongoose = require("mongoose");
const { Schema, model } = mongoose;
const passportLocalMongoose = require("passport-local-mongoose");

//Schema
const userSchema = new Schema({
  username: {
    type: String,
    required: true,
    lowercase: true,
    minLength: 6,
    maxLength: 30,
    unique: true,
  },

  password: {
    type: String,
  },
});

//encryption
userSchema.plugin(passportLocalMongoose);

//Model
const User = model("User", userSchema);

module.exports = { User };
```

app.js file

```jsx
const express = require("express");
const mongoose = require("mongoose");
const ejs = require("ejs");
const { User } = require(__dirname + "/schema/userSchema.js");
const app = express();
const session = require("express-session");
const passport = require("passport");
const PORT = 3000;

app.use(express.json());
app.use(express.urlencoded({ extended: false }));

// cookie setup
app.use(
  session({
    secret: "keyboard cat",
    resave: false,
    saveUninitialized: false,
    // cookie: { secure: true }, // setting this to true will prevent browwser to send back the cookie if network is not secured
    cookie: { maxAge: 1 * 60 * 1000 }, // expiration set to 1 min
  })
);

// passport initialization
app.use(passport.initialize());
app.use(passport.session());

// passport config
passport.use(User.createStrategy());
passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());

//express and ejs setup
app.use(express.static("public/"));
app.set("view engine", "ejs");

// mongoose setup
mongoose.set("strictQuery", false);
const url = "mongodb://127.0.0.1/userDB";
mongoose.connect(url, { useNewUrlParser: true });

// Routes
app.get("/", (req, res) => {
  res.render("home");
});

app.get("/logout", (req, res) => {
  req.logout(err => {
    res.redirect("/");
  });
});

app.get("/secrets", (req, res) => {
  if (req.isAuthenticated()) res.render("secrets");
  else res.redirect("/login");
});

app
  .route("/register")
  .get(function (req, res) {
    res.render("register");
  })
  .post(async function (req, res) {
    try {
      const { username, password } = req.body;
      const register = await User.register({ username }, password);
      passport.authenticate("local", { failureRedirect: "/login" })(
        req,
        res,
        function () {
          res.redirect("/secrets");
        }
      );
    } catch (error) {
      console.log(error.message);
      res.json(error.message);
    }
  });

app
  .route("/login")
  .get(function (req, res) {
    res.render("login");
  })
  .post(
    passport.authenticate("local", { failureRedirect: "/login" }),
    async function (req, res) {
      res.redirect("/secrets");
    }
  );

// app.route('/login')
//     .get(function (req, res) {
//         res.render('login')
//     })
//     .post(async function (req, res) {
//         const { username, password } = req.body
//         const authenticate = User.authenticate()
//         const verify = await authenticate(username, password)
//         if (verify.user) {
//             res.redirect('/secrets')
//         }
//         else {
//             console.log(verify)
//             res.redirect('/login?valid=unauth')
//         }
//     })

app.listen(PORT, function () {
  console.log("Listening on port " + PORT);
});
```

## Bcrypt and jwt

We need bcrypt and jsonwebtoken for this along with cookie parser for this

```js
npm i bcrypt jsonwebtoken cookie-parser
```

Example code:

```jsx
require("dotenv").config();
const cors = require("cors");
const express = require("express");
const mongoose = require("mongoose");
const User = require("./models/users");
const Post = require("./models/newPost");
const fs = require("fs");
const lib = require(__dirname + "/scripts/splitLastOccurrence.js");

const jwt = require("jsonwebtoken");
const privateKey = process.env.JSON_WEB_TOKEN_SECRET_KEY;

const bcrypt = require("bcryptjs");
const salt = bcrypt.genSaltSync(10);

const cookieParser = require("cookie-parser"); // Parse Cookie header and populate req.cookies

const multer = require("multer"); //middleware for multipart form data
const uploadMiddleware = multer({ dest: "uploads/" });

// Express declaration
const app = express();

// Port
const port = 4000;

// Mongoose stuff
const db_url = process.env.MONGO_DB_URL;
mongoose.connect(db_url, { useNewUrlParser: true }); // modifying this to make it work on cycle

// middleware
app.use(cors({ origin: process.env.CLIENT_URL, credentials: true }));
app.use(express.json());
app.use(cookieParser());
app.use("/uploads", express.static(__dirname + "/uploads"));

app.post("/register", uploadMiddleware.single("file"), async (req, res) => {
  try {
    const { username, password, content } = req.body;

    if (password.length < 5) {
      throw new Error("Password must be atleast 6 characters");
    }

    const hash = bcrypt.hashSync(password, salt);

    let newPath = "/uploads/profile-pic-dummy.png";
    if (req.file) {
      const { path, originalname } = req.file;
      newPath = lib.newPath(path, originalname);
      fs.renameSync(path, newPath);
    }
    const user = new User({
      username,
      password: hash,
      content,
      cover: newPath,
    });
    const userDoc = await user.save();

    res.status(200).json("ok");
  } catch (error) {
    res.status(400).json(error.message);
    console.log(error.message);
  }
});

app.post("/login", async (req, res) => {
  try {
    const { username, password } = req.body;
    const userDoc = await User.findOne({ username });

    if (userDoc === null) {
      throw new Error("Cant find user in db");
    }

    const hash = userDoc.password;
    const validateUserPassword = bcrypt.compareSync(password, hash);

    if (validateUserPassword) {
      const token = jwt.sign(
        { username, id: userDoc._id, iat: Math.floor(Date.now() / 1000) - 30 },
        privateKey
      );
      res
        .cookie("token", token)
        .status(200)
        .json({
          username,
          id: userDoc._id,
          cover: userDoc.cover,
          content: userDoc.content,
        }); //cookie has to come first like so
    } else {
      throw new Error("Uesrname password don't match");
    }
  } catch (error) {
    res.status(400).json(error.message);
    console.log(error.message);
  }
});

app.get("/profile", (req, res) => {
  const { token } = req.cookies;
  if (token === "") {
    res.status(200).json(null);
  } else {
    const cookieValidation = jwt.verify(token, privateKey);
    res.status(200).json(cookieValidation);
  }
});

app.post("/logout", (req, res) => {
  res.cookie("token", "").status(200).json("ok");
});

app.post("/newpost", uploadMiddleware.single("file"), async (req, res) => {
  try {
    const { token } = req.cookies;
    if (token === "") {
      throw new Error("user must be logged in to create post");
    }
    const cookieValidation = jwt.verify(token, privateKey);

    const { path, originalname } = req.file;
    const newPath = lib.newPath(path, originalname);
    fs.renameSync(path, newPath);

    const { title, content, summary } = req.body;
    const postDoc = await Post.create({
      summary,
      content,
      title,
      cover: newPath,
      author: cookieValidation.id,
    });

    res.status(200).json("ok");
  } catch (error) {
    res.status(400).json(error.message);
    console.log(error.message);
  }
});

app.listen(port, () => {
  console.log("App running on port " + port);
});
```
