# React Query

[React Query](https://tanstack.com/query/latest) is a library for fetching, caching, and synchronizing server data in React applications.

```
npm i @tanstack/react-query
```

```
npm i @tanstack/react-query-devtools
```

## Table of Contents

- [Why React Query](#why-react-query)
- [Setting Up React Query](#setting-up-react-query)
- [useQuery](#usequery)
- [Mutations](#mutations)
- [React Query Features](#react-query-features)

## Why React Query

1. **Simplified Data Fetching:** React Query abstracts the complexity of data fetching, reducing boilerplate code.
2. **Caching:** It provides built-in caching, which can significantly improve performance by avoiding unnecessary network requests.
3. **Synchronization:** React Query ensures that your data is always up-to-date with minimal effort.
4. **Server State Management:** It helps manage server state efficiently, keeping it in sync with the client state.
5. **Error Handling:** It provides robust error handling and retries for failed requests.

## Setting Up React Query

Create a `QueryClient` instance and wrap your component tree with the `QueryClientProvider`.

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient({
  defaultOptions: { queries: { staleTime: 0 } },
});

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <MyComponent />
    </QueryClientProvider>
  );
}
```

`QueryClient` accepts an object with options. The `{ staleTime: 0 }` option defines how long the fetched data is considered fresh.

## useQuery

The `useQuery` hook is used to fetch and manage data.

```jsx
import { useQuery } from '@tanstack/react-query';

export async function getData() {
  const res = await fetch();
  const data = await res.json();
  return data;
}

function MyComponent() {
  const { data, error, isPending } = useQuery({
    queryKey: ['data'],
    queryFn: getData,
  });

  if (isPending) return <Spinner />;
  if (error) return <Error />;
  return <List data={data} />;
}
```

- `queryKey` - unique identifier for your query.
- `queryFn` - function that fetches the data.

## Mutations

Mutations are used to create, update, or delete data. The `useMutation` hook is used for this purpose.

`useMutation` returns a `mutate` function that calls the function we provided at `mutationFn`.

The `onSuccess` and `onError` functions will run accordingly based on the response.

```jsx
import { useMutation, useQueryClient } from '@tanstack/react-query';
import { updateItem } from './services/updateItem';

function UpdateItem({ id }) {
  const queryClient = useQueryClient();

  function handleInvalidateQueries() {
    queryClient.invalidateQueries({ queryKey: ['data'] });
  }

  const { mutate, isPending } = useMutation({
    mutationFn: (id) => updateItem(id),
    onSuccess: () => handleInvalidateQueries(),
    onError: (err) => console.log(err),
  });

  function handleUpdateItem() {
    mutate(id);
  }

  return (
    <button onClick={handleUpdateItem} disabled={isPending}>
      Delete
    </button>
  );
}
```

The `useQueryClient` hook provides access to the `QueryClient` instance.

**Query Invalidation** allows you to refresh queries to get the latest data. This is useful when you perform mutations that affect the data fetched by queries.

We can perform query invalidation using the `invalidateQueries` function, it accepts the `queryKey` identifier, so it will only invalidate the required queries.

## Prefetching

**Prefetching** - fetching data that might become necessary before we actually need that data to render it on the user interface.

?

## React Query Features

React Query offers a range of features that simplify the process of data fetching and state management in React applications.

### Data Storage in Cache

**React Query caches the data it fetches**, which means that subsequent requests for the same data can be served from the cache instead of making a network request. This reduces the load on your server and speeds up the application for the user.

The cached data is automatically managed by React Query, and it can be configured to expire after a certain period or based on certain conditions.

### Automatic Loading and Error States

React Query simplifies the handling of loading and error states in your application. When you use the `useQuery` hook, it provides `isPending` and `error` states out of the box.

This means you can easily display loading spinners or error messages without having to write custom logic for each data fetch.

### Automatic Re-fetching to Keep State Synched

React Query ensures that your data stays up-to-date by automatically re-fetching data under certain conditions, such as: when the component mounts, when the window is refocused, when the network is reconnected.

This automatic re-fetching keeps your UI in sync with the latest data from the server without requiring manual intervention.

### Pre-fetching

Pre-fetching allows you to fetch data before it is needed, which can improve the user experience by reducing loading times (fetch data in the background and store it in the cache).

This is particularly useful for scenarios where you know the user will navigate to a certain part of the application and you want to have the data ready beforehand.

### Easy Remote State Updating

React Query makes it simple to update remote state using mutations. It provides useful hooks that allow you to perform create, update, or delete operations on the server and automatically update the local cache based on the result.

This ensures that your local state remains consistent with the remote state, reducing the complexity of managing server interactions.

### Offline Support

React Query provides built-in support for offline scenarios. When the application detects that it is offline, it will not attempt to fetch data from the server. Instead, it can use the cached data to continue functioning. Once the connection is restored, it can automatically re-fetch the latest data. This ensures a smooth user experience even in environments with intermittent connectivity.
