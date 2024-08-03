# Suspense

**Suspense:** A built-in React component used to catch or isolate components (or entire subtrees) that are not ready to be rendered (i.e., "suspending").

```js
import { Suspense } from "react";
import Spinner from "Spinner";
import FetchData from "FetchData";

export default function Page() {
  return (
    <div>
      <h1>Home</h1>

      <Suspense fallback={<Spinner />}>
        <FetchData />
      </Suspense>
    </div>
  );
}
```

_In this example:_

- The `Suspense` component catches the `FetchData` component if it suspends.
- The `Spinner` component is shown as a fallback while `FetchData` completes its asynchronous operations.

## Causes of Suspension

- **Data Fetching:** Using libraries that support Suspense, such as React Query and Next.js.
- **Code Loading:** Utilizing React's lazy loading for asynchronous operations.

## Purpose

Suspense provides a native way to handle asynchronous operations declaratively, eliminating the need for `isLoading` state and manual rendering logic.

## How Suspense Works

1. **Detection:** During rendering, React identifies the suspending component and prevents its children from rendering.
2. **Fallback Rendering:** A fallback component (e.g., a spinner) is rendered instead.
3. **Completion:** Once the asynchronous work is complete, React renders the subtree under the Suspense boundary.

## Behind the Scenes

- When a child component fetches data, it throws a promise, triggering the Suspense boundary.
- The fallback component will not be shown again if the function triggering Suspense is wrapped in a transition using React's `startTransition`.

## Integration with Next.js

- In Next.js, all navigation actions are wrapped in transitions. This means if a component re-fetches data during a page transition, the fallback component is not rendered again.
- You can reset the Suspense boundary by passing a unique `key` prop.

## Note

Components do not automatically suspend just because an async operation occurs within them. Integrating async operations with Suspense can be complex, so libraries like Next.js, Remix, and React Query provide built-in support for Suspense.
