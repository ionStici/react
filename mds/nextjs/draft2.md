# draft

# The Server-Client Boundary: Frontend vs Backend

Next.js with RSC + Server Actions

In Next.js, we can think of server components as the backend and then client component as the frontend. This RSC approach allows us to build true full-stack applications in just one codebase. This can result in not needed an intermediary API any more.

Mutations in a Next.js app within the RSC model: Server actions can be used to mutate data on the server directly from client components. Using server actions replaces the POST, PUT requests that we would typically make to the backend API.

## Importing vs. Rendering

A client component renders a server component: we can render a server component inside a client component if we pass the server component as a prop (`children` or other). This works because at the point in which the server component instance is passed as a prop, it has already been imported and executed on the server.

**Dependency Tree:** how modules are imported by other modules. The server modules are imported at the top so that they could be passed as a prop. In the RSC model, it's this dependency tree where the client-server boundaries are established, and not in the component tree. And this means that client components cannot import server components, only other client components. Client components can however render server components as long as these have been passed as props. Again, when a client component renders a server component through a prop, this is possible because both components already have been imported at the top.

- Client components can import only client components. Client components can render other client components, and also server component that have been passed as props.

- Server component can import and also render both client and server components.

Note: If a client components imports and renders a server component (no `'use client'` directive) then that server component will create an instance as a client component. Otherwise, an instance of a server component is created instead.

We should always, whenever possible, move client components as low into the component tree as possible, because all child components of client components will be client components by default and so they will then not be server rendered, even if they could be rendered on the server perfectly fine.
