---
author: kuldeep
datetime: 2023-09-03
title: Material UI Components
slug: "mui"
featured: false
draft: false
tags:
  - mui material-ui
ogImage: ""
description: Material ui setup and reusable components
---

# MUI 

## Table of Contents

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
