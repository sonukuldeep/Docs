---
author: Kuldeep
datetime: 2022-11-26
title: Reactjs hooks
slug: reactjs-hooks
featured: true
draft: false
tags:
  - docs
  - reactjs
ogImage: "/assets/react.png"
description: A blog post for all things related to react hooks.
---

## Table of contents

## useContext

### Example:

_DataContext.js_

```jsx
import { createContext } from "react";

const DataContext = createContext();

export default DataContext;
```

_DataState.js_

```jsx
import React, { useState } from "react";
import DataContext from "./DataContext";

const DataState = props => {
  const [btnState, setBtnState] = useState(false);

  return (
    <DataContext.Provider value={{ btnState, setBtnState }}>
      {props.children}
    </DataContext.Provider>
  );
};

export default DataState;
```

_App.js_

```jsx
import "./App.scss";
import DataState from "./Components/DataState";
import Example from "./ExampleComponent"

function App() {
  return (
    <div className="App">
      <DataState>
        <h3>Hello</h3>
        <Example>
      </DataState>
    </div>
  );
}

export default App;
```

Then in Example.js component where you have to use context

```jsx
import React, { useContext } from "react";
import DataContext from "./DataContext";

const Example = props => {
  const {btnState, setBtnState} = useContext(DataContext);

  return (
    <div>
      <button onClick={()={setBtnState(!btnState)}}>Click me</button>
    </div>
  );
};

export default Example;
```

## useReducer

### example:

_ReducerHelper.js_

```jsx
import { useReducer } from "react";
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case "increment":
      return { count: state.count + 1 };
    case "decrement":
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

const ReducerHelper = () => {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: "increment" })}>+</button>
      <button onClick={() => dispatch({ type: "decrement" })}>-</button>
    </>
  );
};

export default ReducerHelper;
```

## useMemo

### example:

_MemoHelper.js_

```jsx
import { useMemo, useState } from "react";

const delayFunction = dayOrNight => {
  const start = Date.now();
  let i = 0;
  while (i < 10000) {
    i += 0.0001;
  }
  const end = Date.now();
  console.log(`Execution time: ${end - start} ms`);

  if (dayOrNight === "") return "";
  if (dayOrNight === "morning") return "Good morning";
  else return "Just sleep";
};

const MemoHelper = () => {
  const [dayOrNight, setDayOrNight] = useState("");
  const [theme, setTheme] = useState(false);
  // const greeting = delayFunction(dayOrNight) //toggle to see effect of having an expensize function run without useMemo
  const greeting = useMemo(() => {
    return delayFunction(dayOrNight);
  }, [dayOrNight]);

  return (
    <div>
      <fieldset>
        <legend style={theme ? { color: "green" } : { color: "red" }}>
          Type something and check delay in console?
        </legend>
        <input
          type="text"
          value={dayOrNight}
          onChange={e => setDayOrNight(e.target.value)}
        />
      </fieldset>
      <div style={theme ? { color: "green" } : { color: "red" }}>
        <p>Greeting:- {greeting}</p>
      </div>
      <button onClick={() => setTheme(!theme)}>Change Color</button>
      <p>
        Buttton click causes re-render of component but delayFunction only runs
        if dependency changes
      </p>
    </div>
  );
};

export default MemoHelper;
```

## useCallback

### example:

_CallbackParent.js_

```jsx
import { useCallback, useState } from "react";
import CallbackChild from "./CallbackChild";

const CallbackParent = () => {
  const [number, setNumber] = useState(0);
  const [dark, setDark] = useState(false);
  console.log("Parent Component redered");

  const handler = useCallback(() => {
    return [number, number + 1, number + 2];
  }, [number]); //api function call
  //const handler = ()=> { return [number, number + 1, number + 2]} //toggle this api* function to see it run on every render on parent without useCallback hook
  const theme = {
    backgroundColor: dark ? "#333" : "#fff",
    color: dark ? "#fff" : "#333",
  };

  return (
    <>
      <div style={theme}>
        <input
          type="number"
          value={number}
          onChange={e => setNumber(parseInt(e.target.value))}
        />
        <button onClick={() => setDark(prevDark => !prevDark)}>
          Toggle theme
        </button>
        <CallbackChild handler={handler} />
      </div>
    </>
  );
};

export default CallbackParent;
```

_CallbackChild.js_

```jsx
import { useEffect, useState } from "react";

const CallbackChild = ({ handler }) => {
  const [items, setItems] = useState([]);

  useEffect(() => {
    setItems(handler());
    console.log("Child Component redered");
  }, [handler]);

  return (
    <>
      {items.map((item, index) => {
        return <div key={index}>{item}</div>;
      })}
    </>
  );
};

export default CallbackChild;
```

### Note:

_Remember useMemo and useCallback both take advantage of Referential Equality and they do have benifits & related side effects. Therefore, use them only when their use causes significant performence gain. useMemo return result, whereas useCallback returns a callback function which can take an argument too, passed from the child component. Both have similar syntax._

<hr>

## useNavigate

### Install

useNavigate is part of react-router-dom hence we need to install that in order to use this hook

```npx
npm i react-router-dom
```

### Usage

```jsx
import { useNavigate } from "react-router-dom";

export default app = () => {
  const navigate = useNavigate();

  useEffect(() => {
    setTimeout(() => {
      // 👇 Redirects to home page, note the `replace: true`
      navigate("/", { replace: true });
    }, 3000);
  }, []);

  return <p>redirect home in 3 secs</p>;
};
```
