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

## Intersection observer

animate using animate.css

```jsx
Copy code
'use client';

import { InView } from "react-intersection-observer";

export default function IsObserver({ children, classNameInView, classNameNotInView="", triggerOnce={true}, threshold={0}}: { children: React.ReactNode, classNameInView: string, classNameNotInView: string, triggerOnce: bool, threshold: number }) {
    return (
        <InView triggerOnce={triggerOnce} threshold={threshold} >
            {({ inView, ref, entry }) => (
                <div ref={ref} className={inView ? classNameInView : classNameNotInView}>
                    {children}
                </div>
            )}
        </InView >
    );
}
```
