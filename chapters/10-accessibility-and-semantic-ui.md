# 10. Accessibility and semantic UI

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Color mode and appearance](./09-color-mode-and-appearance.md) | [Notes index](../README.md) | [Next: React state and component integration](./11-react-state-and-component-integration.md) |

## Start with meaning

Accessible interfaces work for people using different input methods, screen readers, zoom levels, and display settings. Start with correct HTML semantics, then add Chakra components and styles. A visual shape alone does not give an element the right meaning or behavior.

Use headings in a meaningful order, landmarks for page regions, links for navigation, and buttons for actions.

```jsx
import { Box, Button, Heading, Link, Stack, Text } from "@chakra-ui/react"

export default function AccountPage() {
  return (
    <Box as="main" maxW="3xl" mx="auto" p="6">
      <Stack align="start" gap="4">
        <Heading as="h1" size="2xl">Account settings</Heading>
        <Text>Update the details connected to your account.</Text>
        <Link href="/account/security">Review security settings</Link>
        <Button onClick={() => console.log("Save settings")}>Save settings</Button>
      </Stack>
    </Box>
  )
}
```

A link takes the user somewhere. A button performs an action. Do not use a clickable `div` when a native control already supports keyboard input and interaction semantics.

## Label controls

A form control needs a programmatic name. Use a visible label and Chakra's `Field` composition. Add instructions and errors close to the control they describe.

```jsx
import { Field, Input } from "@chakra-ui/react"

export function DisplayNameField() {
  return (
    <Field.Root required>
      <Field.Label>
        Display name
        <Field.RequiredIndicator />
      </Field.Label>
      <Input name="displayName" autoComplete="nickname" />
      <Field.HelperText>This name appears on your profile.</Field.HelperText>
    </Field.Root>
  )
}
```

Do not use placeholder text as the only label. A placeholder vanishes when someone enters a value and may have low contrast.

## Make focus visible

Every interactive control should be reachable and usable with a keyboard. Chakra components provide keyboard behavior for their documented interactions. Keep a visible focus treatment when changing styles.

```jsx
<Button
  _focusVisible={{
    outline: "2px solid",
    outlineColor: "teal.500",
    outlineOffset: "2px",
  }}
>
  Continue
</Button>
```

Try the interface with Tab, Shift+Tab, Enter, Space, and Escape where relevant. The focus order should follow the visual reading order. A control should not be keyboard reachable if it is hidden from sight.

## Name icon-only controls

If a button shows only an icon, give it an accessible name that describes its action.

```jsx
import { IconButton } from "@chakra-ui/react"

export function ClosePanelButton({ onClose }) {
  return (
    <IconButton aria-label="Close panel" onClick={onClose} variant="ghost">
      �
    </IconButton>
  )
}
```

Do not repeat the same generic label for controls that do different things. If a visible text label already names a control, avoid adding a conflicting accessible name.

## Announce changing status

After an action, communicate what happened. A status region can announce a message without forcing focus to move.

```jsx
import { Button, Stack } from "@chakra-ui/react"
import { useState } from "react"

export function SaveAction() {
  const [status, setStatus] = useState("")

  function save() {
    setStatus("Your changes have been saved.")
  }

  return (
    <Stack align="start" gap="3">
      <Button onClick={save}>Save changes</Button>
      {status ? <p role="status">{status}</p> : null}
    </Stack>
  )
}
```

Use an error message when an action fails and explain what the person can do next. Do not communicate state with color alone; pair color with text, an icon that has a name, or another clear signal.

## Accessibility checklist

- Use native links, buttons, headings, lists, and landmarks.
- Give inputs persistent labels and useful error messages.
- Preserve keyboard access and visible focus.
- Make icon-only controls understandable without their icon.
- Keep text readable against its background in each color mode.
- Test overlays and menus without a pointer.
- Check the page at high zoom and on a narrow viewport.

Automated checks can find some markup problems, but manual keyboard and screen-reader testing still matter. Follow the relevant accessibility standard for the application and audience.

## Practice

1. Navigate the page using only a keyboard.
2. Replace a clickable generic element with the correct native control.
3. Add a visible label, helper text, and invalid message to a form field.
4. Review color contrast and focus indication in both color modes.

## References

- [Chakra UI accessibility](https://chakra-ui.com/docs/components/concepts/accessibility)
- [Chakra UI Field](https://chakra-ui.com/docs/components/field)
- [W3C Web Accessibility Initiative](https://www.w3.org/WAI/fundamentals/accessibility-intro/)
