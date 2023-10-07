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

## React

### What is Reactjs?

React is a declarative, flexible open source front-end JavaScript library developed by Facebook in 2011. It follows the component-based approach for building reusable UI components, especially for single page application(SPA). It is used for developing interactive view layer of web and mobile apps. It was created by Jordan Walke, a software engineer at Facebook.

## What are some of the key features of React?

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
