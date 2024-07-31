# draft

## Environment Variables

https://nextjs.org/docs/app/building-your-application/configuring/environment-variables

```env
SUPABASE_URL=
SUPABASE_KEY=
```

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

## Dynamic Route Segments

`app/about/[team]/page.js`

To use dynamic route segments just create a folder using square brackets.

```jsx
// about/john
export default function Page({ params }) {
  console.log(params); // { team: "john" }
  return <p>{params.team}</p>;
}
```

The `params` prop will be passed to the dynamic route component containing the value of the dynamic segment.

## Dynamic Metadata

```jsx
export async function generateMetadata({ params }) {
  const { name } = await fetchData(params.id);
  return { title: `Page ${name}` };
}

export default function Page({ params }) {
  return null;
}
```

## Error Boundaries

Convention file: `app/error.js`

```jsx
"use client";

export default function Error({ error, reset }) {
  return (
    <>
      <p>{error.message}</p>
      <button onClick={reset}>Reset</button>
    </>
  );
}
```

The error boundary needs to be a client component.

This function will get the `error` object and a `reset` function that resets the error.

We can have nested error boundaries for nested routes.

Error boundaries will catch all errors that happen in the rendering process, this means that errors that happen in callback functions will not be caught by React error boundary. Also, this `error.js` does not catch errors in the root layout.

`app` Routing Conventions:

https://nextjs.org/docs/getting-started/project-structure#app-routing-conventions

## Not Found

`app/not-found.js`

```js
export default function NotFound() {
  return <p>Page Not Found</p>;
}
```

`not-found.js` supports nesting routes as usual, for custom not found messages.

### notFound Function

We can trigger the NotFound component manually by calling the notFound function.

```js
import { notFound } from "next/navigation";

export async function getData(id) {
  const data = await fetch(`api/${id}`);

  if (!data) notFound();

  return data;
}
```