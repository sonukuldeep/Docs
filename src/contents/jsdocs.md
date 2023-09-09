---
author: kuldeep
datetime: 2023-09-09
title: JsDoc
slug: "jsdoc"
featured: false
draft: false
tags:
  - jsdoc
ogImage: ""
description: Js doc
---

**jsdoc.join**

```json
{
  "plugins": ["plugins/markdown"],
  "recurseDepth": 10,
  "source": {
    "include": ["assets/scripts"],
    "includePattern": ".js$",
    "excludePattern": "(node_modules/|jsdoc)"
  },
  "templates": {
    "cleverLinks": true,
    "monospaceLinks": true
  },
  "opts": {
    "destination": "./jsdoc",
    "recurse": true,
    "tutorials": "./tutorials",
    "readme": "./README.md"
  }
}
```

**main.js** (sample)

```js
// @ts-check
const { add, multiply, subtract } = require("./calculator");

/**
 * @file is the root file for this example
 * @author Kuldeep kumar
 * @see {@link https://codethatdev.com|CodeThatDev}
 */

/**
 * Description:- Student data variable
 *
 * @type {string}
 */
const studentname = "John Doe";

/**
 * Description: Grades of marks in different subject
 *
 * @type {Array<number>}
 */
const grades = [98, 90, -50, 0.52];

/**
 * Description:- Random example function
 *
 * @type {{ id: number, name: string }}
 */
const example = {
  id: 1,
  name: "Jon doe",
};

/**
 * Description:- placeholder
 *
 * @param {string} name
 * @returns {void}
 */
function fname(name) {
  const fname = name + "dao";
  console.log(fname);
}

/**
 * Description:- placeholder
 *
 * @param {number} amount - total amount
 * @param {number} tax - tax percent
 * @returns {string}
 */
const calculateTax = (amount, tax) => {
  return `$${amount + tax * amount}`;
};

/**
 * A student class
 * @typedef {Object} Student
 * @property {number} id - Student id
 * @property {string} name - Student name
 * @property {string|number} [age] - Student age
 * @property {boolean} isActive - Student status
 */

/**
 * Description:- Student object
 *
 * @type {Student}
 */
const student = {
  id: 1,
  name: "Jon doe",
  age: 1,
  isActive: false,
};

class Person {
  /**
   * Creates an instance of Person.
   *
   * @constructor
   * @param {{name: string,id:number,rollNo:number}} personInfo
   */
  constructor(personInfo) {
    this.name = personInfo.name;
    this.id = personInfo.id;
    this.rollNo = personInfo.rollNo;
  }

  /**
   * Description:- Return student details
   *
   * @returns {string}
   */
  displayInfo() {
    return `name: ${this.name} id: ${this.id} rollNo: ${this.rollNo}`;
  }
}
/**
 * Person one
 * see {@link Person}
 */
const person1 = new Person({ name: "John dao", id: 2, rollNo: 5 });
person1.displayInfo();

console.log(add(20, 25));
```
