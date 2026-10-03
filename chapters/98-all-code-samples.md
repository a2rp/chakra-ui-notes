# All code samples

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Migration, testing, and troubleshooting](./13-migration-testing-and-troubleshooting.md) | [Notes index](../README.md) | [Next: Complete questions and answers](./99-complete-q-and-a.md) |

## 1. Set up Chakra UI and build a first component

### Code sample 1 (sh)

```sh
node --version
npm create vite@latest chakra-playground -- --template react
cd chakra-playground
npm install
npm install @chakra-ui/react @emotion/react
npm run dev
```

### Code sample 2 (jsx)

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

### Code sample 3 (jsx)

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

### Code sample 4 (jsx)

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

## 2. Component anatomy and composition

### Code sample 1 (jsx)

```jsx
import { Box, Heading, Text } from "@chakra-ui/react"

export function ArticleIntro() {
  return (
    <Box as="article" maxW="prose">
      <Heading as="h2" size="lg">Plan the page before styling it</Heading>
      <Text mt="3">
        Semantic elements describe the purpose of each part of the interface.
      </Text>
    </Box>
  )
}
```

### Code sample 2 (jsx)

```jsx
import { Button, Card } from "@chakra-ui/react"

export function ProjectCard({ project, onOpen }) {
  return (
    <Card.Root maxW="sm" variant="outline">
      <Card.Header>
        <Card.Title>{project.name}</Card.Title>
        <Card.Description>{project.summary}</Card.Description>
      </Card.Header>

      <Card.Body>
        <p>{project.detail}</p>
      </Card.Body>

      <Card.Footer>
        <Button onClick={() => onOpen(project.id)}>Open project</Button>
      </Card.Footer>
    </Card.Root>
  )
}
```

### Code sample 3 (jsx)

```jsx
const projects = [
  { id: "notes", name: "Study notes", summary: "Reusable learning references" },
  { id: "library", name: "Component library", summary: "Accessible interface parts" },
]

export function ProjectList() {
  return (
    <div>
      {projects.map((project) => (
        <ProjectCard
          key={project.id}
          project={{ ...project, detail: "A small project summary." }}
          onOpen={(id) => console.log("Open", id)}
        />
      ))}
    </div>
  )
}
```

### Code sample 4 (jsx)

```jsx
import { Button } from "@chakra-ui/react"

export function ExternalAction() {
  return (
    <Button asChild variant="outline">
      <a href="https://chakra-ui.com/docs" target="_blank" rel="noreferrer">
        Read the component reference
      </a>
    </Button>
  )
}
```

### Code sample 5 (jsx)

```jsx
import { Badge, Stack, Text } from "@chakra-ui/react"

export function StatusSummary({ label, status, description }) {
  return (
    <Stack align="start" gap="2">
      <Badge colorPalette={status === "Ready" ? "green" : "orange"}>
        {status}
      </Badge>
      <Text fontWeight="semibold">{label}</Text>
      <Text color="fg.muted">{description}</Text>
    </Stack>
  )
}
```

## 3. Style props and conditional styles

### Code sample 1 (jsx)

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

### Code sample 2 (jsx)

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

### Code sample 3 (jsx)

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

### Code sample 4 (jsx)

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

### Code sample 5 (jsx)

```jsx
<Box color={isError ? "red.700" : "green.700"}>
  {isError ? "Check the form" : "Looks good"}
</Box>
```

### Code sample 6 (jsx)

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

## 4. Layout and responsive design

### Code sample 1 (jsx)

```jsx
import { Box, Container, Flex, Heading, SimpleGrid, Stack, Text } from "@chakra-ui/react"

const topics = [
  { id: "layout", title: "Layout", detail: "Arrange content into clear page regions." },
  { id: "tokens", title: "Design tokens", detail: "Use shared values for consistent styling." },
  { id: "forms", title: "Forms", detail: "Give people clear labels and feedback." },
]

function TopicCard({ topic }) {
  return (
    <Box borderWidth="1px" borderColor="border" bg="bg.panel" p="5" rounded="lg">
      <Heading size="md">{topic.title}</Heading>
      <Text color="fg.muted" mt="2">{topic.detail}</Text>
    </Box>
  )
}

export default function NotesHome() {
  return (
    <Container maxW="6xl" py={{ base: "8", md: "12" }}>
      <Stack gap={{ base: "8", md: "10" }}>
        <Flex
          align={{ base: "start", md: "center" }}
          direction={{ base: "column", md: "row" }}
          justify="space-between"
          gap="4"
        >
          <Box>
            <Text color="teal.600" fontWeight="semibold">Component library</Text>
            <Heading size={{ base: "xl", md: "2xl" }}>Study by building</Heading>
          </Box>
          <Text maxW="md" color="fg.muted">
            Short examples show how the pieces work together.
          </Text>
        </Flex>

        <SimpleGrid columns={{ base: 1, sm: 2, lg: 3 }} gap="5">
          {topics.map((topic) => <TopicCard key={topic.id} topic={topic} />)}
        </SimpleGrid>
      </Stack>
    </Container>
  )
}
```

### Code sample 2 (jsx)

```jsx
<Text fontSize={{ base: "sm", md: "md", lg: "lg" }}>
  Text grows as more room becomes available.
</Text>
```

### Code sample 3 (jsx)

```jsx
<Heading fontSize="xl" md={{ fontSize: "2xl" }}>
  Responsive heading
</Heading>
```

### Code sample 4 (jsx)

```jsx
import { Button, Flex, Heading, Spacer } from "@chakra-ui/react"

export function PanelHeader({ onCreate }) {
  return (
    <Flex align="center" gap="4">
      <Heading size="md">Projects</Heading>
      <Spacer />
      <Button onClick={onCreate}>Create project</Button>
    </Flex>
  )
}
```

### Code sample 5 (jsx)

```jsx
import { Box, Grid } from "@chakra-ui/react"

export function DashboardLayout() {
  return (
    <Grid templateColumns={{ base: "1fr", lg: "16rem 1fr" }} gap="6">
      <Box as="nav" aria-label="Dashboard">Navigation</Box>
      <Box as="main">Dashboard content</Box>
    </Grid>
  )
}
```

## 5. Theme configuration and design tokens

### Code sample 1 (js)

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

### Code sample 2 (jsx)

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

### Code sample 3 (js)

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

### Code sample 4 (js)

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

### Code sample 5 (jsx)

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

## 6. Recipes and slot recipes

### Code sample 1 (js)

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

### Code sample 2 (jsx)

```jsx
import { Box, useRecipe } from "@chakra-ui/react"

export function Panel({ tone, children }) {
  const recipe = useRecipe({ key: "panel" })
  const styles = recipe({ tone })

  return <Box css={styles}>{children}</Box>
}
```

### Code sample 3 (js)

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

### Code sample 4 (js)

```js
const config = defineConfig({
  theme: {
    slotRecipes: {
      notice: noticeRecipe,
    },
  },
})
```

### Code sample 5 (jsx)

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

## 7. Forms, validation, and accessible feedback

### Code sample 1 (jsx)

```jsx
import { Field, Input } from "@chakra-ui/react"

export function EmailField() {
  return (
    <Field.Root required>
      <Field.Label>
        Email address
        <Field.RequiredIndicator />
      </Field.Label>
      <Input type="email" name="email" autoComplete="email" />
      <Field.HelperText>Use the address connected to your account.</Field.HelperText>
    </Field.Root>
  )
}
```

### Code sample 2 (jsx)

```jsx
import { useState } from "react"
import { Button, Field, Input, Stack } from "@chakra-ui/react"

export default function SignupForm() {
  const [email, setEmail] = useState("")
  const [error, setError] = useState("")
  const [message, setMessage] = useState("")

  function handleSubmit(event) {
    event.preventDefault()
    setMessage("")

    if (!email.trim()) {
      setError("Enter your email address.")
      return
    }

    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) {
      setError("Enter an email address in the usual format.")
      return
    }

    setError("")
    setMessage("Your email is ready to be saved.")
  }

  return (
    <form onSubmit={handleSubmit} noValidate>
      <Stack align="start" gap="4" maxW="md">
        <Field.Root required invalid={Boolean(error)}>
          <Field.Label>
            Email address
            <Field.RequiredIndicator />
          </Field.Label>
          <Input
            type="email"
            name="email"
            autoComplete="email"
            value={email}
            onChange={(event) => {
              setEmail(event.target.value)
              setError("")
            }}
            aria-describedby={error ? "email-error" : undefined}
          />
          {error ? <Field.ErrorText id="email-error">{error}</Field.ErrorText> : null}
        </Field.Root>

        <Button type="submit">Continue</Button>
        {message ? <p role="status">{message}</p> : null}
      </Stack>
    </form>
  )
}
```

## 8. Dialogs, menus, popovers, and overlays

### Code sample 1 (jsx)

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

### Code sample 2 (jsx)

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

### Code sample 3 (jsx)

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

### Code sample 4 (sh)

```sh
npx @chakra-ui/cli snippet add tooltip
```

## 9. Color mode and appearance

### Code sample 1 (sh)

```sh
npx @chakra-ui/cli snippet add color-mode
```

### Code sample 2 (jsx)

```jsx
import { Box, Heading, Text } from "@chakra-ui/react"

export function AppearanceCard() {
  return (
    <Box bg="bg.panel" color="fg" borderColor="border" borderWidth="1px" p="5" rounded="lg">
      <Heading size="md">Appearance</Heading>
      <Text color="fg.muted" mt="2">
        This panel uses values that adapt to light and dark mode.
      </Text>
    </Box>
  )
}
```

### Code sample 3 (jsx)

```jsx
import { ColorModeButton } from "./components/ui/color-mode"

export function SiteHeader() {
  return (
    <header>
      <h1>Account settings</h1>
      <ColorModeButton />
    </header>
  )
}
```

### Code sample 4 (jsx)

```jsx
<Box bg="white" color="gray.900" _dark={{ bg: "gray.900", color: "gray.50" }}>
  A one-off surface with explicit mode values
</Box>
```

## 10. Accessibility and semantic UI

### Code sample 1 (jsx)

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

### Code sample 2 (jsx)

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

### Code sample 3 (jsx)

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

### Code sample 4 (jsx)

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

### Code sample 5 (jsx)

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

## 11. React state and component integration

### Code sample 1 (jsx)

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

### Code sample 2 (jsx)

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

### Code sample 3 (jsx)

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

## 12. Performance, server rendering, and project setup

### Code sample 1 (js)

```js
// src/theme.js
import { createSystem, defaultConfig, defineConfig } from "@chakra-ui/react"

const config = defineConfig({
  theme: {
    tokens: {
      colors: {
        brand: {
          500: { value: "#2563eb" },
        },
      },
    },
  },
})

export const system = createSystem(defaultConfig, config)
```

### Code sample 2 (jsx)

```jsx
import { Box, SimpleGrid } from "@chakra-ui/react"

export function ItemGrid({ items }) {
  return (
    <SimpleGrid columns={{ base: 1, md: 2 }} gap="4">
      {items.map((item) => (
        <Box key={item.id} borderWidth="1px" p="4">{item.name}</Box>
      ))}
    </SimpleGrid>
  )
}
```

### Code sample 3 (text)

```text
src/
  components/
    ProjectCard.jsx
  pages/
    ProjectsPage.jsx
  theme.js
  App.jsx
  main.jsx
```

## 13. Migration, testing, and troubleshooting

### Code sample 1 (sh)

```sh
npm install -D vitest jsdom @testing-library/react @testing-library/user-event @testing-library/jest-dom
```

### Code sample 2 (jsx)

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

### Code sample 3 (jsx)

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

### Code sample 4 (sh)

```sh
npx @chakra-ui/codemod upgrade --dry
npx @chakra-ui/codemod upgrade
```

[Back to notes index](../README.md)
