# 12. Performance, server rendering, and project setup

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: React state and component integration](./11-react-state-and-component-integration.md) | [Notes index](../README.md) | [Next: Migration, testing, and troubleshooting](./13-migration-testing-and-troubleshooting.md) |

## Keep the provider and system stable

Create the Chakra system once in a module, outside React render functions. Mount the application provider near the root so pages share one configuration.

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

Do not rebuild the system inside a component on every render. A stable module export is easier to understand and gives the provider a consistent configuration.

## Render lists predictably

When rendering repeated Chakra components, use a stable key from the data. Avoid using the array index when entries can be inserted, removed, or reordered.

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

React uses the key to match an item between renders. A stable id helps prevent the wrong input value or component state from appearing on a different item after a list changes.

## Optimize after measuring

Readable component composition is usually the best starting point. First look for unnecessary work that affects a real page. Consider memoization when a measured render cost is meaningful and the component receives stable inputs. Adding `memo`, `useMemo`, or `useCallback` everywhere can make state flow harder to follow without improving the experience.

Other useful habits include:

- Render only the data a view needs.
- Keep state near the components that use it.
- Avoid recreating large data sets during every render.
- Use lazy mounting for expensive content that starts hidden when the component supports it.
- Keep images and non-interface assets sized appropriately.

## Use Chakra with server rendering

Chakra supports React applications that use server rendering and React Server Components. Follow the setup guide for the framework because provider placement and client boundaries depend on that framework. Chakra's Vite setup applies to a client-rendered app and is not a drop-in replacement for a Next.js or other server-rendered setup.

Avoid reading browser-only values such as `window`, `document`, or `localStorage` during server rendering. Keep browser-dependent work in effects or a client-only component. For color mode, prefer semantic tokens so ordinary appearance changes are handled by CSS instead of requiring markup to differ between server and client.

## Organize a project as it grows

A small app can begin with a few files:

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

Group code by purpose as the project grows. Keep shared components separate from page-specific content, and put theme configuration in one discoverable module. Avoid creating a deep folder tree before repeated use makes the boundaries clear.

## Keep dependencies intentional

Use Chakra's component and snippet APIs that the project needs. Add an extra form, animation, or icon package only when the app needs its specific behavior. Review package updates with the app's build and interaction checks, especially when a component library makes a major version change.

## Practice

1. Move a system created inside a component into `src/theme.js`.
2. Render a list with stable item ids and check behavior after removing a row.
3. Use a React profiler to find a slow area before adding memoization.
4. Compare the framework setup guide with the actual project structure.

## References

- [Chakra UI with Vite](https://chakra-ui.com/docs/get-started/frameworks/vite)
- [Chakra UI server components](https://chakra-ui.com/docs/components/concepts/server-components)
- [Chakra UI client-only rendering](https://chakra-ui.com/docs/components/client-only)
- [React: Rendering lists](https://react.dev/learn/rendering-lists)
- [React: useMemo](https://react.dev/reference/react/useMemo)
