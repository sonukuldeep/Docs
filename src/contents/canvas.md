---
author: kuldeep
datetime: 2023-07-27
title: Html canvas
slug: "canvas"
featured: false
draft: false
tags:
  - canvas
ogImage: ""
description: Codes for everything related to html canvas
---

# Canvas

## Boilerplate

```js
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

canvas.width = document.documentElement.clientWidth;
canvas.height = document.documentElement.clientHeight;

const gradient = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
gradient.addColorStop(0, "blue");
gradient.addColorStop(0.5, "green");
gradient.addColorStop(1, "red");

ctx.fillStyle = gradient;
ctx.strokeStyle = "white";
ctx.font = `24px Arial`;

const fps = 30; // Frames per second
const frameTime = 1000 / fps; // frameTime between frames in milliseconds
let lastTime = 0;
let deltaTime = 0;
let requestAnimationFrameRef = 0;
```

## Animate function

```js
function animation(currentTime) {
  requestAnimationFrameRef = requestAnimationFrame(animation); // this should always be at the start
  // Calculate the time difference since the last frame
  deltaTime = currentTime - lastTime;

  // Proceed only if enough time has elapsed based on the desired frame rate
  if (deltaTime >= frameRate) {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // do something

    lastTime = currentTime - (deltaTime % frameRate);
  }
}
animation();
// requestAnimationFrame(animation)
```
