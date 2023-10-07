---
author: kuldeep
datetime: 2023-10-06
title: Interview questions
slug: "interview"
featured: false
draft: false
tags:
  - interview
ogImage: ""
description: Collection of interview question and answers
---

# Interview 101

## Table of Context

## React

### What is Reactjs?

React is a declarative, flexible open source front-end JavaScript library developed by Facebook in 2011. It follows the component-based approach for building reusable UI components, especially for single page application(SPA). It is used for developing interactive view layer of web and mobile apps. It was created by Jordan Walke, a software engineer at Facebook.

### What are some of the key features of React?

The main features of React are:

- JSX - JavaScript XML is a syntax extension for JavaScript that is commonly used in the context of React. JSX allows one to write HTML-like code within javaScript, making it easier to create and define the structure of UI component in a way that resembles HTML or XML.
- Component based
- Unidirectional Data flow - Passing props from parent to child
- Virtual DOM
- View oriented

### Can web browsers read JSX directly?

- Web browsers cannot read JSX directly. This is because they are built to only read regular JS objects and JSX is not a regular JavaScript object
- For a web browser to read a JSX file, the file needs to be transformed into a regular JavaScript object. For this, we use Babel

### What is the virtual DOM?

DOM stands for Document Object Model. The DOM represents an HTML document with a logical tree structure. Each branch of the tree ends in a node, and each node contains objects.

![virtual dom](https://www.simplilearn.com/ice9/free_resources_article_thumb/virtualdom.JPG)

React keeps a lightweight representation of the real DOM in the memory, and that is known as the virtual DOM. When the state of an object changes, the virtual DOM changes only that object in the real DOM, rather than updating all the objects.

## Javascript

### Asynchronous code in js

Asynchronous operations in JavaScript are crucial for performing tasks that may take some time to complete, such as making network requests, reading files, or executing long-running computations. Understanding how asynchronous operations work in JavaScript involves several key concepts: the call stack, callback queue (also known as the task queue), Web API, and the event loop.

- Call Stack:
  The call stack is a data structure that keeps track of the execution of functions in JavaScript.
  When a function is called, it is pushed onto the call stack, and when a function returns, it is popped off the stack.
  JavaScript is single-threaded, meaning it can execute only one operation at a time. The call stack helps manage this single-threaded execution.

- Web API:
  Web APIs are provided by the browser (in the case of the browser environment) to perform tasks that are not directly related to JavaScript execution, such as making HTTP requests, interacting with the DOM, and setting timers.
  When you make an asynchronous request, like an HTTP request using fetch, it is handed off to the Web API to execute, and JavaScript execution continues without waiting for the result.

- Callback Queue (Task Queue):
  When an asynchronous operation is completed in the Web API (e.g., an HTTP request finishes, or a timer expires), a callback function associated with that operation is placed in the callback queue.
  The callback queue is also sometimes referred to as the task queue.

- Event Loop:
  The event loop is a continuous process that constantly checks the call stack and the callback queue.
  If the call stack is empty (meaning the JavaScript runtime is not executing any code), the event loop will take the first function from the callback queue and push it onto the call stack for execution.
  This process ensures that asynchronous operations can be processed as soon as the call stack is empty.

### Promise

A Promise in JavaScript is a special object that represents the eventual completion (or failure) of an asynchronous operation and its resulting value. Promises are commonly used for handling asynchronous code in a more structured and manageable way compared to callback functions.

Promises have several stages in their lifecycle, which are represented by three states: `Pending`, `Fulfilled`, and `Rejected`. Here's an overview of each stage:

- **Pending**:

  - When a Promise is created, it starts in the `Pending` state. This means that the asynchronous operation represented by the Promise is still in progress, and the final outcome (success or failure) is not yet determined.
  - During the pending stage, you can attach `.then()` and `.catch()` handlers to the Promise to specify what should happen when the Promise is fulfilled (resolved successfully) or rejected (encounters an error).

- **Fulfilled (Resolved)**:

  - If the asynchronous operation succeeds, the Promise transitions to the `Fulfilled` state.
  - When the Promise is fulfilled, it means that the operation has completed successfully, and it holds a resolved value.
  - The `.then()` handler attached to the Promise will be called with the resolved value, allowing you to process the result of the operation.

- **Rejected**:
  - If the asynchronous operation encounters an error or fails for any reason, the Promise transitions to the `Rejected` state.
  - When the Promise is rejected, it holds a reason or an error object that describes why the operation failed.
  - The `.catch()` handler attached to the Promise will be called with the rejection reason, allowing you to handle errors and perform error-specific logic.

Here's an example of a Promise in action:

```javascript
const myPromise = new Promise((resolve, reject) => {
  // Simulate an asynchronous operation
  setTimeout(() => {
    const success = true; // Change to false to simulate a rejection
    if (success) {
      resolve("Operation successful");
    } else {
      reject("Operation failed");
    }
  }, 2000);
});

myPromise
  .then(result => {
    console.log("Fulfilled:", result);
  })
  .catch(error => {
    console.error("Rejected:", error);
  });
```

Promises are a fundamental tool for working with asynchronous code in JavaScript and are widely used to simplify complex asynchronous operations and improve code readability and maintainability. They provide a clear way to handle success and error scenarios while avoiding callback hell.
