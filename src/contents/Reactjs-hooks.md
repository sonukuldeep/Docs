---

author: Kuldeep
datetime: 2022-11-26
title: Reactjs hooks
slug: reactjs-hooks
featured: true
draft: false
tags:

- docs
- reactjs
  ogImage: "/assets/react.png"
  description:
  A blog post for all things related to react hooks.

---

## Table of contents

## useContext

### Example:-

_DataContext.js_

```jsx
import { createContext } from "react";

const DataContext = createContext();

export default DataContext;
```

_DataState.jsx_

```jsx
import React, { useState } from "react";
import DataContext from "./DataContext";

const DataState = props => {
  const [btnState, setBtnState] = useState(false);

  return (
    <DataContext.Provider value={{ btnState, setBtnState }}>
      {props.children}
    </DataContext.Provider>
  );
};

export default DataState;
```

_App.jsx_

```jsx
import "./App.scss";
import DataState from "./Components/DataState";

function App() {
  return (
    <div className="App">
      <DataState>
        <h3>Hello</h3>
      </DataState>
    </div>
  );
}

export default App;
```
