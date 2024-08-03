# File Conventions

[**`app` Routing Conventions**](https://nextjs.org/docs/getting-started/project-structure#app-routing-conventions)

## Table of Contents

- [layout.js](#layoutjs)
- [page.js](#pagejs)
- [loading.js](#loadingjs)
- [not-found.js](#not-foundjs)
- [error.js](#errorjs)

## `layout.js`

Every Next.js app requires a global layout `app/layout.js`. This file is responsible for the overall HTML structure of your application.

```js
// app/layout.js

export const metadata = {
  title: { template: "%s / Next.js App", default: "Next.js App" },
  description: "Page Description",
};

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

The `RootLayout` component wraps the entire application and ensures a consistent HTML structure, with the `children` prop rendering the content of the current page.

**Metadata:** The `metadata` object allows you to define default metadata, such as the title and description, which can be used throughout your app. The `%s` flag will be replaced by the title of the current page.

**Nested Layout:** If a `layout.js` file is used inside a nested route, for example `contact/layout.js`, then the "contact" route and all its nested routes will use this additional layout as well.

## `page.js`

File with components that render the UI of route segments.

```js
export default function Page() {
  return <h1>I'm a page</h1>;
}
```

## `loading.js`

The `app/loading.js` file can be used to automatically display a global loading indicator during data fetching or route transitions.

```js
// app/loading.js
export default function Loading() {
  return <p>Loading...</p>;
}
```

Enabling a file like this will activate streaming in our application, which means that the client will require JavaScript to work properly.

**Nested Loading:** `app/contact/loading.js` will display a loading indicator during data fetching only for the 'contact' route segment.

## `not-found.js`

UI for not found route segments.

```js
// app/not-found.js
export default function NotFound() {
  return <p>Page Not Found</p>;
}
```

For custom not found messages, we can nest this file within route folders as usual.

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

## `error.js`

```js
// app/error.js

"use client";

export default function Error({ error, reset }) {
  return (
    <div>
      <p>{error.message}</p>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

The error boundary needs to be a client component. This component will receive the `error` object and the `reset` function that can be used to reset the error. We can have nested error boundaries for nested routes.

Error boundaries will catch all errors that happen in the rendering process. This means that errors that happen in callback functions will not be caught by React error boundary. Also, errors in the root layout are not caught.
