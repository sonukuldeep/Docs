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
}

const MyForm: React.FC = () => {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm<FormData>();

  const onSubmit = (data: FormData) => {
    // Handle form submission
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <TextField
        label="Name"
        variant="outlined"
        {...register('name', {
          required: 'This field is required',
        } as RegisterOptions)} // Use RegisterOptions for additional type safety
        error={!!errors.name}
        helperText={errors.name?.message}
      />
      <TextField
        label="Email"
        variant="outlined"
        {...register('email', {
          required: 'This field is required',
          pattern: {
            value: /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i,
            message: 'Invalid email address',
          },
        } as RegisterOptions)} // Use RegisterOptions for additional type safety
        error={!!errors.email}
        helperText={errors.email?.message}
      />
      <TextField
        label="Password"
        variant="outlined"
        type="password"
        {...register('password', {
          required: 'This field is required',
        } as RegisterOptions)} // Use RegisterOptions for additional type safety
        error={!!errors.password}
        helperText={errors.password?.message}
      />
      <Button type="submit" variant="contained" color="primary">
        Submit
      </Button>
    </form>
  );
};

export default MyForm;
```
