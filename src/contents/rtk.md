---
title: RTK Query
author: Internet
datetime: 2023-05-23
slug: "react-toolkit"
featured: true
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

## Introduction video
[Rtk query](https://www.youtube.com/watch?v=u3KlatzB7GM&list=PL0Zuz27SZ-6M1J5I1w2-uZx36Qp6qhjKo)

## Table of Contents

## Installation

```js
# NPM
npm install @reduxjs/toolkit react-redux

# Yarn
yarn add @reduxjs/toolkit react-redux
```

## Basic usage

[Docs](https://redux-toolkit.js.org/tutorials/typescript)
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

### Step setup

#### Create slice file

```ts
//redux/slice/countSlice.ts
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

type CounterStateProps = {
  value: number;
};

const initialState: CounterStateProps = {
  value: 0,
};

const counterSlice = createSlice({
  name: "counter",
  initialState,
  reducers: {
    incremented(state) {
      state.value++;
    },
    // very important to pass PayloadAction type for typescript inference to work
    amountAdded(state, action: PayloadAction<number>) {
      state.value += action.payload;
    },
  },
});

export const { incremented, amountAdded } = counterSlice.actions;
export default counterSlice.reducer;
```

#### Create store<br/>

Create store.ts inside src folder<br/>
Team recommends only one store file per project although more than 1 is possible but not recommended

```ts
// store.ts
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "../redux/slice/counterSlice";

const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

export default store;

// important for type inference
export type AppDispatch = typeof store.dispatch;
export type RootState = ReturnType<typeof store.getState>;
```

#### Configure provider

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

#### Create Hooks file

```ts
// redux/hooks.ts
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import { AppDispatch, RootState } from "./store";

// useAppDispatch and useAppSelector setup helps typescript auto complete
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

#### Use toolkit

```jsx
// app.jsx
import { useAppDispatch, useAppSelector } from "./redux/hooks";
import { incremented, amountAdded } from "./redux/slice/counterSlice";
import "./App.css";

function App() {
  const value = useAppSelector(state => state.counter.value);
  const dispatch = useAppDispatch();

  function handleIncrement() {
    dispatch(incremented());
  }

  function addOnClick() {
    dispatch(amountAdded(2));
  }

  return (
    <>
      <h1>React Redux Toolkit</h1>
      <div className="card">
        <p>Count {value}</p>
        <button style={{ marginInline: "5px" }} onClick={handleIncrement}>
          Click to increment count
        </button>
        <button style={{ marginInline: "5px" }} onClick={addOnClick}>
          Click to increment by 2
        </button>
      </div>
    </>
  );
}

export default App;
```

<hr/>

## RTK Query

[Docs](https://redux.js.org/tutorials/essentials/part-7-rtk-query-basics)

### Basic use

```css
src
└── redux
    ├── store.ts
    ├── hooks.ts
    └── slice
        └── apiSlice.ts

```

#### Create Slice file

```js
// apiSlice.ts
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

const DOGS_API_KEY = 'cbfb51a2-84b6-4025-a3e2-ed8616edf311';

type Breed = {
    id: string;
    name: string;
    image: {
        url: string;
    };
}

const apiSlice = createApi({
    reducerPath: 'api',
    baseQuery: fetchBaseQuery({
        baseUrl: 'https://api.thedogapi.com/v1',
        prepareHeaders(headers) {
            headers.set('x-api-key', DOGS_API_KEY);
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

export default apiSlice
```

#### Include Slice file in store

```js
//store.ts
import { configureStore } from "@reduxjs/toolkit";
import apiSlice from "./slice/dogsApiSlice";

const store = configureStore({
  reducer: {
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: getDefaultMiddleware => {
    return getDefaultMiddleware().concat(apiSlice.middleware);
  },
});

export default store;
```

#### Export query hook

```ts
// hooks.ts
import apiSlice from "./slice/dogsApiSlice";

export const { useFetchBreedsQuery } = apiSlice;
```

#### Configure provider

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

#### Use it

```js
//app.tsx
import "./App.css";
import { useFetchBreedsQuery } from "./redux/hooks";

function App() {
  const { data, isLoading, isSuccess, isError, error } =
    useFetchBreedsQuery(20);

  if (isLoading) {
    return <h2>loading...</h2>;
  } else if (isSuccess) {
    return (
      <>
        <h2>Number of dogs fetched:- {data.length}</h2>
        <div
          style={{
            display: "grid",
            gridTemplateColumns: "repeat(3, 400px)",
            gap: "5px",
          }}
        >
          {data.map(breed => (
            <div
              key={breed.id}
              style={{ border: "2px solid white", borderRadius: "4px" }}
            >
              <h2>{breed.name}</h2>
              <img
                style={{ width: "100%", height: "300px", objectFit: "cover" }}
                src={breed.image.url}
                alt=""
                loading="lazy"
              />
            </div>
          ))}
        </div>
      </>
    );
  } else if (isError) {
    return <div>Error occured {JSON.stringify(error)}</div>;
  } else {
    <h2>Hello there</h2>;
  }
}
export default App;
```

<hr/>

## Async thunk

### Basic fetch post

```css
src
└── redux
    ├── store.ts
    ├── hooks.ts
    └── slice
        └── postSlice.ts

```

#### Slice

```ts
// postSlice.ts
import { createSlice, createAsyncThunk } from "@reduxjs/toolkit";
import axios from "axios";

const POSTS_URL = "https://rickandmortyapi.com/api/character";

export enum LoadingStatus {
  IDLE = "idle",
  LOADING = "loading",
  SUCCEEDED = "succeeded",
  FAILED = "failed",
}

type StateProps = {
  posts: PostProps[]; // structure of data
  status: LoadingStatus;
  error: string | undefined;
};

const initialState: StateProps = {
  posts: [],
  status: LoadingStatus.IDLE, // idle | loading | succeeded | failed
  error: "",
};

export const fetchPosts = createAsyncThunk("posts/fetchPosts", async () => {
  // try catch block is not needed here
  // createAsyncThunk will handle errors here internally
  const response = await axios.get(POSTS_URL);
  return response.data.results;
});

const postsSlice = createSlice({
  name: "posts",
  initialState,
  reducers: {},
  extraReducers(builder) {
    builder
      .addCase(fetchPosts.pending, state => {
        state.status = LoadingStatus.LOADING;
      })
      .addCase(fetchPosts.fulfilled, (state, action) => {
        state.status = LoadingStatus.SUCCEEDED;
        state.posts = action.payload;
      })
      .addCase(fetchPosts.rejected, (state, action) => {
        state.status = LoadingStatus.FAILED;
        state.error = action.error.message;
      });
  },
});

export default postsSlice.reducer;
```

#### store

```ts
// store.ts
import { configureStore } from "@reduxjs/toolkit";
import postReducer from "../redux/slice/postSlice";

export const store = configureStore({
  reducer: {
    posts: postReducer,
  },
});

// important for type inference
export type AppDispatch = typeof store.dispatch;
export type RootState = ReturnType<typeof store.getState>;
```

#### Hooks

```ts
// hooks.ts
// redux/hooks.ts
import { TypedUseSelectorHook, useDispatch, useSelector } from "react-redux";
import { AppDispatch, RootState } from "./store";

// useAppDispatch and useAppSelector setup helps typescript auto complete
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

#### Use

```ts
// App.tsx
import { fetchPosts, LoadingStatus } from "./redux/slice/postSlice";
import { useEffect } from "react";
import { useAppDispatch, useAppSelector } from "./redux/hooks";

export default function App() {
  const { status, error, posts } = useAppSelector(state => state.posts);
  const dispatch = useAppDispatch();

  useEffect(() => {
    if (status === LoadingStatus.IDLE) {
      // fetch all post on component mount
      dispatch(fetchPosts());
    }
  }, []);

  if (status === LoadingStatus.LOADING) return <h2>Loading...</h2>;
  else if (status === LoadingStatus.SUCCEEDED)
    return (
      <div>
        <h1>Hello world</h1>
        {posts.map(post => (
          <div key={post.id} style={{ border: "2px solid #fff" }}>
            <h2>{post.name}</h2>
            <p>{post.gender}</p>
            <img src={post.image} loading="lazy" />
          </div>
        ))}
      </div>
    );
  else if (status === LoadingStatus.FAILED) return <h2>{error}</h2>;
}
```


## ApiSlice setup
```css
src
└── redux
    ├── store.js
    ├── hooks.js
    └── slice
        └── postSlice.js

```

store.js
```js
//store.ts
import { configureStore } from "@reduxjs/toolkit";
import { apiSlice } from './slice/apiSlice';

const store = configureStore({
  reducer: {
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  middleware: getDefaultMiddleware => {
    return getDefaultMiddleware().concat(apiSlice.middleware);
  },
});

export default store;
````

hooks.js
```js
// hooks.ts
import { apiSlice } from "./slice/apiSlice";

export const { useGetPostsQuery, useAddPostsMutation, useUpdatePostsMutation, useDeletePostsMutation } = apiSlice;
```

apiSlice.js
```jsx
// apiSlice.js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";
export const apiSlice = createApi({
  reducerPath: "api",
  baseQuery: fetchBaseQuery({ baseUrl: "https://653c7e46d5d6790f5ec805f4.mockapi.io/" }),
  tagTypes: ['Posts'],
  endpoints: (builder) => ({
    getPosts: builder.query({
      query: () => "/posts",
      providesTags: ['Posts']
    }),
    addPosts: builder.mutation({
      query: (todo) => ({
        url: "/posts",
        method: "POST",
        body: todo
      }),
      invalidatesTags: ["Posts"]
    }),
    updatePosts: builder.mutation({
      query: (todo) => ({
        url: `/posts/${todo.id}`,
        method: "PUT",
        body: todo
      }),
      invalidatesTags: ["Posts"]
    }),
    deletePosts: builder.mutation({
      query: ({ id }) => ({
        url: `/posts/${id}`,
        method: "DELETE",
        body: id
      }),
      invalidatesTags: ["Posts"]
    }),
  })
});
```

index.js
```jsx
// index.js
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'
import './index.css'
import { Provider } from 'react-redux'
import store from './app/redux/store'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
)
```
