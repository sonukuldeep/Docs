---
author: internet
datetime: 2023-02-19
title: React using typescript
slug: react-typescript
featured: false
draft: false
tags:
  - react
  - typescript
ogImage: ""
description: Common react code using typescript
---

# React Typescript

## Table of Context

## useContext

```jsx
import { createContext, ReactNode, useContext, useEffect } from 'react'
import { useState } from 'react'

type UserProps = {
    username: string;
    id: string;
    iat?: number;
}

type ContextProps = {
    userInfo: UserProps | null
    setUserInfo: React.Dispatch<React.SetStateAction<UserProps | null>>
}

const UserContext = createContext<ContextProps>({ userInfo: null, setUserInfo: () => { } })

export const useUserContext = () => useContext(UserContext); // ex. const {userInfo, setUserInfo} = useUserContext()

export function UserContextProvider({ children }: { children: ReactNode }) { // wrap app.js with UserContextProvider
    const [userInfo, setUserInfo] = useState<UserProps | null>(null)

    return (
        <UserContext.Provider value={{ userInfo, setUserInfo }}>
            {children}
        </UserContext.Provider>)
}
```

## useState

```jsx
const [username, setUsername] = (useState < string) | (null > null);
```

## Get data from api

```jsx
import React, { useEffect, useState } from "react";

const fetchData = async () => {
  try {
    // Fetch data from API using async/await
    const response = await fetch("https://api.example.com/data");
    if (!response.ok) {
      throw new Error("Error fetching data");
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error("Error fetching data:", error);
    return null;
  }
};

const MyComponent = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    const getData = async () => {
      const fetchedData = await fetchData();
      if (fetchedData) {
        setData(fetchedData);
      }
    };
    getData();
  }, []);

  return (
    <div>
      <h1>My Component</h1>
      {/* Render the data in your component */}
      <ul>
        {data.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
};

export default MyComponent;
```

## UseRef

```jsx
import React, { useRef } from "react";

const MyComponent: React.FC = () => {
  // Declare a ref using useRef
  const inputRef = (useRef < HTMLInputElement) | (null > null);

  const handleButtonClick = () => {
    // Access the input element using the ref
    if (inputRef.current) {
      console.log("Input value:", inputRef.current.value);
    }
  };

  return (
    <div>
      <h1>My Component</h1>
      {/* Attach the ref to the input element */}
      <input ref={inputRef} type="text" />
      <button onClick={handleButtonClick}>Log Input Value</button>
    </div>
  );
};

export default MyComponent;
```
