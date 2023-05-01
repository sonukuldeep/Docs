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
