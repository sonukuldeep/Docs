---
title: React-redux
author: Internet
datetime: 2023-05-23
slug: "react-redux"
featured: false
draft: false
tags:
  - react
  - redux
ogImage: ""
description: Redux is a predictable state container for JavaScript apps.
---

# React-redux

<iframe style="border: 1px solid rgba(0, 0, 0, 0.1);border-radius:2px;" width="800" height="450" src="https://codesandbox.io/p/sandbox/practical-bird-6cp5mw?embed=1" allowfullscreen></iframe>

## Table of Contents

## Installation

```js
npm i react react-redux

yarn add react react-redux
```

## 4 step Setup

Inside src folder create all the following files

### Add action

```js
// redux/action/index.js
export const incNumber = () => {
  return {
    type: "INCREMENT",
  };
};

export const decNumber = () => {
  return {
    type: "DECREMENT",
  };
};
```

### Add reducers

```js
// redux/reducers/upDown.js
const initialState = 0;

const changeTheNumber = (state = initialState, action) => {
  switch (action.type) {
    case "INCREMENT":
      return state + 1;
    case "DECREMENT":
      return state - 1;
    default:
      return state;
  }
};

export default changeTheNumber;

// redux/reducers/index.js
import { combineReducers } from "redux";
import changeTheNumber from "./upDown";

const rootReducer = combineReducers({
  changeTheNumber,
});

export default rootReducer;
```

### Optional hooks file

If using typescript only then this hooks file is needed else useDispatch and useSelector can be directly imported and used

```js
import store from "./store";
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";

type AppDispatch = typeof store.dispatch;
type RootState = ReturnType<typeof store.getState>;

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

### Add provider to main.js

```js
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";
import { Provider } from "react-redux";
import store from "./redux/store";

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

### And finally use

```js
//app.ts
import { useAppDispatch, useAppSelector } from "./redux/hooks";
import { incNumber, decNumber } from "./redux/action";

const count = useAppSelector(state => state.changeTheNumber);
const dispatch = useAppDispatch();

// dispatch(incNumber())
// dispatch(decNumber())
```

```js
//app.js
import { useDispatch, useSelector } from "react-redux";
import { incNumber, decNumber } from "./redux/action";

const count = useSelector(state => state.changeTheNumber);
const dispatch = useDispatch();

// dispatch(incNumber())
// dispatch(decNumber())
```
