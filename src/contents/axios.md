---
title: Axios
author: Internet
datetime: 2023-05-01
slug: "axios"
featured: false
draft: false
tags:
  - axios
ogImage: ""
description: Axios is a promise-based HTTP Client for node.js and the browser.
---

# Axios

## What is Axios?

Axios is a promise-based HTTP Client for node.js and the browser. It is isomorphic (= it can run in the browser and nodejs with the same codebase). On the server-side it uses the native node.js http module, while on the client (browser) it uses XMLHttpRequests

## Installation

Nodejs

```js
npm install axios
```

cdn

```jsx
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

Imports

```jsx
const axios = require("axios/dist/browser/axios.cjs"); // browser
const axios = require("axios/dist/node/axios.cjs"); // node
import axios from "axios"; //react
```

Example

```js
import axios from "axios";

const todosApi = axios.create({
  baseURL: "http://localhost:3500",
});

export const getTodos = async () => {
  const response = await todosApi.get("/todos");
  return response.data;
};

export const addTodo = async todo => {
  return await todosApi.post("/todos", todo);
};

export const updateTodo = async todo => {
  return await todosApi.patch(`/todos/${todo.id}`, todo);
};

export const deleteTodo = async ({ id }) => {
  return await todosApi.delete(`/todos/${id}`, id);
};

export default todosApi;
```

## Axios making requests

There are multiple methods for creating requests in axios.

axios(config) 
axios(url[, config])

These are basic methods for generating requests in axios.

* axios.request(config)
* axios.get(url[, config])
* axios.delete(url[, config])
* axios.head(url[, config])
* axios.options(url[, config])
* axios.post(url[, data[, config]])
* axios.put(url[, data[, config]])
* axios.patch(url[, data[, config]])

These are method aliases, created for convenience.
Axios Response object

When we send a request to a server, it returns a response. The Axios response object consists of:

*    data - the payload returned from the server
*    status - the HTTP code returned from the server
*    statusText - the HTTP status message returned by the server
*    headers - headers sent by server
*    config - the original request configuration
*    request - the request object

## Axios GET request with callbacks

In this example, we use callbacks.
```js
const axios = require('axios');

axios.get('http://webcode.me').then(resp => {

    console.log(resp.data);
});
```

## Axios GET request with async/await

The following example creates the same request. This time we use async/await syntax.

```js
const axios = require('axios');

async function doGetRequest() {

  let res = await axios.get('http://webcode.me');

  let data = res.data;
  console.log(data);
}

doGetRequest();
```


