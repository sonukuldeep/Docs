---
author: Kris Koishigawa | FreeCodeCamp
datetime: 2023-07-07T15:22:00Z
title: Web components
slug: web-components
featured: false
draft: false
tags:
  - web-components
ogImage: ""
description: Web Components are actually a collection of a few different technologies that allow you to create custom HTML elements.
---

# Web components

## Termonology

- HTML templates: Fragments of HTML markup using &lt;template&gt; elements that won't be rendered until they're appended to the page with JavaScript.
- Custom elements: Widely supported JavaScript APIs that let you create new DOM elements. Once you create and register a custom element using these APIs, you can use it similarly to a React component.
- Shadow DOM: A smaller, encapsulated DOM that is isolated from the main DOM and rendered separately. Any styles and scripts you create for your custom components in the Shadow DOM will not affect other elements in the main DOM.
- append: Accepts Node Objects/DOMStrings
- appendChild: Accepts node objects only

## Demo

[Web component 1](https://codepen.io/Donald-6329/pen/XWyQYee?editors=0010)

## Sample

```html
<!--index.html-->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css"
      integrity="sha512-+4zCK9k+qNFUR5X+cKL9EIR+ZOhtIloNl9GIKS57V1MyNsYpYcUrUeQc9vNfzsWfV28IaLL3i96P9sdNyeRssA=="
      crossorigin="anonymous"
    />
    <link href="style.css" rel="stylesheet" type="text/css" />
    <script src="components/header.js" type="text/javascript" defer></script>
    <script src="components/footer.js" type="text/javascript" defer></script>
  </head>
  <body>
    <header-component></header-component>
    <main>
      <!-- Your page's content -->
    </main>
    <footer-component></footer-component>
  </body>
  <html></html>
</html>
```

```css
<!--style.css-- > * {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  height: 100%;
}

body {
  color: #333;
  font-family: sans-serif;
  display: flex;
  flex-direction: column;
}

main {
  flex: 1 0 auto;
}
```

```js
// components/header.js
const headerTemplate = document.createElement("template");

headerTemplate.innerHTML = `
  <style>
    nav {
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color:  #0a0a23;
    }

    ul {
      padding: 0;
    }
    
    ul li {
      list-style: none;
      display: inline;
    }
    
    a {
      font-weight: 700;
      margin: 0 25px;
      color: #fff;
      text-decoration: none;
    }
    
    a:hover {
      padding-bottom: 5px;
      box-shadow: inset 0 -2px 0 0 #fff;
    }
  </style>
  <header>
    <nav>
      <ul>
        <li><a href="about.html">About</a></li>
        <li><a href="work.html">Work</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>
  </header>
`;

class Header extends HTMLElement {
  constructor() {
    super();
  }

  connectedCallback() {
    const shadowRoot = this.attachShadow({ mode: "closed" });

    shadowRoot.appendChild(headerTemplate.content);
  }
}

customElements.define("header-component", Header);
```

```js
// components/footer.js
const footerTemplate = document.createElement("template");

footerTemplate.innerHTML = `
  <style>
    footer {
      height: 60px;
      padding: 0 10px;
      list-style: none;
      display: flex;
      flex-shrink: 0;
      justify-content: space-between;
      align-items: center;
      background-color: #dfdfe2;
    }

    ul {
      padding: 0;
    }
    
    ul li {
      list-style: none;
      display: inline;
    }
    
    a {
      margin: 0 15px;
      color: inherit;
      text-decoration: none;
    }
    
    a:hover {
      padding-bottom: 5px;
      box-shadow: inset 0 -2px 0 0 #333;
    }
    
    .social-row {
      font-size: 20px;
    }
    
    .social-row li a {
      margin: 0 15px;
    }
  </style>
  <footer>
    <ul>
      <li><a href="about.html">About</a></li>
      <li><a href="work.html">Work</a></li>
      <li><a href="contact.html">Contact</a></li>
    </ul>
    <ul class="social-row">
      <li><a href="https://github.com/my-github-profile"><i class="fab fa-github"></i></a></li>
      <li><a href="https://twitter.com/my-twitter-profile"><i class="fab fa-twitter"></i></a></li>
      <li><a href="https://www.linkedin.com/in/my-linkedin-profile"><i class="fab fa-linkedin"></i></a></li>
    </ul>
  </footer>
`;

class Footer extends HTMLElement {
  constructor() {
    super();
  }

  connectedCallback() {
    const fontAwesome = document.querySelector('link[href*="font-awesome"]');
    const shadowRoot = this.attachShadow({ mode: "closed" });

    if (fontAwesome) {
      shadowRoot.appendChild(fontAwesome.cloneNode());
    }

    shadowRoot.appendChild(footerTemplate.content);
  }
}

customElements.define("footer-component", Footer);
```

## Links

[Mdn docs](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/)
[FreeCodeCamp article](https://www.freecodecamp.org/news/reusable-html-components-how-to-reuse-a-header-and-footer-on-a-website/)
