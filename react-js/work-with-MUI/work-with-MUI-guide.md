# # Overview

Material UI is a library of React UI components that implements Google's Material Design.

## ## Tech Stacks

- React 18
- [MUI v5](https://mui.com/material-ui/getting-started/overview/)

# # Pre-requests

1. You have initialised a React project following the React initialisation guide.

# # Installation

Install MUI with the default settings:

```shell
npm install @mui/material @emotion/react @emotion/styled
```

# # Custom Theme

## ## Remove default styles

- Remove `src/index.css` and `src/App.css`

## ## Custom MUI Theme

- See `src/theme` in `Example`

> You can find more guide and samples from [MUI Doc - Customization](https://mui.com/material-ui/customization/theming/), and [MUI official templates](https://mui.com/store/?utm_source=docs&utm_medium=referral&utm_campaign=sidenav)

## ## Custom Global Styles

- Custom application-scope global styles using `GlobalStyles` of MUI (See `src/theme/globalStyles.tsx`)
- Override styles of third-party components using `GlobalStyles` of MUI

# # Style Engine

> The default style library used for generating CSS styles for MUI components is emotion. All of the MUI components rely on the styled() API to inject CSS into the page. This API is supported by multiple popular styling libraries, which makes it possible to switch between them in MUI.
> 
> It is highly recommended to use [MUI System](https://mui.com/system/getting-started/overview/) ([the sx prop](https://mui.com/system/getting-started/the-sx-prop/) & [styled()](https://mui.com/system/styled/) utility) to style your components and pages.

## ## The sx Prop vs styled()

See the difference in [the official doc](https://mui.com/system/styled/#difference-with-the-sx-prop).

**When to use `sx` prop?**

- When you wish to specify the styles of a MUI component when building your own component/page, normally this styling is **no global**
- When the custom styles are **static** or **not very conditional**

**When to use `styled()`?**

- When you wish to style a component/tag that is not MUI component, normally this styling is **not global**
- When then styling is complex relying on multiple conditions (e.g. theme, props, etc.)

    > It's a good practice to put your styled component in a separate `tsx` file, and import it in other components.
