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
