---
title: Redux
author: Internet
datetime: 2023-05-23
slug: "react-redux-and-react-toolkit"
featured: false
draft: false
tags:
  - react
  - redux
  - redux-toolkit
ogImage: ""
description: Redux is a predictable state container for JavaScript apps.
---

# Redux using react toolkit

![main image](https://wsrv.nl/?url=redux.js.org/img/redux-logo-landscape.png&w=600)

## Table of Contents

## Installation

```js
# NPM
npm install @reduxjs/toolkit react-redux

# Yarn
yarn add @reduxjs/toolkit react-redux
```

## Sample code

1. Create store
   Create store.ts inside src folder

```ts
import { configureStore, createSlice } from "@reduxjs/toolkit";

const initialState = { value: 0 };
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

export const { increment, decrement } = countSlice.actions;
const store = configureStore({
  reducer: {
    count: countSlice.reducer,
  },
});

export default store;
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
import { decrement, increment } from "./store";
import { useDispatch, useSelector } from "react-redux";

type StateProps = {
  count: {
    value: number,
  },
};

function App() {
  const dispatch = useDispatch();
  const username = useSelector((state: StateProps) => state.count.value);

  return (
    <div className="App">
      <h1>{username}</h1>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrment</button>
    </div>
  );
}

export default App;
```
