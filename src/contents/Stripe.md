---
author: kuldeep
datetime: 2023-01-16
title: Stripe quick setup
slug: "stripe"
featured: false
draft: false
tags:
  - stripe
  - payment
ogImage: ""
description: Basic setup of stripe
---

# Stripe basic setup

## For detailed visit

[Docs](https://stripe.com/docs/checkout/quickstart)
[setup product](https://dashboard.stripe.com/test/products?active=true)

## Basic Server setup

_server.js_

```js
//env setup
require("dotenv").config();

// This is your test secret API key.
const stripe = require("stripe")(process.env.SECRET_KEY);
const cors = require("cors");
const express = require("express");
const app = express();

//products info -- product id is linked to pricing etc
const product = {
  1: process.env.SHOPIFY_ID,
  2: process.env.BLOG_ID,
  3: process.env.ADVERTISEMENT_ID,
};

//init port number
const PORT = process.env.PORT || 5000;

// allwed address
const domain = process.env.DOMAIN;

//middleware
app.use(
  cors({
    origin: domain,
  })
);

app.use(express.static("public"));
app.use(express.json());

// Stripe call
app.post("/create-checkout", async (req, res) => {
  const lineItems = [];
  const items = req.body.items;
  items.forEach(item => {
    lineItems.push({
      price: product[item.id],
      quantity: 1,
    });
  });

  stripe.checkout.sessions
    .create({
      line_items: lineItems,
      mode: "payment",
      success_url: `${domain}/success`,
      cancel_url: `${domain}/cancel`,
    })
    .then(session => {
      res.status(200).send(JSON.stringify({ url: session.url }));
    })
    .catch(err => {
      console.error(err);
      res.status(500).send(err);
    });

  // alternate using async await
  // const session = await stripe.checkout.sessions.create({
  //     line_items: lineItems,
  //     mode: 'payment',
  //     success_url: `${domain}/success`,
  //     cancel_url: `${domain}/cancel`,
  // });

  // res.redirect(303, session.url); redirect no longer works! https://stackoverflow.com/questions/68630229/stripe-checkout-example-running-into-cors-error-from-localhost

  // res.send(JSON.stringify({url: session.url}))
});

// listen
app.listen(PORT, console.log(`Server running on port ${PORT}`));

// stripe docs
// https://stripe.com/docs/checkout/quickstart
```

_packages to install on server_

```node
npm i express stripe dotenv cors
```

## Client side

```js
function checkoutHandler(cartItems) {
  const listItems = cartItems.map(item => ({ id: item.id })); // stripe requires an array of object with product ids
  fetch("http://localhost:5000/create-checkout", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({ items: listItems }),
  })
    .then(res => res.json())
    .then(data => window.location.assign(data.url));
}
```
