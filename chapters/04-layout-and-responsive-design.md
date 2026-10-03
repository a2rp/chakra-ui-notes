# 4. Layout and responsive design

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Style props and conditional styles](./03-style-props-and-conditional-styles.md) | [Notes index](../README.md) | [Next: Theme configuration and design tokens](./05-theme-configuration-and-design-tokens.md) |

## Start with the content structure

Choose a layout component based on how content relates:

- `Stack` places items in one direction with consistent spacing.
- `Flex` aligns items along a row or column and can distribute free space.
- `Grid` defines rows and columns for a deliberate two-dimensional layout.
- `SimpleGrid` creates a regular grid with a small amount of configuration.
- `Container` keeps page content within a readable maximum width.

Use the simplest layout that explains the page. Add nested layout components when a section has its own alignment or spacing needs.

## Build a responsive page

Chakra uses mobile-first breakpoints. Base styles apply at narrow widths, and breakpoint values add or change styles as the available width grows.

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

On a small screen the heading and supporting text stack, and the cards use one column. Wider breakpoints place the text side by side and add grid columns. The content order stays the same, so users do not need to learn a different reading order at each width.

## Responsive values

Object syntax makes breakpoint intent explicit:

```jsx
<Text fontSize={{ base: "sm", md: "md", lg: "lg" }}>
  Text grows as more room becomes available.
</Text>
```

You can also put the breakpoint on the prop:

```jsx
<Heading fontSize="xl" md={{ fontSize: "2xl" }}>
  Responsive heading
</Heading>
```

For most components, object syntax is easier to scan because every value is named. Array syntax is available, but omitted breakpoints can make arrays harder to maintain.

## Layout with Flex and Grid

Use Flex for one-dimensional alignment, such as a toolbar:

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

Use Grid when regions align in both directions:

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

The navigation becomes a regular section on small screens and a separate column on large screens. This preserves access to it without making a narrow viewport scroll horizontally.

## Spacing and sizing

Use spacing tokens with `gap`, padding, and margin so the rhythm stays consistent. Use `maxW` to control line length and content width. Prefer fluid widths such as `w="full"` inside a bounded container over fixed pixel widths for major page regions.

Do not use spacing to fake a missing layout relationship. If items should form a row, use Flex or a horizontal Stack. If items should align in columns, use Grid or SimpleGrid.

## Check a layout at real widths

- Resize the viewport and check the narrowest supported width.
- Look for horizontal overflow, clipped controls, and long headings.
- Keep touch targets reachable and leave enough space between actions.
- Confirm that the reading order still makes sense at each breakpoint.
- Do not hide essential information merely to avoid adapting the layout.

## Practice

1. Add a fourth topic and check that the grid responds without manual positioning.
2. Change the breakpoint where the header becomes horizontal.
3. Add a navigation column that becomes a top section on narrow screens.
4. Test long text and narrow screens, not just short sample labels.

## References

- [Chakra UI responsive design](https://chakra-ui.com/docs/styling/responsive-design)
- [Chakra UI Stack](https://chakra-ui.com/docs/components/stack)
- [Chakra UI SimpleGrid](https://chakra-ui.com/docs/components/simple-grid)
- [Chakra UI Flex and Grid](https://chakra-ui.com/docs/styling/style-props/flex-and-grid)
