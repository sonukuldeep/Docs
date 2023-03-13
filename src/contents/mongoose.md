---
author: w3school and Bootcamp
datetime: 2023-03-12
title: Mongoose
slug: mongodb-mongoose
featured: true
draft: false
tags:
  - mongodb mongoose
ogImage: ""
description: MongoDb driver mongoose
---

# Mongoose

## Table of contents

## Terminology

### Schemas

Everything in Mongoose starts with a Schema. Each schema maps to a MongoDB collection and defines the shape of the documents within that collection.

```js
import mongoose from "mongoose";
const { Schema } = mongoose;

const blogSchema = new Schema({
  title: String, // String is shorthand for {type: String}
  author: String,
  body: String,
  comments: [{ body: String, date: Date }],
  date: { type: Date, default: Date.now },
  hidden: Boolean,
  meta: {
    votes: Number,
    favs: Number,
  },
});
```

If you want to add additional keys later, use the [.add](https://mongoosejs.com/docs/api/schema.html#schema_Schema-add) method.

### Model

Models are fancy constructors compiled from Schema definitions. An instance of a model is called a document. Models are responsible for creating and reading documents from the underlying MongoDB database.

```js
const schema = new mongoose.Schema({ name: "string", size: "string" });
const Tank = mongoose.model("Tank", schema);
```

- The first argument is the "singular name" of the collection your model is for.
- Mongoose automatically looks for the plural, lowercased version of your model name.

Thus, for the example above, the model Tank is for the tanks collection in the database.

> Note that no tanks will be created/removed until the connection your model uses is open. Every model has an associated connection. When you use mongoose.model(), your model will use the default mongoose connection.

### Document

Mongoose documents represent a one-to-one mapping to documents as stored in MongoDB. Each document is an instance of its Model.

Document and Model are distinct classes in Mongoose. The Model class is a subclass of the Document class. When you use the Model constructor, you create a new document.

```js
const MyModel = mongoose.model("Test", new Schema({ name: String }));
const doc = new MyModel();

doc instanceof MyModel; // true
doc instanceof mongoose.Model; // true
doc instanceof mongoose.Document; // true
```

In Mongoose, a "document" generally means an instance of a model. You should not have to create an instance of the Document class without going through a model.

<hr>

## Full code:

````js
    // Import the mongoose module
    const mongoose = require("mongoose")

    mongoose.set('strictQuery', false)

    const url = 'mongodb://127.0.0.1/'

    const dbName = 'fruitsDB'

    // Define the database URL to connect to.
    const mongoDB = url + dbName

    // Connecting to database
    mongoose.connect(mongoDB, { useNewUrlParser: true })

    // This is the schema
    const fruitSchema = new mongoose.Schema({
        name: String,
        rating: Number,
        review: String
    })

    // This is the model of the above schema
    const Fruit = mongoose.model("Fruit", fruitSchema)

    // Instance of model
    const apple = new Fruit({
        name: "Apple",
        rating: 9,
        review: "An apple a day keeps the doctor away"
    })

    // Saving document
    apple.save((err) => {
        if (err)
            console.error(err);
        else // saved
            console.log('successfully saved')
    });
    ```
````
