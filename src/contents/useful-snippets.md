---
author: kuldeep
datetime: 2023-02-23
title: Useful Snippets
slug: "snippets"
featured: false
draft: false
tags:
  - snippets
ogImage: ""
description: Useful snippets that I found online
---

# Snippets

## Table of contents

## Forms

### Html form

```html
<form action="." method="post" class="hform">
  <fieldset>
    <legend>Comment form</legend>
    <p>
      <label>Name</label> <input type="text" name="name" value="" id="name" />
    </p>
    <p>
      <label>Url</label>
      <input type="text" name="address" value="" id="address" />
    </p>
    <p>
      <label>Email</label> <input type="text" name="city" value="" id="city" />
    </p>
    <p>
      <label>Comment</label>
      <textarea name="comment" rows="8" cols="40"></textarea>
    </p>
    <p class="checkbox">
      <input type="checkbox" name="remember" value="" id="remember" />
      <label>Remember</label>
    </p>
  </fieldset>
  <p><input type="submit" name="submit" value="Save" class="button" /></p>
</form>
```

### Checkout React hook form

[link](https://react-hook-form.com/get-started)

### Form in React with controlled input

```jsx
function ContactForm() {
  const [name, setName] = React.useState("");
  const [email, setEmail] = React.useState("");
  const [message, setMessage] = React.useState("");

  function handleSubmit(event) {
    event.preventDefault();
    console.log("name:", name);
    console.log("email:", email);
    console.log("message:", message);
  }

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Name</label>
        <input
          id="name"
          type="text"
          value={name}
          onChange={e => setName(e.target.value)}
        />
      </div>
      <div>
        <label htmlFor="email">Email</label>
        <input
          id="email"
          type="email"
          value={email}
          onChange={e => setEmail(e.target.value)}
        />
      </div>
      <div>
        <label htmlFor="message">Message</label>
        <textarea
          id="message"
          value={message}
          onChange={e => setMessage(e.target.value)}
        />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Form in react with uncontrolled input

```jsx
function NoRefsForm() {
  const handleSubmit = e => {
    e.preventDefault();
    const form = e.target;
    console.log("email", form.email, form.elements.email);
    console.log("name", form.name, form.elements.name);
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="email">Email address</label>
        <input id="email" name="email" />
      </div>
      <div>
        <label htmlFor="name">Full Name</label>
        <input id="name" name="name" />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Form in react using typescript (uncontrolled)

```tsx
import React from "react";

function NoRefsForm() {
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const form = e.target as HTMLFormElement;
    const formData = new FormData(form);
    const email = formData.get("email");
    const name = formData.get("name");
    console.log("email", email);
    console.log("name", name);
    form.reset();
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="email">Email address</label>
        <input id="email" name="email" />
      </div>
      <div>
        <label htmlFor="name">Full Name</label>
        <input id="name" name="name" />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}

export default NoRefsForm;
```

### Form in react with uncontrolled input using useRef

```jsx
function NoRefsForm() {
  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const form = e.currentTarget;
    console.log("email", form.email.value);
    console.log("name", form.fname.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="email">Email address</label>
        <input id="email" name="email" />
      </div>
      <div>
        <label htmlFor="fname">Full Name</label>
        <input id="fname" name="fname" />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Form in react using typesript (useRef hook)

```tsx
function ContactForm() {
  const nameRef = useRef<HTMLInputElement>(null);
  const emailRef = useRef<HTMLInputElement>(null);
  const messageRef = useRef<HTMLTextAreaElement>(null);

  return (
    <form>
      <div>
        <label htmlFor="name">Name</label>
        <input id="name" type="text" ref={nameRef} />
      </div>
      <div>
        <label htmlFor="email">Email</label>
        <input id="email" type="email" ref={emailRef} />
      </div>
      <div>
        <label htmlFor="message">Message</label>
        <textarea id="message" ref={messageRef} />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}
```

<hr/>

## How to detect click outside React component?

```tsx
function useOutsideAlerter(ref: React.RefObject<HTMLElement>) {
  useEffect(() => {
    function handleOutsideClick(event: MouseEvent) {
      if (ref.current && !ref.current.contains(event.target as Node)) {
        alert("you just clicked outside of box!");
      }
    }

    document.addEventListener("click", handleOutsideClick);
    return () => document.removeEventListener("click", handleOutsideClick);
  }, [ref]);
}
```

[source](https://www.geeksforgeeks.org/how-to-detect-click-outside-react-component/)
