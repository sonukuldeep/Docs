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
![main image](https://www.cheggindia.com/wp-content/uploads/2021/02/HR-Interview-Questions-for-Freshers-2023-Detailed-Guide-with-Answers.png)

## Table of Contents

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

### Synthetic events

In React, synthetic events are a way to handle and interact with browser events in a consistent and cross-browser-compatible manner. They are an abstraction layer on top of native browser events, provided by React to simplify event handling and provide a more predictable and unified API for working with events across different browsers.

Here are some key points about synthetic events in React:

- Cross-Browser Compatibility: Synthetic events are designed to work consistently across different web browsers, abstracting away some of the inconsistencies and quirks that can exist in browser-specific event implementations.

- Performance Optimization: React uses a technique called event delegation to improve performance. Instead of attaching event listeners to individual DOM elements, React attaches a single event listener to the top-level document or a container element. When an event occurs, React determines which component's event handler should be called. This can lead to more efficient event handling in applications with many components.

- Event Pooling: React uses an event pooling mechanism to reduce memory usage and improve performance. When an event is triggered, React reuses the same event object for all event handlers, making it more efficient than creating a new event object for each handler.

- Automatic Cleanup: React takes care of automatically cleaning up event listeners when a component is unmounted. This helps prevent memory leaks that can occur if you manually attach event listeners.

### Lifecycle methods

In React, the lifecycle of a component refers to the series of events or methods that a component goes through from its creation and mounting in the DOM to its updating, unmounting, and removal from the DOM.

- Initial Stage (Mounting):

        constructor(): This is the first method called when a component is being created. It's used to initialize the component's state and bind event handlers. Avoid side effects here, as it's called only once.

        render(): The render method is responsible for rendering the component's UI. It returns JSX that describes what should be displayed on the screen. It's called every time the component needs to re-render.

        componentDidMount(): After the component's initial render, this method is called. It's commonly used for performing side effects, such as making network requests or initializing third-party libraries. It's a good place to set up timers or subscriptions as well.

- Update Stage (Updating):

        static getDerivedStateFromProps(): This method is called whenever new props are received or before render in the initial render. It's used to compute a new state based on changes in props. It's a static method and should not have side effects.

        shouldComponentUpdate(): This method allows you to control whether the component should re-render when the props or state change. By default, it returns true. You can implement custom logic to optimize performance by preventing unnecessary renders.

        render(): If shouldComponentUpdate returns true (or is not implemented), render is called again to re-render the component with updated props and state.

        componentDidUpdate(): After a component's update has been rendered, this method is called. It's often used for side effects based on changes in props or state, such as updating the DOM or making additional network requests.

- Unmount Stage (Unmounting):
  componentWillUnmount(): This method is called just before the component is removed from the DOM. It's used for cleaning up resources, like event listeners or timers, to prevent memory leaks.

### Functional component vs class based components

- **Simplicity**:

  - Functional components are simpler to write and understand. They don't involve the complexities of class definitions, constructors, or lifecycle methods.
  - Less boilerplate code means fewer opportunities for bugs, and it makes it easier for developers, including beginners, to work with React.

- **Ease of Understanding**:

  - Functional components focus primarily on rendering UI based on props and state, making it easier to reason about their behavior.
  - The code in a functional component flows top to bottom, which aligns well with how we think about UI composition.

- **No 'this' Keyword**:

  - Functional components don't use the `this` keyword, eliminating the need for explicit binding of event handlers.
  - This simplifies the code and helps avoid common issues related to the context of `this`.

- **Improved Performance**:

  - Functional components are generally faster to render than class components. They have a smaller memory footprint and don't involve the overhead of class instances.
  - React can optimize functional components more effectively in some cases.

- **Hooks**:

  - Functional components are the recommended way to use React Hooks. Hooks provide a more intuitive way to manage state, side effects, and component lifecycles.
  - Hooks make it easier to reuse stateful logic across components.

- **Reusability**:

  - Functional components are inherently more reusable because they are essentially functions that can be easily composed and combined.
  - This reusability can lead to cleaner and more maintainable code.

- **Simplified Lifecycle with Hooks**:

  - Functional components with Hooks simplify the management of component lifecycles and side effects, replacing the need for numerous lifecycle methods in class components.
  - Hooks like `useEffect` provide a unified way to handle side effects.

- **Easier Testing**:

  - Functional components are easier to test because they are pure functions. You can test their behavior by providing props and examining the output.
  - Testing is simplified as there's no need to mock or handle component instances.

- **Better Performance Optimization**:

  - With React's `React.memo` and `useMemo` from Hooks, you can easily optimize functional components to prevent unnecessary re-renders.
  - These optimizations can lead to better application performance.

- **Consistency with Upcoming Features**:
  - React is actively evolving, and many new features and optimizations are designed with functional components and Hooks in mind.
  - Using functional components aligns your codebase with the direction of the React library and ensures compatibility with future updates.

In summary, functional components offer simplicity, ease of understanding, and improved performance compared to class-based components. They encourage a more declarative and functional programming style, making them the preferred choice for many React developers, especially in modern React applications.

## Javascript

### Closure

A closure is a fundamental concept in JavaScript (and many other programming languages) that refers to the ability of a function to "remember" its lexical scope even after that function has finished executing. In other words, a closure allows a function to maintain access to variables and parameters from its outer or enclosing function's scope, even when the outer function has completed its execution.

```js
function outerFunction() {
  const outerVar = "I am from the outer function";

  function innerFunction() {
    console.log(outerVar); // innerFunction has access to outerVar
  }

  return innerFunction;
}

const myClosure = outerFunction();
myClosure(); // Outputs: "I am from the outer function"
```

Closures play a significant role in React, as they enable various essential features and patterns in React applications. Here are some of the significant ways in which closures are used in React:

- **State Management**:

  - React's state management relies on closures to maintain the state of a component.
  - When you use the `useState` Hook in a functional component, it returns an array with the current state value and a function to update that state. The state value is captured within a closure, allowing it to persist across re-renders.

  ```jsx
  const [count, setCount] = useState(0); // count is maintained via closure
  ```

- **Event Handlers**:

  - Event handlers in React often use closures to capture and remember the component's current state or props when an event handler is defined.
  - This ensures that the correct state or props are available when an event occurs, even if the event happens later when the component has re-rendered.

  ```jsx
  function MyComponent() {
    const [count, setCount] = useState(0);

    const handleClick = () => {
      setCount(count + 1); // count is captured via closure
    };

    return <button onClick={handleClick}>Increment</button>;
  }
  ```

- **Private Variables and Functions**:

  - Closures allow you to create private variables and functions within a component. This is valuable for encapsulating logic and data within a component without exposing them to the outside world.

  ```jsx
  function Counter() {
    let privateValue = 0; // private variable

    const increment = () => {
      privateValue++;
      console.log(privateValue);
    };

    return (
      <div>
        <button onClick={increment}>Increment</button>
      </div>
    );
  }
  ```

- **Custom Hooks**:

  - Custom Hooks, which are reusable functions in React, frequently utilize closures to maintain internal state and logic. This allows you to abstract complex behavior and reuse it across different components.

  ```jsx
  function useCounter(initialValue) {
    const [count, setCount] = useState(initialValue);

    const increment = () => {
      setCount(count + 1); // count captured via closure
    };

    return { count, increment };
  }
  ```

- **Memoization and Optimization**:
  - Closures can be used for memoization and performance optimization techniques in React. You can create functions that cache and reuse the results of expensive computations or calculations.

Closures are an integral part of React's component architecture and state management. They enable components to encapsulate their internal state, handle events effectively, and maintain data privacy, which are crucial aspects of building scalable and maintainable React applications.

### javascript for OOP

JavaScript is a multi-paradigm programming language, and it supports Object-Oriented Programming (OOP) concepts, including objects, classes (introduced in ES6), inheritance, encapsulation, polymorphism, and abstraction. While it has some differences from classical OOP languages, JavaScript's OOP features make it versatile for object-oriented coding.

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

- Pending:

  - When a Promise is created, it starts in the `Pending` state. This means that the asynchronous operation represented by the Promise is still in progress, and the final outcome (success or failure) is not yet determined.
  - During the pending stage, you can attach `.then()` and `.catch()` handlers to the Promise to specify what should happen when the Promise is fulfilled (resolved successfully) or rejected (encounters an error).

- Fulfilled (Resolved):

  - If the asynchronous operation succeeds, the Promise transitions to the `Fulfilled` state.
  - When the Promise is fulfilled, it means that the operation has completed successfully, and it holds a resolved value.
  - The `.then()` handler attached to the Promise will be called with the resolved value, allowing you to process the result of the operation.

- Rejected:
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

### Strict mode

Strict mode is a development mode feature that helps you write cleaner and more reliable code by catching and highlighting potential problems in your application. It is not specific to React but is a JavaScript feature that can be applied to React applications (as well as other JavaScript code).

The main benefits of using strict mode in React are as follows:

- Detecting Unsafe Lifecycles and Deprecated APIs: Strict mode helps you identify and address components with unsafe lifecycles (e.g., componentWillMount, componentWillUpdate, and componentWillReceiveProps) and deprecated APIs. It logs warnings to the console for these issues.

- Identifying Side-Effects: It helps identify side-effects in your render methods. For example, if a component's render method causes a mutation that is not part of rendering (e.g., modifying a global variable), strict mode will catch and warn you about it.

- Warns About Legacy String Refs: It warns you if you are using string refs (e.g., ref="myRef") instead of callback refs (e.g., ref={node => this.myRef = node}), which is recommended.

- Detecting Unexpected Side-Effects: Strict mode intentionally "double-invokes" certain functions, like constructor, render, and componentDidUpdate, to help detect side-effects. This can catch bugs that might not be apparent in non-strict mode.

- Improves Development Tool Warnings: It helps development tools like React DevTools to provide better error messages and warnings.

It's important to note that strict mode is intended for development use only. You should not use it in production as it can have a performance impact. When you build your application for production, strict mode-related code and checks are automatically stripped out.

### Rehydration error
In React, rehydration errors can occur when there is a mismatch between the initial server-rendered HTML content and the subsequent client-side rendering. These errors can disrupt the intended behavior of your application and lead to issues. Here are some common causes of rehydration errors in React:

1. **Component Mismatch**: If the component structure or hierarchy on the client-side differs from the server-rendered HTML, React might encounter problems when trying to reconcile the differences.

2. **Data Mismatch**: If the data used to render components on the client doesn't match the data used for server-side rendering, you can run into issues. This can happen if the data fetch or API calls on the client differ from those on the server.

3. **Inconsistent State**: If there's a discrepancy in the state of a component between the server and client, React might not be able to rehydrate the component correctly. This could be due to differences in component state or props.

4. **Misconfigured Routing**: Issues with client-side routing can lead to rehydration errors. If the server renders one route, but the client attempts to render a different route, problems can occur.

5. **Differences in Event Handlers**: If you have event handlers or other side effects that behave differently on the server and client, this can cause rehydration errors.

6. **Third-party Libraries**: Third-party libraries and components that are not designed with server-side rendering in mind can introduce rehydration problems.

7. **Async Operations**: If there are asynchronous operations that depend on client-side data and they don't execute in the same order as on the server, this can cause issues.

To avoid rehydration errors, it's essential to ensure consistency between the initial server-rendered HTML and the client-side rendering. This can involve using strategies like code splitting, ensuring that your data fetching is consistent, and handling client-side routing appropriately. Additionally, consider server-side rendering frameworks like Next.js that simplify this process and reduce the chances of rehydration errors.
