---
title: RTK Query
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

<iframe
  src="https://codesandbox.io/p/sandbox/react-toolkit-kq3g1d"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
  sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

## Table of Contents

## Installation

```js
# NPM
npm install @reduxjs/toolkit react-redux

# Yarn
yarn add @reduxjs/toolkit react-redux
```

## Basic usage

Barebone example<br/>
Document structure

```css
└── src
    └── redux
        ├── slice
        │   └── countSlice.ts
        ├── hooks.ts
        └── store.ts
```

### 4 Step setup

1. Create slice file

```ts
//redux/slice/countSlice.ts
import { createSlice } from "@reduxjs/toolkit";

const initialState = { value: 0 };

export const countSlice = createSlice({
  name: "count",
  initialState,
  reducers: {
    increment: state => {
      state.value = state.value + 1;
    },
    decrement: state => {
      state.value = state.value - 1;
    },
    incrementByAmount: (state, action) => {
      state.value = action.payload;
    },
  },
});

export default countSlice;

export const { increment, decrement, incrementByAmount } = countSlice.actions;
```

1. Create store<br/>
   Create store.ts inside src folder<br/>
   Team recommends only one store file per project although more than 1 is possible

```ts
// store.ts
import { configureStore } from "@reduxjs/toolkit";
import { apiSlice } from "./slice/dogsSlice";
import countSlice from "./slice/countSlice";

const store = configureStore({
  reducer: {
    count: countSlice.reducer,
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: getDefaultMiddleware => {
    return getDefaultMiddleware().concat(apiSlice.middleware);
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
import store from "./redux/store";

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

1. Create Hooks file

```ts
// redux/hooks.ts
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import store from "./store";
import { apiSlice } from "./slice/dogsSlice";

type AppDispatch = typeof store.dispatch;
type RootState = ReturnType<typeof store.getState>;

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;

export const { useFetchBreedsQuery } = apiSlice;
```

3. Use toolkit

```jsx
// app.jsx
import "./App.css";
import { useState } from "react";
import {
  useAppDispatch,
  useAppSelector,
  useFetchBreedsQuery,
} from "./redux/hooks";
import {
  decrement,
  increment,
  incrementByAmount,
} from "./redux/slice/countSlice";

function App() {
  const dispatch = useAppDispatch();
  const count = useAppSelector(state => state.count.value);
  const [num, setNum] = useState(0);
  const { data, isSuccess, isLoading, isError } = useFetchBreedsQuery();

  if (isLoading) return <div>loading animation</div>;

  if (isError) return <div>Got and error when fetching data</div>;

  if (isSuccess)
    return (
      <div className="App">
        <h1>{count}</h1>
        <button onClick={() => dispatch(increment())}>Increment</button>
        <button onClick={() => dispatch(decrement())}>Decrment</button>
        <div>
          <input
            type="text"
            value={num}
            onChange={e => setNum(Number(e.target.value) || 0)}
          />
          <button onClick={() => dispatch(incrementByAmount(num))}>
            Change
          </button>
        </div>

        <div style={{ marginTop: "50px" }}>
          <p style={{ textTransform: "uppercase" }}>
            No of dogs fetched {data.length}
          </p>
          {data.map(item => (
            <>
              <img
                loading="lazy"
                src={item.image.url}
                alt={item.name}
                style={{ width: "600px" }}
              />
              <p>{item.name}</p>
            </>
          ))}
        </div>
      </div>
    );
}

export default App;
```

## RTK Query

[Docs](https://redux.js.org/tutorials/essentials/part-7-rtk-query-basics)

### Basic use

1. Create Slice file

```js
// dogs-slice.ts
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

const DOGS_API_KEY = "cbfb51a2-84b6-4025-a3e2-ed8616edf311";

interface Breed {
  id: string;
  name: string;
  image: {
    url: string;
  };
}

export const apiSlice = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({
    baseUrl: "https://api.thedogapi.com/v1",
    prepareHeaders(headers) {
      headers.set("x-api-key", DOGS_API_KEY);
      return headers;
    },
  }),
  endpoints(builder) {
    return {
      fetchBreeds: builder.query<Breed[], number | void>({
        query(limit = 10) {
          return `/breeds?limit=${limit}`;
        },
      }),
    };
  },
});
```

2. Include Slice file in store and export query hook

```js
//store.ts
import { configureStore } from "@reduxjs/toolkit";
import { apiSlice } from "./dogs-slice";

const store = configureStore({
  reducer: {
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: getDefaultMiddleware => {
    return getDefaultMiddleware().concat(apiSlice.middleware);
  },
});

export default store;

export const { useFetchBreedsQuery } = apiSlice;
```

3. Configure provider
   Configure provider and pass store.ts to it in main.tsx

```ts
// main.ts
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

4. Use it

```js
//app.tsx
import "./App.css";
import { useFetchBreedsQuery } from "./store";

function App() {
  const { data, isSuccess, isLoading, isError, error } = useFetchBreedsQuery();

  if (isLoading) return <div>loading animation</div>;

  if (isError) return <div>Got an error: {error.toString()}</div>;

  if (isSuccess)
    return (
      <div className="App">
        <div style={{ marginTop: "50x" }}>
          <p>No of dogs fetched {data.length}</p>
          {data.map(item => (
            <>
              <img
                loading="lazy"
                src={item.image.url}
                alt={item.name}
                style={{ width: "600px" }}
              />
              <p>{item.name}</p>
            </>
          ))}
        </div>
      </div>
    );
}

export default App;
```
