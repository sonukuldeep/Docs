---
author: Me
datetime: 2023-10-20
title: Github
slug: "github"
featured: false
draft: false
tags:
  - github
ogImage: ""
description: Settings on github.
---

How to configure CI CD on github and deploy on github pages?

1.  Add this file to the root of the project /.github/workflows/deploy.yml (content for this file is below)
2.  Configure github... Go to settings - actions general and enable read and write permissions under Workflow permissons also toggle:- allow GitHub Actions to create and approve pull requests below it
3.  If using vite add this to vite.config.js (see below)
4.  Ultimately goto settings - pages - and select gh-pages for github page and click save

```js
// vite.config.js
export default defineConfig({
  plugins: [react()],
  base: "/test/", //test is the repo name - repo name should be all lowercase too
});
```

```yml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 18

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v2
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

## Useful link:-

(setup github pages)[https://www.youtube.com/watch?v=XhoWXhyuW_I]
