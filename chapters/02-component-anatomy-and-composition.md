# 2. Component anatomy and composition

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Set up Chakra UI and build a first component](./01-setup-and-first-component.md) | [Notes index](../README.md) | [Next: Style props and conditional styles](./03-style-props-and-conditional-styles.md) |

## Think in small components

A useful interface is built from components with clear jobs. A page can own its data and state, a card can organize related information, and a button can expose a focused action. Chakra UI components are still React components, so props, children, events, and composition follow normal React rules.

Use a primitive such as `Box` when you need a styled container. Choose a semantic component such as `Heading`, `Text`, or `Button` when its meaning and behavior match the content.

## Use semantic HTML

The element exposed to assistive technology and browser tools should match its role. A title should be a heading, navigation should be a nav element, and an action should be a button.

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

The `as` prop changes the underlying HTML element while keeping the Chakra styling props. Use it when the visual component is suitable but its default HTML tag is not. Do not choose a tag only for its default appearance.

## Compose compound components

Some components have named parts that work together. Chakra UI v3 uses dot notation for compound components such as `Card.Root`. This makes each structural part explicit.

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

Here the root owns the card container. The header groups the title and summary, the body holds the main content, and the footer groups an action. This structure is easier to scan and change than one large generic box.

## Keep data outside repeated markup

When several cards share a shape, store the changing content in data and map it to components. Give each rendered item a stable key.

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

For a real page, keep the project records complete in one place and pass them through unchanged. The example shows the mapping pattern; avoid inventing data inside the render loop.

## Compose behavior with `asChild`

The `asChild` prop lets a Chakra component supply its behavior and styling to a child element. This is useful when the child should keep its native HTML behavior, such as navigation through an anchor.

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

The rendered control remains an anchor, so it opens a URL and supports normal link actions. For an action that changes application state, use a button instead of an anchor.

Custom components used with `asChild` should pass received props and refs to the actual DOM element. If they discard those values, the composed component may lose its click handler, accessible attributes, focus behavior, or styling.

## Build a reusable interface part

A component should expose the information that changes and keep its stable structure inside. Use defaults for optional presentation, but make important required values explicit.

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

Keep visual decisions close to the component that owns them. Keep business decisions, such as what counts as ready, in the parent or data layer when they can change independently.

## Composition checklist

- Choose HTML semantics before visual styling.
- Prefer Chakra components whose behavior matches the need.
- Use compound parts when a component has meaningful internal structure.
- Keep repeated content in data and render it with stable keys.
- Pass props and refs through custom components used for composition.
- Keep components small enough that their props and purpose are easy to understand.

## References

- [Chakra UI composition](https://chakra-ui.com/docs/components/concepts/composition)
- [Chakra UI Card](https://chakra-ui.com/docs/components/card)
- [React: Passing props to a component](https://react.dev/learn/passing-props-to-a-component)
