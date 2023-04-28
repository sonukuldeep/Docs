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
  - next-auth
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
const express = require("express");
const mongoose = require("mongoose");
const User = require("./models/users");

const jwt = require("jsonwebtoken");
const privateKey = process.env.JSON_WEB_TOKEN_SECRET_KEY;

const bcrypt = require("bcryptjs");
const salt = bcrypt.genSaltSync(10);

const cookieParser = require("cookie-parser"); // Parse Cookie header and populate req.cookies

// Express declaration
const app = express();

// Port
const port = 4000;

// Mongoose stuff
const db_url = process.env.MONGO_DB_URL;
mongoose.connect(db_url, { useNewUrlParser: true }); // modifying this to make it work on cycle

// middleware
app.use(express.json());
app.use(cookieParser());

app.post("/register", async (req, res) => {
  try {
    const { username, password, content } = req.body;

    if (password.length < 5) {
      throw new Error("Password must be atleast 6 characters");
    }

    const hash = bcrypt.hashSync(password, salt);

    const user = new User({
      username,
      password: hash,
      content,
    });

    const userDoc = await user.save();

    res.status(200).json("ok");
  } catch (error) {
    console.log(error.message);
    res.status(400).json(error.message);
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
      const token = jwt.sign({ username, id: userDoc._id }, privateKey, {
        expiresIn: "1h",
      });
      res.cookie("token", token).status(200).json({
        username,
        id: userDoc._id,
        content: userDoc.content,
      }); //cookie has to come first like so
    } else {
      throw new Error("Uesrname & password don't match");
    }
  } catch (error) {
    res.status(400).json(error.message);
    console.log(error.message);
  }
});

app.get("/profile", (req, res) => {
  const { token } = req.cookies;
  // instead of checking like this create a middleware
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

app.listen(port, () => {
  console.log("App running on port " + port);
});
```

<hr>

## Passport and Oauth

Using google in this case
Go to google [console](https://console.cloud.google.com/welcome) and create an api
With api created you should have CLIENTID and CLIENTSECRET
We need that. Note for callback url use http://localhost:3000/auth/google/callback

### app.js or server.js

```js
require("dotenv").config();
const express = require("express");
const app = express();
const mongoose = require("mongoose");
const { User } = require(__dirname + "/schema/userSchema.js");
const passport = require("passport");
const session = require("express-session");
const GoogleStrategy = require("passport-google-oauth20").Strategy;

// port
const PORT = 3000;

// middleware
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
passport.serializeUser(function (user, cb) {
  // https://www.passportjs.org/concepts/authentication/strategies/
  process.nextTick(function () {
    return cb(null, {
      id: user.id,
      username: user.username,
      picture: user.picture,
    });
  });
});

passport.deserializeUser(function (user, cb) {
  process.nextTick(function () {
    return cb(null, user);
  });
});

// mongoose setup
mongoose.set("strictQuery", false);
const url = "mongodb://127.0.0.1/userDB";
mongoose.connect(url, { useNewUrlParser: true });

// Google stratergy
passport.use(
  new GoogleStrategy(
    {
      //https://www.passportjs.org/packages/passport-google-oauth20/
      clientID: process.env.CLIENT_ID,
      clientSecret: process.env.CLIENT_SECRET,
      callbackURL: "http://localhost:3000/auth/google/callback",
    },
    async function (accessToken, refreshToken, profile, cb) {
      try {
        const user = await User.findOne({ username: profile.displayName });
        if (!user) {
          const user = await User.create({ username: profile.displayName });
          return cb(null, user);
        }
        return cb(null, user);
      } catch (error) {
        console.log(error.message);
      }
    }
  )
);

// Routes
app.get("/", function (req, res) {
  res.render("home");
});

app.get(
  "/auth/google",
  passport.authenticate("google", { scope: ["profile"] })
);

app.get(
  "/auth/google/callback",
  passport.authenticate("google", { failureRedirect: "/login" }),
  function (req, res) {
    // Successful authentication, redirect home.
    res.redirect("/");
  }
);

app.get("/logout", function (req, res) {
  req.logout();
  res.redirect("/");
});

app.post("/secrets", function (req, res) {
  if (req.isAuthenticated()) res.render("secrets");
  else res.redirect("/login");
});

app.listen(PORT, function () {
  console.log("Listening on port " + PORT);
});
```

### user schema file

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

<hr>

## Using Next Auth

[Documentation](https://next-auth.js.org/getting-started/introduction)

Package:

```jsx
npm i next-auth
```

Providers have to be configured as per the docs

[Use website](https://blog.logrocket.com/nextauth-js-for-next-js-client-side-authentication/)

### Setup

#### For Nextjs 13.2 or higher

Documentation isn't complete yet
[link](https://next-auth.js.org/configuration/initialization#route-handlers-app) <br>

#### For Nextjs 13.1 and lower

api/auth/[...nextauth].js

```jsx
import NextAuth from "next-auth";
import GoogleProvider from "next-auth/providers/google";
import GitHubProvider from "next-auth/providers/github";

export const authOptions = {
  // Configure one or more authentication providers
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    }),
    GitHubProvider({
      clientId: process.env.GITHUB_ID,
      clientSecret: process.env.GITHUB_SECRET,
    }),
  ],
};

export default NextAuth(authOptions);
```

### Add provider

\_app.tsx

```jsx
import "@/styles/globals.css";
import type { AppProps } from "next/app";
import { SessionProvider } from "next-auth/react";
import { Session } from "next-auth";

interface AppPropsWithSession extends AppProps {
  session: Session;
}

export default function App({
  Component,
  pageProps,
  session,
}: AppPropsWithSession) {
  return (
    <SessionProvider session={session}>
      <Component {...pageProps} />
    </SessionProvider>
  );
}
```

.env.local

```js
GOOGLE_CLIENT_ID= get from google // [Get from here](https://console.developers.google.com/apis/credentials)
GOOGLE_CLIENT_SECRET= get from google
GITHUB_ID= get from github // [get from here](https://github.com/settings/apps)
GITHUB_SECRET= get from github


NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET= run 'openssl rand -base64 32' and paste here
```

### Usage

#### Example:

Login

```jsx
import { GetServerSideProps } from "next";
import { useSession, signIn, signOut } from "next-auth/react";

const Login = () => {
  const { data: session } = useSession();
  if (session) {
    // if not logged in session is null
    return (
      <div>
        <p>You are logged in as {session.user?.name}</p>
        <button onClick={() => signOut()}>Sign out</button>
      </div>
    );
  } else {
    return (
      <div>
        <p>You're not logged in click sign in to login</p>
        <button onClick={() => signIn()}>Sign in</button>
      </div>
    );
  }
};

export default Login;
```

Restrict access

```jsx
import { useSession, signOut } from "next-auth/react";

const restricted = () => {
  const { data: session } = useSession({ required: true }); // forces user to log in to view the page
  return <div>restricted</div>;
};

export default restricted;
```

Access session serverside

```jsx
import { getSession, GetSessionParams } from "next-auth/react";

const serverside = ({ order }: OrderProps) => {
  return (
    <div>
      <p>{order.firstName}</p>
      <p>{order.lastName}</p>
    </div>
  );
};

export default serverside;

export const getServerSideProps = async (context: GetSessionParams) => {
  const session = await getSession(context);
  if (!session) {
    console.log("user not logged in... redirecting");
    return {
      redirect: {
        destination: "/login",
      },
    };
  } else {
    return {
      props: {
        order: {
          firstName: "Dummy",
          lastName: "data",
        },
      },
    };
  }
};
```

> Successfully login session in google will log like this

```jsx
{
  user: {
    name: 'abc def',
    email: 'abc@gmail.com',
    image: 'abcdef'
  },
  expires: 'time here'
}
```

While a logged out session will be null
