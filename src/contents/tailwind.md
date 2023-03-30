---
author: kuldeep
datetime: 2023-03-30
title: Tailwind components
slug: tailwind-components
featured: true
draft: false
tags:
  - tailwind
  - react
ogImage: ""
description: A collection of Tailwind components for fast production
---

# Tailwind Components

## Table of Contents

## Navbar

### Simple Navbar with animated hamburger icon

![navbar1](/assets/navbar1.png)
![navbar1](/assets/navbar1.1.png)

External package used: hamburger-react

```jsx
npm i hamburger-react
```

Btn component

```jsx
import React from 'react'

type BtnProps = {
    variant: number;
    href: string
    children: React.ReactNode;
}

const btnStyles = [
  'border-2 rounded px-6 py-1 hover:text-gray-950 hover:border-gray-950/60 transition',
  'border-2 rounded px-6 py-2 text-white',
]

const Btn = ({ variant, href, children }: BtnProps) => {
  const style = btnStyles[variant]
  return (
    <a className={style} href={href}>{children}</a>
  )
}

export default Btn
```

Navbar

```jsx
import { useState } from 'react'
import Hamburger from 'hamburger-react'
import Btn from './Btn'

enum BtnType {
    NavbarLong = 0,
    NavbarShort,
}

const Navbar = () => {
    const [isOpen, setOpen] = useState(false)
    return (
        <nav className='max-w-7xl mx-auto flex items-center justify-between px-4'>
            <a href='#'>
                Logo
            </a>
            <div className="hidden md:flex space-x-20 font-roboto text-gray-700/70">
                <Btn variant={BtnType.NavbarLong} href={'#'}>Blog</Btn>
                <Btn variant={BtnType.NavbarLong} href={'#'}>Contacts</Btn>
                <Btn variant={BtnType.NavbarLong} href={'#'}>About</Btn>
                <Btn variant={BtnType.NavbarLong} href={'#'}>Login</Btn>

            </div>
            <div className="md:hidden relative"><Hamburger toggled={isOpen} toggle={setOpen} rounded />
                <div className={`${isOpen ? 'max-h-[300px] py-2' : 'max-h-0 overflow-hidden py-0'} transition-all ease-in-out duration-200 flex flex-col font-roboto text-2xl text-gray-700 absolute -right-4 space-y-3 px-8 bg-gradient-to-t from-indigo-500 to-cyan-500 w-screen`}>
                <Btn variant={BtnType.NavbarShort} href={'#'}>Blog</Btn>
                <Btn variant={BtnType.NavbarShort} href={'#'}>Contacts</Btn>
                <Btn variant={BtnType.NavbarShort} href={'#'}>About</Btn>
                <Btn variant={BtnType.NavbarShort} href={'#'}>Login</Btn>
                </div>
            </div>
        </nav>
    )
}

export default Navbar
```
