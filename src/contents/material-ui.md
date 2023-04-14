---
author: internat
datetime: 2023-04-14
title: Material-ui
slug: material-ui
featured: false
draft: false
tags:
  - react
  - material-ui
ogImage: ""
description: Hard to find solution from material ui
---

# Material ui

![main image](https://res.cloudinary.com/practicaldev/image/fetch/s--wAw1dBzS--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://thepracticaldev.s3.amazonaws.com/i/vb6ai56xqgpc0bcfn92y.png)

## Table of Contents

## Form- react-hook-form

```jsx
import React from 'react';
import { useForm, RegisterOptions } from 'react-hook-form';
import TextField from '@mui/material/TextField';
import Button from '@mui/material/Button';

interface FormData {
  name: string;
  email: string;
  password: string;
  message: string;
}

const MyForm: React.FC = () => {
  const {
    register, // Register function from react-hook-form
    handleSubmit, // Submit handler from react-hook-form
    formState: { errors }, // Errors object from react-hook-form
  } = useForm<FormData>(); // Initialize useForm hook with FormData type as generic

  const onSubmit = (data: FormData) => {
    // Handle form submission
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Name field */}
      <TextField
        label="Name"
        variant="outlined"
        {...register('name', {
          required: 'This field is required',
        } as RegisterOptions)}
        error={!!errors.name} // Check if errors exist for 'name' field
        helperText={errors.name?.message} // Display error message for 'name' field
      />

      {/* Email field */}
      <TextField
        label="Email"
        variant="outlined"
        {...register('email', {
          required: 'This field is required',
          pattern: {
            value: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i,
            message: 'Invalid email address',
          },
        } as RegisterOptions)}
        error={!!errors.email} // Check if errors exist for 'email' field
        helperText={errors.email?.message} // Display error message for 'email' field
      />

      {/* Password field */}
      <TextField
        label="Password"
        variant="outlined"
        type="password" // Set input type to password
        {...register('password', {
          required: 'This field is required',
        } as RegisterOptions)}
        error={!!errors.password} // Check if errors exist for 'password' field
        helperText={errors.password?.message} // Display error message for 'password' field
      />

      {/* Message textarea field */}
      <TextField
        label="Message"
        variant="outlined"
        multiline // Set multiline prop to true for textarea
        rows={4} // Set number of rows for textarea
        {...register('message', {
          required: 'This field is required',
        } as RegisterOptions)}
        error={!!errors.message} // Check if errors exist for 'message' field
        helperText={errors.message?.message} // Display error message for 'message' field
      />

      {/* Submit button */}
      <Button type="submit" variant="contained" color="primary">
        Submit
      </Button>
    </form>
  );
};

export default MyForm;

```
