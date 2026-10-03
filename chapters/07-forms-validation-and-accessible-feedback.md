# 7. Forms, validation, and accessible feedback

[Back to notes index](../README.md)

| Previous | Notes index | Next |
| --- | --- | --- |
| [Previous: Recipes and slot recipes](./06-recipes-and-slot-recipes.md) | [Notes index](../README.md) | [Next: Dialogs, menus, popovers, and overlays](./08-dialogs-menus-popovers-and-overlays.md) |

## Give every field a clear name

A label tells people what information belongs in a control. Helper text can explain a format or consequence. Error text should describe what needs to change. Chakra UI v3 groups these parts with `Field`.

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

Keep the label visible. Placeholder text is a hint that disappears while typing, so it should not replace the label. Use a useful `name` and suitable input type so browsers can support autofill and validation.

## Handle a form with React state

Controlled fields keep their current value in React state. This is useful when the screen needs to validate input, show a live summary, or submit values through application code.

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

The form owns submission. It prevents a page reload, checks the value, and reports a result. The field's `invalid` prop lets Chakra expose an invalid state, while the error text explains the problem. A status message uses `role="status"` so assistive technology can announce the result without moving focus.

This validation is only an example of client-side feedback. Important rules must also be checked by the server because browser code can be changed or bypassed.

## Choose controlled or native form handling

Use controlled state when the UI depends on the current value or when React handles submission. A simple native form can be easier when the browser should manage basic required and type validation. In either approach, give controls labels and provide clear error feedback.

Do not validate only on blur if users can submit without visiting every field. On submit, report errors and help the user locate the first invalid field when the form is long.

## Use the right feedback pattern

- Associate visible instructions with the field they explain.
- Put an error next to the affected control.
- Explain how to fix an error instead of only saying that it is invalid.
- Keep entered values when validation fails.
- Announce asynchronous success or failure with a status region.
- Do not rely on red color alone to communicate an error.

## Practice

1. Add a required name field and its helper text.
2. Clear an error when the user edits the field.
3. Add a confirmation message after a valid submit.
4. Test the form with keyboard only and with an empty value.

## References

- [Chakra UI Field](https://chakra-ui.com/docs/components/field)
- [Chakra UI Input](https://chakra-ui.com/docs/components/input)
- [React: Managing state](https://react.dev/learn/managing-state)
- [MDN: Client-side form validation](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Forms/Form_validation)
