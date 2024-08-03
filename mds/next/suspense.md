## Suspense

**Suspense** : built-in React component that we can use to catch/isolate components (or entire subtrees) that are not ready to be rendered ("suspending").

What causes a component to be suspending? Fetching data using a library that supports Suspense (React Query & Next.js), and Loading code with React's lazy loading.

Native way to support asynchronous operations in a declarative way (no more isLoading state and render login).

How Suspense works: During the render process, React finds the suspending component and discards its children from rendering, then a fallback component is rendered instead. Once the async work is done, so once the suspended component is ready, React will render the subtree under the Suspense boundary. How Suspense knows if the child component is fetching data? Behind the scenes the child component throws a promise which will then trigger the Suspense boundary.

The fallback will NOT be shown again if the function that triggered the Suspense (e.g. fetch call) is wrapper in a transition using React `startTransition`. In Next.js all navigation pages are wrapped inside transitions, this means if a component re-fetches data as a result of a page transition, the fallback will not be rendered again, we can reset the Suspense boundary by passing a unique key prop.

Note: Components do NOT automatically suspend just because an async operation is happening inside them. Integrating async operations with Suspense is hard, so we use libraries such as Next.js, Remix and React Query that implemented Suspense.

```jsx
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
