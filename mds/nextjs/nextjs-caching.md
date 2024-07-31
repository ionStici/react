# Caching Mechanisms in Next.js

[Caching in Next.js | Documentation](https://nextjs.org/docs/app/building-your-application/caching)

**Caching** is the process of storing data in a temporary storage location, reducing the need for repeated fetching or computation, allowing for quicker access and reduced latency when the same data is needed again.

In **Next.js**, caching (enabled by default) optimizes performance by storing server-side data, pre-rendered pages, and client-side navigation data.

## Table of Contents

- [1. Request Memoization](#1-request-memoization)
- [2. Data Cache](#2-data-cache)
- [3. Full Route Cache](#3-full-route-cache)
- [4. Routes Cache](#4-routes-cache)
- [Caching Configuration](#caching-configuration)
  - [Revalidation Techniques](#revalidation-techniques)
  - [Opting Out of Caching](#opting-out-of-caching)
- [Incremental Static Regeneration (ISR)](#incremental-static-regeneration-isr)
  - [Route Level](#route-level)
  - [Component Level](#component-level)

## 1. Request Memoization

**Request Memoization** is a per-request server-side caching technique for storing data from identical GET requests during a single page render cycle.

If multiple components request the same data during one page render, this mechanism ensures that only one network request is made, and the data is reused across the components on the server.

Useful when the same data is needed in different parts of the component tree, eliminating the need to fetch the data at the top level and pass it down the component tree via props.

Limitations: Works only with the native `fetch` function and identical requests (same URL and options).

## 2. Data Cache

**Data Cache** is a persistent server-side mechanism that stores fetched data, meaning the same data remains available across multiple server requests to all users and even survives re-deployments of the application, unless explicitly revalidated.

The Data Cache ensures that once data is fetched, subsequent requests for the same data can be served quickly without re-fetching from the original source.

This mechanism supports Incremental Static Regeneration (ISR), allowing static pages to be regenerated with new data. Revalidation strategies, either time-based or on-demand, control when the cache should be refreshed with new data.

## 3. Full Route Cache

**Full Route Cache** stores on the server entire static pages as HTML and RSC payloads.

This caching mechanism is established during the build process and helps deliver pre-rendered content quickly to users. It is tied to the Data Cache, meaning that when the Data Cache is invalidated or the application is redeployed, the Full Route Cache is also updated.

## 4. Routes Cache

**Routes Cache** is a client-side mechanism that stores pre-fetched pages and the pages visited by the user within the browser. This caching technique enhances the user experience by enabling quick navigation between pages, providing a smooth Single Page Application feel.

However, since this cache can hold both static and dynamic pages, it may lead to displaying stale data if dynamic content is not revalidated. Pages in the Routes Cache are stored temporarily, by default static pages are cached for up to 5 minutes and dynamic pages for around 30 seconds (can be adjusted). Users can force a refresh by performing a hard reload or reopening the browser tab.

### Note

This is how caching works in production when the app is deployed. During development caching is not activated.

## Caching Configuration

### Revalidation Techniques

- **Request Memoization:** Not applicable.

- **Data Cache:**

  - _Automatic (Time-based):_ Set with `export const revalidate = <time>` in seconds.
  - _Manual (On-Demand):_ Use `revalidatePath` or `revalidateTag`.

- **Full Route Cache:** Will revalidate when Data Cache revalidates.

- **Router Cache:** Can be refreshed using `revalidatePath`, `revalidateTag`, `router.refresh`, or by modifying cookies with `cookies.set` or `cookies.delete`.

### Opting Out of Caching

- **Request Memoization:** Use `AbortController`.

- **Data Cache:**

  - _Entire Page:_ Set `export const revalidate = 0;` (always revalidates).
  - _Individual Request:_ Use `fetch("", { cache: "no-store" })`.
  - _Server Component:_ Use `noStore()`.

- **Full Route Cache:** Opting out of Data Cache also opts out of Full Route Cache.

- **Router Cache:** Cannot be disabled.

## Incremental Static Regeneration (ISR)

### Route Level

To activate ISR, set `export const revalidate = <time>` in a route component to determine how often the page should be regenerated.

```js
// page.js
export const revalidate = 3600; // in seconds
```

### Component Level

```js
import { unstable_noStore as noStore } from "next/cache";

function FetchDataComponent() {
  noStore();
}
```
