# Next.js Fundamentals

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

### Underscore Folders

`app/_components/Nav.js`

Folders beginning with an underscore are considered private and are excluded from the routing system.

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
  title: {
    template: "%s / Next.js App",
    default: "Next.js App",
  },
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

The `RootLayout` component wraps your application and ensures a consistent HTML structure, with the `children` prop rendering the content of your individual pages.

The `metadata` object allows you to define default metadata, such as the title and description, which can be used throughout your app. The `%s` flag will be replaced by the title of the current page.

### Nested Layout

If a `layout.js` file is used inside a nested route, for example `contact/layout.js`, then the "contact" route and all its nested routes will use this local layout.

## Metadata

We can export metadata from each individual page, so that will then override the metadata from the root layout.

```jsx
// app/contact/page.js

export const metadata = {
  title: "Contact Us",
};

export default function Page() {
  return <h1>Contact Us</h1>;
}
```

### Favicon

To add a favicon to next.js application all you have to do is to add the icon file to the root of the `app` folder. The file needs to have the name of `icon.*`, the extension doesn't matter.

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

The `app/loading.js` file can be used to automatically display a global loading indicator during data fetching or route transitions.

```jsx
// app/loading.js
export default function Loading() {
  return <p>Loading...</p>;
}
```

Enabling a file like this will active streaming in our application, which means that the client will require JavaScript to work properly.

### Route based Loading Indicator

`app/contact/loading.js` will display a loading indicator during data fetching only for the "contact" route segment.

## Loading and Optimizing Fonts

the `next/font` package provides built-in support for Google Fonts and other font sources. The `Roboto` function from `next/font/google` is imported and called with an object containing configuration options. This function handles the loading of the font files. The options include:

- `subsets` : Specifies the character sets to include, such as "latin".
- `display` : Defines how the font should be displayed while loading; "swap" ensures that text remains visible during the font load by using a fallback font initially.
- `weight` : Specifies the font weights to be loaded, such as "400" and "700".
- `style` : Defines the styles to be included, such as "normal" and "italic".

```jsx
// app/layout.js

import { Roboto } from "next/font/google";

const roboto = Roboto({
  subsets: ["latin"],
  display: "swap",
  weight: ["400", "700"],
  style: ["normal", "italic"],
});

export default function RootLayout({ children }) {
  return (
    <html className={`${roboto.className}`}>
      <body>{children}</body>;
    </html>
  );
}
```

By specifying subsets, weights, and styles, you only load the necessary parts of the font, which reduces the page's load time and improves performance. The `display: "swap"` setting helps maintain a better user experience by minimizing flash of unstyled text.

The `roboto` variable holds the `className` property which can be used to apply the font.

### Variable Fonts vs Static Fonts

- **Variable Fonts** maintain a single file having all the possible variations.
- **Static Fonts** maintain a different file for each variation.

Use [**Variable Fonts**](https://fonts.google.com/variablefonts) for better performance and flexibility.

If loading a variable font, you don't have to specify the font weight.

## Optimizing Images

Next.js `<Image />` Component.

```jsx
import Image from "next/image";

export default function Page() {
  return <Image src="logo.png" alt="Logo" width="50" height="50" />;
}
```

What the `Image` component does:

1. It automatically serve correctly sized images im modern formats, such as WebP, and only when necessary.
2. It prevents layout shifts, it forces us to specify the exact width and height of the image.
3. It automatically lazy loads images only when they actually enter the viewport.

### Statically Imported Image

When we serve a statically imported image to the `Image` component, Next.js will be able to analyze the image first, and then we do not need to specify the width and height.

```jsx
import Image from "next/image";
import logo from "@/public/logo.png";

export default function Page() {
  return <Image src={logo} alt="Logo" priority={true} quality={75} />;
}
```

Also, in this situation we can add a few other properties:

- `priority` : Should only be used when the image is visible above the fold. Defaults to `false`.
- `quality` : The quality of the image defined in percentages.
- `placeholder="blur"` : will make the image blur at loading.

### fill

```jsx
import Image from "next/image";

export default function Page() {
  return (
    <div className="relative aspect-square">
      <Image src="bg.jpg" fill class="object-cover object-top" />
    </div>
  );
}
```

- `fill` : this prop will make the image fill the entire relative containing block.
- `object-fit: cover;` and `object-position: top;` complement this technique.

`fill` allows us to use static images without specifying width and height.

### Notes

If the image `src` comes from an api, you have to specify that in the `next.config.mjs` file.

https://nextjs.org/docs/messages/next-image-unconfigured-host

# draft

Custom react hook that next.js provides that gives the current url (works in client components only).

```js
"use client";
import { usePathname } from "next/navigation";

function Component() {
  const pathname = usePathname(); // /about
}
```

<br>
<br>
<br>

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
