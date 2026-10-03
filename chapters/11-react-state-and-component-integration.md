# 11. React state and component integration

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Accessibility and semantic UI](./10-accessibility-and-semantic-ui.md) | [Notes index](../README.md) | [Next: Performance, server rendering, and project setup](./12-performance-server-rendering-and-project-setup.md) |

## Decide where state belongs

React state belongs in the closest component that needs to read or update it. Keep local input state inside a form when no other part needs it. Move state up when sibling components must coordinate. Avoid copying a value into multiple state variables when one value can be derived.

Chakra controls accept ordinary React props and callbacks. Some interactive compound components return a details object from their change callbacks, so read the documented value from that object.

## Control a checkbox

A controlled checkbox reflects React state. Update that state from `onCheckedChange`:

```jsx
import { useState } from "react"
import { Checkbox, Stack, Text } from "@chakra-ui/react"

export function SetupChecklist() {
  const [complete, setComplete] = useState(false)

  return (
    <Stack align="start" gap="3">
      <Checkbox.Root
        checked={complete}
        onCheckedChange={({ checked }) => setComplete(Boolean(checked))}
      >
        <Checkbox.HiddenInput />
        <Checkbox.Control />
        <Checkbox.Label>Mark setup as complete</Checkbox.Label>
      </Checkbox.Root>

      <Text color="fg.muted">
        {complete ? "Setup is complete." : "Setup is still in progress."}
      </Text>
    </Stack>
  )
}
```

The `Boolean` conversion keeps this example's state strictly true or false. Chakra's checkbox can also represent an indeterminate state for parent and child selections.

## Control tabs

Tabs can own their selected value internally with `defaultValue`, or React can control the selection with `value` and `onValueChange`. Controlled tabs are useful when the selection also changes other page state.

```jsx
import { useState } from "react"
import { Tabs, Text } from "@chakra-ui/react"

export function ProjectTabs() {
  const [activeTab, setActiveTab] = useState("overview")

  return (
    <Tabs.Root
      value={activeTab}
      onValueChange={({ value }) => setActiveTab(value)}
    >
      <Tabs.List>
        <Tabs.Trigger value="overview">Overview</Tabs.Trigger>
        <Tabs.Trigger value="activity">Activity</Tabs.Trigger>
        <Tabs.Indicator />
      </Tabs.List>

      <Tabs.Content value="overview">
        <Text>Project details and current status.</Text>
      </Tabs.Content>
      <Tabs.Content value="activity">
        <Text>Recent project changes.</Text>
      </Tabs.Content>
    </Tabs.Root>
  )
}
```

Each trigger value must match the value of its content panel. Chakra handles the tab interaction pattern; keep the content and the selected value in sync.

## Connect reusable components to application data

A presentation component can accept a value and a callback without owning the full application data source.

```jsx
import { Checkbox } from "@chakra-ui/react"

function CompletionControl({ checked, onChange }) {
  return (
    <Checkbox.Root
      checked={checked}
      onCheckedChange={({ checked: nextChecked }) => onChange(Boolean(nextChecked))}
    >
      <Checkbox.HiddenInput />
      <Checkbox.Control />
      <Checkbox.Label>Completed</Checkbox.Label>
    </Checkbox.Root>
  )
}
```

The parent can update local state, call an API, or save the new value. Keep the component's contract small so it remains useful in multiple screens.

## Controlled and uncontrolled choices

Use `defaultValue` or `defaultChecked` when the component can manage its own state after the initial value. Use `value` or `checked` with a callback when React must own and observe every change.

Do not switch a control between controlled and uncontrolled modes during its lifetime. Choose one pattern when the component first renders. For text inputs, a controlled value should usually start as an empty string rather than `undefined`.

## Avoid state that can be derived

If `activeTab` determines which panel is visible, do not also store a second boolean for every panel. Derive the display from the selected tab. Fewer independent values mean fewer inconsistent states to debug.

## Practice

1. Store a checkbox value in a parent and pass it to a child control.
2. Add a third tab and keep every trigger value paired with a content value.
3. Use an uncontrolled default for a control that does not affect surrounding UI.
4. Add a submit button that reads the current form state.

## References

- [Chakra UI Checkbox](https://chakra-ui.com/docs/components/checkbox)
- [Chakra UI Tabs](https://chakra-ui.com/docs/components/tabs)
- [React: Sharing state between components](https://react.dev/learn/sharing-state-between-components)
- [React: Choosing the state structure](https://react.dev/learn/choosing-the-state-structure)
