# The useFormStatus Hook

The [`useFormStatus`](https://react.dev/reference/react-dom/hooks/useFormStatus) hook provides status information about the form submission.

**Important:** `useFormStatus` must be used inside a component that is located inside the `form` element, and not directly within `form`.

```js
"use client";

import { useFormStatus } from "react-dom";

export default function Button({ children, pendingLabel }) {
  const { pending } = useFormStatus();

  return (
    <button disabled={pending}>{pending ? pendingLabel : children}</button>
  );
}
```

The `pending` boolean property becomes `true` if the `from` is pending submission, otherwise `false`.

**Use case:** Useful for providing feedback to the user when the form is currently submitting or not.
