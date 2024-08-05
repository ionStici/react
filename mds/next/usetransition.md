# The `useTransition` Hook

`useTransition` is a React Hook that lets you update the state without blocking the UI. This hook marks state updates as transitions.

```js
import { useTransition } from "react";

export default function DeleteItem({ itemId, onDelete }) {
  const [isPending, startTransition] = useTransition();

  const handleDelete = () => {
    if (confirm("Delete Item")) {
      startTransition(() => onDelete(itemId));
    }
  };

  return (
    <button onClick={handleDelete}>
      {isPending ? "Deleting..." : "Delete"}
    </button>
  );
}
```

**Use Case:** Render a loading indicator when you call a server action from a button (callback), and not from a form.

Behind the scenes React uses Suspense boundaries to manage these transitions.
