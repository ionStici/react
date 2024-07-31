# Dynamic and Static Rendering (SSR and SSG)

## Table of Contents

- [Introduction](#introduction)
- [Static Site Generation (SSG)](#static-site-generation-ssg)
- [Dynamic Rendering (SSR)](#dynamic-rendering-ssr)
- [When Next.js Switches to Dynamic Rendering](#when-nextjs-switches-to-dynamic-rendering)
- [Configuring SSG](#configuring-ssg)
- [Making Dynamic Pages Static With generateStaticParams](#making-dynamic-pages-static-with-generatestaticparams)
- [Terminology](#terminology)

## Introduction

- **Server-Side Rendering in Next.js:** Each page route is individually rendered on the server either statically or dynamically. So it's not the entire app being static or dynamic, instead its each route individually.

- **Static Site Generation (SSG):** HTML is generated at build time and served as static files to all users. This approach is ideal for content that doesn't change often.

- **Dynamic SSR:** HTML is generated on each request, suitable for pages with frequently changing data.

- **Partial Pre-Rendering (PPR)** is an additional strategy that mixes dynamic and static rendering within the same route.

## Static Site Generation (SSG)

In static rendering, the bundler generates the HTML and other static assets during the build process. By default, all routes are rendered statically, even when a component fetches some data. And if all routes in an app are static, the entire app can be exported as a static site using SSG.

Static pages are faster because they are pre-generated and don't need to be rebuilt for each request. This not only saves time and resources but also enables easy hosting on CDNs (Content Delivery Networks).

**Incremental Static Regeneration (ISR):** This feature allows specific routes to be re-rendered periodically in the background by refetching updated data without requiring a full rebuild.

## Dynamic Rendering (SSR)

In dynamic rendering, HTML is generated at request time for each incoming request. Useful when data changes frequently or is personalized for each user, such as in pages that depend on request-specific information like URL search parameters.

A route can automatically switch to dynamic rendering based on certain conditions, and when deployed to platforms like Vercel, they are treated as serverless functions.

## When Next.js Switches to Dynamic Rendering

Next.js will switch a route to dynamic rendering under the following conditions:

1. **Dynamic Segments:** If a route uses dynamic segments (e.g., `params`), the segment values are only known at request time, necessitating dynamic rendering.
2. **Use of searchParams:** If a page component uses `searchParams`, it indicates a dependency on request-specific data.
3. **Use of headers() or cookies():** These functions indicate a need for request-specific data handling in server components.
4. **Uncached Data Requests:** If a server component makes uncached data requests, the route will be dynamically rendered.

These scenarios depend on incoming requests, meaning that the route must be rendered anew to ensure the data is up-to-date and accurate.

## Configuring SSG

We can deploy static sites generated with SSG to any hosting providers that support static assets. To export your Next.js app as a static site, use the following configuration:

```mjs
// next.config.mjs

/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export",
};

export default nextConfig;
```

By setting the `output: "export"` next.js configuration, our app will get exported as static assets.

After running the `build` script, we get an `out` folder containing static assets and the RSC payload txt files for each route, necessary for client-side navigation.

To support images when using the `Image` component, which relies on Vercel's image optimization API, you can either avoid using the component or use a service like Cloudinary for image optimization.

## Making Dynamic Pages Static With generateStaticParams

To make dynamic pages static, you can use the `generateStaticParams` function to specify all possible values for a dynamic URL segment. This allows Next.js to pre-render these pages as static content.

```jsx
// app/about/[id]/page.js

export async function generateStaticParams() {
  const data = await getData();
  const ids = data.map((item) => ({ id: String(item.id) }));

  return ids; // [{ id: '1' }, { id: '2' }, { id: '3' }]
}

export default function Page({ params }) {
  return null;
}
```

This function should return an array of objects, each containing a key that represents the dynamic segment.

If you have a finite set of values for a dynamic segment of the URL, it's always a good idea to tell Next.js about those by using the `generateStaticParams` function, so that the route component could be statically pre-rendered.

## Terminology

**Content Delivery Network (CDN):** A network of servers distributed globally to cache and deliver a website's static content from locations closer to the user. This minimizes latency and speeds up content delivery. Most hosting providers like Vercel, Netlify, Render, will automatically host all your website's static assets on a global CDN like this.

**Serverless Computing:** A cloud computing model where application code, typically backend logic, runs in stateless containers. The server infrastructure is automatically managed, only active during the execution of the code, such as with Vercel's serverless functions. This approach is different from traditional server hosting, where the server is always running.

**The "Edge":** Refers to delivering content or running code as close as possible to the user. CDNs provide edge delivery for static assets, while edge computing involves running code on servers located near the user, reducing latency. Note: we can select certain routes to run on the edge when deployed to Vercel.

**Incremental Static Regeneration (ISR):** A Next.js feature allowing static pages to be updated in the background after deployment, without a full rebuild. This keeps content fresh while maintaining the performance benefits of static pages.
