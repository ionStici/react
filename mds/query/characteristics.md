# RQ Characteristics

React Query offers a range of features that simplify the process of data fetching and state management in React applications

## Data Storage in Cache

**React Query caches the data it fetches**, which means that subsequent requests for the same data can be served from the cache instead of making a network request. This reduces the load on your server and speeds up the application for the user.

The cached data is automatically managed by React Query, and it can be configured to expire after a certain period or based on certain conditions.

## Automatic Loading and Error States

React Query simplifies the handling of loading and error states in your application. When you use the `useQuery` hook, it provides `isPending` and `error` states out of the box.

This means you can easily display loading spinners or error messages without having to write custom logic for each data fetch.

## Automatic Re-fetching to Keep State Synched

React Query ensures that your data stays up-to-date by automatically re-fetching data under certain conditions, such as: when the component mounts, when the window is refocused, when the network is reconnected.

This automatic re-fetching keeps your UI in sync with the latest data from the server without requiring manual intervention.

## Pre-fetching

Pre-fetching allows you to fetch data before it is needed, which can improve the user experience by reducing loading times (fetch data in the background and store it in the cache).

This is particularly useful for scenarios where you know the user will navigate to a certain part of the application and you want to have the data ready beforehand.

## Easy Remote State Updating

React Query makes it simple to update remote state using mutations. It provides useful hooks that allow you to perform create, update, or delete operations on the server and automatically update the local cache based on the result.

This ensures that your local state remains consistent with the remote state, reducing the complexity of managing server interactions.

## Offline Support

React Query provides built-in support for offline scenarios. When the application detects that it is offline, it will not attempt to fetch data from the server. Instead, it can use the cached data to continue functioning. Once the connection is restored, it can automatically re-fetch the latest data. This ensures a smooth user experience even in environments with intermittent connectivity.
