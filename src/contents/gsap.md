---
author: kuldeep
datetime: 2023-09-01
title: Gsap
slug: "gsap"
featured: true
draft: false
tags:
  - gsap animation
ogImage: ""
description: GSAP, or the GreenSock Animation Platform, is a popular and powerful JavaScript library used for creating smooth and engaging animations on websites and web applications.
---

# Gsap

GSAP, or the GreenSock Animation Platform, is a popular and powerful JavaScript library used for creating smooth and engaging animations on websites and web applications. It provides a comprehensive set of tools and features for animating HTML elements, SVG graphics, and more. GSAP offers precise control over animations, including properties like position, rotation, scale, and opacity. It is known for its high performance and cross-browser compatibility, making it a preferred choice among web developers for creating interactive and visually appealing user experiences. GSAP's intuitive syntax and rich documentation make it accessible for both beginners and experienced developers looking to enhance their web projects with dynamic animations.

- A [github](https://github.com/sonukuldeep/Gsap_and_plugins/tree/main) repo is dedicated to github. Each branch in this repo is a chapter.
- [Codepen](https://codepen.io/Donald-6329/pen/NWEOwQN) containing all my experiments with gsap
- [Codepen 2](https://codepen.io/Donald-6329/pen/XWyxZPb) card animation with gsap

## How to get gsap autocomplete to work in vs code?

- Copy types directory from gsap [github](https://github.com/greensock/GSAP) to the root of your project and rename it to types
- create an empty `jsconfig.json` file in root directory.
- vc code will have auto complete now

## Some common gsap usage

### Gsap to

```js
gsap.to("#animated-element", {
  duration: 2,
  x: 200,
  y: 200,
  z: 200,
  rotation: 360,
  backgroundColor: "blue",
  scale: 1.5,
  scaleX: 1.5, // 3D scaling in the X direction
  scaleY: 0.5, // 3D scaling in the Y direction
  scaleZ: 2, // 3D scaling in the Z direction
  opacity: 0.5,
  borderRadius: "50%",
  ease: "bounce.out",
  yoyo: true, // Animation will play forward and backward
  repeat: -1, // Infinite animation loop
  delay: 1, // Delay the animation start by 1 second
  skewX: 45, // Skew the element by 45 degrees on the X-axis
  onStart: () => {
    // Callback function when animation starts
    console.log("Animation started!");
  },
  onComplete: () => {
    // Callback function when animation completes
    console.log("Animation completed!");
  },
});
```

### Gsap from

```js
gsap.from(".header", { duration: 1, y: "-100%", ease: "bounce" });

gsap.from(".link", { duration: 1, opacity: 0, delay: 1, stagger: 0.5 });

gsap.from(".right", { duraton: 1, x: "-100vw", delay: 1 });

gsap.from(".left", { duraton: 1, x: "-100%", delay: 1.5 });

gsap.fromTo(
  ".button",
  { opacity: 0, scale: 0, rotation: 720 },
  { duration: 1, delay: 3.5, opacity: 1, scale: 1, rotation: 0 }
);
```

### Scroll trigger

```js
gsap.registerPlugin(ScrollTrigger);

gsap.to(".square", {
  x: 700,
  duration: 3,
  scrollTrigger: {
    trigger: ".square",
    start: "top 80%", //400, //400(px) or top/center/bottom top/center/bottom or top/center/bottom 50% or it can be a function too
    end: "top 50%", //() => `+=${document.querySelector(".square").offsetHeight}`
    markers: true,
    toggleClass: "red",
  },
});
```

### Timeline

#### Ex 1

```js
const timeLine = gsap.timeline({
  defaults: { duration: 1 },
});

timeLine
  .from(".header", { y: "-100%", ease: "bounce" })
  .from(".link", { opacity: 0, stagger: 0.5 })
  .from(".right", { x: "-100vw" }, 1)
  .from(".left", { x: "-100%" }, "<0.5") // after 1sec relative to the above animation
  .to(".footer", { y: 0, ease: "elastic" })
  .fromTo(
    ".button",
    { opacity: 0, scale: 0, rotation: 720 },
    { opacity: 1, scale: 1, rotation: 0 }
  );

document.querySelector(".button").addEventListener("click", () => {
  // timeLine.timeScale(2);
  timeLine.reverse();
});
```

#### Ex 2

```js
gsap.registerPlugin(ScrollTrigger);

const tl = gsap.timeline({
  scrollTrigger: {
    trigger: ".box",
    markers: true,
    start: "top 80%",
    end: "top 30%",
    scrub: 1,
  },
});
tl.to(".box", { x: 500, duration: 2 })
  .to(".box", { y: 200, duration: 3 })
  .to(".box", { x: 0, duration: 2 });
```
