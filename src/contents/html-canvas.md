---
author: ChatGpt
datetime: 2023-07-13
title: Canvas element tips tricks
slug: html-canvas
featured: false
draft: false
tags:
  - docs
ogImage: ""
description: Some tips and tricks related to animation on canvas element
---

#Html canvas

## FPS

The `requestAnimationFrame` method in JavaScript is designed to synchronize your code with the browser's rendering pipeline, which typically runs at the monitor's refresh rate (usually 60 frames per second). This means that the callback function provided to `requestAnimationFrame` will be called as frequently as possible, which may vary depending on the browser and the device.

If you want to achieve a fixed frame rate using `requestAnimationFrame`, you can manually control the timing within your callback function. Here's an example of how you can achieve a fixed frame rate of 30 frames per second:

```javascript
var fps = 30; // Frames per second
var interval = 1000 / fps; // Interval between frames in milliseconds
var lastTime = 0;

function animate(timestamp) {
  // Calculate the time difference since the last frame
  var elapsedTime = timestamp - lastTime;

  // Proceed only if enough time has elapsed based on the desired frame rate
  if (elapsedTime > interval) {
    // Update your animation or game logic here

    // Render your animation or game state here

    // Update the last time to the current timestamp
    lastTime = timestamp;
  }

  // Request the next frame
  requestAnimationFrame(animate);
}

// Start the animation
requestAnimationFrame(animate);
```

In this example, the `animate` function is the callback provided to `requestAnimationFrame`. It checks if enough time has passed since the last frame based on the desired frame rate. If it has, you can update your animation or game logic and render the updated state. If not enough time has elapsed, the function simply requests the next frame without performing any updates, effectively skipping that frame.

By adjusting the `fps` variable, you can control the frame rate of your animation. Keep in mind that if your code takes longer to execute than the specified frame rate, you may experience dropped frames or a slower overall frame rate.
