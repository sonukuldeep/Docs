---
author: kuldeep
datetime: 2023-02-23
title: Useful Stuff
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

### Multi step form in react/nextjs using typesript

```tsx
import React, { useState } from "react";

interface FormData {
  field1: string;
  field2: string;
  field3: string;
}

export default function MultiStepForm() {
  const [step, setStep] = useState(1);
  const [formData, setFormData] = useState<FormData>({
    field1: "",
    field2: "",
    field3: "",
  });

  const handleNext = () => {
    setStep(step + 1);
  };

  const handlePrevious = () => {
    setStep(step - 1);
  };

  const handleSubmit = () => {
    // Handle form submission, e.g., send data to server
    console.log("Form submitted:", formData);
  };

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = e.target;
    setFormData(prevData => ({
      ...prevData,
      [name]: value,
    }));
  };

  return (
    <div className="pt-20 max-w-5xl mx-auto">
      {step === 1 && (
        <Step1
          onNext={handleNext}
          handleChange={handleChange}
          formData={formData}
        />
      )}
      {step === 2 && (
        <Step2
          onNext={handleNext}
          onPrevious={handlePrevious}
          handleChange={handleChange}
          formData={formData}
        />
      )}
      {step === 3 && (
        <Step3
          onSubmit={handleSubmit}
          onPrevious={handlePrevious}
          handleChange={handleChange}
          formData={formData}
        />
      )}
      {/* <pre>{JSON.stringify(formData, null, 2)}</pre> */}
    </div>
  );
}

// Step 1
function Step1({
  onNext,
  handleChange,
  formData,
}: {
  onNext: () => void;
  handleChange: (e: React.ChangeEvent<HTMLInputElement>) => void;
  formData: FormData;
}) {
  return (
    <div className="flex flex-col items-center">
      <h2>Step 1 of 3</h2>
      <input
        type="text"
        name="field1"
        placeholder="Field 1"
        value={formData.field1}
        onChange={handleChange}
      />
      <button onClick={onNext} disabled={formData.field1.length === 0}>
        Next
      </button>
    </div>
  );
}

// Step 2
function Step2({
  onNext,
  onPrevious,
  handleChange,
  formData,
}: {
  onNext: () => void;
  onPrevious: () => void;
  handleChange: (e: React.ChangeEvent<HTMLInputElement>) => void;
  formData: FormData;
}) {
  return (
    <div className="flex flex-col items-center">
      <h2>Step 2 of 3</h2>
      <input
        type="text"
        name="field2"
        placeholder="Field 2"
        value={formData.field2}
        onChange={handleChange}
      />
      <button onClick={onPrevious}>Previous</button>
      <button onClick={onNext} disabled={formData.field2.length === 0}>
        Next
      </button>
    </div>
  );
}

// Step 3
function Step3({
  onSubmit,
  onPrevious,
  handleChange,
  formData,
}: {
  onSubmit: () => void;
  onPrevious: () => void;
  handleChange: (e: React.ChangeEvent<HTMLInputElement>) => void;
  formData: FormData;
}) {
  return (
    <div className="flex flex-col items-center">
      <h2>Step 3 of 3</h2>
      <input
        type="text"
        name="field3"
        placeholder="Field 3"
        value={formData.field3}
        onChange={handleChange}
      />
      <div className="flex">
        <button onClick={onPrevious}>Previous</button>
        <button onClick={onSubmit} disabled={formData.field3.length === 0}>
          Submit
        </button>
      </div>
    </div>
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

<hr>

## Detect click outside the element
```tsx
import React, { useEffect, useRef, useState } from 'react';

function App() {
  const [isClickOutside, setIsClickOutside] = useState(false);
  const targetRef = useRef<HTMLDivElement | null>(null);

  useEffect(() => {
    // Function to handle the click event
    function handleClickOutside(event: MouseEvent) {
      if (targetRef.current && !targetRef.current.contains(event.target as Node)) {
        setIsClickOutside(true);
      } else {
        setIsClickOutside(false);
      }
    }

    // Attach the event listener when the component mounts
    document.addEventListener('click', handleClickOutside);

    // Remove the event listener when the component unmounts
    return () => {
      document.removeEventListener('click', handleClickOutside);
    };
  }, []);

  return (
    <div>
      <div ref={targetRef}>
        <p>Click inside this element</p>
      </div>
      {isClickOutside && <p>Click outside the element!</p>}
    </div>
  );
}

export default App;
```

<hr/>

## Prisma setup in Node js

[main image](https://logos-world.net/wp-content/uploads/2022/04/Prisma-Logo.png)

Installation

- Install express, typescript, etc
- Install prisma using the following commands "npm i -D prisma"
- Install prisma client with "npm i @prisma/client"
- Initialize prisma with "npx prisma init --datasource-provider sqlite" or mysql or postgresql etc
- This will give us .env file and prisma/schema.prisma
- Prisma/schema.prisma is where you add all your schema
- When you are done adding schema run "npx prisma migrate dev --name init"
- This creates a migration which help us work with db
- If you ever need to generate prisma client again do "npx prisma generate"
- This completes prisma setup in node js

For more details check Prisma on Next js or prisma docs [here](https://www.prisma.io/docs/getting-started)

## Prisma on Next js

[link](https://kuldeep-docs.netlify.app/posts/nextjs/#prisma)

<hr/>

## Custom events in js

Custom event can be created using

- new CustomEvent()
- new Event()

CustomEvent lets you pass data while Event doesn't

### Syntax for CustomEvent

```js
// Define the custom event with data
const customEventData = { message: "Hello, this is custom data!" };
const customEvent = new CustomEvent("myCustomEvent", {
  detail: customEventData,
});

// Add an event listener to respond to the custom event
document.addEventListener("myCustomEvent", function (event) {
  console.log("Custom event triggered with data:", event.detail);
});

// Trigger the custom event
document.dispatchEvent(customEvent);

// Trigger custom event with new data
customEvent.detail = { message: "Hello, this is custom data 2!" };
document.dispatchEvent(customEvent);
```

### Syntax for Event

```js
// Define the custom event
const customEvent = new Event("myCustomEvent");
// Add an event listener to respond to the custom event
document.addEventListener("myCustomEvent", function (event) {
  console.log("Custom event triggered!", event);
});

// Trigger the custom event
document.dispatchEvent(customEvent);
// Define the custom event
const customEvent = new Event("myCustomEvent");

// Add an event listener to respond to the custom event
document.addEventListener("myCustomEvent", function (event) {
  console.log("Custom event triggered!", event);
});

// Trigger the custom event
document.dispatchEvent(customEvent);
```

## React form with FormData 
```tsx
import React, { FormEvent } from 'react';

const MyForm: React.FC = () => {
  const handleSubmit = async (event: FormEvent) => {
    event.preventDefault();

    const form = event.target as HTMLFormElement;
    const formData = new FormData(form);

    try {
      const response = await fetch('YOUR_API_ENDPOINT', {
        method: 'POST',
        body: formData,
      });

      if (response.ok) {
        // Request was successful, handle the response here.
        console.log('Form data submitted successfully');
      } else {
        // Request failed, handle the error here.
        console.error('Failed to submit form data');
      }
    } catch (error) {
      // Handle network or other errors here.
      console.error('Error:', error);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Name</label>
        <input type="text" id="name" name="name" />
      </div>
      <div>
        <label htmlFor="email">Email</label>
        <input type="email" id="email" name="email" />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
};

export default MyForm;
```
