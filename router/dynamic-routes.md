# Dynamic Routes

## Table of Content

- [Dynamic Routes and URL Parameters](#url-parameters)
- [The useParams Hook](#the-useparams-hook)
- [useSearchParams and Query Parameters](#usesearchparams-and-query-parameters)
- [Storing State in the URL](#storing-state-in-the-url)

<br>

## Dynamic Routes and URL Parameters

**Dynamic Routes** allow to handle different URL segments with just one route configuration.

**URL Parameters:** are the dynamic parts of a URL that change based on the context.

```jsx
<Route path="posts/:id" element={<Post />} />
```

- `:id` is a URL parameter, it's a variable part of the URL that can take different values, allowing the same route to match multiple different paths.
- A single route can have multiple parameters `posts/:post/:id`

<br>

## The useParams Hook

```jsx
import { useParams } from "react-router-dom";

function Post() {
  const { id } = useParams();
}
```

We can access the **URL parameters** from a **dynamic route** using the **`useParams` hook**.

`useParams` extracts the `:id` parameter from the URL, for example given the `posts/537` URL, `useParams` will return `{ id: '537' }`.

<br>

## useSearchParams and Query Parameters

**Query Parameters** are appended to the end of the URL with a `?` and usually follow the format `key=value`. They are often used for optional filters, search terms, storing state.

For example in this URL `url.com/posts?search=react&seaction=props`, `search=react` and `seaction=props` are query parameters.

**`useSearchParams`** is a hook that provides a way to read and manipulate the query parameters in the URL.

```jsx
import { useSearchParams } from "react-router-dom";

function Post() {
  const [searchParams, setSearchParams] = useSearchParams();

  // Get a specific query parameter
  const query = searchParams.get("search");
  const section = searchParams.get("section");

  // Function to update the query parameter
  const update = (newQuery, section) => {
    setSearchQuery({ search: newQuery, section });
  };
}
```

In this example, `useSearchParams` is used to read and update the `search` and `section` query parameters, all without reloading the page.

`useSearchParams` is similar to `useState`, it returns a stateful value and a function to update them.

It's helpful for features like search filters, pagination, etc., where the application state is reflected and controlled via URL query parameters.

<br>

## Storing State in the URL

The URL is an excellent place to store UI state and an alternative to `useState` in some situations. Examples: open/closed panels, currently selected list item, list sorting order, applied list filters, pagination, and much more.

1. Easy way to store state in a **global place**, accessible to **all components** in the app
2. Good way to **"pass" data** from one page into the next page
3. Makes it possible to **bookmark and share** the page with the exact UI state it had at the time
