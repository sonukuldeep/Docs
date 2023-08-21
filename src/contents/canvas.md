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
  if (deltaTime >= frameTime) {
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // do something

    lastTime = currentTime - (deltaTime % frameTime);
  }
}
animation(0);
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

<hr/>

## GetImageData

The `getImageData()` method returns an `ImageData` object, which contains a `data` property that represents the pixel data of the captured region. The `data` property is a one-dimensional array with four values per pixel: red, green, blue, and alpha values.

[getImageData check the js](https://codepen.io/Donald-6329/pen/rNoNRze)

Here's an example of how you can access and print sample pixel data from the `ImageData` object:

```javascript
// Get the canvas element
var canvas = document.getElementById("myCanvas");

// Get the 2D drawing context
var ctx = canvas.getContext("2d");

// Draw an image onto the canvas
var img = document.getElementById("myImage");
ctx.drawImage(img, 0, 0);

// Get the image data
var imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

// Access pixel data
var data = imageData.data;

// Print sample pixel data
for (var i = 0; i < 4 * 10; i += 4) {
  var red = data[i];
  var green = data[i + 1];
  var blue = data[i + 2];
  var alpha = data[i + 3];

  console.log(
    "Pixel at index",
    i / 4,
    ": R =",
    red,
    "G =",
    green,
    "B =",
    blue,
    "A =",
    alpha
  );
}
```

In the above example, we iterate over the `data` array in increments of 4 to access each pixel. We then retrieve the red, green, blue, and alpha values for each pixel using the appropriate indices.

The `for` loop in the example prints the sample pixel data for the first 10 pixels. You can adjust the loop parameters to access and print data for a different range of pixels if desired.

Keep in mind that the pixel data is represented as integers ranging from 0 to 255. The red, green, blue, and alpha values provide the color and transparency information for each pixel.

<hr/>

## globalCompositeOperation

`globalCompositeOperation` is a property used in the HTML5 Canvas API, which is a powerful tool for rendering graphics and images on a web page. This property allows you to control how newly drawn shapes or images are combined with existing content on the canvas.

The `globalCompositeOperation` property determines the way pixels from the newly drawn shape or image will interact with pixels that are already on the canvas. It essentially defines the blending mode or composition operation to be applied.

Here's how it works:

```javascript
context.globalCompositeOperation = "operation";
```

Where `context` is the 2D drawing context of the canvas, and `'operation'` is a string representing the desired composition operation.

Some common composition operations include:

1. `source-over` (default): The new shape/image is drawn on top of the existing content.
2. `source-in`: The new shape/image is only drawn where it overlaps with the existing content.
3. `source-out`: The new shape/image is drawn everywhere except where it overlaps with the existing content.
4. `source-atop`: The new shape/image is drawn on top of the existing content and only where they overlap.
5. `destination-over`: The existing content is drawn on top of the new shape/image.
6. `destination-in`: The existing content is only shown where it overlaps with the new shape/image.
7. `destination-out`: The existing content is only shown where it does not overlap with the new shape/image.
8. `destination-atop`: The existing content is shown on top of the new shape/image and only where they overlap.
9. `lighter`: The colors of the new shape/image and existing content are combined, creating a lighter effect.
10. `darker`: The colors of the new shape/image and existing content are combined, creating a darker effect.
11. `xor`: Pixels are only drawn where the new shape/image and existing content do not overlap.

These are just a few examples of the available composition operations. The `globalCompositeOperation` property allows for various creative possibilities when combining different shapes and images on the canvas.

Here's an example of how you might use `globalCompositeOperation`:

```javascript
const canvas = document.getElementById("myCanvas");
const context = canvas.getContext("2d");

context.fillStyle = "blue";
context.fillRect(50, 50, 100, 100);

context.globalCompositeOperation = "source-atop";

context.fillStyle = "red";
context.beginPath();
context.arc(100, 100, 50, 0, Math.PI * 2, false);
context.fill();
```

In this example, a blue rectangle is drawn on the canvas, and then a red circle is drawn on top using the `source-atop` composition operation, resulting in the red circle being visible only where it overlaps with the blue rectangle.

<hr />

## Basics

[Reference](https://www.w3schools.com/tags/ref_canvas.asp)

### Drawing Methods

There are only 3 methods to draw directly on the canvas:

- [fillRect()](https://www.w3schools.com/tags/canvas_fillrect.asp)
- [strokeRect()](https://www.w3schools.com/tags/canvas_strokerect.asp)
- [clearRect()](https://www.w3schools.com/tags/canvas_clearrect.asp)

Path methods:

- [beginPath()](https://www.w3schools.com/tags/canvas_beginpath.asp)
- [closePath()](https://www.w3schools.com/tags/canvas_closepath.asp)
- [isPointInPath()](https://www.w3schools.com/tags/canvas_ispointinpath.asp)
- [moveTo()](https://www.w3schools.com/tags/canvas_moveto.asp)
- [lineTo()](https://www.w3schools.com/tags/canvas_lineto.asp)
- [fill()](https://www.w3schools.com/tags/canvas_fill.asp)
- [rect()](https://www.w3schools.com/tags/canvas_rect.asp)
- [stroke()](https://www.w3schools.com/tags/canvas_stroke.asp)
- [bezierCurveTo()](https://www.w3schools.com/tags/canvas_beziercurveto.asp)
- [arc()](https://www.w3schools.com/tags/canvas_arc.asp)
- [arcTo()](https://www.w3schools.com/tags/canvas_arcto.asp)
- [quadraticCurveTo()](https://www.w3schools.com/tags/canvas_quadraticcurveto.asp)

Text methods:

- [direction](https://www.w3schools.com/tags/canvas_direction.asp)
- [fillText()](https://www.w3schools.com/tags/canvas_filltext.asp)
- [font](https://www.w3schools.com/tags/canvas_font.asp)
- [measureText()](https://www.w3schools.com/tags/canvas_measuretext.asp)
- [strokeText()](https://www.w3schools.com/tags/canvas_stroketext.asp)
- [textAlign](https://www.w3schools.com/tags/canvas_textalign.asp)
- [textBaseline](https://www.w3schools.com/tags/canvas_textbaseline.asp)

Colors, Styles, and Shadows

- [addColorStop()](https://www.w3schools.com/tags/canvas_addcolorstop.asp)
- [createLinearGradient()](https://www.w3schools.com/tags/canvas_createlineargradient.asp)
- [createPattern()](https://www.w3schools.com/tags/canvas_createpattern.asp)
- [createRadialGradient()](https://www.w3schools.com/tags/canvas_createradialgradient.asp)
- [fillStyle](https://www.w3schools.com/tags/canvas_fillstyle.asp)
- [lineCap](https://www.w3schools.com/tags/canvas_linecap.asp)
- [lineJoin](https://www.w3schools.com/tags/canvas_linejoin.asp)
- [lineWidth](https://www.w3schools.com/tags/canvas_linewidth.asp)
- [miterLimit](https://www.w3schools.com/tags/canvas_miterlimit.asp)
- [shadowBlur](https://www.w3schools.com/tags/canvas_shadowblur.asp)
- [shadowColor](https://www.w3schools.com/tags/canvas_shadowcolor.asp)
- [shadowOffsetX](https://www.w3schools.com/tags/canvas_shadowoffsetx.asp)
- [shadowOffsetY](https://www.w3schools.com/tags/canvas_shadowoffsety.asp)
- [strokeStyle](https://www.w3schools.com/tags/canvas_strokestyle.asp)
