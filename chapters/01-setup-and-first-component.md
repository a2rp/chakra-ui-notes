# 1. Set up Chakra UI and build a first component

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| Notes index | [Notes index](../README.md) | [Next: Component anatomy and composition](./02-component-anatomy-and-composition.md) |

## What Chakra UI provides

Chakra UI is a React component library with accessible interface components and style props. These notes use Chakra UI v3 with React and JavaScript. Examples use JSX. Do not mix v2 setup snippets with v3 examples because their provider and component APIs differ.

## Create a Vite React project

Use Node.js 20 or newer. In a terminal, run:

```sh
node --version
npm create vite@latest chakra-playground -- --template react
cd chakra-playground
npm install
npm install @chakra-ui/react @emotion/react
npm run dev
```

Open the local address shown by Vite. Keep the development server running while editing files.

## Add the provider

The provider makes Chakra's styling system available to every component inside it. Replace `src/main.jsx` with:

```jsx
import React from "react"
import ReactDOM from "react-dom/client"
import { ChakraProvider, defaultSystem } from "@chakra-ui/react"
import App from "./App.jsx"

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <ChakraProvider value={defaultSystem}>
      <App />
    </ChakraProvider>
  </React.StrictMode>,
)
```

The v3 provider receives its styling system through `value`. The default system is enough for a first screen. Chakra's CLI can also add reusable provider snippets with `npx @chakra-ui/cli snippet add`, including a color mode setup.

## Build a first screen

Replace `src/App.jsx` with:

```jsx
import { Button, Heading, Stack, Text } from "@chakra-ui/react"

export default function App() {
  return (
    <Stack align="start" gap="4" maxW="xl" mx="auto" p="8">
      <Text color="teal.600" fontWeight="semibold">
        My component library notes
      </Text>
      <Heading size="2xl">Build a clear first screen</Heading>
      <Text color="fg.muted" lineHeight="tall">
        Chakra components are React components. Their props control appearance,
        while React handles application behavior.
      </Text>
      <Button colorPalette="teal" onClick={() => alert("It works")}>Try the button</Button>
    </Stack>
  )
}
```

`Stack` places its children in a vertical layout with consistent spacing. `Heading` marks the page title, `Text` holds supporting copy, and `Button` provides an interactive control. Props such as `p`, `mx`, `gap`, and `fontWeight` are style props. `onClick` is a normal React event handler.

## How the app renders

Vite's `index.html` contains the `root` element. React mounts `App` into that element. The Chakra provider wraps `App`, so components below it can access the styling system. Keep the provider at the application root when the whole app uses Chakra.

## Make a reusable component

```jsx
import { Button } from "@chakra-ui/react"

function SaveButton({ onSave, children = "Save changes" }) {
  return <Button colorPalette="blue" onClick={onSave}>{children}</Button>
}

export default function SettingsActions() {
  function saveSettings() {
    console.log("Settings saved")
  }

  return <SaveButton onSave={saveSettings} />
}
```

The parent supplies the action through `onSave`. `SaveButton` owns the shared presentation. Keeping those responsibilities separate makes the component reusable.

## Common setup problems

- **Blank page:** inspect the browser console and Vite terminal for import or JSX errors.
- **Styles missing:** confirm the component is inside `ChakraProvider` and that v3 uses `value={defaultSystem}`.
- **Import error:** check that the package is installed and the component comes from `@chakra-ui/react`.
- **An example does not match:** verify that the example is for v3, since v2 APIs differ.
- **Unsupported Node version:** install Node.js 20 or newer and restart the terminal.

## Practice

1. Change the stack width, padding, and gap.
2. Add a second button with another `colorPalette`.
3. Replace the alert with a message rendered in the page.
4. Move `SaveButton` to its own file and import it into `App.jsx`.

## References

- [Chakra UI installation](https://chakra-ui.com/docs/get-started/installation)
- [React: Your first component](https://react.dev/learn/your-first-component)
