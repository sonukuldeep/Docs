---
author: kuldeep
datetime: 2023-08-06
title: Lottie files
slug: "lottie"
featured: false
draft: false
tags:
  - lottie animation
ogImage: ""
description: Animating lottie files
---

# Lottie animation

## Docs

- [Airbnb site](https://airbnb.io/lottie/#/web)
- [Docs](https://github.com/LottieFiles/lottie-player)
- [Lottie react](https://github.com/LottieFiles/lottie-react)
- [Lottie web](https://github.com/airbnb/lottie-web/)

## Demo

- [Scroll animation](https://codepen.io/Donald-6329/pen/OJaqGEd?editors=1000)
- [Animate on hold](https://codepen.io/mdavid1/pen/ZEaGMoo)
- [Scroll animation with offset](https://codepen.io/mdavid1/pen/mdqdJJx?editors=0010)

## Tip

use dotlottie when possible due to small file size else use bodymovin by airbnb

## Using bodymovin

bodymovin is from airbnb rest all packages are from lottiefiles.com

### Install

```js
# with npm
npm install lottie-web

# with bower
bower install bodymovin
```

- [Docs](https://airbnb.io/lottie/#/web)
- [Docs](https://github.com/airbnb/lottie-web/tree/master)

index.html

```html
<body>
  <div class="container">
    <div class="svg hide" id="svg"></div>
    <h1>My card title</h1>
    <p>
      Lorem, ipsum dolor sit amet consectetur adipisicing elit. Eum voluptatem
      minima cupiditate reiciendis deleniti, consequatur corporis autem quis
      blanditiis facilis nisi quam at error repellat. Voluptates dolorum iure
      laboriosam explicabo?
    </p>
    <button>Get Started</button>
  </div>
  <script
    src="https://cdnjs.cloudflare.com/ajax/libs/bodymovin/5.12.2/lottie.min.js"
    integrity="sha512-jEnuDt6jfecCjthQAJ+ed0MTVA++5ZKmlUcmDGBv2vUI/REn6FuIdixLNnQT+vKusE2hhTk2is3cFvv5wA+Sgg=="
    crossorigin="anonymous"
    referrerpolicy="no-referrer"
  ></script>
  <script src="/main.js"></script>
</body>
```

main.js

```js
const button = document.querySelector("button");
const svgContainer = document.getElementById("svg");

const animateItem = bodymovin.loadAnimation({
  container: svgContainer, // Required
  path: "https://assets2.lottiefiles.com/packages/lf20_u4yrau.json", // Required
  // path: './data.json', // Required
  renderer: "svg", // Required
  loop: false, // Optional
  autoplay: false, // Optional
  name: "getting started", // Name for future reference. Optional.
});

button.addEventListener("click", () => {
  animateItem.goToAndPlay(0, true);
});
```

style.css

```css
body {
  font-family: "Poppins";
  background-color: rgb(27, 27, 27);
  display: grid;
  place-content: center;
  height: 100vh;
  color: #fff;
}

.container {
  position: relative;
  width: 60%;
  margin: 0 auto;
  background-color: #232323;
  padding: 2rem;
}

h1 {
  margin: 0;
}

p {
  color: grey;
}

button {
  background-color: #343434;
  margin-top: 2rem;
  border: none;
  padding: 1rem;
  color: #fff;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 0.2rem;
  outline: 0;
  cursor: pointer;
}

button:hover {
  background-color: #424242;
}

button:active {
  background-color: orange;
}

#svg {
  width: 200px;
  position: absolute;
  top: -40px;
  left: -30px;
  pointer-events: none;
}
```

## dotlottie

dot lottie files are smaller compared to other lottie json file hence it should be the prefered method of implementing lottie animation on websites.

- [Docs](https://docs.lottiefiles.com/dotlottie-players/)
- [github](https://github.com/dotlottie/player-component/tree/master/packages/player-component)
- [dotlottie methods](https://docs.lottiefiles.com/dotlottie-players/components/player-component/methods)

### Install

use cdn, npm package doesn't work well with vanilla js

With the cdn method no additional import is needed on js side

```js
<script src="https://unpkg.com/@dotlottie/player-component@latest/dist/dotlottie-player.js"></script>
```

src in dotlottie-player custom component does not take .json only .lottie works<br>
however, as shown in example 2 load function can be used to load .json from js

### Example 1

Nothing fancy

```html
<dotlottie-player
  autoplay
  controls
  loop
  mode="normal"
  src="animation_lmxafm56.lottie"
  style="width: 320px"
>
</dotlottie-player>
<script src="https://unpkg.com/@dotlottie/player-component@latest/dist/dotlottie-player.js"></script>
```

### Example 2

- Animate on load
- Animate on show
- Animate on scroll

```html
<div class="spacer"></div>
<div class="spacer"></div>
<div class="spacer"></div>
<dotlottie-player style="width: 320px"></dotlottie-player>
<button id="reset">reset</button>
<div class="spacer"></div>

<script src="https://unpkg.com/@dotlottie/player-component@latest/dist/dotlottie-player.js"></script>
<script>
  const player = document.querySelector("dotlottie-player");

  player.load("./animation_lmxafm56.lottie").then(() => player.play());
  //player.load('./animation_lmxafm56.lottie').then(() => player.playOnShow({ threshold: [1] })) // threshold btw [0-1] - 0.5 means start playing when half of the animation is on view
  //player.load('./animation_lmxafm56.lottie').then(() => player.playOnScroll({ threshold: [0.2, 0.5] })) // 0.2 means start at 20% from the bottom of viewport and complete at 50% of viewport
</script>
```

### Example 3

Animate on click

```html
<div class="spacer"></div>
<div class="spacer"></div>
<div class="spacer"></div>
<dotlottie-player style="width: 320px" mode="normal"></dotlottie-player>
<button id="reset">reset</button>
<div class="spacer"></div>
<script src="https://unpkg.com/@dotlottie/player-component@latest/dist/dotlottie-player.js"></script>
<script type="module">
  const player = document.querySelector("dotlottie-player");

  player.load("./animation_lmxafm56.lottie").then(() => {
    player.addEventListener("click", () => {
      player.play();
      player.seek(0);
    });
  });
</script>
```

### Example 4

Animate on hover

```html
<div class="spacer"></div>
<div class="spacer"></div>
<div class="spacer"></div>
<dotlottie-player
  style="width: 320px"
  mode="normal"
  src="./animation_lmxafm56.lottie"
  hover
  loop
></dotlottie-player>
<button id="reset">reset</button>
<div class="spacer"></div>
<script src="https://unpkg.com/@dotlottie/player-component@latest/dist/dotlottie-player.js"></script>
```

## Lottie-interactivity ft dotlottie

[Docs](https://docs.lottiefiles.com/lottie-interactivity/)

### Animate on click

```html
<body>
  <div class="spacer"></div>
  <div class="spacer"></div>
  <div class="spacer"></div>
  <dotlottie-player style="width: 320px" mode="normal"></dotlottie-player>
  <div class="spacer"></div>
  <script src="https://unpkg.com/@dotlottie/player-component@latest/dist/dotlottie-player.js"></script>
  <script src="https://unpkg.com/@lottiefiles/lottie-interactivity@latest/dist/lottie-interactivity.min.js"></script>
  <script type="module">
    const player = document.querySelector("dotlottie-player");
    player.load("./animation_lmxafm56.lottie");

    player.addEventListener("ready", () => {
      LottieInteractivity.create({
        player: player.getLottie(),
        mode: "cursor",
        actions: [
          {
            type: "click",
            forceFlag: true,
          },
        ],
      });
    });
  </script>
</body>
```

### Animate on scroll

```html
<body>
  <div class="spacer"></div>
  <div class="spacer"></div>
  <div class="spacer"></div>
  <dotlottie-player style="width: 320px" mode="normal"></dotlottie-player>
  <div class="spacer"></div>
  <script src="https://unpkg.com/@dotlottie/player-component@latest/dist/dotlottie-player.js"></script>
  <script src="https://unpkg.com/@lottiefiles/lottie-interactivity@latest/dist/lottie-interactivity.min.js"></script>
  <script type="module">
    const player = document.querySelector("dotlottie-player");
    player.load("./animation_lmxafm56.lottie");

    player.addEventListener("ready", () => {
      LottieInteractivity.create({
        player: player.getLottie(),
        mode: "scroll",
        actions: [
          {
            visibility: [0, 0.6],
            type: "seek",
            frames: [0],
          },
        ],
      });
    });
  </script>
</body>
```

## Using lottie-player Web Component

[Docs](https://github.com/LottieFiles/lottie-player)

### Install

```js
<script src="https://unpkg.com/@lottiefiles/lottie-player@1.5.7/dist/lottie-player.js"></script>
// or
npm install --save @lottiefiles/lottie-player
```

### Demo

```html
/* put in html */
<lottie-player
  src="https://lottie.host/9f19ba8a-a077-4095-b61b-d46337ee40ba/yJaFIt38QW.json"
  background="#ffffff"
  speed="1"
  style="width: 300px; height: 300px"
  loop
  controls
  autoplay
  direction="1"
  mode="normal"
></lottie-player>
```
