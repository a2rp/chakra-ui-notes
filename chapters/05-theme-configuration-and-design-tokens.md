# 5. Theme configuration and design tokens

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Layout and responsive design](./04-layout-and-responsive-design.md) | [Notes index](../README.md) | [Next: Recipes and slot recipes](./06-recipes-and-slot-recipes.md) |

## What the system contains

A Chakra system combines configuration, design tokens, recipes, conditions, and styling utilities. In a v3 app, you create or extend that system, then pass it to `ChakraProvider`.

Use `defaultConfig` as the base when you want the built-in component styles and tokens along with your additions. Starting from an empty configuration is a separate decision and can remove defaults your interface expects.

## Define a small brand palette

Create `src/theme.js`:

```js
import { createSystem, defaultConfig, defineConfig } from "@chakra-ui/react"

const config = defineConfig({
  theme: {
    tokens: {
      colors: {
        brand: {
          50: { value: "#eff6ff" },
          500: { value: "#2563eb" },
          700: { value: "#1d4ed8" },
          900: { value: "#172554" },
        },
      },
      fonts: {
        body: { value: "Inter, system-ui, sans-serif" },
        heading: { value: "Inter, system-ui, sans-serif" },
      },
    },
  },
})

export const system = createSystem(defaultConfig, config)
```

Token values are objects with a `value` property. The token path becomes the name used in component props, such as `brand.500`. A numbered palette gives components related choices for emphasis and contrast.

Use the custom system at the application root:

```jsx
import React from "react"
import ReactDOM from "react-dom/client"
import { ChakraProvider } from "@chakra-ui/react"
import { system } from "./theme.js"
import App from "./App.jsx"

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <ChakraProvider value={system}>
      <App />
    </ChakraProvider>
  </React.StrictMode>,
)
```

## Use semantic tokens for purpose

A raw token names a value. A semantic token names what the value means in the interface. For example, a panel background should use a panel token rather than a specific shade.

Add this inside the same `theme` configuration:

```js
semanticTokens: {
  colors: {
    panel: {
      value: {
        base: "{colors.brand.50}",
        _dark: "{colors.brand.900}",
      },
    },
    panelText: {
      value: {
        base: "{colors.gray.900}",
        _dark: "{colors.gray.50}",
      },
    },
  },
},
```

The full configuration becomes:

```js
const config = defineConfig({
  theme: {
    tokens: {
      colors: {
        brand: {
          50: { value: "#eff6ff" },
          500: { value: "#2563eb" },
          700: { value: "#1d4ed8" },
          900: { value: "#172554" },
        },
      },
    },
    semanticTokens: {
      colors: {
        panel: {
          value: { base: "{colors.brand.50}", _dark: "{colors.brand.900}" },
        },
        panelText: {
          value: { base: "{colors.gray.900}", _dark: "{colors.gray.50}" },
        },
      },
    },
  },
})
```

Use semantic tokens in components:

```jsx
import { Box, Text } from "@chakra-ui/react"

export function BrandedPanel() {
  return (
    <Box bg="panel" color="panelText" borderWidth="1px" p="5" rounded="lg">
      <Text color="brand.700" fontWeight="bold">Account overview</Text>
      <Text mt="2">The panel follows the active color mode.</Text>
    </Box>
  )
}
```

Semantic tokens can change with theme conditions while the component continues to use the same name. This prevents every component from having its own light and dark color decisions.

## Organize theme files as they grow

Start with one theme file. Split tokens, recipes, and conditions into separate modules only when the configuration becomes difficult to scan. Keep the final `defineConfig` and `createSystem` calls easy to find.

When several components share the same value, make it a token. When a value is local and unlikely to recur, a direct style value can be clearer than adding a permanent token.

## Theme mistakes to avoid

- Do not omit the `value` wrapper around custom token values.
- Do not replace a semantic token with a fixed shade if the interface supports more than one color mode.
- Do not start from an empty system accidentally when you expect the built-in Chakra defaults.
- Check text and background contrast in both light and dark appearances.
- Keep token names about purpose and usage, not a specific screen or temporary feature.

## Practice

1. Add a second brand color and use it on a button.
2. Add a semantic border token for panels.
3. Switch the color-mode provider and confirm the panel updates.
4. Search the app for repeated color literals and decide which belong in the theme.

## References

- [Chakra UI theming overview](https://chakra-ui.com/docs/theming/overview)
- [Chakra UI tokens](https://chakra-ui.com/docs/theming/tokens)
- [Chakra UI semantic tokens](https://chakra-ui.com/docs/theming/semantic-tokens)
