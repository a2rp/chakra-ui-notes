# 3. Style props and conditional styles

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Component anatomy and composition](./02-component-anatomy-and-composition.md) | [Notes index](../README.md) | [Next: Layout and responsive design](./04-layout-and-responsive-design.md) |

## Style props map to CSS

Chakra style props let you express common CSS properties directly on a component. Short aliases reduce repeated syntax: `bg` sets the background, `p` sets padding, `mt` sets top margin, and `rounded` sets border radius.

```jsx
import { Box, Text } from "@chakra-ui/react"

export function Notice() {
  return (
    <Box bg="blue.50" borderWidth="1px" borderColor="blue.200" p="4" rounded="md">
      <Text color="blue.900" fontWeight="medium">
        Your settings have been saved.
      </Text>
    </Box>
  )
}
```

Use the complete property when it reads more clearly. For example, `padding` may be easier to understand than `p` in a complex component. Chakra also accepts regular CSS property names as style props.

## Prefer tokens for shared visual values

Chakra's built-in and custom design tokens represent repeatable values such as color, spacing, font size, and radius. Use them so related components stay consistent.

```jsx
import { Box, Text } from "@chakra-ui/react"

export function ProfilePanel() {
  return (
    <Box bg="bg.panel" color="fg" borderColor="border" borderWidth="1px" p="6" rounded="lg">
      <Text color="fg.muted">Account</Text>
      <Text fontSize="xl" fontWeight="bold">Ashish Ranjan</Text>
    </Box>
  )
}
```

The semantic tokens `bg.panel`, `fg`, `fg.muted`, and `border` describe purpose instead of a fixed shade. They can adapt when the active color mode changes. Chapter 5 covers how to define project tokens.

For a one-off value, a direct CSS value can be useful. If the same value appears in several places or should change with the theme, make it a token.

## Style interaction states

Conditional style props begin with an underscore and target a UI state. They are useful for hover, pressed, focus-visible, disabled, invalid, and selected states.

```jsx
import { Button } from "@chakra-ui/react"

export function ContinueButton({ disabled, onContinue }) {
  return (
    <Button
      colorPalette="teal"
      disabled={disabled}
      onClick={onContinue}
      _hover={{ bg: "teal.700" }}
      _active={{ bg: "teal.800" }}
      _focusVisible={{ outline: "2px solid", outlineColor: "teal.500", outlineOffset: "2px" }}
      _disabled={{ opacity: "0.55", cursor: "not-allowed" }}
    >
      Continue
    </Button>
  )
}
```

Keep a visible focus style. Keyboard users need to know which control is active. Use native state such as `disabled` on the component so both browser behavior and styling reflect the same state.

## Apply styles based on data

A conditional style can reflect a state attribute. This helps when a component or your own markup already exposes a state through a data or ARIA attribute.

```jsx
import { Box } from "@chakra-ui/react"

export function SyncStatus({ syncing }) {
  return (
    <Box
      data-loading={syncing ? "" : undefined}
      color="fg.muted"
      _loading={{ color: "blue.700", fontWeight: "semibold" }}
    >
      {syncing ? "Syncing your changes" : "All changes saved"}
    </Box>
  )
}
```

For application state, it is often simpler to choose styles in JavaScript:

```jsx
<Box color={isError ? "red.700" : "green.700"}>
  {isError ? "Check the form" : "Looks good"}
</Box>
```

Use the pattern that makes the relationship easiest to read. Avoid duplicating the same state in unrelated attributes and props.

## Add a small custom selector

When there is no suitable style prop, the `css` prop accepts a CSS style object. Use it for a specific selector or property, not as a reason to move every style into a large inline object.

```jsx
import { Box } from "@chakra-ui/react"

export function IconTile({ children }) {
  return (
    <Box
      display="flex"
      alignItems="center"
      gap="3"
      css={{
        "& > svg": {
          flexShrink: 0,
        },
      }}
    >
      {children}
    </Box>
  )
}
```

## Avoid common styling problems

- Use the component's documented style prop names and values.
- Prefer tokens for shared colors, spacing, and sizes.
- Do not remove focus indication without replacing it with a clear visible style.
- Apply a real `disabled` or `aria-invalid` state when the control is actually disabled or invalid.
- Keep complex selectors in a CSS file when they become hard to understand inline.
- Check whether a color is readable in every color mode used by the page.

## Practice

1. Add hover, focus-visible, and disabled styles to a button.
2. Replace a hard-coded gray with a semantic foreground or background token.
3. Create a notice with success and error states.
4. Use browser keyboard navigation to verify the focus style.

## References

- [Chakra UI style props](https://chakra-ui.com/docs/styling/style-props)
- [Chakra UI conditional styles](https://chakra-ui.com/docs/styling/conditional-styles)
- [Chakra UI colors](https://chakra-ui.com/docs/theming/colors)
