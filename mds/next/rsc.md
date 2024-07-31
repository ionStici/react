# React Server Components (RSC)

## Table of Contents

- [Introduction](#introduction)
- [Comparing Server and Client Components](#comparing-server-and-client-components)
  - [Server Components](#server-components)
  - [Client Components](#client-components)
- [Traditional React vs React with RSC](#traditional-react-vs-react-with-rsc)
  - [Traditional React](#traditional-react)
  - [React with RSC](#react-with-rsc)
  - [Key Differences](#key-differences)

## Introduction

- **React Server Components (RSC):** architecture for building full-stack React applications, where the server components are rendered on the server and streamed to the client, optimizing performance by reducing client-side JavaScript and improving the speed of the initial page load.

- **Server Components:** These components are only rendered on the server, and since they run exclusively on the server, the client does not require any JS to render them.

  _With Server Components we essentially can build the application's backend also with React._

- **Client Components:** These are regular React components that run on the client, handling all the interactivity. To use a client component, you must specify the `'use client'` directive at the top of the module.

- **Integration:** RSC is not active by default and must be implemented by a framework like Next.js or Remix, as it requires server integration. React.js just provides the RSC architecture, which these frameworks then implement.

- **Default in Next.js:** In a Next.js App Router project, components in the `app` folder use this new RSC architecture, making server component the default.

- **Why RSC:** The React Server Components (RSC) architecture combines Server-Side Rendering with Client-Side Interactivity, and by leveraging the strengths of both, it enables developers to build highly performant, SEO-friendly, and maintainable applications.

  _Unified Development:_ by combining Server and Client Components, you can write frontend code alongside backend code, resulting in full-stack applications using only React components.

  _Reduced Client Bundle:_ Server components do not ship any JavaScript to the client, allowing for the import of large libraries without impacting client performance.

## Comparing Server and Client Components

### Server Components

- **State / Hooks:** Not Applicable.
- **Props:** Server components can pass any props to other server components and only serializable props to client components.
- **Data Fetching:** Server components are ideal for data fetching since they run on the server.
- **Import / Render:** Server components can import and render both server and client components.
- **When Re-render:** Re-renders occur based on URL changes or navigation events.

### Client Components

- **State / Hooks:** Applicable.
- **Props:** Client components can pass props to other client components, but not to server components.
- **Data Fetching:** handled on the client, preferably using a library.
- **Import:** Client components can only import other client components.
- **Render:** Client components can render both client and server components, but server components must be passed as props.
- **When Re-render:** Re-renders occur when there are changes to state or props.

## Traditional React vs React with RSC

### Traditional React

- Components directly fetch data on the client side.
- The fetched data is used to render the view.
- User interactions update the component state.
- Components re-render when their state updates.

### React with RSC

- **Server Components:** handle data fetching on the server.
- **Client Components:** manage user interactions and display the view.

_Data Flow of RSC:_

1. Data is fetched and processed by server components on the server.
2. Server components pass that data to client components as props.
3. Client components use these props to render the view.
4. User interactions update the state within client components.
5. State updates in client components trigger their re-rendering.
6. URL changes cause server components to fetch new data and update the client components.

### Key Differences

- **Separation of Concerns:** In the RSC model, server components handle data fetching, while client components focus on interactivity.

- **Efficiency:** The RSC model improves performance by minimizing the JavaScript needed on the client, as server components manage data fetching.

- **Re-rendering Triggers:** In the traditional model, re-renders are primarily triggered by state changes in client components. In the RSC model, re-renders can also be triggered by URL changes, causing server components to fetch updated data.
