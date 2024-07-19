# React Server Components

**React Server Components (RSC):** full-stack architecture for building React applications, where the server becomes an integral part of the React component tree. This approach allows React components to extend to the server.

- **Integration:** RSC is not active by default and must be implemented by a framework list Next.js or Remix, as it requires server integration. React.js just provides the RSC architecture, which these frameworks then implement.

- **Default in Next.js:** In a Next.js App Router project, components in the `app` folder use this new RSC architecture, making server components the default.

- **Server Components:** These components are only rendered on the server and are typically responsive for fetching data. Since they run solely on the server, the app requires zero JavaScript to be downloaded for them.

  With Server components we essentially can build the application's backend also with React.

- **Client Components:** These are regular React components that run on the client, handling all the interactivity. To use a client component, you must specify the `'use client'` directive at the top of the module.

  By combining Server and Client Components, you can write frontend code alongside backend code, allowing for a seamless development experience.

<br>
<br>
<br>
<br>
<br>

## Why RSC

React Server Components (RSC) offer several advantages for building modern web applications by combining the strengths of server-side rendering and client-side interactivity.

- **Improved Performance:** Server Components are rendered on the server, reducing the amount of JavaScript sent to the client and resulting in faster initial load times.

- **Seamless Data Fetching:** Fetching data on the server eliminates the need for additional data-fetching libraries on the client, improving performance.

- **Optimized SEO:** Server-rendered components enhance SEO by ensuring that content is available to search engines without requiring JavaScript execution.

## Differences Between Client and Server Components

### Client Components

<br>
<br>
<br>

## Pros and Cons of the RSC Architecture

### Pros

- We can compose entire full-stack apps with React components alone (+ server actions)
- One single codebase for front and back-end
- Server components have more direct and secure access to the data source (no API, no exposing API keys, etc.)
- Eliminate client-server waterfalls by fetching all the data needed for a page at once before sending it to the client (not each component)
- "Disappearing code": server components ship no JS, so they can import huge libraries "for free"

### Cons

- Makes React more complex
- More things to learn and understand
- Things like Context API don't work in server components
- More decisions to make: "Should this be a client or a server component?", "Should I fetch this data on the server or the client". etc/
- Sometimes you still need to build an API (for example if you also have a mobile app)
- Can only be used within a framework
