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
