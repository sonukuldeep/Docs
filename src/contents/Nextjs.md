---
author: kuldeep
datetime: 2023-02-04
title: Nextjs
slug: "nextjs"
featured: true
draft: false
tags:
  - nextjs
  - react
ogImage: ""
description: Everything related to Nextjs
---

# NextJs

## Table of contents

## Static site config

getStaticProps and getServerSideProps work in page elemnt only.
we need getStaticProps and getStaticPaths for this.
_With nextjs 13 and above getStaticProps and getStaticPaths have been depricated_

_blog.tsx_

```jsx
import React from "react";
import { GetStaticPaths, GetStaticProps } from "next";
import DetailedBlog from "../../../components/DetailedBlog";

interface props {
  blog: { id: number };
}

const index = ({ blog }: props) => {
  console.log(blog);
  return <DetailedBlog />;
};

export default index;

// Generates `/posts/1` and `/posts/2`
export const getStaticPaths: GetStaticPaths = async () => {
  const ids = [1, 2, 3, 4, 5, 6];
  const paths = ids.map(id => ({ params: { id: id.toString() } }));
  return {
    paths,
    fallback: false, // can also be true or 'blocking'
  };
};

// `getStaticPaths` requires using `getStaticProps`
export const getStaticProps: GetStaticProps = async context => {
  let blog = {};
  if (context.params) {
    blog = { id: context.params.id };
  }
  return {
    // Passed to the page component as props
    props: { blog },
  };
};
```

or <br>
Use serverSideProps

```jsx
export const getServerSideProps: GetServerSideProps = async context => {
  const { params } = context;
  const id = params?.id;
  console.log(id);
  return {
    props: {
      page: id,
    },
  };
};
```

<img src="https://i.ibb.co/wpL1xS7/Screenshot-2023-02-05-205246.png" alt="folder-structure-image">

Mark the slug in square brackets

<hr>

## Next image api

Known Browser Bugs

- Safari 15 and 16 display a gray border while loading. Safari 16.4 fixed this issue. Possible solutions:

  - Use CSS @supports (font: -apple-system-body) and (-webkit-appearance: none) { img[loading="lazy"] { clip-path: inset(0.6px) } }

- Use priority if the image is above the fold
  - Firefox 67+ displays a white background while loading. Possible solutions:
    - Enable AVIF formats
    - Use placeholder="blur"

<hr>

### Required Props

#### Src

Must be one of the following:

1. A [statically imported](https://nextjs.org/docs/basic-features/image-optimization#local-images) image file, or
1. A path string. This can be either an absolute external URL, or an internal path depending on the [loader](https://nextjs.org/docs/api-reference/next/image#loader) prop.

When using an external URL, you must add it to [remotePatterns](https://nextjs.org/docs/api-reference/next/image#remote-patterns) in next.config.js.

<hr>

#### Width

The width property represents the rendered width in pixels, so it will affect how large the image appears.

Required, except for statically imported images or images with the fill property.

#### Height

The height property represents the rendered height in pixels, so it will affect how large the image appears.

Required, except for statically imported images or images with the fill property.

#### Alt

The alt property is used to describe the image for screen readers and search engines.

<hr>

### Optional Props

The &lt;Image /&gt; component accepts a number of additional properties beyond those which are required. This section describes the most commonly-used properties of the Image component. Find details about more rarely-used properties in the [Advanced Props](https://nextjs.org/docs/api-reference/next/image#advanced-props) section.

<hr>

#### Loader

A custom function used to resolve image URLs.

A loader is a function returning a URL string for the image, given the following parameters:

    src
    width
    quality

Here is an example of using a custom loader:

```jsx
import Image from "next/image";

const myLoader = ({ src, width, quality }) => {
  return `https://example.com/${src}?w=${width}&q=${quality || 75}`;
};

const MyImage = props => {
  return (
    <Image
      loader={myLoader}
      src="me.png"
      alt="Picture of the author"
      width={500}
      height={500}
    />
  );
};
```

Alternatively, you can use the loaderFile configuration in next.config.js to configure every instance of next/image in your application, without passing a prop.

<hr>

#### Fill

A boolean that causes the image to fill the parent element instead of setting width and height.

The parent element must assign position: "relative", position: "fixed", or position: "absolute" style.

By default, the img element will automatically be assigned the position: "absolute" style.

The default image fit behavior will stretch the image to fit the container. You may prefer to set object-fit: "contain" for an image which is letterboxed to fit the container and preserve aspect ratio.

Alternatively, object-fit: "cover" will cause the image to fill the entire container and be cropped to preserve aspect ratio. For this to look correct, the overflow: "hidden" style should be assigned to the parent element.

<hr>

#### Sizes

A string that provides information about how wide the image will be at different breakpoints. The value of sizes will greatly affect performance for images using fill or which are styled to have a responsive size.

The sizes property serves two important purposes related to image performance:

First, the value of sizes is used by the browser to determine which size of the image to download, from next/image's automatically-generated source set. When the browser chooses, it does not yet know the size of the image on the page, so it selects an image that is the same size or larger than the viewport. The sizes property allows you to tell the browser that the image will actually be smaller than full screen. If you don't specify a sizes value in an image with the fill property, a default value of 100vw (full screen width) is used.

Second, the sizes property configures how next/image automatically generates an image source set. If no sizes value is present, a small source set is generated, suitable for a fixed-size image. If sizes is defined, a large source set is generated, suitable for a responsive image. If the sizes property includes sizes such as 50vw, which represent a percentage of the viewport width, then the source set is trimmed to not include any values which are too small to ever be necessary.

For example, if you know your styling will cause an image to be full-width on mobile devices, in a 2-column layout on tablets, and a 3-column layout on desktop displays, you should include a sizes property such as the following:

```jsx
import Image from "next/image";
const Example = () => (
  <div className="grid-element">
    <Image
      src="/example.png"
      fill
      sizes="(max-width: 768px) 100vw,
              (max-width: 1200px) 50vw,
              33vw"
    />
  </div>
);
```

This example sizes could have a dramatic effect on performance metrics. Without the 33vw sizes, the image selected from the server would be 3 times as wide as it needs to be. Because file size is proportional to the square of the width, without sizes the user would download an image that's 9 times larger than necessary.

<hr>

#### Quality

The quality of the optimized image, an integer between 1 and 100, where 100 is the best quality and therefore largest file size. Defaults to 75.

#### Priority

When true, the image will be considered high priority and preload. Lazy loading is automatically disabled for images using priority.

#### Placeholder

A placeholder to use while the image is loading. Possible values are blur or empty. Defaults to empty.

When blur, the blurDataURL property will be used as the placeholder. If src is an object from a static import and the imported image is .jpg, .png, .webp, or .avif, then blurDataURL will be automatically populated.

<hr>

## Image Component

[Detailed docs](https://nextjs.org/docs/basic-features/image-optimization)

The Next.js Image component, next/image, is an extension of the HTML <img> element, evolved for the modern web. It includes a variety of built-in performance optimizations to help you achieve good Core Web Vitals. These scores are an important measurement of user experience on your website, and are factored into Google's search rankings.

Some of the optimizations built into the Image component include:

- Improved Performance: Always serve correctly sized image for each device, using modern image formats
- Visual Stability: Prevent Cumulative Layout Shift automatically
- Faster Page Loads: Images are only loaded when they enter the viewport, with optional blur-up placeholders
- Asset Flexibility: On-demand image resizing, even for images stored on remote servers

<hr>

## Next Auth

This has a dedicated section
[link](https://kuldeep-docs.netlify.app/posts/authentication/#using-next-auth)

<hr>

## Integrate mongoose into nextjs 13.4

### Mongoose config file

```ts
//utils/mongoose.ts
import mongoose from "mongoose";

if (!process.env.MONGO_URI) {
  throw new Error('Invalid/Missing environment variable: "MONGO_URI"');
}

const uri = process.env.MONGO_URI;

const connectMongo = async () => mongoose.connect(uri);

export default connectMongo;
```

### Mongoose model

```ts
// models/test.ts
import { Schema, model, models } from "mongoose";

const testSchema = new Schema({
  name: String,
  email: {
    type: String,
    required: true,
    unique: true,
  },
});

const Test = models.Test || model("Test", testSchema);

export default Test;
```

### Use

```ts
import connectMongo from "@/utils/mongoose";
import Test from "@/models/testModel";

async function fetchDocuments() {
  console.log("CONNECTING TO MONGO");
  await connectMongo();
  console.log("CONNECTED TO MONGO");

  console.log("FETCHING DOCUMENT");
  const test = await Test.find();
  console.log("DATA FETCHED");
  return test;
}

const Home = async () => {
  const data = await fetchDocuments();
  return (
    <>
      <main className="flex flex-col gap-2">
        <h1 className="text-xl">How does this work?</h1>
        <p>
          Mongodb data is fetched from the root page component and the data is
          rendered here as below.
        </p>
        <p>
          Any form of data can be fetched in server side in advanced and served
          provided all models are defined in /models folder
        </p>
        <ul className="grid grid-cols-3 gap-2">
          {data.map(data => (
            <li className="border p-2 rounded" key={data.id}>
              name:- {data.name} <br /> email:- {data.email}
            </li>
          ))}
        </ul>
      </main>
    </>
  );
};

export default Home;
```

## Applying font and metadata
```jsx
import { Poppins } from 'next/font/google';
import Header from './componets/Header';
import './globals.css';

const poppins = Poppins({
  weight: ['400', '700'],
  subsets: ['latin'],
});

export const metadata = {
  title: 'Traversy Media',
  description: 'Web development tutorials and courses',
  keywords:
    'web development, web design, javascript, react, node, angular, vue, html, css',
};

export default function RootLayout({ children }) {
  return (
    <html lang='en'>
      <body className={poppins.className}>
        <Header />
        <main className='container'>{children}</main>
      </body>
    </html>
  );
}
```
