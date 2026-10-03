# Complete questions and answers

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: All code samples](./98-all-code-samples.md) | [Notes index](../README.md) | Notes index |

Use these questions to review the concepts and find a related chapter for the full examples.

## Foundations

### 1. What is Chakra UI?

Chakra UI is a React component library with styling props, design-system tools, and accessible interaction components. It helps build interfaces by composing reusable pieces instead of styling every control from scratch.

### 2. Which Chakra UI version do these notes use?

The examples target Chakra UI v3 with React and JavaScript. Check the linked official docs when working in a project on a different major version.

### 3. What is needed to start a Chakra UI Vite app?

Use Node.js 20 or newer, create a React app with Vite, install its dependencies, then install `@chakra-ui/react` and `@emotion/react`. The setup and first component chapter shows the complete commands.

### 4. Why does the app need a Chakra provider?

The provider makes Chakra's styling system available to components in its React subtree. Put it around the application at the root so pages share one configuration.

### 5. What does `defaultSystem` do?

It is Chakra's built-in styling system. It is a convenient starting point before the app needs custom tokens, recipes, or other configuration.

## Components and composition

### 6. What are style props?

They are component props that map to CSS properties, such as `p` for padding, `bg` for background, and `gap` for layout spacing. They make common styling decisions concise and can use theme tokens.

### 7. When should I use `Box` instead of a semantic component?

Use `Box` for a generic styled container. Use components such as `Heading`, `Text`, `Button`, and `Link` when their meaning and behavior match the content.

### 8. What is the difference between `as` and `asChild`?

`as` changes the underlying element while keeping the component's styles. `asChild` composes a component's props and behavior onto its child, which is useful when the child must retain native link or custom component behavior.

### 9. What is a compound component?

It is a component made from named parts that work together, such as `Card.Root`, `Card.Header`, and `Card.Body`. The parts make the structure and role of each region clear.

### 10. How should I create a reusable component?

Keep its purpose focused, accept changing data or behavior through props, and keep repeated structure inside it. Put business decisions in a parent or data layer when they can change independently.

## Styling and layout

### 11. How do responsive values work?

Chakra supports responsive object values such as `fontSize={{ base: "sm", md: "lg" }}`. Base values apply to narrow screens, and later breakpoint values adjust the style as the viewport grows.

### 12. What does mobile-first mean here?

The smallest layout is the starting point. Add wider-screen changes at breakpoints instead of designing a large screen first and trying to compress it.

### 13. How do I style hover and keyboard focus?

Use conditional style props such as `_hover` and `_focusVisible`. Keep focus visible so keyboard users can tell which control is active.

### 14. What is the difference between a raw token and a semantic token?

A raw token identifies a value, such as a color shade. A semantic token identifies a purpose, such as panel background or muted text, and can change by color mode while components keep the same token name.

### 15. When should I use the `css` prop?

Use it for a small selector or CSS property that has no clear style prop. Keep larger or complicated selector rules in a stylesheet so they remain easy to maintain.

### 16. Which layout component should I choose?

Use Stack for items in one direction, Flex for one-dimensional alignment, Grid for defined rows and columns, SimpleGrid for regular responsive columns, and Container to bound page width.

## Themes and recipes

### 17. What do `defineConfig` and `createSystem` do?

`defineConfig` describes theme changes and other styling configuration. `createSystem` combines that configuration with the chosen base config to create the system passed to the provider.

### 18. Why should a theme token value have a `value` property?

Chakra's token format stores the CSS value in an object with a `value` key. The structure also allows extra information such as a description.

### 19. When should I define a semantic token?

Define one when a color or style has a repeated purpose across the interface, especially when its value changes by color mode. A semantic name makes the component easier to understand than a raw shade.

### 20. What is a recipe?

A recipe describes base styles and variants for one styled unit. Register it in the system and call it from components when those variants should be reused.

### 21. What is a slot recipe?

A slot recipe coordinates styles across several named parts of one component. Use it for custom multi-part components whose variants change more than one part.

## Forms and accessibility

### 22. What does `Field` add to an input?

It groups a label, required indicator, helper text, and error text with a form control. This gives the field a clear name and keeps instructions near the control.

### 23. Can placeholder text replace a label?

No. Placeholder text disappears as the user types and may have low contrast. Keep a visible label that remains available after entering a value.

### 24. Is client-side validation enough?

No. Client validation helps people correct input before submitting, but important rules must also be checked by the server because browser code can be changed or bypassed.

### 25. How should an error be presented?

Set the field invalid state, place a clear error near the field, and explain how to fix the problem. Do not rely on color alone.

### 26. How can a save message be announced?

Render it in a status region such as an element with `role="status"`. This can announce the result without moving keyboard focus.

### 27. Do icon-only buttons need a label?

Yes. Give each one an accessible name that describes its action, such as `aria-label="Close panel"`. A decorative icon by itself does not explain the action.

## State and overlays

### 28. What is a controlled component?

A controlled component receives its current value from React state and reports changes through a callback. Use it when other parts of the application need to observe or react to that value.

### 29. When is an uncontrolled component suitable?

Use `defaultValue` or `defaultChecked` when the component can manage its value internally after the initial render and the surrounding UI does not need to track every change.

### 30. What should a controlled checkbox callback read?

Chakra's `onCheckedChange` passes a details object. Read its `checked` value and update React state from that value.

### 31. How do controlled tabs stay synchronized?

Keep a selected string in React state, pass it to `Tabs.Root` as `value`, and update it from `onValueChange`. Each trigger value must match a content value.

### 32. When should I use a dialog?

Use a dialog for a focused prompt, confirmation, or short task that needs attention. Give it a meaningful title and keep the default keyboard and focus behavior.

### 33. When should I use a menu?

Use a menu for a short list of commands or related navigation actions. It has keyboard interaction behavior that a regular list of links does not need.

### 34. When should I use a popover?

Use a popover for nearby explanatory or interactive content that is related to its trigger. Use a dialog for a substantial task or decision that deserves focused attention.

### 35. What belongs in a tooltip?

A tooltip can give a brief, supplemental description of a control. Do not place the only copy of an important instruction in a tooltip.

### 36. Why use a portal for an overlay?

A portal can render an overlay outside a clipping or stacking context, helping it appear above surrounding content. Check nested overlays and scroll containers because positioning may need adjustment.

## Appearance, testing, and maintenance

### 37. How does Chakra UI v3 provide color mode?

Chakra uses `next-themes` with provider and helper components available through its color mode snippet. Use semantic tokens for ordinary light and dark differences.

### 38. Should I memoize every Chakra component?

No. Start with clear composition and measure a real slow view. Add memoization only when the measured cost and component inputs justify it.

### 39. What should a component test check?

Test behavior a person can observe, such as finding a button by role and name, activating it, and seeing the expected result. Avoid tying tests to generated CSS class names.

### 40. What should I do before migrating to v3?

Read the official migration guide, record current package versions, review codemod changes, update the provider and changed compound components, then run tests and check interactions manually.

### 41. What is a good first step when a Chakra import fails?

Check that the installed package version matches the example, confirm the component is exported from the documented package path, and compare the syntax with the current component reference.

### 42. Why might a custom theme color not appear?

Check the token path and the required `value` wrapper, then confirm the application provider uses the system that contains the token.

### 43. How do I find all the chapter code in one place?

Open [All code samples](./98-all-code-samples.md). It collects the fenced examples from the core chapters.

## References

- [Chakra UI documentation](https://chakra-ui.com/docs/get-started/installation)
- [Chakra UI migration guide](https://chakra-ui.com/docs/get-started/migration)
- [React documentation](https://react.dev/learn)
