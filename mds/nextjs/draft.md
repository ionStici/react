# Draft

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
