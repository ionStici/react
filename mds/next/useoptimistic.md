# Optimistic UI

**Optimistic UI** is a technique used to improve user experience by immediately updating the UI to reflect the expected outcome of an operation, even before the operation completes. This makes the application feel faster and more responsive.

## Benefits of Optimistic UI

- **Improved User Experience:** By immediately reflecting changes in the UI, users perceive the application as faster and more responsive.
- **Reduced Wait Time:** Users don't have to wait for the server response before seeing the results of their actions.

## Example: The `useOptimistic` Hook

```js
"use client";

import { useOptimistic } from "react";
import { deleteItem } from "apis";
import Item from "components";

export default function List({ items }) {
  const [optimisticList, optimisticDelete] = useOptimistic(
    items,
    (currentItems, itemId) => {
      return currentItems.filter((item) => item.id !== itemId);
    }
  );

  async function handleDelete(itemId) {
    optimisticDelete(itemId);
    await deleteItem(itemId);
  }

  return (
    <ul>
      {optimisticList.map((item, i) => {
        return <Item key={i} item={item} onDelete={handleDelete} />;
      })}
    </ul>
  );
}
```

**The `useOptimistic` hook takes in two arguments:** (1) the initial state, (2) a state update function which will determine the next optimistic state.

**Outputs:** (1) the optimistic data, (2) a function that triggers the state update function defined as the second argument.

The component renders the list of items using `optimisticList`, ensuring that the UI is updated optimistically. If the async operation fails, the deleted item is put back.
