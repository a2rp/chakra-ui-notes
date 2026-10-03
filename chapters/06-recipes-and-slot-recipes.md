# 6. Recipes and slot recipes

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Theme configuration and design tokens](./05-theme-configuration-and-design-tokens.md) | [Notes index](../README.md) | [Next: Forms, validation, and accessible feedback](./07-forms-validation-and-accessible-feedback.md) |

## Why recipes help

A recipe keeps a component's base appearance and its named variants together. Use one when the same visual decision appears in several places or when a component needs a small set of deliberate styles. A recipe is not needed for a one-off element with only a couple of style props.

A single-part recipe describes one styled element. A slot recipe describes several named parts that belong together, such as the root, title, and description of a custom notice.

## Create a single-part recipe

Define a recipe and register it in the system configuration:

```js
import { createSystem, defaultConfig, defineConfig, defineRecipe } from "@chakra-ui/react"

const panelRecipe = defineRecipe({
  base: {
    borderWidth: "1px",
    borderColor: "border",
    borderRadius: "lg",
    p: "5",
  },
  variants: {
    tone: {
      quiet: { bg: "bg.subtle" },
      raised: { bg: "bg.panel", boxShadow: "md" },
    },
  },
  defaultVariants: {
    tone: "quiet",
  },
})

const config = defineConfig({
  theme: {
    recipes: {
      panel: panelRecipe,
    },
  },
})

export const system = createSystem(defaultConfig, config)
```

The `base` styles apply to every variant. The `tone` variant selects one of two presentations. `defaultVariants` makes the common choice explicit when a caller does not provide one.

Use the recipe from a component:

```jsx
import { Box, useRecipe } from "@chakra-ui/react"

export function Panel({ tone, children }) {
  const recipe = useRecipe({ key: "panel" })
  const styles = recipe({ tone })

  return <Box css={styles}>{children}</Box>
}
```

The recipe key must match the key under `theme.recipes`. The caller chooses a named variant without copying the style object.

## Create a slot recipe

A slot recipe styles several pieces under one shared variant. Define the slots first, then provide a base style for each part.

```js
import { defineSlotRecipe } from "@chakra-ui/react"

export const noticeRecipe = defineSlotRecipe({
  slots: ["root", "title", "description"],
  base: {
    root: {
      borderWidth: "1px",
      borderRadius: "md",
      p: "4",
    },
    title: {
      fontWeight: "bold",
    },
    description: {
      color: "fg.muted",
      mt: "1",
    },
  },
  variants: {
    tone: {
      info: {
        root: { bg: "blue.50", borderColor: "blue.200" },
        title: { color: "blue.900" },
      },
      warning: {
        root: { bg: "orange.50", borderColor: "orange.200" },
        title: { color: "orange.900" },
      },
    },
  },
})
```

Register it under `theme.slotRecipes`:

```js
const config = defineConfig({
  theme: {
    slotRecipes: {
      notice: noticeRecipe,
    },
  },
})
```

Use the registered slots in a React component:

```jsx
import { Box, Text, useSlotRecipe } from "@chakra-ui/react"

export function Notice({ tone = "info", title, children }) {
  const recipe = useSlotRecipe({ key: "notice" })
  const styles = recipe({ tone })

  return (
    <Box css={styles.root}>
      <Text css={styles.title}>{title}</Text>
      <Text css={styles.description}>{children}</Text>
    </Box>
  )
}
```

Because the recipe uses a React hook, call it at the top level of the component. Do not call it inside a condition or a loop.

## Choose the right recipe type

Use a single-part recipe for an element whose variants affect one visual unit, such as a button or badge. Use a slot recipe when variants need coordinated changes across several related parts. Use an existing Chakra compound component when it already provides the behavior and structure you need.

Keep variant names meaningful to the interface, such as `tone`, `size`, or `emphasis`. Avoid adding combinations that are never used. A smaller set of clear choices is easier to maintain.

## Keep themes maintainable

- Register recipes once in the system configuration.
- Use semantic tokens inside recipes when the component should adapt to color mode.
- Give variants names that describe the decision callers are making.
- Keep state behavior in the component and visual decisions in its recipe.
- Check how compound variants interact with responsive values before combining them.

## Practice

1. Add a compact size variant to the panel recipe.
2. Add a success tone to the notice slot recipe.
3. Replace repeated card styles with a recipe only if those styles truly recur.
4. Verify all variants in both color modes.

## References

- [Chakra UI recipes](https://chakra-ui.com/docs/theming/recipes)
- [Chakra UI slot recipes](https://chakra-ui.com/docs/theming/slot-recipes)
- [Chakra UI customization overview](https://chakra-ui.com/docs/theming/customization/overview)
