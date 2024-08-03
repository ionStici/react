# Routing and Navigation

## Table of Contents

- [Introduction](#introduction)
- [Creating Routes](#creating-routes)
- [Dynamic Route Segments](#dynamic-route-segments)
- [The Link Component](#the-link-component)
  - [usePathname Hook](#usepathname-hook)
- [Underscore Folders](#underscore-folders)

## Introduction

Next.js uses a file-system based router. Folders are used to define routes and files inside are used to create UI for that route segment.

## Creating Routes

Each subdirectory within `app` corresponds to a route segment of the URL path.

Every route is represented by a React component exported from a `page.js` file within the respective folder. The folder's name becomes the URL segment for that route.

- `app/page.js` : represents the Home Page.
- `app/about/page.js` : corresponds to the "about" route.
- `app/about/team/page.js` : sets up the "about/team" nested route.

To create nested routes, you simply nest folders within each other.

## Dynamic Route Segments

Dynamic routes are denoted by folders with square brackets.

`app/about/[team]/page.js`

```jsx
// about/john

export default function Page({ params }) {
  console.log(params); // { team: "john" }

  return <p>{params.team}</p>;
}
```

The `params` prop will be passed to the dynamic route component containing the value of the dynamic URL segment.

## The Link Component

The `Link` component is used to navigating between pages.

```jsx
import Link from "next/link";

export default function Page() {
  return <Link href="/about">About</Link>;
}
```

### usePathname Hook

Custom react hook that next.js provides that gives the current url (works in client components only).

```js
"use client";

import { usePathname } from "next/navigation";

function Component() {
  const pathname = usePathname(); // /about
}
```

## Underscore Folders

`app/_components/Nav.js`

Folders beginning with an underscore are considered private and are excluded from the routing system.
