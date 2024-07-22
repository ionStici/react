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

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Differences between Client and Server Components

### Server Components

- Server component cannot use React state or hooks, and no lifting state up available.

- **Props:** Server components can pass props to client components (props must be serializable, so no functions or classes).

- **Data fetching:** Server components are ideal for data fetching because they run on the server.

<br>
<br>
<br>

Can import:
Client and server components: Server components can import both client and server components, providing flexibility in component structure.

Can render:
Client and server components: Server components can render both other server components and client components.

When re-render?:
On URL change (navigation): Server components re-render based on URL changes or navigation events, ensuring fresh data is fetched and rendered appropriately.

### Client Components

- Client components can only import other client components, they cannot import server components.

- Client components can render both other client components and server components. Server component must be passed as props.

<br>

- **State/hooks & Lifting state up & Props & Data fetching:** Available.

- **Can Import:** Client components can only import other client components, they cannot import server components.

- **Can Render:** Client components can render both other client components and server components. Server components must be passed as props.

- **When Re-render:** Client components re-render when there is a change in their state or props.

### Server Components

<br>
<br>
<br>

## Traditional React vs React with RSC

### Traditional React

1. **Fetch Data:** Components fetch data directly.
2. **Display:** The fetched data is used to display the view.
3. **Interaction:** User interactions update the state.
4. **Re-render:** State updates re-render the component.

### React with RSC

- **Server Components** are responsible for fetching data on the server.
- **Client Components** handle user interactions and display the view.

#### Data Flow of RSC

1. **Fetch Data:** Server components fetch data and process it on the server.
2. **Props:** Data is passed as props from server components to client components.
3. **Display:** Client components use the props to render the view.
4. **Interaction:** User interactions update the state within the client components.
5. **Re-render:** State updates within client components trigger a re-render of those components.
6. **Navigation:** URL changes cause the server components to re-fetch data and update the client components.

### Key Differences

- **Separation of Concerns:** In the RSC model, data fetching is handled by server components, while client components focus on interactivity.

- **Efficiency:** The RSC model optimizes performance by reducing the amount of JavaScript needed on the client, as server components handle data fetching.

- **Re-rendering Triggers:** In the traditional model, re-renders are triggered by state changes in client components. In the RSC model, re-renders can also be triggered by URL changes, causing server components to fetch new data.
