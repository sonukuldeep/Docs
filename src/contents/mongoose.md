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

[Documentation](https://mongoosejs.com/docs/guides.html)

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

All available methods on [schema](https://mongoosejs.com/docs/api/schema.html)

<hr>

#### Data validation

- Validation is defined in the SchemaType
- Validation is middleware. Mongoose registers validation as a pre('save') hook on every schema by default.
- Validation always runs as the first pre('save') hook. This means that validation doesn't run on any changes you make in pre('save') hooks.
- You can disable automatic validation before save by setting the validateBeforeSave option
- You can manually run validation using doc.validate() or doc.validateSync()
- You can manually mark a field as invalid (causing validation to fail) by using doc.invalidate(...)
- Validators are not run on undefined values. The only exception is the required validator.
- When you call Model#save, Mongoose also runs subdocument validation. If an error occurs, your Model#save promise rejects
- Validation is customizable

Remember "unique" option is not a validator. [link](https://mongoosejs.com/docs/faq.html#unique-doesnt-work)

Mongoose also supports validation for update(), updateOne(), updateMany(), and findOneAndUpdate() operations. Update validators are off by default - you need to specify the runValidators option.

To turn on update validators, set the runValidators option for update(), updateOne(), updateMany(), or findOneAndUpdate(). <br><b>Be careful: update validators are off by default because they have several caveats.</b>

Instead update using save() [link](https://mongoosejs.com/docs/documents.html#updating-using-save)

We can add data validation to schema like so

```js
const fruitSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, "Name field is required, don't skip it"],
  },
  rating: {
    type: Number,
    min: 1,
    max: 10,
  },
});
```

<hr>

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

All available methods on [model](https://mongoosejs.com/docs/api/model.html)

<hr>

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

All available methods on [document](https://mongoosejs.com/docs/api/document.html)

<hr>

### Middlware

Middleware (also called pre and post hooks) are functions which are passed control during execution of asynchronous functions. Middleware is specified on the schema level and is useful for writing plugins.

Mongoose has 4 types of middleware: document middleware, model middleware, aggregate middleware, and query middleware.

More on [middleware](https://mongoosejs.com/docs/middleware.html)

<hr>

## Example code:

```js
// Import the mongoose module
const mongoose = require("mongoose");

mongoose.set("strictQuery", false);

const url = "mongodb://127.0.0.1/";

const dbName = "fruitsDB";

// Define the database URL to connect to.
const mongoDB = url + dbName;

// Connecting to database
mongoose.connect(mongoDB, { useNewUrlParser: true });

// This is the schema
const fruitSchema = new mongoose.Schema({
  name: String,
  rating: Number,
  review: String,
});

// This is the model of the above schema
const Fruit = mongoose.model("Fruit", fruitSchema);

// Instance of model
const apple = new Fruit({
  name: "Apple",
  rating: 9,
  review: "An apple a day keeps the doctor away",
});

// Saving document
apple.save(err => {
  if (err) console.error(err);
  // saved
  else console.log("successfully saved");
});
```

<hr>

## Few important functions.

### save

Saves this document by inserting a new document into the database if document is.New is true(document created using new keyword), or sends an updateOne operation only with the modifications to the database, it does not replace the whole document in the latter case.

Syntax:

```js
const <variable name> = new <collection name>(object)
<variable name>.save()
```

Example:

```js
const user = new User({ username, password });
const userDoc = await user.save();
```

### create

Shortcut for saving one or more documents to the database. MyModel.create(docs) does new MyModel(doc).save() for every doc in docs.

This function triggers the save() middleware.
Syntax:

```js
<collection name>.create(object or array of objects)
```

Example:

```js
// Insert one new `Character` document
await Character.create({ name: "Jean-Luc Picard" });

// Insert multiple new `Character` documents
await Character.create([{ name: "Will Riker" }, { name: "Geordi LaForge" }]);
```

### insertMany

The insertMany() function is used to insert multiple documents into a collection. It accepts an array of documents to insert into the collection.

Syntax:

```js
<collection name>.insertMany(array of documents, [options], callback)
```

Example:

```js
    ...
    // Fruit schema goes here
    // Fruit model goes here

    const kiwi = new Fruit({
        name: 'Kiwi',
        rating: 5,
        review: 'Never ate'
    })

    const mango = new Fruit({
        name: "Mango",
        rating: 9,
        review: "favourite fruit"
    })

    Fruit.insertMany([kiwi, mango], (err) => { // add more than my docmunet
        if (err)
            console.error(err);
        else // saved
            console.log('successfully saved')
    })

    or

    // Function call
    User.insertMany([
        { name: 'Gourav', age: 20},
        { name: 'Kartik', age: 20},
        { name: 'Niharika', age: 20}
    ]).then(function() {
        console.log("Data inserted")  // Success
    }).catch(function(error){
        console.log(error)      // Failure
    });

```

<hr>

### find

The find() function is used to find particular data from the MongoDB database. It takes 3 arguments and they are query (also known as a condition), query projection (used for mentioning which fields to include or exclude from the query), and the last argument is the general query options (like limit, skip, etc).

Syntax:

```js
  <collection name>.find(filter, [projection], [options], callback)
```

Example:

```js
Fruit.find({}, function (err, result) {
  if (err) console.log(err);
  else {
    console.log("Search was successful and here are the results" + result);
    const names = result.map(res => res.name);
    console.log(names);
  }
});

or;

// Only one parameter [query/condition]
// Find all documents that matches the
// condition name='Punit'
User.find({ name: "Punit" }, function (err, docs) {
  if (err) {
    console.log(err);
  } else {
    console.log("First function call : ", docs);
  }
});

or;

// Only Two parameters [condition, query projection]
// Here age:0 means don't include age field in result
User.find({ name: "Punit" }, { age: 0 }, function (err, docs) {
  if (err) {
    console.log(err);
  } else {
    console.log("Second function call : ", docs);
  }
});

or;

// All three parameter [condition, query projection,
// general query options]
// Fetch first two records whose age >= 10
// Second parameter is null i.e. no projections
// Third parameter is limit:2 i.e. fetch
// only first 2 records
User.find({ age: { $gte: 10 } }, null, { limit: 2 }, function (err, docs) {
  if (err) {
    console.log(err);
  } else {
    console.log("Third function call : ", docs);
  }
});
```

<hr>

### findOne

The findOne() function is used to find one document according to the condition. If multiple documents match the condition, then it returns the first document satisfying the condition.

Syntax:

```js
<collection name>.findOne([filter], [projection], [options], callback)
```

Example:

```js
// Find only one document matching
// the condition(age >= 5)
User.findOne({ age: { $gte: 5 } }, function (err, docs) {
  if (err) {
    console.log(err);
  } else {
    console.log("Result : ", docs);
  }
});
```

<hr>

### updateOne

The updateOne() function is used to update the first document that matches the condition.

Syntax:

```js
<collectionn name>.updateOne(filter, update, [options], callback)
```

Example:

```js
// Find all documents matching the condition
// (age >= 5) and update first document
// This function has 4 parameters i.e.
// filter, update, options, callback
User.updateOne({ age: { $gte: 5 } }, { name: "ABCD" }, function (err, docs) {
  if (err) {
    console.log(err);
  } else {
    console.log("Updated Docs : ", docs);
  }
});
```

<hr>

### updateMany

The updateMany() function is same as update(), except MongoDB will update all documents that match the filter. It is used when the user wants to update all documents according to the condition.

Syntax:

```js
<collection name>.updateMany(filter, update, [options], callback)
```

Example:

```js
// Find all documents matching the
// condition(age>=5) and update all
// This function has 4 parameters i.e.
// filter, update, options, callback
User.updateMany({ age: { $gte: 5 } }, { name: "ABCD" }, function (err, docs) {
  if (err) {
    console.log(err);
  } else {
    console.log("Updated Docs : ", docs);
  }
});
```

<hr>

### deleteOne

Deletes the first document that matches conditions from the collection. It returns an object with the property deletedCount indicating how many documents were deleted. Behaves like remove(), but deletes at most one document regardless of the single option.

Syntax:

```js
<collection name>.deleteOne(filter, [options], callback)
```

Example:

```js
Fruit.deleteOne({ _id: "63fbce4d7f5c436aa036fff5" }, err => {
  if (err) console.error(err);
  else console.log("Successfully deleted document");
});
```

Note if the document doesn't exist this won't throw an err

<hr>

### deleteMany

Deletes all of the documents that match conditions from the collection. It returns an object with the property deletedCount containing the number of documents deleted. Behaves like remove(), but deletes all documents that match conditions regardless of the single option.

Syntax:

```js
<collection name>.deleteMany(filter, [options], callback)
```

Example:

```js
Fruit.deleteMany({ name: "Strawberrry" }, err => {
  if (err) console.error(err);
  else console.log("Successfully deleted document");
});
```

<hr>

### findOneAndUpdate

The findOneAndUpdate() function is used to find a matching document and update it according to the update arg, passing any options, and returns the found document (if any) to the callback.

Syntax:

```js
<collection name>.findOneAndUpdate([filter], [update], [option], callback)
```

Example:

```js
// Find document matching the condition(age >= 5)
// and update first document with new name='Anuj'
// This function has 4 parameters i.e. filter,
// update, options, callback
User.findOneAndUpdate(
  { age: { $gte: 5 } },
  { name: "Anuj" },
  null,
  function (err, doc) {
    if (err) {
      console.log(err);
    } else {
      console.log("Original Doc : ", doc);
    }
  }
);
```

<hr>

### findByIdAndUpdate

Issues a mongodb findAndModify update command by a document's \_id field. findByIdAndUpdate(id, ...) is equivalent to findOneAndUpdate({ \_id: id }, ...).

Finds a matching document, updates it according to the update arg, passing any options, and returns the found document (if any).

Syntax:

```js
<collection name>.findByIdAndUpdate(id, [update], [options], callback)
```

Example:

```js
User.findByIdAndUpdate(id: "1235563624236367",
    {name:"Anuj"}, null, function (err, docs) {
    if (err){
        console.log(err)
    }
    else{
        console.log("Original Docs : ", docs);
    }
});
```

<hr>

### findOneAndDelete

Issue a MongoDB findOneAndDelete() command.

Finds a matching document, removes it, and returns the found document (if any).

This function triggers the following middleware.

    findOneAndDelete()

This function differs slightly from Model.findOneAndRemove() in that findOneAndRemove() becomes a MongoDB findAndModify() command, as opposed to a findOneAndDelete() command. For most mongoose use cases, this distinction is purely pedantic. You should use findOneAndDelete() unless you have a good reason not to.

Syntax:

```js
<collection name>.findOneAndDelete(filter, [options], callback)
```

Example:

```js
// Find only one document matching the
// condition(age >= 5) and delete it
User.findOneAndDelete({ age: { $gte: 5 } }, function (err, docs) {
  if (err) {
    console.log(err);
  } else {
    console.log("Deleted User : ", docs);
  }
});
```

<hr>

### findByIdAndDelete

Issue a MongoDB findOneAndDelete() command by a document's \_id field. In other words, findByIdAndDelete(id) is a shorthand for findOneAndDelete({ \_id: id }).

This function triggers the following middleware.

    findOneAndDelete()

Syntax:

```js
<collection name>.findByIdAndDelete(id, [options], callback)
```

Example:

```js
// Find a document whose
// id=5ebadc45a99bde77b2efb20e and remove it
var id = "5ebadc45a99bde77b2efb20e";
User.findByIdAndDelete(id, function (err, docs) {
  if (err) {
    console.log(err);
  } else {
    console.log("Deleted : ", docs);
  }
});
```

<hr>
