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

<img width="500px" src="https://res.cloudinary.com/practicaldev/image/fetch/s--wAw1dBzS--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://thepracticaldev.s3.amazonaws.com/i/vb6ai56xqgpc0bcfn92y.png"/>

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

<hr>

React- form with checkbox

```jsx
import React from 'react';
import { useForm, RegisterOptions } from 'react-hook-form';
import TextField from '@mui/material/TextField';
import Button from '@mui/material/Button';
import FormControlLabel from '@mui/material/FormControlLabel';
import Checkbox from '@mui/material/Checkbox';
import FormHelperText from '@mui/material/FormHelperText'; // Add import for FormHelperText

interface FormData {
  name: string;
  email: string;
  password: string;
  message: string;
  acceptTerms: boolean;
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
      {/* Name field */}
      <TextField
        label="Name"
        variant="outlined"
        {...register('name', {
          required: 'This field is required',
        } as RegisterOptions)}
        error={!!errors.name}
        helperText={errors.name?.message}
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
        error={!!errors.email}
        helperText={errors.email?.message}
      />

      {/* Password field */}
      <TextField
        label="Password"
        variant="outlined"
        type="password"
        {...register('password', {
          required: 'This field is required',
        } as RegisterOptions)}
        error={!!errors.password}
        helperText={errors.password?.message}
      />

      {/* Message textarea field */}
      <TextField
        label="Message"
        variant="outlined"
        multiline
        rows={4}
        {...register('message', {
          required: 'This field is required',
        } as RegisterOptions)}
        error={!!errors.message}
        helperText={errors.message?.message}
      />

      {/* Checkbox field */}
      <FormControlLabel
        control={
          <Checkbox
            {...register('acceptTerms', {
              required: 'You must accept the terms and conditions',
            } as RegisterOptions)}
            color="primary"
          />
        }
        label="I accept the terms and conditions"
      />

      {/* Error message for checkbox */}
      {errors.acceptTerms && (
        <FormHelperText error>{errors.acceptTerms.message}</FormHelperText>
      )}

      {/* Submit button */}
      <Button type="submit" variant="contained" color="primary">
        Submit
      </Button>
    </form>
  );
};

export default MyForm;
```

<hr>

## React- form with checkbox and dropdown

Error msg in drop down doesnot go away in first attempt

```jsx
import React from 'react';
import { useForm, RegisterOptions } from 'react-hook-form';
import TextField from '@mui/material/TextField';
import Button from '@mui/material/Button';
import FormControlLabel from '@mui/material/FormControlLabel';
import Checkbox from '@mui/material/Checkbox';
import FormHelperText from '@mui/material/FormHelperText'; // Add import for FormHelperText
import Box from '@mui/material/Box';
import MenuItem from '@mui/material/MenuItem';
import FormControl from '@mui/material/FormControl';
import InputLabel from '@mui/material/InputLabel';
import Select from '@mui/material/Select';

interface FormData {
    name: string;
    email: string;
    password: string;
    message: string;
    acceptTerms: boolean;
    country: string;
}

const MyForm: React.FC = () => {
    const {
        register,
        handleSubmit,
        formState: { errors },
    } = useForm<FormData>();

    const onSubmit = (data: FormData) => {
        console.log(data);
    };

    return (
        <form onSubmit={handleSubmit(onSubmit)}>
            <Box sx={{ display: 'flex', flexDirection: 'column', gap: '10px' }}>
                {/* Name field */}
                <TextField
                    label="Name"
                    variant="outlined"
                    {...register('name', {
                        required: 'This field is required',
                    } as RegisterOptions)}
                    error={!!errors.name}
                    helperText={errors.name?.message}
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
                    error={!!errors.email}
                    helperText={errors.email?.message}
                />

                {/* Password field */}
                <TextField
                    label="Password"
                    variant="outlined"
                    type="password"
                    {...register('password', {
                        required: 'This field is required',
                    } as RegisterOptions)}
                    error={!!errors.password}
                    helperText={errors.password?.message}
                />

                {/* Message textarea field */}
                <TextField
                    label="Message"
                    variant="outlined"
                    multiline
                    rows={4}
                    {...register('message', {
                        required: 'This field is required',
                    } as RegisterOptions)}
                    error={!!errors.message}
                    helperText={errors.message?.message}
                />
                {/* Country dropdown field */}
                <FormControl variant="outlined" fullWidth error={Boolean(errors.country)}>
                    <InputLabel htmlFor="country-label">Country</InputLabel>
                    <Select
                        labelId="country-label"
                        label="Country"
                        {...register('country', { required: 'This field is required' })}
                    >
                        <MenuItem value="USA">USA</MenuItem>
                        <MenuItem value="Canada">Canada</MenuItem>
                        <MenuItem value="UK">UK</MenuItem>
                        <MenuItem value="Australia">Australia</MenuItem>
                    </Select>
                    {errors.country && (
                        <FormHelperText error>{errors.country.message}</FormHelperText>
                    )}
                </FormControl>
                {/* Checkbox field */}
                <FormControlLabel
                    control={
                        <Checkbox
                            {...register('acceptTerms', {
                                required: 'You must accept the terms and conditions',
                            } as RegisterOptions)}
                            color="primary"
                        />
                    }
                    label="I accept the terms and conditions"
                />

                {/* Error message for checkbox */}
                {errors.acceptTerms && (
                    <FormHelperText error>{errors.acceptTerms.message}</FormHelperText>
                )}

                {/* Submit button */}
                <Button type="submit" variant="contained" color="primary">
                    Submit
                </Button>
            </Box>
        </form>
    );
};

export default MyForm;
```

<hr/>

## Navbar

`Header.tsx`

```ts
import {
  AppBar,
  Box,
  Button,
  Divider,
  Drawer,
  IconButton,
  SxProps,
  Toolbar,
  Typography,
} from "@mui/material";
import { Fastfood, Menu } from "@mui/icons-material";
import { NavLink } from "react-router-dom";
import { useState } from "react";

function Header() {
  const [openDrawer, setOpenDrawer] = useState(false);

  return (
    <Box>
      <AppBar component={"nav"} sx={{ bgcolor: "black" }}>
        <Toolbar>
          {/* hambergan icon */}
          <IconButton
            color="inherit" // it stops animating if color is not provided
            aria-label={"open-drawer"}
            edge="start"
            sx={styles.icon}
            onClick={() => setOpenDrawer(pre => !pre)}
          >
            <Menu sx={{ color: "goldenrod" }} />
          </IconButton>
          {/* Nav heading and icon */}
          <Typography
            color={"goldenrod"}
            variant="h6"
            component={"div"}
            sx={{ flexGrow: 1 }}
          >
            <Fastfood /> My Restaurant
          </Typography>
          <Box sx={styles.navLinkContainer}>
            {/* navlinks */}
            <NavbarWrapper onclick={() => {}} />
          </Box>
        </Toolbar>
      </AppBar>
      <Drawer
        variant="temporary"
        open={openDrawer}
        onClose={() => setOpenDrawer(pre => !pre)}
        sx={styles.mobileNavLinkContainer}
      >
        <Typography variant="h5" sx={{ textAlign: "center", margin: 1 }}>
          Menu
        </Typography>
        <Divider></Divider>
        <NavbarWrapper onclick={() => setOpenDrawer(pre => !pre)} />
      </Drawer>
      <Box>
        <Toolbar /> {/* works as a spacer  */}
      </Box>
    </Box>
  );
}

export default Header;

function NavbarWrapper({ onclick }: { onclick: () => void }) {
  return (
    <>
      <Button
        component={NavLink} // NavLink adds an active class to the component which can be targeted in css
        to="/"
        onClick={onclick}
      >
        Home
      </Button>
      <Button component={NavLink} to="/menu" onClick={onclick}>
        Menu
      </Button>
      <Button component={NavLink} to="/about" onClick={onclick}>
        About
      </Button>
      <Button component={NavLink} to="/contact" onClick={onclick}>
        Contact
      </Button>
    </>
  );
}

type StylesProps = {
  [key: string]: SxProps;
};
const styles: StylesProps = {
  navLinkContainer: {
    display: { xs: "none", sm: "flex" },
    gap: 5,
    listStyle: "none",
    textDecoration: "none",
    "& a": {
      color: "goldenrod",
    },
    "& a.active": {
      color: "blueviolet",
    },
  },
  mobileNavLinkContainer: {
    display: { xs: "block", sm: "none" },
    "& .MuiDrawer-paper": {
      boxSizing: "border-box",
      width: "200px",
      "& a": {
        color: "goldenrod",
      },
      "& a.active": {
        color: "blueviolet",
      },
    },
  },
  icon: {
    mr: 2,
    display: { sm: "none" },
  },
};
```

<hr/>

## Theme

Setup
`src/App.tsx`

```tsx
//...other imports
import { CssBaseline, SxProps, ThemeProvider } from "@mui/material";
import theme from "./lib/theme";
function App() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      {/* other stuff here */}
    </ThemeProvider>
  );
}
```

`src/lib/theme.ts` - there are certain limitation to where customColors will work so check before use

```ts
import { createTheme } from "@mui/material";
import { green, grey, indigo } from "@mui/material/colors";

declare module "@mui/material/styles" {
  interface Palette {
    customNeutral: {
      light: string;
      medium: string;
      normal: string;
      main: string;
    };
    customGreen: Palette["primary"];
  }

  interface PaletteOptions {
    customNeutral?: {
      light: string;
      medium: string;
      normal: string;
      main: string;
    };
    customGreen?: PaletteOptions["primary"];
  }
}

declare module "@mui/material/styles" {
  interface TypographyVariants {
    link: React.CSSProperties;
    cardTitle: React.CSSProperties;
    h6: React.CSSProperties;
    h7: React.CSSProperties;
    h8: React.CSSProperties;
  }

  interface TypographyVariantsOptions {
    link?: React.CSSProperties;
    cardTitle?: React.CSSProperties;
    h6?: React.CSSProperties;
    h7?: React.CSSProperties;
    h8?: React.CSSProperties;
  }
}

declare module "@mui/material/Typography" {
  interface TypographyPropsVariantOverrides {
    link: true;
    cardTitle: true;
    h6: true;
    h7: true;
    h8: true;
  }
}

let theme = createTheme({
  palette: {
    primary: {
      main: indigo[500],
      light: indigo["A700"],
    },
    secondary: {
      main: indigo[50],
    },
    customNeutral: {
      light: grey[50],
      medium: grey[200],
      normal: grey[700],
      main: grey[900],
    },
    customGreen: {
      main: green[800],
    },
  },
});

theme = createTheme(theme, {
  typography: {
    link: {
      fontSize: "0.8rem",
      fontWeight: 500,
      color: theme.palette.primary.main,
      display: "block",
      cursor: "pointer",
      [theme.breakpoints.up("md")]: {
        fontSize: "0.9rem",
      },
    },
    cardTitle: {
      fontSize: "1.2rem",
      display: "block",
      fontWeight: 500,
    },
    h6: {
      fontSize: "1rem",
    },
    h7: {
      fontSize: "0.8rem",
    },
    h8: {
      fontSize: "0.7rem",
    },
  },
});

export default theme;
```
