## Static vs Dynamic Rendering

## Types of SSR

1. **Static Site Generation (SSG):** The HTML is generated at build time by the bundler, and is served like that to all users.
2. **Dynamic SSR:** The HTML is generated on each request, making it ideal for pages with frequently changing data.

# Types of SSR: Static vs Dynamic Rendering

## Server-Side Rendering in Next.js

Next.js is a React framework, so rendering is done by React, following the rules of SRC model. In SSR, both Server and Client components are rendered on the server on the initial render.

Next.js uses the React and React-dom libraries in order to render each route one by one on the server.

Each route can be either static (pre-rendered) or dynamic. So it's not the entire app that is either rendered statically or dynamically, instead its each route individually.

There is also **Partial Pre-Rendering (PPR)** which mixes dynamic and static rendering in the same route.

## Static vs Dynamic Rendering

### Static Rendering

HTML is generated at built time.

Useful when data doesn't change often and is not personalized to the user.

**Incremental Static Regeneration (ISR)** - a route can be re-rendered periodically in the background by refetching the routes data from time to time.

By default, all routes are rendered statically, even when a component fetches some data

Static pages are way faster, first because they have already been pre-generated and don't need to be rebuilt for every request, which saves time and resources, and second because static assets like this can very easily be hosted on a so-called CDN (content delivery network).

If all routes of an app are static, the entire app can be exported as a static site (SSG).

### Dynamic Rendering

HTML is generated at request time (for each new request reaches the server).

Useful if the data changes frequently and is personalized to the user.

Dynamic rendering is also necessary when rendering a route requires information that depends on request (e.g. search params from the url).

A route automatically switches to dynamic rendering in certain conditions.

Dynamic routes when deployed to vercel will become a serverless function.

## When Next.js Switches to Dynamic Rendering

Usually, developers don't directly choose whether a route should be static or dynamic. Next.js will automatically switch to dynamic rendering in the following scenarios:

1. The route has a dynamic segment (page uses `params`)

   The dynamic segments can only be known at request time, therefore the route needs to become dynamic.

2. `searchParams` are used in the page component.

3. `headers()` or `cookies()` are used in any of the route's server components.

4. An uncached data request is made in any of the route's server components.

These scenarios depend on the incoming request, which means that the route must be rendered a new for each of these incoming requests.

## Terminology

**Content Delivery Network (CDN):** A network of servers located around the glove that cache and deliver a website's static content (HTML, CSS, JS, images) from as close as possible to each user. The advantage of CDN is that the data doesn't have to travel across the entire planet from the website's host to the user, it will just come from the server that is physically closest to the user. Most hosting providers like Vercel, Netlify, Render, will automatically host all your website's static assets on a global CDN like this.

With the **Serverless Computing** model, we can run application code, usually backend code, without managing the server ourselves. Instead, we can just run single functions on a cloud provider such as AWS or Vercel or many others, and we call these functions: **Serverless Functions**. The server is initialized and active only for the duration the serverless function is running, unlike a traditional Node.js app where the server is constantly running. _Remember: each dynamic route becomes a serverless function._ This means that out next.js app is a collection of serverless functions with the servers automatically managed by Vercel. If we have static routes, none of this applies, in this case all of these static routes will simply be built on build time and then hosted on a CDN

**The "edge":** "As close as possible to the user". A CDN is part of an "edge" network, but there is also serverless "edge" computing. This is serverless computing that does not happen on a central server, but on a network that's distributed around the globe, as close as possible to the user (like a CDN but for running code). Important: we can select certain routes to run on the edge when deployed to Vercel.

**Incremental Static Regeneration (ISR):** A Next.js feature that allows developers to update the content of a static page, in the background, even after the website has already been built and deployed. This happens by re-fetching the data of a component on entire route after a certain interval.

## Notes

Next.js decides how each of the routes are rendered, either static or dynamic.

## Making Dynamic Pages Static With generateStaticParams

Create a `generateStaticParams` function to let Next.js know about all the possible values of a dynamic URL segment, so that we can then export those pages as static pages.

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

This function needs to return an array of object, and each object should contain a key representing the dynamic segment.

If you have a finite set of values for a dynamic segment of a URL, it's always a good idea to tell Next.js about those by using the `generateStaticParams` function, this is a lot better for performance.

## Static Side Generation (SSG)

Static Side Generation - deploy your site to any hosting provider that supports static sites.

```mjs
// next.config.mjs

/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export",
};

export default nextConfig;
```

By defining this `output: "export"` next.js configuration, our site will get exported as static assets.

After we run the build script with this configuration, we get a `out` folder containing static assets, and also we get txt files for each route which is a real RSC payload, this is just what's necessary for Next.js to implement the client side navigation, so that makes it fell like we are still on a SPA.

To make the images work using the `Image` component, because this component uses Vercel's image optimization API, (1) we can either don't use the `Image` component, (2) or use a service like Cloudinary.
