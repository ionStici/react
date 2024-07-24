# React Server Components (RSC)

- **React Server Components (RSC):** full-stack architecture for building React applications, where the server becomes an integral part of the React component tree. This approach allows React components to extend to the server.

- **Server Components:** These components are only rendered on the server and are typically responsible for fetching data. Since they run solely on the server, the app requires zero JavaScript to be downloaded for them.

  _With Server components we essentially can build the application's backend also with React._

- **Client Components:** These are regular React components that run on the client, handling all the interactivity. To use a client component, you must specify the `'use client'` directive at the top of the module.

- **Integration:** RSC is not active by default and must be implemented by a framework like Next.js or Remix, as it requires server integration. React.js just provides the RSC architecture, which these frameworks then implement.

- **Default in Next.js:** In a Next.js App Router project, components in the `app` folder use this new RSC architecture, making server component the default.

- **Why RSC:** The React Server Components (RSC) architecture combines Server-Side Rendering with Client-Side Interactivity, and by leveraging the strengths of both, it enables developers to build highly performant, SEO-friendly, and maintainable applications.

  _Unified Development:_ by combining Server and Client Components, you can write frontend code alongside backend code, resulting in full-stack applications using only React components.

  _Reduced Client Bundle:_ Server components do not ship any JavaScript to the client, allowing for the import of large libraries without impacting client performance.

## Differences between Client and Server Components

### Server Components

- **State / Hooks / Lifting State Up:** Not supported.
- **Props:** Server components can pass props to client components, but these props must be serializable (no functions or classes).
- **Data Fetching:** Ideal for data fetching since they run on the server.
- **Import / Render:** Server components can import and render both server and client components.
- **When Re-render:** Re-renders occur based on URL changes or navigation events.

### Client Components

- **State / Hooks / Lifting State Up:** Supported.
- **Data Fetching:** Handled on the client, ideally using a library.
- **Import:** Client components can only import other client components.
- **Render:** Client components can render both client and server components, but server components must be passed as props.
- **When Re-render:** Re-renders occur when there are changes to state or props.

## Traditional React vs React with RSC

### Traditional React

1. **Fetch Data:** Components directly fetch data on the client side.
2. **Display:** The fetched data is used to render the view.
3. **Interaction:** User interactions update the component state.
4. **Re-render:** Components re-render when their state updates.

### React with RSC

- **Server Components** handle data fetching on the server.
- **Client Components** manage user interactions and display the view.

_Data Flow of RSC:_

1. **Fetch Data:** Data is fetched and processed by server components on the server.
2. **Props:** The server components pass the data as props to client components.
3. **Display:** Client components use these props to render the view.
4. **Interaction:** User interactions update the state within client components.
5. **Re-render:** State changes in client components trigger their re-rendering.
6. **Navigation:** URL changes cause server components to fetch new data and update the client components.

### Key Differences

- **Separation of Concerns:** In the RSC model, server components handle data fetching, while client components focus on interactivity.

- **Efficiency:** The RSC model improves performance by minimizing the JavaScript needed on the client, as server components manage data fetching.

- **Re-rendering Triggers:** In the traditional model, re-renders are primarily triggered by state changes in client components. In the RSC model, re-renders can also be triggered by URL changes, causing server components to fetch updated data.
