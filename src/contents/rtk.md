---
title: RTK
author: Internet
datetime: 2023-05-23
slug: "react-toolkit"
featured: false
draft: false
tags:
  - react
  - redux
  - redux-toolkit
ogImage: ""
description: Redux Toolkit is our official standard approach for writing Redux logic.
---

# Redux Toolkit

![main image](https://wsrv.nl/?url=hybridheroes.de/blog/content/images/size/w2000/2022/03/redux-toolkit-1400.jpg&w=600)

## Table of Contents

## Installation

```js
# NPM
npm install @reduxjs/toolkit react-redux

# Yarn
yarn add @reduxjs/toolkit react-redux
```

## Basic usage

### 3 Step setup

1. Create store
   Create store.ts inside src folder

```ts
import { configureStore, createSlice } from "@reduxjs/toolkit";
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";

const initialState = { value: 0 };
// put this in a seperate file and import it
const countSlice = createSlice({
  name: "count",
  initialState,
  reducers: {
    increment: state => {
      state.value = state.value + 1;
    },
    decrement: state => {
      state.value = state.value - 1;
    },
  },
});

const store = configureStore({
  reducer: {
    count: countSlice.reducer,
  },
});

export default store;
export const { increment, decrement } = countSlice.actions;

type AppDispatch = typeof store.dispatch;
type RootState = ReturnType<typeof store.getState>;

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

2. Configure provider

Configure provider and pass store.ts to it in main.tsx

```ts
import React from "react";
import ReactDOM from "react-dom/client";
import { Provider } from "react-redux";
import App from "./App";
import "./index.css";
import store from "./store";

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

3. Use toolkit

```jsx
import "./App.css";
import { decrement, increment, useAppDispatch, useAppSelector } from "./store";

function App() {
  const dispatch = useAppDispatch();
  const count = useAppSelector(state => state.count.value);

  return (
    <div className="App">
      <h1>{count}</h1>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrment</button>
    </div>
  );
}

export default App;
```
