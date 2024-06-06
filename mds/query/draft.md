# React Query

[React Query](https://tanstack.com/query/latest) is a library for fetching, caching, and synchronizing server data in React applications.

```
npm i @tanstack/react-query
```

```
npm i @tanstack/react-query-devtools
```

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
import { deleteItem } from './services/deleteItem';

function DeleteItem({ id }) {
  const queryClient = useQueryClient();

  function handleInvalidateQueries() {
    queryClient.invalidateQueries({ queryKey: ['data'] });
  }

  const { mutate, isPending } = useMutation({
    mutationFn: id => deleteItem(id),
    onSuccess: () => handleInvalidateQueries(),
    onError: err => console.log(err),
  });

  function handleDeleteItem() {
    mutate(id);
  }

  return (
    <button onClick={handleDeleteItem} disabled={isPending}>
      Delete
    </button>
  );
}
```

The `useQueryClient` hook provides access to the `QueryClient` instance.

**Query Invalidation** allows you to refresh queries to get the latest data. This is useful when you perform mutations that affect the data fetched by queries.

We can perform query invalidation using the `invalidateQueries` function, it accepts the `queryKey` identifier, so it will only invalidate the required queries.
