# Next.js

## Table of Content

- [What is Next.js](#what-is-nextjs)
- [1. Feature: Server-side Page (Pre-)Rendering](#1-feature-server-side-page-pre-rendering)
  - [SSR vs SSG](#ssr-vs-ssg)
- [2. Feature: File-based Routing](#2-feature-file-based-routing)
- [3. Feature: Build Fullstack React Apps](#3-feature-build-fullstack-react-apps)
- [Creating a Next.js Project](#creating-a-nextjs-project)
- [Serverless and Serverless Functions](#serverless-and-serverless-functions)

<br>

## What is Next.js

**React** is a declarative JS library (it provides helpful functions) for building interactive **user interfaces**.

**Next.js** is a (_fullstack_) React **framework** that gives you building blocks to create web applications.

**Next.js** enhances **React** by adding additional core features.

<br>

## 1. Feature: Server-side Page (Pre-)Rendering

**Server-Side Rendering** - The page requested is rendered and filled on the server.

<br>

**Client-side Rendering** - If we take a look at the **Page Source** of a loaded React app, we see that all it contains is an empty `div` with a `root` id.

Within this `div`, React will render the entire application on the client-side.

Key Idea: the HTML document sent to the user from the server is initially empty.

Potential Problems of client-side rendering: not fast enough UX, lack of SEO.

<br>

**Server-side Rendering** - The page is pre-rendered on the server, then the complete page is served to the user and to search engines (faster & better SEO).

Next.js has built-in server-side rendering by default, it automatically pre-renders the pages, which is great for SEO and initial load experiences.

Even if Next.js pre-renders on the server, we still get a React application running in the browser.

<br>

### SSR vs SSG

- **Server-side Rendering (SSR)**

  - SSR is a rendering technique where the HTML of a page is generated on the server for each request.
  - Beneficial for SEO as Search Engine Crawlers can easily index the server-rendered content.
  - Faster **First Contentful Paint (FCP)** compared to **Client-Side Rendering (CSR)**.
  - SSR can be resource-intensive on the server.

- **Static Site Generation (SSG)**

  - SSG pre-generates "static" HTML pages once at build time, then these pages are used for every request, with no further processing on the server.
  - SSG is highly performant, it reduces the load on the server. The pre-generated pages can be served directly from a Content Delivery Network (CDN).
  - Beneficial for SEO as the pages are already rendered and can be easily crawled by search engines.

- **Differences between SSR and SSG**

  - _Rendering Time:_ SSR renders pages at runtime (when a user requests the page), while SSG renders pages at build time.
  - _Performance:_ SSG has better performance (the pages are pre-generated & served via CDN). SSR may have higher server load and latency as the server needs to render pages on-the-fly for each request.
  - _SEO:_ Both SSR and SSG are good for SEO as they generate fully-rendered pages that are easily indexable by search engines.
  - _Use Cases:_ SSR is used for dynamic apps where the content changes frequently. SSG is more suited for static or less dynamic content.

In Next.js we can use both SSR & SSG rendering methods within the same project, allowing for a highly optimized and flexible architecture.

**Latency:** time it takes for a request to go from the client to the server and back to the client.

<br>

## 2. Feature: File-based Routing

**File-based Routing** is a routing paradigm where the structure of your URLs is determined by the file structure of your `pages` directory.

A **Router** watches the URL, and if it changes, the router will prevent the browser's default behavior of sending a request, instead it will render a different react component on the page, giving the illusion that the page has changed.

With file-based routing we change what is visible on the screen based on the url, but without sending an extra request to a server.

Next.js provides a special `pages` folder where we can define the routes and paths using files and folders.

<br>

## 3. Feature: Build Fullstack React Apps

Next.js has backend capabilities for adding standalone backend code. We can easily add backend APIs to a React project using Node.js code.

We don't need to build a standalone Rest API project, instead we can work on a single Next.js & React project, then add and combine the backend API code.

### Next.js backend features:

- Server-Side Rendering (SSR)

- **API Routes:** build a backend along with the frontend in a single project. Inside the `pages/api` directory, we can create RESTful or GraphQL API Endpoints.

- **Serverless Functions:** With API routes, Next.js provides serverless functions, which are functions that run on-demand, scaling automatically as needed.

- Next.js supports various Authentication strategies. There are many libraries available for setting up authentication in a Next.js application.

<br>

## Creating a Next.js Project

```
npx create-next-app@latest
```

Because certain features of "App Router" is still in alpha, opt for "Pages Router" instead.

### Analyzing a Next.js App Directory

- `pages` here we create files for each application page (file-based routing).

- `public` directory for public resources.

- `styles` for CSS.

- `_app.js` is the root component, where all page components are rendered in.

In Next.js we don't get an HTML document, and that's because next.js has built-in pre-rendering, it gives you that single-page-application dynamically pre-rendered when the request reaches the server.

What we see on the page after live hosting, is the result of the `index.js` file from `pages` directory. The component from `pages/index.js` is pre-rendered. In the Page Source we can see that the document is filled with content.

The `public/` folder serves the resources it contains statically, as part of the overall application, this means that we can reference whatever is in this folder from our code.

And because Next.js will server images statically if we place them in the `public/` folder, we can reference them from JSX like `<img src="/img.jpg" />`, so without the need of importing the image.

<br>

## Serverless and Serverless Functions

The term "Serverless" does not imply that there are no servers involved, but rather that server management and capacity planning are abstracted away from the developer. In a serverless architecture, developers can build and deploy code without worrying about the underlying infrastructure.

Serverless architectures are often **event-driven**. Functions are triggered by events like HTTP requests, database operations, or queue messages. Each event triggers a serverless function which processes the event and can interact with other systems.

**Serverless functions** are the building blocks of a serverless architecture. They are small, single-purpose functions that are executed in response to events.
