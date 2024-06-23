---
author: Kuldeep
datetime: 2023-01-01
title: Tips
slug: "tips"
featured: false
draft: false
tags:
  - react responsive-image git-config
ogImage: ""
description: Collection of random tips and tricks i came across in the internet.
---

![tips image](https://eugensystems.com/wp-content/uploads/2016/03/Tips1.jpg)

# Tips

## Table of contents

## Scroll to Element

The most simple way is to use ref to store the reference of the element that you want to scroll to. And call myRef.current.scrollIntoView() to scroll it into the view.

Refs in React have many different applications. One of the most common uses is to reference elements in the DOM. By referencing the element, you also gain access element’s interface. This is essential to capture the element to which we want to scroll.

useRef returns a mutable ref object whose .current property is initialized to the passed argument (initialValue). useRef() is useful for more than the ref attribute.

```bash
const divRef = useRef(null);
```

> In Functional Component:

```bash
import React, { useRef } from 'react';

const App = () => {
  const scollToRef = useRef();

  return (
    <div className="container">
      <button onClick={() => scollToRef.current.scrollIntoView()}>
        Scroll
      </button>
      <div ref={scollToRef}>scroll Me</div>
    </div>
  );
};

export default App;
```

> In class component:

```bash
class TestComponent extends Component {
  constructor(props) {
    super(props);
    this.testRef = React.createRef();
  }
  scrollToElement = () => this.testRef.current.scrollIntoView();

  render() {
    return <div ref={this.testRef}>Element you want in view</div>;
  }
}
```

> ScrollInView takes these arguments

```bash
element.scrollIntoView({behavior:"smooth", block: "end", inline:"nearest"})
```

[source](https://bosctechlabs.com/scroll-to-an-element-in-react/)

## Responsive image component

### html

```html
<img
  src="https://picsum.photos/id/237/1200/1200"
  alt="random image"
  loading="lazy"
  sizes="(max-width: 400px) 300px,(max-width: 700px) 500px, 700px"
  srcset="
    https://picsum.photos/id/2/600/600      500w,
    https://picsum.photos/id/23/800/800     700w,
    https://picsum.photos/id/237/1000/1000 1100w
  "
/>
```

- According to mdn if srcset is provided src is ignored and these must be sizes attribute for the browser to make smart choice for image download
- From my extensive testing i have come to a conclusion that both chrome and mozilla only use 2-3 of these images hence providing more is a waste
- Sizes and srcset are not linked 1-1. sizes just tell the browser how much space the image will takeup in each viewpoint specified.
- Depending on size alloted to image at each viewpoint the browser then downloads the best image according to the list of images in srcset

### Lasyloading using vanilla js

```html
<div
  class="blurred-img"
  style="background-image: url('https://picsum.photos/id/237/20/20?blur')"
>
  <img
    src="https://picsum.photos/id/237/1200/1200"
    alt="random image"
    loading="lazy"
    sizes="(max-width: 400px) 300px,(max-width: 700px) 500px, 700px"
    srcset="
      https://picsum.photos/id/2/600/600      500w,
      https://picsum.photos/id/23/800/800     700w,
      https://picsum.photos/id/237/1000/1000 1100w
    "
  />
</div>
```

```css
.blurred-img {
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
  width: 700px;
}

.blurred-img img {
  opacity: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  background-size: cover;
  transition: opacity 250ms ease-in;
}

.blurred-img::before {
  content: "";
  position: absolute;
  inset: 0;
  opacity: 0;
  animation: pulse 2s infinite;
  background-color: white;
}

@keyframes pulse {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 0.1;
  }
  100% {
    opacity: 0;
  }
}

.blurred-img.loaded::before {
  animation: none;
  content: none;
}

.blurred-img.loaded img {
  opacity: 1;
}

@media (max-width: 700px) {
  .blurred-img {
    width: 500px;
  }
}
@media (max-width: 400px) {
  .blurred-img {
    width: 300px;
  }
}
```

```js
const blurredImageDiv = document.querySelector(".blurred-img");

const img = blurredImageDiv.querySelector(".blurred-img img");

function loaded() {
  blurredImageDiv.classList.add("loaded");
  blurredImageDiv.style.backgroundImage = "none";
}

if (img.complete) {
  loaded();
} else img.addEventListener("load", loaded);
```

### Nextjs image

[Docs](https://nextjs.org/docs/app/building-your-application/optimizing/images)

#### Local image

```jsx
import Image from "next/image";
import profilePic from "./me.png";

export default function Page() {
  return (
    <Image
      src={profilePic}
      alt="Picture of the author"
      // width={500} automatically provided
      // height={500} automatically provided
      // blurDataURL="data:..." automatically provided
      // placeholder="blur" // Optional blur-up while loading
    />
  );
}
```

#### Remote image

```jsx
import Image from "next/image";

export default function Page() {
  return (
    <Image
      src="https://s3.amazonaws.com/my-bucket/profile.png"
      alt="Picture of the author"
      width={500}
      height={500}
      blurDataURL={"base64-encoded image (10px or less)"}
      placeholder="blur"
    />
  );
}
```

### Unknown image size

If size of image is unknown then a container can be used instead. I dont recommend it since it is not clear from the documentation if loader creates srcset images to server to devices of different sizes.

```jsx
import Image from "next/image";

export default function Page() {
  return (
    <div className="grid-element">
      <Image
        fill
        src="/example.png"
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
      />
    </div>
  );
}
```

## Multi account git configuration

This multi account git configuration is folder based meaning depending on the current working directory, the configuration will change accordingly, automatically.

```npm
# ~/.gitconfig
[includeIf "gitdir:C:/Work/"]
	path = C:/Users/Sonukuldeep/.gitconfig.work
[includeIf "gitdir:C:/Personal/"]
	path = C:/Users/Sonukuldeep/.gitconfig.personal
[core]
	excludesfile = C:/Users/Sonukuldeep/.gitignore_global
[init]
	defaultBranch = main
```

```npm
# C:/Personal/

[user]
	name = sonukuldeep
	email = sonukuldip@xyz.com

[github]
	user = "sonukuldeep"

[core]
	sshCommand = "ssh -i C:/Users/sonuk/.ssh/idrsa1"
```

```npm
# C:/Work/

[user]
	name = xyz-company
	email = xyz-company@xyz.com

[github]
	user = "xyz-company"

[core]
	sshCommand = "ssh -i C:/Users/sonuk/.ssh/idrsa2"
```

Then when you're working in any git folder inside work directory then git config specific to work will be used. Some goes for personal folder.

## Google sheet database

Add this to google app sheet under extension after making appropriate changes in the below script

```js
const sheetName = "DATABASE";
const param1 = "count";
const param1CellIndex = 0;
const param2 = "type";
const param2CellIndex = 1;

function doPost(req) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName(sheetName);
  const cell1 = req.parameter[param1];
  const cell2 = req.parameter[param2];

  sheet.insertRowBefore(2);
  sheet.getRange(2, 1, 1, 1).setValue(cell1);
  sheet.getRange(2, 2, 1, 1).setValue(cell2);

  const response = HtmlService.createHtmlOutput();
  return response;
}

function doGet(req) {
  const p1 = req.parameter[param1];
  const doc = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = doc.getSheetByName(sheetName);
  const values = sheet.getDataRange().getValues();

  const output = [];

  // Loop through the values and varruct the output array
  for (let i = 1; i < values.length; i++) {
    const row = {};
    row[param1] = values[i][param1CellIndex]; // Assuming count is in the 1st column (index starts from 0)
    row[param2] = values[i][param2CellIndex]; // Assuming type is in the 1st column (index starts from 0)
    output.push(row);
  }

  // filter
  if (p1) {
    const outputToReturn = output.filter(obj =>
      obj[param1].toString().toLowerCase().includes(p1.toLowerCase())
    );
    const response = ContentService.createTextOutput(
      JSON.stringify({ data: outputToReturn })
    );
    response.setMimeType(ContentService.MimeType.JSON);
    return response;
  } else {
    const response = ContentService.createTextOutput(
      JSON.stringify({ data: output })
    );
    response.setMimeType(ContentService.MimeType.JSON);
    return response;
  }
}
```
