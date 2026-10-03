# 13. Migration, testing, and troubleshooting

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Performance, server rendering, and project setup](./12-performance-server-rendering-and-project-setup.md) | [Notes index](../README.md) | Notes index |

## Test what a person can do

A useful component test checks behavior through the interface: can a person find a control by its role and name, activate it, and observe the result? This stays meaningful even if the component's internal styling changes.

Install the test tools in a Vite React project:

```sh
npm install -D vitest jsdom @testing-library/react @testing-library/user-event @testing-library/jest-dom
```

Configure Vitest to use a browser-like environment for component tests, following the Vitest guide. Add a small component with a real action:

```jsx
import { useState } from "react"
import { Button } from "@chakra-ui/react"

export function SaveAction({ onSave }) {
  const [status, setStatus] = useState("")

  function handleSave() {
    onSave()
    setStatus("Your changes have been saved.")
  }

  return (
    <>
      <Button onClick={handleSave}>Save changes</Button>
      {status ? <p role="status">{status}</p> : null}
    </>
  )
}
```

Test it with Testing Library:

```jsx
import { ChakraProvider, defaultSystem } from "@chakra-ui/react"
import { render, screen } from "@testing-library/react"
import userEvent from "@testing-library/user-event"
import { expect, test, vi } from "vitest"
import { SaveAction } from "./SaveAction.jsx"

test("saves changes and announces the result", async () => {
  const user = userEvent.setup()
  const onSave = vi.fn()

  render(
    <ChakraProvider value={defaultSystem}>
      <SaveAction onSave={onSave} />
    </ChakraProvider>,
  )

  await user.click(screen.getByRole("button", { name: "Save changes" }))

  expect(onSave).toHaveBeenCalledTimes(1)
  expect(screen.getByRole("status")).toHaveTextContent(
    "Your changes have been saved.",
  )
})
```

The test finds the button by its user-facing role and name, clicks it, and checks both the callback and status message. Add the jest-dom matchers to the Vitest setup file if the project does not already register them.

Prefer assertions about visible behavior over assertions about generated class names. Chakra's generated CSS can change while the feature continues to work correctly.

## Move from Chakra UI v2 to v3

A major version migration can change package setup, component composition, and color mode. Read the official migration guide and make the change in a branch or a saved commit so the result can be reviewed.

A careful sequence is:

1. Record the current package versions and run the existing checks.
2. Read the v3 migration guide for the components used by the app.
3. Review the proposed codemod changes before applying them.
4. Update the packages and provider setup.
5. Migrate compound components such as cards, dialogs, menus, and tabs.
6. Check theme tokens, recipes, forms, and color mode.
7. Run tests, the production build, and manual keyboard checks.

The official codemod can assist with changes. Review its output rather than treating an automated edit as proof that the interface is correct:

```sh
npx @chakra-ui/codemod upgrade --dry
npx @chakra-ui/codemod upgrade
```

Examples of structural changes include card parts moving to names such as `Card.Root` and `Card.Body`, and tab panels receiving explicit matching values. Use the migration page for the complete mapping because component APIs can change independently.

## Troubleshoot common symptoms

- **Provider error or missing styles:** confirm the app uses one root provider and that the v3 system is passed with `value`.
- **An export cannot be found:** confirm the imported name and compound component shape in the current component reference.
- **Custom color is missing:** check the token path, the `value` object, and that the app's provider receives the system that defines it.
- **Light and dark colors do not switch:** confirm the color mode provider is installed and prefer semantic tokens for mode-aware styles.
- **Dialog or menu is clipped:** inspect the portal and positioning setup, especially inside scrollable or nested overlays.
- **A control cannot be found by keyboard:** use the documented Chakra component parts and test the interaction with Tab, arrow keys, Enter, Space, and Escape.
- **A test cannot find a button:** use the accessible name rendered by the component and wrap Chakra components in the provider.
- **Server and browser output differ:** avoid reading browser-only values during server rendering and use the framework's client-only pattern where needed.

## Before sharing a change

- Run the unit tests and production build.
- Open the main screens at narrow and wide widths.
- Check light and dark appearances.
- Try forms, dialogs, menus, and tabs with the keyboard.
- Review the browser console and package changes.
- Verify that links and visible labels still describe the actual action.

## References

- [Chakra UI migration to v3](https://chakra-ui.com/docs/get-started/migration)
- [Chakra UI installation](https://chakra-ui.com/docs/get-started/installation)
- [Vitest guide](https://vitest.dev/guide/)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/)
- [Testing Library user-event](https://testing-library.com/docs/user-event/intro/)
