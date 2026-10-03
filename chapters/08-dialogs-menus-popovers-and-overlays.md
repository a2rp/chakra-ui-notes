# 8. Dialogs, menus, popovers, and overlays

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Forms, validation, and accessible feedback](./07-forms-validation-and-accessible-feedback.md) | [Notes index](../README.md) | [Next: Color mode and appearance](./09-color-mode-and-appearance.md) |

## Pick an overlay by purpose

Overlays place temporary content above the page. Use the component whose interaction matches the job:

- A **dialog** asks for a decision or gathers information that needs attention.
- A **menu** presents a short list of commands or navigation actions.
- A **popover** shows related interactive or explanatory content near its trigger.
- A **tooltip** gives a brief description of a control. It should not hold essential instructions.

Do not use an overlay just to hide ordinary page content. Keeping important information visible makes the page easier to use.

## Build a dialog

Chakra UI v3 dialogs use compound parts. The trigger opens the dialog, the backdrop separates it from the page, and the content contains a clear title, body, and actions.

```jsx
import { Button, Dialog, Portal } from "@chakra-ui/react"

export function RemoveProjectDialog() {
  return (
    <Dialog.Root>
      <Dialog.Trigger asChild>
        <Button colorPalette="red" variant="outline">Remove project</Button>
      </Dialog.Trigger>

      <Portal>
        <Dialog.Backdrop />
        <Dialog.Positioner>
          <Dialog.Content>
            <Dialog.Header>
              <Dialog.Title>Remove this project?</Dialog.Title>
            </Dialog.Header>
            <Dialog.Body>
              This action removes the project from your workspace.
            </Dialog.Body>
            <Dialog.Footer>
              <Dialog.CloseTrigger asChild>
                <Button variant="outline">Cancel</Button>
              </Dialog.CloseTrigger>
              <Button colorPalette="red" onClick={() => console.log("Remove project")}>
                Remove project
              </Button>
            </Dialog.Footer>
            <Dialog.CloseTrigger aria-label="Close dialog" />
          </Dialog.Content>
        </Dialog.Positioner>
      </Portal>
    </Dialog.Root>
  )
}
```

The confirmation action should perform the change, while Cancel only dismisses the dialog. Connect the action to the application state or server request in a real feature.

A modal dialog should have a useful title, keep keyboard focus inside while open, close with Escape when appropriate, and return focus to the control that opened it. Chakra handles these interaction details by default. Do not turn off focus handling without a specific, tested reason.

## Build a command menu

A menu is for commands, not a form with many fields. Its items have menu keyboard behavior, including arrow-key movement and selection.

```jsx
import { Button, Menu, Portal } from "@chakra-ui/react"

export function ProjectActions({ onRename, onArchive }) {
  return (
    <Menu.Root>
      <Menu.Trigger asChild>
        <Button variant="outline">Project actions</Button>
      </Menu.Trigger>
      <Portal>
        <Menu.Positioner>
          <Menu.Content>
            <Menu.Item value="rename" onSelect={onRename}>Rename</Menu.Item>
            <Menu.Item value="archive" onSelect={onArchive}>Archive</Menu.Item>
          </Menu.Content>
        </Menu.Positioner>
      </Portal>
    </Menu.Root>
  )
}
```

Each item has a unique value. For navigation, render an item as a link with `asChild` and preserve anchor behavior. Do not use menu roles for a regular list of page links that should behave like normal navigation.

## Use a popover for nearby detail

A popover is useful for a small contextual panel that may include controls. It should have a visible trigger, and people should be able to dismiss it with Escape or by interacting outside when appropriate.

```jsx
import { Button, Popover, Portal } from "@chakra-ui/react"

export function HelpPopover() {
  return (
    <Popover.Root>
      <Popover.Trigger asChild>
        <Button variant="ghost">About this setting</Button>
      </Popover.Trigger>
      <Portal>
        <Popover.Positioner>
          <Popover.Content>
            <Popover.Arrow>
              <Popover.ArrowTip />
            </Popover.Arrow>
            <Popover.Title>Automatic updates</Popover.Title>
            <Popover.Body>
              Updates install when they are available and the device is idle.
            </Popover.Body>
          </Popover.Content>
        </Popover.Positioner>
      </Portal>
    </Popover.Root>
  )
}
```

Use a dialog instead when the user must focus on a confirmation or a substantial form. Avoid nesting floating controls without checking how their portals and focus order interact.

## Keep tooltip content brief

A tooltip supplements a control with a short description. It must not be the only place an important instruction appears because touch users may not have hover, and keyboard users need a focusable trigger. Chakra's generated snippet provides a convenient wrapper:

```sh
npx @chakra-ui/cli snippet add tooltip
```

Use the generated tooltip around a control and keep the same essential meaning visible in the surrounding interface when it matters.

## Test overlays with a keyboard

1. Tab to the trigger and open the overlay with its keyboard interaction.
2. Move through its content without using a pointer.
3. Confirm Escape dismisses it when that is appropriate.
4. Check that focus returns to the trigger after a dialog closes.
5. Confirm menus and popovers are not clipped by a scroll container.

## References

- [Chakra UI Dialog](https://chakra-ui.com/docs/components/dialog)
- [Chakra UI Menu](https://chakra-ui.com/docs/components/menu)
- [Chakra UI Popover](https://chakra-ui.com/docs/components/popover)
- [Chakra UI Tooltip](https://chakra-ui.com/docs/components/tooltip)
