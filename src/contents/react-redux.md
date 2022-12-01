---
author: Kuldeep
datetime: 2022-12-01
title: Redux react library
slug: reactjs-redux
featured: true
draft: false
tags:
  - redux
  - reactjs
ogImage: "/assets/redux.png"
description: A redux example.
---

![Redux](/assets/redux.png)

### Dependencies

```bash
npm install react-redux redux
```

### Insert the following as below

_index.js_

```bash
  <StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </StrictMode>
```

_App.js_

```bash
import "./styles.css";
import { useSelector, useDispatch } from "react-redux";
import { bindActionCreators } from "redux";
import { actionCreators } from "./state/index";
export default function App() {
  const state = useSelector((state) => state);
  const dispatch = useDispatch();
  const { depositMoney, withdrawMoney } = bindActionCreators(
    actionCreators,
    dispatch
  );

  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <p>{state.account}</p>
      <button onClick={() => depositMoney(1000)}>Deposit money</button>
      <button onClick={() => withdrawMoney(1000)}>Withdraw money</button>
    </div>
  );
}
```

_state/index.js_

```bash
export * as actionCreators from "./action-creators/index";
```

_state/store.js_

```bash
import { createStore, applyMiddleware } from "redux";
import reducers from "./reducers/index";
import thunk from "redux-thunk";
export const store = createStore(reducers, {}, applyMiddleware(thunk));
```

_state/action-creators/index.js_

```bash
export const depositMoney = (amount) => {
  return (dispatch) => {
    dispatch({
      type: "deposit",
      payload: amount
    });
  };
};

export const withdrawMoney = (amount) => {
  return (dispatch) => {
    dispatch({
      type: "withdraw",
      payload: amount
    });
  };
};
```

_state/reducers/accountReducer.js_

```bash
const reducer = (state = 0, action) => {
  switch (action.type) {
    case "deposit":
      return state + action.payload;
    case "withdraw":
      return state - action.payload;
    default:
      return state;
  }
};

export default reducer;
```

_state/reducers/index.js_

```bash
import { combineReducers } from "redux";
import accountReducer from "./accountReducer";

const reducers = combineReducers({
  account: accountReducer
});

export default reducers;
```
