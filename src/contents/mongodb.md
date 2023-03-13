---
author: w3school and Bootcamp
datetime: 2023-02-24
title: MongoDb shell commands
slug: mongodb-commands
featured: false
draft: false
tags:
  - mongodb
ogImage: ""
description: MongoDb commands
---

# MongoDb

<img src="https://www.syncfusion.com/books/MongoDB_3_Succinctly/Images/mongodb-data-structure-organization.png" alt="mongodb file structure">

## Table of contents

## Basic:-

### Show DB:-

Below commands returns all database in the server

```js
show dbs
```

### To show collection:-

Make sure you're inside a db and use the following command to get the all the collection names

```js
show collections
```

## Create DB:-

use &lt;database name&gt;

Example:

```js
use shopDB
```

The above command creates a database if it isn't already there and the console switches to the db specified

## Create collection:-

Collection can be created in 2 ways

### insertOne

db.&lt;collection name&gt;.insertOne({\_id: &lt;id #&gt;, name: "&lt;name&gt;"})

Example:

```js
db.products.insertOne({ _id: 1, name: "Pensil" });
```

You can also create a collection with multiple documents at once as shown below

Example:

```js
db.products.insertMany([
  { _id: 1, name: "Pensil" },
  { _id: 2, name: "Eraser" },
]);
```

### createCollection

db.createCollection("&lt;collection name&gt;")

Example:

```js
db.createCollection("Products");
```

<hr>

## To read collection:-

There are 2 methods to find a document in a MongoDB collection, find() and findOne().

### find

db.&lt;collection name&gt;.find(query,projection)

Here query is the document you want to find and projection determines the visiblity of data within the document

Example:

```js
db.products.find(); //Displays all document in collection
db.products.fund({ name: "Pensil" }); //Displays all documents that match the field's value
db.products.find({ price: { $gt: 1 } }); //Displays all documumt whose price is greater than 1
db.products.find({ _id: 1 }, { name: 1, _id: 0 }); //1st param matches the record, and in 2nd param, 1 means show and 0 don't show
```

### findOne

db.&lt;collection name&gt;.findOne()

To select only one document, we can use the findOne() method.
This method also take two arguments as find method above

<hr>

## Insert document in collection

There are 2 methods to insert documents into a collection.

### insertOne

db.&lt;collection name&gt;.insertOne()

Example:

```jsx
db.posts.insertOne({
  title: "Post Title 1",
  body: "Body of post.",
  category: "News",
  likes: 1,
  tags: ["news", "events"],
  date: Date(),
});
```

### insertMany

db.&lt;collection name&gt;.insertMany()

Example:

```jsx
db.posts.insertMany([
  {
    title: "Post Title 2",
    body: "Body of post.",
    category: "Event",
    likes: 2,
    tags: ["news", "events"],
    date: Date(),
  },
  {
    title: "Post Title 3",
    body: "Body of post.",
    category: "Technology",
    likes: 3,
    tags: ["news", "events"],
    date: Date(),
  },
]);
```

<hr>

## Update document in collection:-

To update an existing document we can use the updateOne() or updateMany() methods.
The first parameter is a query object to define which document or documents should be updated.
The second parameter is an object defining the updated data.

### updateOne

db.&lt;collection name&gt;.updateOne({\_id: &lt;id #&gt;},{$set:{&lt;field&gt;:&lt;value&gt;}})

Example:

```js
db.products.updateOne({ _id: 1 }, { $set: { stock: 32 } });
```

### updateMany

db.&lt;collection name&gt;.updateMany()

```jsx
db.posts.updateMany({}, { $inc: { likes: 1 } }); //Update likes on all documents by 1.
```

<hr>

## Delete record:-

We can delete documents by using the methods deleteOne() or deleteMany().
These methods accept a query object. The matching documents will be deleted.

### deleteOne

db.&lt;collection name&gt;.deleteOne({\_id: &lt;id #&gt;})

Example:

```js
db.products.deleteOne({ _id: 1 });
```

### deleteMany

db.&lt;collection name&gt;.deleteMany()
The deleteMany() method will delete all documents that match the query provided.

<hr>
