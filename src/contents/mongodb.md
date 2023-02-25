---
author: w3school and Nootcamp
datetime: 2023-02-24
title: Important MongoDb commands
slug: mongodb-commands
featured: false
draft: false
tags:
  - mongodb
ogImage: ""
description: MongoDb commands
---

# MongoDb

## Table of contents

## Basic:-

### Show DB:-

show &lt;database name&gt;

Example:

```js
show dbs
```

### Create DB:-

use &lt;database name&gt;

Example:

```js
use shopDB
```

### Create collection:-

db.&lt;collection name&gt;.insertOne({\_id: &lt;id #&gt;, name: "&lt;name&gt;"})

Example:

```js
db.products.insertOne({ _id: 1, name: "Pensil" });
```

### To show collection:-

show &lt;collection name&gt;

Example:

```js
show shopDB
```

### To read collection:-

db.&lt;collection name&gt;.find(query,projection)

Here query is the field you want to find and projection determines the visiblity of data that will be returned

Example:

```js
db.products.find();
db.products.fund({ name: "Pensil" });
db.products.find({ price: { $gt: 1 } });
db.products.find({ _id: 1 }, { name: 1, _id: 0 }); //1st param matches the record, in 2nd param, 1 means show and 0 don't show
```

### Add new entry to collection:-

db.&lt;collection name&gt;.updateOne({\_id: &lt;id #&gt;},{$set:{&lt;field&gt;:&lt;value&gt;}})

Example:

```js
db.products.updateOne({ _id: 1 }, { $set: { stock: 32 } });
```

### Delete record:-

db.&lt;collection name&gt;.deleteOne({\_id: &lt;id #&gt;})

Example:

```js
db.products.deleteOne({ _id: 1 });
```
