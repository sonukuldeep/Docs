---
title: Jquery
author: kuldeep
datetime: 2023-02-14
slug: promise
featured: false
draft: false
tags:
  - jquery
ogImage: ""
description: useful Jquery commands
---

<img src="https://www.devopsschool.com/blog/wp-content/uploads/2022/03/jquery.png" alt="jquery image">

# Jquery

jQuery is a JavaScript library designed to simplify HTML DOM tree traversal and manipulation, as well as event handling, CSS animation, and Ajax. It is free, open-source software using the permissive MIT License. As of Aug 2022, jQuery is used by 77% of the 10 million most popular websites.

## How to add

Include the cdn in the head is the easiest way

```html
<script
  src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.3/jquery.min.js"
  integrity="sha512-STof4xm1wgkfm7heWqFJVn58Hm3EtS31XFaagaa8VMReCXAkQnJZ+jEy8PCC/iT18dFy95WcExNHFTqLyp72eQ=="
  crossorigin="anonymous"
  referrerpolicy="no-referrer"
></script>
```

## Some useful examples

### Added or remove class

```js
$("h1").addClass("big-font margin-20");

$("h1").removeClass("margin-20");

$("h1").hasClass("margin-20"); //returns boolean
```

### Modify innerHtml innerText

```js
$("h1").text("bye");

$("button").text("dont click"); //updates all instance of button text.

$("button").html("<em>Hello</em>");
```

### Get and set element attribute

```js
$("img").attr("src");

$("img").attr("src", "change to something else");

$("div").attr("class"); //gets only the class of first div
```

### Add event listner

```js
$("button").click(function () {
  //assigns click event to all h1
  console.log($("h1").removeClass("big-font"));
});

$("input").keydown(function (e) {
  $("h1").text(e.key);
});

$("h1").on("click", function () {
  //do something on click
});
```

### Modify content inside the selector

```js
$("h1").before("<h2>It's monday</h2>");

$("h1").after("<h2>good morning</h2>");

$("h1").prepend("<span>whats app</span>"); //adds inside h1 before the text

$("h1").append("<span>whats down</span>"); //adds inside h1 after  text
```

### Animation

```js
$("h1").remove();

$("h1").show();

$("h1").hide();

$("h1").toggle();

$("h1").fadeOut();

$("h1").fadeIn();

$("h1").fadeToggle();

$("h1").slideUp();

$("h1").slideDown();

$("h1").slideToggle();

$("h1").animate({ opacity: 0.5 }); //inside curly brackets only css property with number can be added
```

### Animations can chained one after another and they run in that order

```js
$("h1").slideUp().slideDown().apnimate({ opacity: 0.5 });
```
