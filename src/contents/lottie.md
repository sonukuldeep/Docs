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
- [Lottie player](https://github.com/LottieFiles/lottie-player)
- [Lottie react](https://github.com/LottieFiles/lottie-react)
- [Lottie interactivity](https://docs.lottiefiles.com/lottie-interactivity/)
- [jLottie](https://github.com/LottieFiles/jlottie)
- [Lottie web](https://github.com/airbnb/lottie-web/)

## Demo

- [Scroll animation](https://codepen.io/Donald-6329/pen/OJaqGEd?editors=1000)
- [Animate on hold](https://codepen.io/mdavid1/pen/ZEaGMoo)
- [Scroll animation with offset](https://codepen.io/mdavid1/pen/mdqdJJx?editors=0010)

## Tip

Use bodymovin or lottie-player

## Using lottie player

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

## Using bodymovin

[Docs](https://airbnb.io/lottie/#/web)

html

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

Js file

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

css

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
