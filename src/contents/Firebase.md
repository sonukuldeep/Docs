---
title: Firebase
author: Kuldeep
datetime: 2023-01-04
slug: firebase
featured: false
draft: false
tags:
  - firebase
  - react
description: Firebase doc
---

![cover image](https://firebase.google.com/images/social.png)

# Firebase

## Table of contents

## Configuration

### Install

```jsx
npm i firebase
```

### Create Project

Create a project by visiting this [link](https://console.firebase.google.com/)

### Firebase Config file

Go to Project Overview -> Project setting and copy the config to Firebase/Firebase-config.js

```jsx
import { initializeApp } from "firebase/app";

const firebaseConfig = {
  apiKey: "AIzaS...",
  authDomain: "sho...",
  projectId: "sho...",
  storageBucket: "sho...",
  messagingSenderId: "981...",
  appId: "1:98...",
};

export const FirebaseApp = initializeApp(firebaseConfig); /* export this */
```

<hr>

## Sign In

### How to use

In order to use firebase we first need to import exported firebase config and few other things

```jsx
import { FirebaseApp } from "somewhere/Firebase/Firebase-config.js";
import {
  getAuth,
  onAuthStateChanged,
  signInWithPopup,
  signInWithRedirect,
  getRedirectResult,
  GoogleAuthProvider,
  signOut,
} from "firebase/auth";
```

<hr>

### Initialize app and provider

_Out side the component_

```jsx
const auth = getAuth(FirebaseApp);
const GoogleProvider = new GoogleAuthProvider();
```

... Other providers can also be initialized similarly... check [Facebook](https://firebase.google.com/docs/auth/web/facebook-login) [Microsoft](https://firebase.google.com/docs/auth/web/microsoft-oauth)

<hr>

### Auth state

Recommended component

_Outside the component or within useEffect_

```jsx
onAuthStateChanged(auth, user => {
  if (user) {
    const uid = user.uid;
    // Do something
    console.log({ uid });
    // ...
  } else {
    // User is signed out
    // ...
  }
});
```

[source](https://firebase.google.com/docs/auth/web/start#set_an_authentication_state_observer_and_get_user_data)

<hr>

### Sigin with redirect(recommended)

```jsx
const signInHandler = () => {
  signInWithRedirect(auth, GoogleProvider);
};
```

If we use redirect then we also need to have _getRedirectResult_ or have auth state checked with _onAuthStateChanged_

_Out side the component or within useEffect_ in the later case you can use hook(useNavigate) to redirect to homepage after successful login

```jsx
getRedirectResult(auth)
  .then(result => {
    // This gives you a Google Access Token. You can use it to access Google APIs.
    const credential = GoogleAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;

    // The signed-in user info.
    const user = result.user;
    console.log({ user, token });
    redirect("/");
  })
  .catch(error => {
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error?.message;
    // The email of the user's account used.
    const email = error.customData?.email;
    // The AuthCredential type that was used.
    const credential = GoogleAuthProvider.credentialFromError(error);
    // ...
    console.log({ errorCode, errorMessage, email, credential });
  });
```

[source](https://firebase.google.com/docs/auth/web/google-signin)

<hr>

### Sigin In with popup

```jsx
signInWithPopup(auth, GoogleProvider)
  .then(result => {
    // This gives you a Google Access Token. You can use it to access the Google API.
    const credential = GoogleAuthProvider.credentialFromResult(result);
    const token = credential.accessToken;
    // The signed-in user info.
    const user = result.user;
    // ...
  })
  .catch(error => {
    // Handle Errors here.
    const errorCode = error.code;
    const errorMessage = error.message;
    // The email of the user's account used.
    const email = error.customData.email;
    // The AuthCredential type that was used.
    const credential = GoogleAuthProvider.credentialFromError(error);
    // ...
  });
```

[source](https://firebase.google.com/docs/auth/web/google-signin?authuser=0#handle_the_sign-in_flow_with_the_firebase_sdk)

<hr>

### SiginOut user

```jsx
const signOutUser = () => {
  signOut(auth)
    .then(() => {
      // Sign-out successful.
      console.log("sigin out successful");
    })
    .catch(error => {
      // An error happened.
      console.log("encountered error during sigin out ", error);
    });
};
```

[source](https://firebase.google.com/docs/auth/web/password-auth#next_steps)

<hr>

## Firebase cloud

### Initialize

```jsx
// Initialize Cloud Firestore and get a reference to the service
const db = getFirestore(FirebaseApp); //const db = getFirestore(app); same thing
```

This can be initialized in firebase config file or in the file where you want to use cloud db functions

### Read data

```jsx
import { getFirestore, doc, setDoc, getDoc } from "firebase/firestore";

const populateUserInDB = async uid => {
  const userRef = doc(db, "users", uid); // reference uid document, containing the data, in "users" collection within database
  const userSnap = await getDoc(userRef); // read data from ref object
  if (userSnap.exists()) {
    //data exist so do something
    console.log("Document data:", userSnap.data());
  } else {
    // doc.data() will be undefined in this case
    console.log("No such document!, hence creating data");
  }
};
```

<hr>

### Write data

```jsx
import { doc, setDoc } from "firebase/firestore";

const docRef = doc(db, "cities", "LA");
const data = {
  name: "Los Angeles",
  state: "CA",
  country: "USA",
};
// Add a new document in collection "cities"
await setDoc(docRef, data);
```

[source](https://firebase.google.com/docs/firestore/manage-data/add-data?authuser=0#set_a_document)

<hr>

### Update data

```jsx
import { doc, updateDoc } from "firebase/firestore";

const washingtonRef = doc(db, "cities", "DC");

// Set the "capital" field of the city 'DC'
await updateDoc(washingtonRef, {
  capital: true,
});
```

[source](https://firebase.google.com/docs/firestore/manage-data/add-data?authuser=0#update-data)
