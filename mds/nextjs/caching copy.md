### 3. Full Route Cache

Server

This mechanism stores the entire static pages in the form of HTML and RSC payload at the build time.

- Building static routes and storing them as HTML and RSC payload.
- How long? Until the "Data Cache" is invalidated (or app is re-deployed).

### 4. Routes Cache

- Where? Client
- This cache is used to store in the browser all the pre-fetched pages, as well as all pages that the user visits while navigating around the app.
- This applies to static and dynamic routes.
- The idea behind this cache is that having all the pages stored in memory allows for instant navigation, gives the user the feel of SPA.
- The problem with this cache is the fact that pages are not requested from the server again, as the user navigates back and forth, which can lead to stale data being displayed. Pages are stored for 30 seconds if they are dynamic and for 5 minutes if they're static with no way of revalidating this cache.
- Unless the user performs a hard reload or closes and reopens the tag, we run into the possibility of rendering outdated data.

## Note

Note: This is how caching works in production when the app is deployed. During development caching is not activated.

## Caching Configuration

How to configure each cache, how we can revalidate each cache and how we can opt out.

### How to Revalidate?

- Request Memoization: Not Applicable

- Data Cache: Time-based revalidation (automatic) for all data on page `export const revalidate = <time>`. Alternatively we can Time-based revalidation (automatic) for one data request `fetch("", { next: { revalidate: <time> } })`. And On-Demand Revalidation (manual): `revalidatePath` or `revalidateTag`. Revalidating the Data Cache also revalidates Full Route Cache (ISR).

- Router Cache: `revalidatePath` or `revalidateTag` in server actions. `router.refresh`. `cookies.set` or `cookies.delete` in SA.

### How to opt out?

- Request Memoization: `AbortController`

- Data Cache: Entire page `export const revalidate = 0;` 0 seconds means to always revalidate. Individual Request: `fetch("", { cache: "no-store" })`. Individual server component: `noStore()`. These forces page to become dynamic, which also opts out of the Full Route Cache. As the page becomes dynamic, the full route cache no longer caches it, because this cache is only for static pages.

- Router Cache: Not possible.

## Incremental Static Generation

Route Level.

To activate ISG all we have to do is to export the `revalidate` variable from a route component with a positive time value. The value of this var depends on how often the data changes.

```js
// page.js
export const revalidate = 3600; // in seconds
```

### Component Level

```js
import { unstable_noStore as noStore } from "next/cache";

function FetchDataComponent() {
  noStore();

  return null;
}
```
