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
- [The useQuery hook](#the-usequery-hook)
- [The useMutation Hook](#the-usemutation-hook)
- [The useQueryClient Hook](#the-usequeryclient-hook)
- [The defaultOptions Object](#the-defaultoptions-object)
- [Prefetching](#prefetching)
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
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

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

## The useQuery hook

The `useQuery` hook is used to fetch and manage data.

```jsx
import { useQuery } from "@tanstack/react-query";

async function getData() {
  const { data } = await axios.get("api/data");
  return data;
}

function MyComponent() {
  const { data, error, isPending } = useQuery({
    queryKey: ["data"],
    queryFn: getData,
  });

  if (isPending) return <Spinner />;
  if (error) return <Error error={error} />;

  return <List data={data} />;
}
```

- `queryKey` - unique identifier for your query.
- `queryFn` - function that fetches the data.

## The useMutation Hook

Mutations are used to create, update, or delete data. The `useMutation` hook is used for this purpose.

`useMutation` returns a `mutate` function that calls the function we provide at `mutationFn`.

The `onSuccess` and `onError` functions will run accordingly based on the response.

```jsx
import { useMutation, useQueryClient } from "@tanstack/react-query";
import { updateItem as updateApi } from "./apis/updateItem";

function useUpdateItem() {
  const queryClient = useQueryClient();

  const { mutate: updateItem, isPending } = useMutation({
    mutationFn: (id) => updateApi(id),
    onSuccess: (data) => queryClient.invalidateQueries({ queryKey: ["data"] }),
    onError: (err) => console.log(err),
  });

  return { updateItem, isPending };
}

function Item() {
  const { updateItem, isPending } = useUpdateItem();

  return (
    <button onClick={() => updateItem(23)} disabled={isPending}>
      Update Item
    </button>
  );
}
```

## The useQueryClient Hook

The `useQueryClient` hook provides access to the `QueryClient` instance.

```jsx
import { useQueryClient } from "@tanstack/react-query";

function Component() {
  const queryClient = useQueryClient();
}
```

```jsx
// 1. Invalidate queries to refetch them the next time they are used.
queryClient.invalidateQueries("data");

// 2. Manually refetch queries.
queryClient.refetchQueries("data");

// 3. Get the current cached data for a query.
queryClient.getQueryData("data");

// 4. Manually set the cached data for a query.
queryClient.setQueryData("data", newData);

// 5. Remove queries from the cache.
queryClient.removeQueries("data");

// 6. Reset the state of queries to their initial state.
queryClient.resetQueries("data");
```

## The defaultOptions Object

The `defaultOptions` object is used to set global defaults for all queries or mutations within your application. It has two main properties: `queries` and `mutations`.

```jsx
const options = { defaultOptions: { queries: {}, mutations: {} } };
```

### `queries`

- `staleTime` : The amount of time in milliseconds that a query's data is considered fresh. Setting `staleTime` to `0` means that the data is immediately considered stale and will be refetched on every mount.

- `cacheTime` : The amount of time in milliseconds that unused or inactive query data remains in the cache. The default is 5 minutes. After this time, the data is garbage collected.

- `refetchOnWindowFocus` : If `true`, the query will refetch on window focus. The default is `true`.

- `refetchOnReconnect` : If `true`, the query will refetch on reconnect. The default is `true`.

- `refetchOnMount` : If `true`, the query will refetch on component mount. The default is `true`.

- `retry` : The number of times to retry a failed query before throwing an error. The default is `3`.

- `retryDelay` : The delay between retry attempts in milliseconds. The default is an exponential backoff function.

- `enabled` : If `false`, the query will not automatically run. This can be useful for dependent queries that should only run under certain conditions.

- `suspense` : If `true`, the query will be used with React's Suspense. The default is `false`.

- `initialData` : Initial data to use while the query is being fetched. This can be useful for server-side rendering or providing an initial cache state.

### `mutations`

- `retry` : The number of times to retry a failed mutation before throwing an error. The default is 0 (no retries).

- `retryDelay` : The delay between retry attempts in milliseconds. This can be a static value or a function that receives the retry attempt index and returns the delay.

- `onMutate` : A function that fires before the mutation function is fired and is passed the same variables the mutation function would receive. Useful for optimistic updates.

- `onError` : A function that fires if the mutation encounters an error. It receives the error, the variables that were passed to the mutation, and the mutation context.

- `onSuccess` : A function that fires if the mutation is successful. It receives the data returned from the mutation, the variables that were passed to the mutation, and the mutation context.

- `onSettled` : A function that fires when the mutation is either successfully fetched or encounters an error. It receives the data or error, the variables that were passed to the mutation, and the mutation context.

### Use options directly within useQuery and useMutation

```jsx
useQuery({
  queryKey: ["data"],
  queryFn: getData,
  initialData: "Important Data",
  retry: 2,
  retryDelay: 2000,
});
```

```jsx
useMutation({
  mutationFn: (id) => updateApi(id),
  onSuccess: (data) => console.log("Success!")
  onError: (err) => console.log(err.message),
  onSettled: () => console.log("Settled"),
});
```

## Prefetching

**Prefetching** in React Query is a technique used to load data into the query cache before it is needed, improving the user experience by reducing perceived load times. This is especially useful for data that you know will be needed soon, such as when navigating between pages or displaying related data.

```jsx
queryClient.prefetchQuery("data", getData);
```

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
