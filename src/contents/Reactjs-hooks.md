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

function App() {
  return (
    <div className="App">
      <DataState>
        <h3>Hello</h3>
      </DataState>
    </div>
  );
}

export default App;
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
