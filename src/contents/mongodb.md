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

Show DB:- show <database name>

```js
eg. show dbs
```

Create DB:- use <database name>

```js
eg. use shopDB
```

Create collection:- db.<collection name>.inseartOne({\_id: <id #>, name: "<name>"})

```js
eg.db.products.insertOne({ _id: 1, name: "Pensil" });
```

To show collection:- show collections

```js
show shopDB
```

To read collection:- db.<collection>.find(query,projection)

```js
eg.db.products.find();
db.products.fund({ name: "Pensil" });
db.products.find({ price: { $gt: 1 } });
db.products.find({ _id: 1 }, { name: 1, _id: 0 }); //1st param matches the record, in 2nd param, 1 means show and 0 don't show
```

Add new entry to collection:- db.<collection name>.updateOne({\_id: <id #>},{$set:{<field>:<value>}})

```js
eg.db.products.updateOne({ _id: 1 }, { $set: { stock: 32 } });
```

Delete record:- db.<collection>.deleteOne({\_id: <id #>})

```js
eg.db.products.deleteOne({ _id: 1 });
```
