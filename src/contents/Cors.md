---
author: Kuldeep
datetime: 2022-12-21
title: Understanding cors
slug: "cors"
featured: false
draft: false
tags:
  - cors
  - server
ogImage: ""
description: CORS, Cross-Origin Resource Sharing.
---

# CORS (Cross-Origin Resource Sharing)

### What Is CORS?

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any origins (domain, scheme, or port) other than its own from which a browser should permit loading resources.

With CORS by default all requests between two different origins are blocked. CORS does not care about requests between the same origin, though. If you have a request that goes from http://localhost:3000 to http://localhost:3000/items that will be allowed since they are the same origin and CORS does not apply to same origin requests.

If you want to make a request between your client and server and they are on different URLs then you need to pass down the Access-Control-Allow-Origin header from your server to your client with each request. The value of this header is just the origin the server allows to access the data.

Depending on what server language you are using there is most likely an easy way to do this. Lets see how to do it using the cors library with express in Node.js.

All we need to do is install the cors library and then write the following line of code in your Express app.

```shell
const express = require('express')
const app = express()
const cors = require('cors')

//init port number
const PORT = process.env.PORT || 5000

// allwed address
const url = "http://localhost:1234"

//middleware
app.use(
  cors({
    origin: url,
  })
)

// Server code

// listen
app.listen(PORT, (console.log(`Server running on port ${PORT}`)))
```

This code tells your app to send down the Access-Control-Allow-Origin header with the value of http://localhost:1234. If you want to just allow all URLS to access your site you can instead use \* as the origin value in the header and that will allow cross origin requests from any URL. This will work for most of your CORS issues, but there are some instances where you need to do a bit of extra work.

### CORS Preflight

If we are making a PUT request or some other complex request to a cross origin URL, then doing just the above will not actually work. This is because the browser will send a preflight request to the server asking the server if they are allowed to make this PUT request. This preflight request will contain the Access-Control-Request-Method and Access-Control-Request-Headers headers. These headers contain the value of the method and headers that the client wants to use in the request and the server will return back if the method and headers are valid.

The server does this by returning down a Access-Control-Allow-Methods header that contains a list of all the allowed methods (GET, POST, PUT, etc.). The server also sends down a Access-Control-Allow-Headers header that contains all the allowed headers for the request. If the allowed methods and headers match the requested headers and method then the request is allowed to go through.

This may sound complicated but if you are using the cors library in express it is very easy.

```shell
...
app.use(cors({
  origin: 'http://localhost:1234',
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type']
}))
...
```

This will send back the allowed headers and methods headers to the client based on the data passed to the cors function. Also, if we do not set the allowedHeaders key then it will default to the same list sent up by the client in the Access-Control-Request-Headers which is why generally you do not need to manually set the allowedHeaders.

### Dealing With Credentials

The final difficult thing to deal with for CORS is cookies. By default CORS will not send our cookies along with a request unless we specifically tell it to. In order to allow our cookies to be sent, we first need to tell the request that it is with credentials by setting the credentials option in the fetch request to include.

```shell
fetch(url, { credentials: 'include' })
```

In order to accept the credentials our server needs to specify the Access-Control-Allow-Credentials with a value of true. Again, in express with the cors library this is simple to do.

```shell
const express = require('express')
const app = express()
const cors = require('cors')

app.use(cors({
  origin: 'http://localhost:1234',
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type'],
  credentials: true
}))

// Server code

app.listen(3000)
```
