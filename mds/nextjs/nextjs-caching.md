# Caching in Next.js

**What is Caching:** Caching involves storing fetched or computed data in a temporary location for future access. This prevents the need to re-fetch or re-compute data, improving performance and reducing costs.

**Caching in Next.js:** In Next.js, caching is utilized extensively to enhance application performance. It includes mechanisms for caching data, revalidating it, and managing cache life cycles.

**Default Behavior:** Caching is enabled by default in Next.js, which can sometimes lead to unexpected behaviors. Not all caches can be disabled, and multiple APIs in Next.js influence caching behavior.

## Caching Mechanisms in Next.js

### 1. Request Memoization

- **Location:** Server-side

- **Description:** Caches data from similar GET requests during a single page render cycle. If multiple components request the same data during one page render, only one network request is made, and the data is reused across the components.

- **Use Case:** Useful when the same data is needed in different parts of the component tree. It eliminates the need to fetch data at the top level and pass it down via props.

- **Limitations:** Works only with the native `fetch` function and identical requests (same URL and options). Also, this is a React feature, and so it is specific to the React component tree.

### 2. Data Cache

- **Location:** Server-side

- **Description:** Caches data fetched during a route's lifecycle or from a single fetch request. This data persists across multiple requests and even survives re-deployments unless explicitly revalidated.

- **Use Case:**

- **Revalidation:**

<br>
<br>
<br>
<br>
<br>

- Caches data that has been fetched in a specific route or from a single fetch request.
- The data stays there indefinitely, even across de-deploys, unless we decide to revalidate the cache (clear the cache and refetch the data).
- This means that the data is available across multiple requests from different users, and it even survives when the app is redeployed. All users requesting the same data, next.js would only have made one fetch request, so every user would get the exact same data.
- When this data is revalidated, the corresponding static page will simply be regenerated, and this is the whole idea behind ISR, this cache mechanism and then revalidating it that makes ISR possible.
- This is actually a great cache, it prevents so many network requests and boosts performance.
