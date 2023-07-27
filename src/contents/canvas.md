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
let lastCollisionTime = 0; // change this with id instead
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

## Collision

```ts
interface ICircleCollisionProps {
  x: number;
  y: number;
  radius: number;
}
interface IRectangleCollisionProps {
  x: number;
  y: number;
  width: number;
  height: number;
}

function detectCollision(
  circle: ICircleCollisionProps,
  rectangle: IRectangleCollisionProps
): boolean {
  if (lastTime - lastCollisionTime < 20) return false; // helps prevent double collision
  if (
    circle.x + circle.radius > rectangle.x &&
    circle.x - circle.radius < rectangle.x + rectangle.width &&
    circle.y + circle.radius > rectangle.y &&
    circle.y - circle.radius < rectangle.y + rectangle.height
  ) {
    lastCollisionTime = lastTime;
    return true;
  } else return false;
}

function detectRectangleCollision(
  rectangle1: IRectangleCollisionProps,
  rectangle2: IRectangleCollisionProps
): boolean {
  if (lastTime - lastCollisionTime < 20) return false; // helps prevent double collision
  if (
    rectangle1.x + rectangle1.width > rectangle2.x &&
    rectangle1.x < rectangle2.x + rectangle2.width &&
    rectangle1.y + rectangle1.height > rectangle2.y &&
    rectangle1.y < rectangle2.y + rectangle2.height
  ) {
    lastCollisionTime = lastTime;
    return true;
  } else return false;
}
```

## Shake

```ts
class ShakeOnHit {
  shake: number;
  amplitude: number;
  switch: number;
  damping: number;
  vibrateX: number;
  vibrateY: number;
  constructor() {
    this.shake = 0;
    this.amplitude = 2;
    this.switch = 1;
    this.damping = 0.9;
    this.vibrateX = 0;
    this.vibrateY = 0;
  }

  vibrate() {
    if (this.shake > 0) {
      this.vibrateX = this.switch * this.amplitude * this.shake;
      this.vibrateY = this.amplitude * this.shake;
      this.shake *= this.damping;
    }
  }
  runEveryFrame() {
    this.switch *= -1;
  }
}
```
