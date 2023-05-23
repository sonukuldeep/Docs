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

## RTK Query

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
  const { data, isSuccess, isLoading, isError } = useFetchBreedsQuery();

  if (isLoading) return <div>loading animation</div>;

  if (isError) return <div>Got and error when fetching data</div>;

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
