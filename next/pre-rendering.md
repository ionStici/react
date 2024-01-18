# Page Pre-Rendering & Data Fetching

## Table of Content

- [Introduction](#introduction)
- [Static Site Generation & "getStaticProps"](#static-site-generation-with-getstaticprops)
- ["getStaticProps"](#getstaticprops)
- [Running Server-side Code](#running-server-side-code--using-the-filesystem)
- [Incremental Static Generation (ISR)](#incremental-static-generation-isr)
- [The "context" Argument](#the-context-argument)
- ["notFound" property](#notfound-property)
- ["redirect" property](#redirect-property)

<br>
 
## Introduction

In traditional React applications, the browser initially receives an empty html document with a single `div` tag in which the JavaScript bundle will render all the content and components. This may lead to delay in website rendering and poor SEO indexing.

With Next.js, on the other hand, the browser receives a pre-rendered version of the page from the server. Next.js prepares the page in advance by pre-building all the html content and by pre-loading all the data that will eventually be needed.

Even so, the JavaScript bundle is also sent, next.js will hydrate with React code once loaded, so that eventually React takes over the pre-rendered page.

**Consider:** Pre-rendering only affects the initial load, once the page is hydrated, which happenes right after pre-rendering, then we have a traditional single-page-application, React takes over and handles the frontend. If we visit another page of our website, that page is not pre-rendered, instead created on the client by React, only the initial page visit is pre-rendered so that we don't get an empty page until React is ready.

<br>

**Page Pre-Rendering** - generating the HTML for a page in advance, instead of having it all done by the client-side JavaScript.

Next.js has two forms of pre-rendering: **Static-site Generation (SSG)** and **Server-side Rendering (SSR)**

- SSG is when the HTML is generated in advance at build time (before deployment) and will be reused (the html) on each request.

- SSR is when the HTML is generated on each server request (after deployment).

We can mix SSG and SSR. Pre-rendering ensures that your SPA is indexed properly by search engines, and also it can improve performance.

<br>

**Data Fetching** - retrieving data from a backend for your page.

- For SSG, next.js provides the `getStaticProps` function to fetch data at build time.

- For SSR, next.js provides the `getServerSideProps` function to fetch data on each request.

- Next.js also provides the `getStaticPaths` function, used for dynamic routes, which lets you specify which paths you want to pre-render.

These data fetching methods run on the server-side, meaning they do not send any client-side JavaScript to the browser that would affect the SEO of the page.

<br>

Together, these concepts allow you to build _performant, SEO-friendly, data-rich applications_ using Next.js and React.

<br>

## Static Site Generation with "getStaticProps"

**Static Site Generation** - Pre-generate pages (HTML and data) during the **build process**.

After deployment, these pre-generated pages are served instantly for incoming requests. These pre-generated pages once served are still hydrated with React. The idea is that the pages sent to the client are not empty, but populated with content.

`getStaticProps` provides a server-side data fetching mechanism that operates at build time, enabling you to generate static, SEO-friendly, and highly optimized pages that load quickly and provide a great user experience.

- **Build-Time Execution**

  After `next build`, the `getStaticProps` functions in your pages are executed on the server-side during this build process. This is where the data-fetching happens.

- **Server-side Context**

  While `getStaticProps` operates at build time, it runs in a Node.js environment, which is a server-side context. This means you have access to server-side functionalities like connecting to a database, accessing the file system, making HTTP requests to external APIs, and more.

- **Generation of Static HTML**

  The data fetched by `getStaticProps` is used to generated static HTML for each page. This static HTML, along with the fetched data, is saved and later served from a CDN for each incoming request to that page.

- **Client-side Hydration**

  Once the static HTML reaches the client (browser), Next.js "hydrates" the page to add interactivity using React. The data fetched by `getStaticProps` is passed as props to your page component, enabling you to use this data for rendering.

- **Separation of Concerns**

This setup allows for a clean separation of concerns: Server-side operations (like data fetching) happen in `getStaticProps`, whilc client-side rendering and interactivity are handled by your page component.

_How to pre-generate a page?_ Next.js pre-generates pages by default. To implicitly indicate that a page should be pre-generated, just include the `getStaticProps` function.

The `getStaticProps` function can be used only in `pages/` files. In this function we can run any code that would normally run on the server-side only (Node.js Runtime Environment). So, in this function we don't run client-side code, we don't have access to certain client-specific features like the `window` object. Code that we write in `getStaticProps` will not be included in the bundle sent to the browser, we can safely include database credentials in here.

<br>

## "getStaticProps"

`getStaticProps` function can be added to any page file, and only there. You need to export it so that Next.js will call it when it pre-generates the page. This function confirms to Next.js that this page should be pre-generated.

```js
// pages/index.js

export default function HomePage(props) {
  const { data } = props;

  return <h1>Hello World!</h1>;
}

export async function getStaticProps(context) {
  const response = await fetch("api");
  const data = response.json();

  return {
    props: {
      data,
    },
  };
}
```

`getStaticProps` must return an object with a `props` key.

1. `getStaticProps` runs at build time on the server-side
2. It fetches data before the component is pre-rendered
3. Then passes that data as `props` to the page component
4. Then the page component will render, accessing that data through props

None of this happens on the client-side, but during the build process, in a node.js environment.

If we view the Page Source, we can see that the HTML document is filled, so fetching the data did not happen on the client, but on the server.

The `next build` script will create the optimized production-ready build, it's this step which pre-generates the pages. In the terminal we get information about the static pages it generated.

The output of `next build` can be found in `.next/server/pages/index.html`

The `next start` script will start the production ready page with a node.js server.

<br>

## Running Server-side Code & Using the Filesystem

```js
import path from "path";
import fs from "fs/promises";

export default function HomePage(props) {
  const { data } = props;

  return null;
}

export async function getStaticProps() {
  const filePath = path.join(process.cwd(), "data", "todos.json");
  const jsonData = await fs.readFile(filePath);
  const data = JSON.parse(jsonData);

  return { props: { data } };
}
```

Code inside the `getStaticProps` function is executed on the server-side in a Node.js environment, we can use Node.js core modules in here, this means that we can work with the file-system.

Next.js will split our code in a clever way so that imports like `fs` and functions like `getStaticProps` will not make to the final client-side bundle.

The `fs` module from `"fs/promises"` is a special version of the `fs` module that uses promises. Now, `fs.readFile()` will return a promise, so we have to `await` for the response.

The `path` node.js module contains helpful functionalities for building paths. Using `path.join()` we can build a path that will be consumed by `fs.readFile()`

`process.cwd()` node.js object gives the current working directory, the root. This object will tell `path.join()` to start from the root directory, then the following arguments are the paths to the data we are looking for.

<br>

## Incremental Static Generation (ISR)

Next.js mechanism to provide fresh content with a static-site generation approch.

```js
export async function getStaticProps() {
  const data = "...";

  return {
    props: { data },
    revalidate: 10, // Re-generate the page every 10 seconds
  };
}
```

Code from `getStaticProps` executes on the server-side, but not on an actual server which serves the application, instead it runs on our machine in the build process when we run `next build`.

What to do if data changes frequently? Re-build & Re-deploy all the time? Or maybe `useEffect`?

Even if `getStaticProps` function executes when we run the `build` script, we can continuously update the page after deployment without re-deploying, using a Next.js built-in feature:

**Incremental Static Generation** allows you to update static pages after they have been generated, on a per-page basis, without needing to rebuild the entire site.

Specify a `revalidate` period in seconds, this tells next.js how often to regenerate a given page.

### Regeneration Process

When a request is made to a page, Next.js will check the revalidation period.

If the page is "stale" (older than the revalidation period), Next.js will server the stale page to the user for a faster response, while triggering a regeneration process on the server.

The regeneration process involves executing `getStaticProps` function again on the server to fetch the latest data and generate a new version of the page.

Once the regeneration process is complete, the newly generated page replaces the old (stale) page in the cache. Subsequent requests from users will receive the update page until it becomes stale again, at which point the process repeats.

Regeneration occurs on the server-side when a stale page is requested, allowing for the page to be update without blocking user requests. The client-side is not involved in the regeneration process, it merely receives the potentially stale page.

<br>

## The "context" Argument

The `getStaticProps(context)` function when called by Next.js, receives a `context` object argument.

```js
export async function getStaticProps(context) {}
```

`context` is an object with extra information about the page when the function is executed, it holds dynamic params, dynamic path segment values, and more.

<br>

## "notFound" property

```js
export async function getStaticProps() {
  // ... data fetching

  if (!data) {
    // if data is not found, return 404 error
    return {
      notFound: true,
    };
  }

  return {
    // otherwise, return the data as a prop
    props: { data },
  };
}
```

The `notFound` key when set to `true`, the page will return a `404` error, therefore `404.html` will render.

<br>

## "redirect" property

`redirect` the user to another page

```js
export async function getStaticProps() {
  // ... data fetching

  if (!data) {
    // if data is not found, redirect to another page
    return {
      redirect: {
        destination: "/no-data",
        permanent: false, // This redirection is not permanent
      },
    };
  }

  // otherwise, return data to props
  return {
    props: { data },
  };
}
```

If no `data`, then `redirect` the page to `no-data` route instead of this current page.

- `destination` - the url to redirect to.

- `permanent` - a boolean indicating whether the redirection is permanent `true` or temporary `false`. A permanent redirect will signal search engines to update their indexes with the new URL.

<br>

## "getStaticPaths" & Dynamic Parameters

For a dynamic page `pages/[id].js`, the default behavior is not to pre-generate the page with `getStaticProps`, and because `getStaticProps` implicitly tells Next.js to pre-render a page, then it will give us an error if we try. Dynamic routes are generated just-in-time on the server, and when generating static pages, next.js will not know in advance how many pages to generate.

To mix `getStaticProps` and dynamic routes we need to give Next.js additional information about our dynamic routes, for example which paths to pre-generate, so that eventually multiple pages can be pre-generated by Next.js by knowing the pages in advance. And we do this using the `getStaticPaths()` function.

### "getStaticPaths"

The goal of `getStaticPaths` is to tell Next.js which instances of a dynamic page `[id].js` should be generated, it helps to specify which paths or routes should be pre-rendered at build time when using Static Site Generation (SSG).

```js
export async function getStaticPaths() {
  return {
    path: [
      { params: { id: "1" } },
      { params: { id: "2" } },
      { params: { id: "3" } },
    ],
    fallback: false,
  };
}
```

The above function, tells Next.js that the dynamic routes that contains this function, should pre-generate 3 times with those 3 values.

Next.js will call `getStaticProps` 3 times for these different ids. Again, `getStaticPaths` is requried to tell Next.js which concrete instances of this dynamic page must be pre-generated.

<br>
<br>
<br>
<br>
<br>

<!-- <!--  -->

### "getStaticPaths" & Link Prefetching: Behind The Scenes

Pre-rendering of multiple instances of a dynamic page - after `build`.

It pre-renders: for our dynamic page `[id].js` it generates 3 instances `p1 p2 p3`, 3 html pages. Besies that by default it pre-generates the main `index.html` and `404.html` pages.

Besides the pre-generated html files we also have the json files for pre-loading that data if we would navigate to one of this pages directly through a link, not for the initial page load.

Pre-fetching data - in production, if we check the network devtools tab, we see that json files for the dynamic pre-generated page were loaded `p1.json`.. it pre-fetched the props for the dynamic page that would need to be loaded if you click on one of the links that direct to those pages. Again, it pre-fetched this data even before accessing the actual dynamic pages, so on the main page. And now, when we click on a link, we don't send a request to the server and load the pre-rendered html file, instead we stay in this single page app which was loaded and hydrated after the initla request. Instead, JS will render a new page for us just as it would do in a regular app without nextjs, but the data needed for this page is coming from the pre-fetched json file, which it loaded and read under the hood on our behalf, so it doesn't need to fetch the data after we navigated to that page, but so that the data is already there when we do navigate, to show the page faster.

### Fallback Pages

The `fallback` key can help if we have a lot of pages that would need to be pre-generated.

With `fallback: true` We tell next.js that even pages which are not listen in `getStaticPaths`, can be valid, but they are not pre-generated, instead they're pre-generated just in time, when a request reaches the server.

This allows us to pre-generate highlt visited pages by specify the `params` for them, and postpone the generation to less frequented pages to the server, so that they are only pre-generated when they're needed.

BUT, if instead of clicking on a link on our page, but instead directly accessing a url, and sending a new request, then we get an error. Why, because this dynamic pre-generation does not finish instantly.

So, when using this `fallback: true` feature, we should be prepared to return a fallback state in the react component:

```js
function Page(props) {
  const { data } = props;

  if (!data) return <p>Loading</p>;

  return <h1>{data.title}</h1>;
}
```

With this, the page will be rendered, by first rendering the loading paragraph, and then when the data is done loading next.js will give it to the component and the that component will render with that data.

`fallback: 'blocking'` with this we don't need the fallback check in the component, with this next.js will wait for the page to fully be pre-generated on the server before it serves that - this will take a little bit longer for the visitor of the page to get a respone.

## Loading Paths Dynamically

```js
function ProductDetailPage(props) {
  const { loadedProduct } = props;

  return (
    <Fragment>
      <h1>{loadedProduct.title}</h1>
      <p>{loadedProduct.description}</p>
    </Fragment>
  );
}

async function getData() {
  const filePath = path.join(process.cwd(), "data", "dummy-backend.json");
  const jsonData = await fs.readFile(filePath);
  const data = JSON.parse(jsonData);
  return data;
}

export async function getStaticProps(context) {
  const { params } = context;
  const productId = params.pid;

  const data = await getData();
  const product = data.products.find((product) => product.id === productId);

  return {
    props: {
      loadedProduct: product,
    },
  };
}

export async function getStaticPaths() {
  const data = await getData();

  const ids = data.products.map((product) => product.id);
  const pathsWithParams = ids.map((id) => ({ params: { pid: id } }));

  return {
    paths: pathsWithParams,
    fallback: false,
  };
}
```

## Fallback Pages & "Not Found" Pages

Trying to request a page that doesn't exist, but also: `fallback: true` if an ID value is not found, we still want to render a page. Use `notFound`:

```js
export async function getStaticProps(context) {
  const product = undefined;

  if (!product) {
    return { notFound: true };
  }

  return {
    props: {
      loadedProduct: product,
    },
  };
}
```

<br>
<br>
<br>
<br>
<br>

## "getServerSideProps" for Server-side Rendering (SSR)

Static Generation AND **Server-side Rendering**.

Inside getStaticProps and getStaticPaths we don't have access to the actual incoming request - instead, these are generally called when the project is `build`, except with incremental static generation.

Server-side rendering means that you do need to pre-render a page for every incoming request. So not at most every second, but really for every incoming request, and/or you need access to the concrete request object that is reaching the server, because for example you need to extract cookies.

Next.js supports: run "real server-side code" - next.js gives you a function which is executed whenever a request for this page reaches the server. So that's then no pre-generated in advance during build time or every couple of seconds but it's really code that runs on the server only, only after you deployed it, and which is then re-executed for every incoming request. This function needs to be exported and needs to be added to a page component file.

```js
export async function getServerSideProps() {}
```

NextJS will execute this function whenever a request for this page is made.

Therefore, you should ONLY use either `getStaticProps()` or `getServerSideProps`, because they kind of clash. They fulfill the same purpose, they get props for the component so that nextjs is then able to render that component, but they run at different points of time .

### getServerSideProps

```js
export async function getServerSideProps(context) {
  return {
    props: {
      username: "Max",
    },
    notFound: false,
    redirect: false,
  };
}
```

It accepts the same keys as `getStaticProps` except the `revalidate` key, because `getServerSideProps` per definition runs for every incoming request, so there's no need to revalidate after a certain amount of seconds because it will always run again.

The props of `getServerSideProps` will then be made available to the component function, but it will not be called in advanced whenw e `build` the project, but really for every incoming request.

Important: this `getServerSideProps` only executes on the server after deployment (also on our development server), but it's not statically pre-generated.

### context

The `context` object of `getServerSideProps` function - here we get access to request and response objects, and still to the `params` as well.

```js
export async function getServerSideProps(context) {
  const { params, req, res } = context;
}
```

These are default nodejs objects for incoming [messages](https://nodejs.org/api/http.html#http_class_http_serverresponse) and for [responses](https://nodejs.org/api/http.html#http_class_http_incomingmessage).

Getting access to this kind of data can be useful for example when we need a special header or cookie data.

`getServerSideProps()` is good if we want to ensure that this function runs for every incoming request, so that it's never statically pre-generated, for situations when we have highly dynamic data that can get outdated fast. And we can still write server-side code here or reach out to json files from the project directory, and it still returns props for the component function, the only key difference is the kind of data we get access to in the `context` object and the timing of this function.

### Dynamic Pages & "getServerSideProps"

When we use dynamic routes with `getStaticProps`, we need `getStaticPaths` to tell nextjs explicitly the routes. With `getServerSideProps` everything works well without `getStaticPaths`, why? Because `getServerSideProps` runs on the server anyways, so nextjs does not pre-generate any pages at all anyway.

```js
// [uid].js

function UserIdPage(props) {
  return <h1>{props.id}</h1>;
}

export default UserIdPage;

export async function getServerSideProps(context) {
  const { params } = context;

  const userId = params.uid;

  return {
    props: {
      id: "userid-" + userId,
    },
  };
}
```

### "getServerSideProps": Behind The Scenes

Note that details about the build we get in the terminal where we run the script.

Pages that use `getServerSideProps` are not pre-generated, fact denoted by the **lambda** λ symbol, instead will be pre-rendered on the server only.

Why we use these function - `getStaticProps getStaticPaths getServerSideProps` - Prepare data for our components ahread of time OR on the server so that we ca serve a finished page to the client, that can offer a better user experience to our users, since they get the finished page and all the content is there right from the start, but it also help us with search engines, with SEO, because search engines also see the finished page.

<br>

## Implementing Client-Side Data Fetching

**Client-Side Data Fetching with Next.js**

**Firebase** is a service offered by Google which gives you an entire backend with a lot of features, one of te features is a database with an attached API.

We will use `realtime` database.

_Standard react way ot sending a request:_

```js
import { useEffect, useState } from "react";

function LastSalesPage() {
  const [sales, setSales] = useState();
  const [isLoading, setIsLoading] = useState(false);

  useEffect(() => {
    setIsLoading(true);

    fetch("https://nextjs-tst-default-rtdb.firebaseio.com/sales.json")
      .then((res) => res.json())
      .then((data) => {
        const transformedSales = [];

        for (const key in data) {
          transformedSales.push({
            id: key,
            username: data[key].username,
            volume: data[key].volume,
          });
        }

        setSales(transformedSales);
        setIsLoading(false);
      });
  }, []);

  if (isLoading) {
    return <p>Loading...</p>;
  }

  if (!sales) {
    return <p>No data yet</p>;
  }

  return (
    <ul>
      {sales.map((sale) => (
        <li key={sale.id}>
          {sale.username} - ${sale.volume}
        </li>
      ))}
    </ul>
  );
}

export default LastSalesPage;
```

If we inspect the source we see the paragraph `No data yet`, why? when next.js pre-renders the page, it will not execute `useEffect`, it will just return and pre-render the very basic first version of waht the component returns, and that is the paragraph `No data yet`.

SO, we still have pre-rendering BUT without the data, because we're fetching the data on the client side now, with the above approach.

## useSWR

The above pattern is such a common pattern that we can use a custom hook for this job, `SWR` hook.

swr - stale-while-revalidate

https://swr.vercel.app/

This is a react hook developed by the next.js team, but we can use it in non-next.js projects as well.

This is a hook that under the hood, still will send a http request, by default using the fetch api. It gives us nice built-in features like caching and automatic revalidation, less code.

`npm install swr`

```js
import { useState } from "react";
import useSWR from "swr";

function Comp() {
  const [sales, setSales] = useState();

  const { data, errors } = useSWR(
    "https://nextjs-tst-49974-default-rtdb.firebaseio.com/sales.json"
  );

  useEffect(() => {
    if (data) {
      const transformedSales = [];

      for (const key in data) {
        transformedSales.push({
          id: key,
          username: data[key].username,
          volume: data[key].volume,
        });
      }
      setSales(transformedSales);
    }
  }, [data]);

  if (error) {
    return <p>Failed to load.</p>;
  }

  if (!data || !sales) {
    return <p>Loading...</p>;
  }

  return (
    <ul>
      <li>
        {sales.map((sale) => (
          <li key={sale.id}>
            {sale.username} - ${sale.volume}
          </li>
        ))}
      </li>
    </ul>
  );
}
```

We can use `useSWR` directly in a component. `useSWR` wants at least one argument, which is an identifier for the request it should send, typically the URL of that request. It is also called an identifier because this hook will bundle multiple requests to the same URL, which are sent in a certain timeframe into one request to avoid sending dozens of small requests.

The second argument is a fethcer function that describes how the reqeust should be sent. By default it will use the `fetch` api.

So, when the component is loaded, the url request will be made. The data returned by this hook will be an object which contains the fetched data and potential error information.

We then use `useEffect` hook to format the data we get from firebase.

This `useSWR` is convinient but not mandatory.

<hr/>

Note: You must now add a default "fetcher" when working with `useSWR`:

```js
useSWR(<request-url>, (url) => fetch(url).then(res => res.json()))
```

<br>

## Combining Pre-Fetching With Client-Side Fetching

Combine: Client-side data fetching + Server-side pre-rendering = good user experience.

Use case: pre-render a basic snapshot, then still fetch the latest data from the client.

```js
import { useEffect, useState } from "react";
import useSWR from "swr";

export default function LastSalesPage(props) {
  const [sales, setSales] = useState(props.sales);

  const { data, error } = useSWR(
    "https://nextjs-tst-49974-default-rtdb.firebaseio.com/sales.json",
    (url) => fetch(url).then((res) => res.json())
  );

  useEffect(() => {
    if (data) {
      const transformedSales = [];

      for (const key in data) {
        transformedSales.push({
          id: key,
          username: data[key].username,
          volume: data[key].volume,
        });
      }

      setSales(transformedSales);
    }
  }, [data]);

  if (error) {
    return <p>Error</p>;
  }

  if (!data && !sales) {
    return <p>Loading...</p>;
  }

  return (
    <ul>
      {sales.map((sale) => {
        return <li key={sale.id}>{sale.username + " - " + sale.volume}</li>;
      })}
    </ul>
  );
}

export async function getStaticProps() {
  const response = await fetch(
    "https://nextjs-tst-49974-default-rtdb.firebaseio.com/sales.json"
  );

  const data = await response.json();
  const transformedSales = [];

  for (const key in data) {
    transformedSales.push({
      id: key,
      username: data[key].username,
      volume: data[key].volume,
    });
  }

  return { props: { sales: transformedSales }, revalidate: 2 };
}
```

The data now is part of the pre-rendered page that we fetched inside `getStaticProps`, because we can see it when inspecting the source page,

If new data updates, the source page will not have it, because it wasn't at the time when the page prerendered, when `getsStaticProps` run, but in the browser it will update because then the code runs on the client side, and client-side data fetching kicks-in.

<br>
