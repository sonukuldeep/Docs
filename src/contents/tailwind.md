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

## Theming in React/Nextjs

[Link to repo](https://github.com/TomIsLoading/tailwind-themeing)

### Important files

```css
src
│── App.jsx
│── styles
│   ├── global.css
│   └── utils.css
└── tailwind.config.js
```

```tsx
// App.jsx/layout.tsx
import "@/styles/globals.css";
import type { AppProps } from "next/app";
import { useEffect } from "react";

export default function App({ Component, pageProps }: AppProps) {
  useEffect(() => {
    // NOTE: This should be set based on some kind of toggle or theme selector.
    // I've added this here for demonstration purposes
    localStorage.setItem("theme", "light");

    // If the user has selected a theme, use that
    const selectedTheme = localStorage.getItem("theme");

    if (selectedTheme) {
      document.body.classList.add(selectedTheme);

      // Else if the users OS preferences prefers dark mode
    } else if (window.matchMedia("(prefers-color-scheme: dark)").matches) {
      document.body.classList.add("dark");

      // Else use light mode
    } else {
      document.body.classList.add("light");
    }
  }, []);

  return <Component {...pageProps} />;
}
```

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

.light {
  --background: 245, 245, 245;

  --border: 212, 212, 212;
  --card: 255, 255, 255;

  --copy-primary: 23, 23, 23;
  --copy-secondary: 115, 115, 115;

  --cta: 139, 92, 246;
  --cta-active: 124, 58, 237;
  --cta-text: 255, 255, 255;

  background: rgba(var(--background));
}

.dark {
  --background: 0, 0, 0;

  --border: 38, 38, 38;
  --card: 23, 23, 23;

  --copy-primary: 250, 250, 250;
  --copy-secondary: 115, 115, 115;

  --cta: 99, 102, 241;
  --cta-active: 79, 70, 229;
  --cta-text: 255, 255, 255;

  background: rgba(var(--background));
}

.red {
  --background: 254, 202, 202;

  --border: 185, 28, 28;
  --card: 239, 68, 68;

  --copy-primary: 250, 250, 250;
  --copy-secondary: 254, 226, 226;

  --cta: 250, 250, 250;
  --cta-active: 254, 226, 226;
  --cta-text: 239, 68, 68;

  background: rgba(var(--background));
}

:root {
  --grape: 114, 35, 204;
}

/* :root {
  --background: 245, 245, 245;

  --border: 212, 212, 212;
  --card: 255, 255, 255;

  --copy-primary: 23, 23, 23;
  --copy-secondary: 115, 115, 115;

  --cta: 139, 92, 246;
  --cta-active: 124, 58, 237;
  --cta-text: 255, 255, 255;
}

body {
  background: rgba(var(--background));
}

@media (prefers-color-scheme: dark) {
  :root {
    --background: 0, 0, 0;

    --border: 38, 38, 38;
    --card: 23, 23, 23;

    --copy-primary: 250, 250, 250;
    --copy-secondary: 115, 115, 115;

    --cta: 99, 102, 241;
    --cta-active: 79, 70, 229;
    --cta-text: 255, 255, 255;
  }
} */
```

```ts
// tailwind.config.ts
import type { Config } from "tailwindcss";

const config: Config = {
  content: [
    "./src/pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/components/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/app/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        background: "rgba(var(--background))",
        border: "rgba(var(--border))",
        card: "rgba(var(--card))",
        "copy-primary": "rgba(var(--copy-primary))",
        "copy-secondary": "rgba(var(--copy-secondary))",
        cta: "rgba(var(--cta))",
        "cta-active": "rgba(var(--cta-active))",
        "cta-text": "rgba(var(--cta-text))",

        grape: "rgba(var(--grape))",
      },
    },
  },
  plugins: [],
};
export default config;
```
