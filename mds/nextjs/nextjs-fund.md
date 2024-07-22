# Next.js Fundamentals

[**Next.js**](https://nextjs.org/docs) is a React framework for building full-stack web applications. You use React Components to build user interfaces, and Next.js for additional features and optimizations.

## Table of Content

- [Setting Up a Next.js Project](#setting-up-a-nextjs-project)
- [Creating Routes](#creating-routes)
- [Link](#link)
- [The `layout` file](#the-layout-file)
- [Fetching Data in a Page](#fetching-data-in-a-page)
- [Client Components](#client-components)
- [Displaying a Loading Indicator](#displaying-a-loading-indicator)

## Setting Up a Next.js Project

```
npx create-next-app my-next-app
```

## Creating Routes

In Next.js, the `app` directory is used to define routes. Each subdirectory within `app` corresponds to a segment of the URL path.

Every route is represented by a React Component exported from a `page.js` file within the respective folder. The folder's name becomes the URL segment for that route.

To create nested routes, you simply nest folders within each other.

- `app/page.js` : represents the Home Page.
- `app/about/page.js` : corresponds to the "about" route.
- `app/about/team/page.js` : sets up the "about/team" nested route.

## Link

The `Link` component is used to navigating between pages.

```jsx
import Link from "next/link";

export default function Page() {
  return <Link href="/about">About</Link>;
}
```

## The `layout` file

Every Next.js app requires a global layout, defined in the `app/layout.js` file. This file is responsible for the overall HTML structure of your application.

```jsx
// app/layout.js

export const metadata = {
  title: "Next.js",
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

The `metadata` object allows you to define default metadata, such as the title and description, which can be used throughout your app.

The `RootLayout` component wraps your application and ensures a consistent HTML structure, with the `children` prop rendering the content of your individual pages.

## Fetching Data in a Page

In the `app` folder, each component is a server component. Therefore, data fetching occurs on the server.

```jsx
export default async function Page() {
  const response = await fetch("api");
  const data = await response.json();

  return null;
}
```

The fetched data is cached, meaning that subsequent requests for the same data will retrieve it from the cache rather than making a new fetch request.

## Client Components

The `'use client'` directive designates a component as a client component.

```jsx
"use client";

import { useState } from "react";

export default function Counter({ c }) {
  const [count, setCount] = useState(c);

  return <p>{count}</p>;
}
```

**Props:** Client components can receive props from server components.

**Initial Render:** Initially, all components, including client components, are rendered on the server.

**Hydration:** Once the components are sent to the client, client components are hydrated to add interactivity.

## Displaying a Loading Indicator

The `app/loading.js` file can be used to automatically display a loading indicator during data fetching or route transitions.

```jsx
// app/loading.js
export default function Loading() {
  return <p>Loading...</p>;
}
```
