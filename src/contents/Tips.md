---
author: Kuldeep
datetime: 2023-01-01
title: Tips
slug: "tips"
featured: false
draft: false
tags:
  - react
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
