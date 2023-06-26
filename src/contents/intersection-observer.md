---
author: internet
datetime: 2023-04-07
title: Intersection observer
slug: intersection-observer
featured: false
draft: false
tags:
  - react
  - typescript
  - javascript
  - nextjs
ogImage: ""
description: Intersection observer implementation
---

# Intersection observer

## Table of Context

## Intersection observer in Nextjs

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

[link](https://www.franciscomoretti.com/how-to-animate-on-scroll-with-react-intersection-observer-and-tailwind-in-a-nextjs-app)

<hr>

## Intersection observer in javascript

[mdn](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)
[My pen](https://codepen.io/Donald-6329/pen/vYVEBQj)

```jsx
function buildThresholdList(numOfSteps) {
  let thresholds = [];

  for (let i = 1.0; i <= numOfSteps; i++) {
    let ratio = i / numOfSteps;
    thresholds.push(ratio);
  }

  thresholds.push(0);
  return thresholds;
}

// getting cards to listen to
const cards = document.querySelectorAll(".card");

// Set things up
window.addEventListener(
  "load",
  event => {
    createObserver();
  },
  false
);

function createObserver() {
  let observer;

  let options = {
    root: null,
    rootMargin: "0px",
    // numberOfSteps = 10 here
    threshold: buildThresholdList(10),
  };

  observer = new IntersectionObserver(handleIntersect, options);

  // pass objects to observe
  cards.forEach(card => {
    observer.observe(card);
  });
}

function handleIntersect(entries, observer) {
  entries.forEach(entry => {
    // the condition that needs changing
    entry.target.classList.toggle("show", entry.isIntersecting);
    entry.target.style.opacity = entry.intersectionRatio.toFixed(2);
    console.log(entry.intersectionRatio.toFixed(2));

    // run once
    // if (entry.isIntersecting)
    //     observer.unobserve(entry.target)
  });
}
```

```html
<div class="card-container">
  <div class="card show"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
  <div class="card"></div>
</div>
```

```css
.card-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  align-items: center;
}

.card {
  background: white;
  border: 1px solid black;
  border-radius: 0.25rem;
  transform: translateX(100px);
  opacity: 0;
  transition: 300ms;
  min-height: 200px;
  min-width: 200px;
  background-image: url("https://source.unsplash.com/200x200");
}

.card.show {
  transform: translateX(0);
  opacity: 1;
}
```

## Alternative. Using scroll listner

script.js

```js
// getting cards to listen to
const cards = document.querySelectorAll(".animate-on-scroll");

const windowHeight = window.innerHeight; // gets viewport height

const animateProperty = {
  scale: "--scale",
  ease: "--easeIn",
  scroll: "--scroll",
};

function animateFunction(element, property, delay = 1, propertyVariable = "") {
  const top = element.getBoundingClientRect().top; // gets element top relative to view port
  const topFactor = delay; // lower value to delay animation and vice versa
  const range = topFactor * windowHeight - top; // if window - top is + then object is in view
  const step = range / windowHeight; // responsible for a steady increment/decrement from 0-1 during scroll. nothing else
  if (range > 0 && range <= windowHeight) {
    // animate only when object is in view and in limit
    document.body.style.setProperty(property + propertyVariable, step); //sets css variable that can be used for animation
  }
}

window.addEventListener(
  "scroll",
  () => {
    animateFunction(cards[0], animateProperty.ease, 1, (propertyVariable = 1));
    animateFunction(cards[10], animateProperty.scale, 0.7);
    animateFunction(cards[14], animateProperty.scroll, 0.5);
  },
  false
);
```

css file

```css
.animate-on-scroll.scale {
  animation: scale 1s linear infinite;
  animation-play-state: paused;
  animation-delay: calc(var(--scale) * -1s);

  animation-iteration-count: 1;
  animation-fill-mode: both;
}

@keyframes scale {
  0% {
    opacity: 0.5;
    transform: scale(1, 1);
  }

  60% {
    opacity: 1;
    transform: scale(1.4, 1.4);
  }

  100% {
    opacity: 1;
    transform: scale(1.4, 1.4);
  }
}

.animate-on-scroll.scroll {
  animation: rotate 1s linear infinite;
  animation-play-state: paused;
  animation-delay: calc(var(--scroll) * -1s);

  animation-iteration-count: 1;
  animation-fill-mode: both;
}

@keyframes rotate {
  to {
    transform: translateX(-35vw);
  }
}

.animate-on-scroll.easeIn1 {
  animation: easeIn 1s linear infinite;
  animation-play-state: paused;
  animation-delay: calc(var(--easeIn1) * -1s);

  animation-iteration-count: 1;
  animation-fill-mode: both;
}

@keyframes easeIn {
  0% {
    opacity: 0;
    transform: translateY(100px);
  }

  60% {
    opacity: 1;
    transform: translateY(0px);
  }

  100% {
    opacity: 1;
    transform: translateY(0px);
  }
}
```

## Using scroll listner change background color

Create a css variable and add this generic style

```css
:root {
  --nav-bg: rgba(255, 255, 255, 0);
}
/* changing nav bar color dynamically */
.bg-dynamic {
  background-color: var(--nav-bg);
}
```

Add this to react componet

```tsx
import { useEffect, useRef } from "react";

export default function ScrollAnimation() {
  const nav = useRef<HTMLDivElement>(null);
  const root = document.documentElement;

  const animateProperty = {
    color: "--nav-bg",
  };

  function animateFunction(element: HTMLDivElement, property: string) {
    const top = element.getBoundingClientRect().top;
    const color = "rgba(255, 255, 255, " + Alpha(top) + ")";
    root.style.setProperty(property, color);
  }

  function Alpha(top: number) {
    let range;
    range = top >= 0 ? 0 : top >= -400 ? top : -400;
    const output = ((range * -1) / 450).toFixed(2);
    return output.toString();
  }

  useEffect(() => {
    if (nav.current) {
      const animatedDiv = nav.current!;
      window.addEventListener(
        "scroll",
        () => {
          animateFunction(animatedDiv, animateProperty.color);
        },
        false
      );

      return () => {
        window.removeEventListener("scroll", () => {
          animateFunction(animatedDiv, animateProperty.color);
        });
      };
    }
  }, []);

  return (
    <div ref={nav}>
      <h1>Hello world</h1>
    </div>
  );
}
```
