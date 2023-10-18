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

## Customization 
[Docs](https://v5-0-6.mui.com/customization/how-to-customize/)

### Overriding nested component styles

You can use the browser dev tools to identify the slot for the component you want to override. It can save you a lot of time. The styles injected into the DOM by MUI rely on class names that follow a simple pattern: [hash]-Mui[Component name]-[name of the slot].

⚠️ These class names can't be used as CSS selectors because they are unstable, however, MUI applies global class names using a consistent convention: Mui[Component name]-[name of the slot].


<img width="300px" src="https://v5-0-6.mui.com/static/images/customization/dev-tools.png"/>

In this example, the styles are applied with .css-ae2u5c-MuiSlider-thumb so the name of the component is Slider and the name of the slot is thumb.

You now know that you need to target the .MuiSlider-thumb class name for overriding the look of the thumb:
```jsx
    <Slider
      defaultValue={30}
      sx={{
        width: 300,
        color: 'success.main',
        '& .MuiSlider-thumb': {
          borderRadius: '1px',
        },
      }}
    />
```

### Overriding styles with class names

If you would like to override the styles of the components using classes, you can use the className prop available on each component. For overriding the styles of the different parts inside the component, you can use the global classes available for each slot, as described in the previous section.

You can find examples of this using different styles libraries in the Styles library interoperability guide.
State classes

The components special states, like hover, focus, disabled and selected, are styled with a higher CSS specificity. Specificity is a weight that is applied to a given CSS declaration.

In order to override the components' special states, you need to increase specificity. Here is an example with the disable state and the Button component using a pseudo-class (:disabled):
```css
.Button {
  color: black;
}

/* Increase the specificity */
.Button:disabled {
  color: white;
}
```

```jsx
.Button {
  color: black;
}

/* Increase the specificity */
.Button:disabled {
  color: white;
}
```

```jsx
<Button disabled className="Button">
```

Sometimes, you can't use a CSS pseudo-class, as the state doesn't exist in the web specification. Let's take the MenuItem component and its selected state as an example. In such cases you can use a MUI equivalent of CSS pseudo-classes - state classes. Target the .Mui-selected global class name to customize the special state of the MenuItem component:

```jsx
.MenuItem {
  color: black;
}

/* Increase the specificity */
.MenuItem.Mui-selected {
  color: blue;
}
```

```jsx
<MenuItem selected className="MenuItem">
```

## Customize components
### TextField
```js
    <TextField
      id="standard-basic"
      label="Standard"
      variant="outlined"
      sx={{
        width: "100px",
        "& .MuiInputBase-root": { height: "30px" },
        "& .MuiFormLabel-root": { top: "-11px" },
      }}
    />
```

### Collapse
```jsx
import * as React from "react"; 
import ListSubheader from "@mui/material/ListSubheader"; 
import List from "@mui/material/List"; 
import ListItemButton from "@mui/material/ListItemButton"; 
import ListItemIcon from "@mui/material/ListItemIcon"; 
import ListItemText from "@mui/material/ListItemText"; 
import Collapse from "@mui/material/Collapse"; 
import DraftsIcon from "@mui/icons-material/Drafts"; 
import ExpandLess from "@mui/icons-material/ExpandLess"; 
import ExpandMore from "@mui/icons-material/ExpandMore"; 

import PersonIcon from "@mui/icons-material/Person"; 
import EditIcon from "@mui/icons-material/Edit"; 
import FaceRetouchingNaturalIcon from 
	"@mui/icons-material/FaceRetouchingNatural"; 
import ArticleIcon from "@mui/icons-material/Article"; 
import LogoutIcon from "@mui/icons-material/Logout"; 

export default function NestedList() { 
	const [open, setOpen] = React.useState(true); 

	const handleClick = () => { 
		setOpen(!open); 
	}; 

	return ( 
		<> 
			<h1 style={{ color: "green" }}> 
				GeeksForGeeks</h1> 

			<List 
				sx={{ 
					width: "100%", maxWidth: 360, 
					bgcolor: "background.paper"
				}} 
				component="nav"
				aria-labelledby="nested-list-subheader"
				subheader={ 
					<ListSubheader component="div"
						id="nested-list-subheader"> 
						Setting 
					</ListSubheader> 
				} 
			> 
				<ListItemButton> 
					<ListItemIcon> 
						<PersonIcon /> 
					</ListItemIcon> 
					<ListItemText primary="My Profile" /> 
				</ListItemButton> 
				<ListItemButton> 
					<ListItemIcon> 
						<DraftsIcon /> 
					</ListItemIcon> 
					<ListItemText primary="My Courses" /> 
				</ListItemButton> 
				<ListItemButton onClick={handleClick}> 
					<ListItemIcon> 
						<EditIcon /> 
					</ListItemIcon> 
					<ListItemText primary="Edit" /> 
					{open ? <ExpandLess /> : <ExpandMore />} 
				</ListItemButton> 
				<Collapse in={open} timeout="auto" unmountOnExit> 
					<List component="div" disablePadding> 
						<ListItemButton sx={{ pl: 4 }}> 
							<ListItemIcon> 
								<FaceRetouchingNaturalIcon /> 
							</ListItemIcon> 
							<ListItemText primary="Edit Profile" /> 
						</ListItemButton> 

						<ListItemButton sx={{ pl: 4 }}> 
							<ListItemIcon> 
								<ArticleIcon /> 
							</ListItemIcon> 
							<ListItemText primary="Edit Articles" /> 
						</ListItemButton> 
					</List> 
				</Collapse> 

				<ListItemButton> 
					<ListItemIcon> 
						<LogoutIcon /> 
					</ListItemIcon> 
					<ListItemText primary="Logout" /> 
				</ListItemButton> 
			</List> 
		</> 
	); 
} 
```
