# What are Server Actions?

**React + Next.js:** Enabling interactive full-stack applications.

Server Actions are part of the RSC Architecture. While Server Components handle data fetching, Server Actions manage data mutations. This combination allows for building interactive full-stack applications entirely in React.

## Server Actions Overview

Server Actions are asynchronous functions that run exclusively on the server, enabling data mutations. Here's how to create and use them:

### 1. Standalone file (Recommended):

- Use the `'use server'` directive at the top of the file.
- The function can be imported into any component.

### 2. Async Function in a Server Component

- Use the `'use server'` directive at the top of the function body.
- The function can be used within the server component of passed to a client component (unlike functions).

## Key Points

- **`'use server'` directive:** This directive is only for server actions, not for server components.

- **API Endpoints:** Next.js creates an API endpoint for each server action function. When a server action is called, a POST request is made to its URL. The function itself never reaches the client.

- **No Manual API Creates:** There is no need to manually create APIs or route handlers for data mutations.

- **Requires Running Web Server:** Unlike server components, which can run at build time, server actions requires a running web server.

## Calling Server Actions

Server Actions can be invoked from:

1. The `action` attribute in a `<form>` element (in both server and client components)

2. Event handlers and `useEffect` (only in client components)

## Using Server Actions

Server Actions are code running on the back-end, so we need to assume that inputs are unsafe. Server Actions can:

1. Perform data mutations (create, update, delete)

2. Update the UI with new data by revalidating the cache using `revalidatePath` and `revalidateTag`

3. Work with cookies and perform other backend operations
