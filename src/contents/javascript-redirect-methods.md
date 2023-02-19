---
author: geeksforgeeks
datetime: 2023-02-19
title: Javascript redirect methods
slug: javascript-redirect
featured: false
draft: false
tags:
  - javascript
ogImage: ""
description: Describes redirect 3 methods used in js
---

# Difference between

> **window.location.href, window.location.replace and window.location.assign in JavaScript**

Window.location is a property that returns a Location object with information about the document’s current location. This Location object represents the location (URL) of the object it is linked to i.e. holds all the information about the current document location (host, href, port, protocol, etc.)

All three commands are used to redirect the page to another page/website but differ in terms of their impact on the browser history.

## window.location.href Property:

- The href property on the location object stores the URL of the current webpage.
- On changing the href property, a user can navigate to a new URL, i.e. go to a new webpage.
- It adds an item to the history list (so that when the user clicks the “Back” button, he/she can return to the current page).
- Updating the href property is considered to be faster than using the assign() function (as calling a function is slower than accessing the property).

Syntax:

```js
window.location.href = "https://www.geeksforgeeks.org";
```

Example

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="getCurrentLocation()">Get URL</button>
    <button onclick="setCurrentLocation()">Change URL</button>

    <script>
      function getCurrentLocation() {
        // Get current location
        var loc = window.location.href;
        alert(loc);
      }
      function setCurrentLocation() {
        // Change current location
        var newloc = "https://www.geeksforgeeks.org/";
        window.location.href = newloc;
      }
    </script>
  </body>
</html>
```

can also be written as

```js
// Less favoured
window.location = "https://www.geeksforgeeks.org";

// More favoured
window.location.href = "https://www.geeksforgeeks.org";
```

<hr>

## window.location.replace Property:

- The replace function is used to navigate to a new URL without adding a new record to the history.
- As the name suggests, this function “replaces” the topmost entry from the history stack, i.e., removes the topmost entry from the history list, by overwriting it with a new entry.
- So, when the user clicks the “Back” button, he/she will not be able to return to the current page.
- Hence, the major difference between the assign() and replace() methods is that the replace() function will delete the current page from the session history.
- The replace function does not wipe out the entire page history, nor does it make the “Back” button non-functional.

Syntax:

```js
window.location.replace("https://geeksforgeeks.org/web-development/");
```

Example:

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="replaceLocation()">Replace current webpage</button>
    <script>
      function replaceLocation() {
        // Replace the current location
        // with new location
        var newloc = "https://www.geeksforgeeks.org/";
        window.location.replace(newloc);
      }
    </script>
  </body>
</html>
```

<hr>

## window.location.assign Property:

- The assign function is similar to the href property as it is also used to navigate to a new URL.
- The assign method, however, does not show the current location, it is only used to go to a new location.
- Unlike the replace method, the assign method adds a new record to history (so that when the user clicks the “Back” button, he/she can return to the current page).
- However, rather than updating the href property, calling a function is considered safer and more readable.
- The assign() method is also preferred over href as it allows the user to mock the function and check the URL input parameters while testing.

Syntax:

```js
window.location.assign("https://geeksforgeeks.org/");
```

Example:

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="assignLocation()">Go to new webpage</button>

    <script>
      function assignLocation() {
        // Go to another webpage (geeksforgeeks)
        var newloc = "https://www.geeksforgeeks.org/";
        window.location.assign(newloc);
      }
    </script>
  </body>
</html>
```

[source](https://www.geeksforgeeks.org/difference-between-window-location-href-window-location-replace-and-window-location-assign-in-javascript/)
