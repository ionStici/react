# File-based Routing

## Table of Content

- [Introduction](#introduction)
- [Dynamic Routes](#dynamic-routes)
- [Catch-all Routes](#catch-all-routes)
- ["Link" Component](#the-link-component)
- [Navigating Programmatically](#navigating-programmatically)
- [Adding a Custom 404 Page](#adding-a-custom-404-page)
- [File-based Routing Notes](#file-based-routing-notes)

<br>

## Introduction

**Next.js** infer routes from the folder structure by rendering react components.

`pages/` directory contains the files that infer routes.

`pages/index.js` special file / main page.

`pages/about.js` next.js will infer the route: `/about`

Each file in the `pages/` directory represents a URL path accessible by the name of the file.

### Nested Routes

`pages/blog/index.js` creates `/blog`

`pages/blog/preview.js` creates `/blog/preview`

In any folder, the `index.js` file is treated as the default route for that folder.

<br>

## Dynamic Routes

`pages/blog/[id].js` creates `/blog/any`

The square brackets turn the file into a dynamic route, any url string becomes valid by triggering this component whenever the url does not have an explicit route.

Then, based on the value of the url segment, we can dynamically render different React components.

### useRouter Hook - extract dynamic routes

```js
import { useRouter } from "next/router";

function Page() {
  const rotuer = useRouter();

  router.pathname; // /blog/[id]
  router.query; // {id: 'any'}
  router.query.id; // 'any'

  return null;
}
```

### Building Nested Dynamic Routes

`pages/blog/[topic]/[id].js` dynamic file within a **dynamic folder**.

`/blog/tech/next` this will trigger the file structure from above.

The URL strings `tech` and `next` are processed dyamically by next.js

```js
router.query; // {topic: 'tech', id: 'next'}
```

<br>

## Catch-all Routes

The catch-all file route will catch all the path segments in the url chain.

Brackets with **three dots** followed by a placeholder name.

Directory structure: `pages/blog/[topic]/[...slug].js`

URL example: `/blog/tech/next/welcome/1`

```js
const router = useRouter();
router.query; // { topic: 'tech', slug: ['next', 'welcome', '1'] }
```

We get an array with all the url segments in order.

<br>

## The "Link" Component

The `<Link>` component helps us navigate to new routes, by preventing the browser's default behavior of sending a http request.

```jsx
import Link from "next/link";

function Page() {
  return (
    <>
      <Link href="/">Home</Link>;
      <br />
      <Link
        href={{
          pathname: `/blog/[topic]/[...slug]`,
          query: { topic: "tech", slug: ["next", "welcome"] },
        }}
      >
        Next
      </Link>
      // Navigate to: /blog/tech/next/welcome
    </>
  );
}
```

To dynamically navigate using the `Link` component, the `href` prop can receive an object with `pathname` for the actual path, and `query` for the path placeholder.

The `replace` prop of the `Link` component will prevent pushing a new route, instead it will replace the current page with that new rotue, this will not store the routes in the browser history API.

<br>

## Navigating Programmatically

```js
import { useRouter } from "next/router";

export default function HomePage() {
  const router = useRouter();

  function navigate() {
    // router.push("/blog");

    router.push({
      pathname: "/blog/[topic]/[...slug]",
      query: { topic: "tech", slug: ["next", "welcome"] },
    });
  }

  return <button onClick={navigate}>Navigate</button>;
}
```

Using the `router.push()` method, we can programmatically navigate to a different page.

Just like `Link`, `push()` accepts an object with routes as well.

`router.replace()` to replace the current page instead of pushing (we can't go back after navigation).

<br>

## Adding a Custom 404 Page

`/pages/404.js` Next.js will always load the component returned by `404.js` file when a 404 error arises.

<br>

## File-based Routing Notes

Regular React components should not be in the `pages/` folder, this folder should only contain page files, because whatever files we create inside here, it will then generate and automatically lead to routes. Instead, create a `components/` folder for react components, outside `pages/`.

`pages/blog/[post].js` and `pages/blog/[...post].js` paths - the route containing `[post].js` will be triggered when the url will contain only 1 segment, such as `blog/next`, but `[...post].js` file route will be triggered only when the url will contain more than 1 segment, such as `blog/next/welcome`, so these 2 routes will not interfere with each other. The three dots path consumes as unlimited amount of segments
