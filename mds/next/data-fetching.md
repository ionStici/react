# Data Fetching

## Fetching Data on the Server

In the `app` folder, by default components are rendered on the server, therefore data fetching occurs on the server.

```jsx
export default async function Page() {
  const response = await fetch("api");
  const data = await response.json();

  return null;
}
```

The fetched data is cached, meaning that subsequent requests for the same data will retrieve it from the cache rather than making a new fetch request.

**Note:** Try to pass as little data as possible to client components.

## Data Fetching Strategies

Instead of fetching data from multiple APIs in the same route component, we should delegate the fetching job to inner components.

```js
// page.js
export default async function Page() {
  const data = await fetch("main/api");

  return (
    <main>
      <RenderData data={data} />

      <Suspense fallback={<Spinner />}>
        <InnerData data={data} />
      </Suspense>
    </main>
  );
}
```

```js
// InnerData.js
import getLeftData from "apis";
import getRightData from "apis";

import { Left, Right } from "components";

export default async function InnerData({ data }) {
  const [left, right] = Promise.all([
    getLeftData(data.id),
    getRightData(data.id),
  ]);

  return (
    <div>
      <Left left={left} />
      <Right right={right} />
    </div>
  );
}
```

```js
// components.js

"use client";

export function Left({ left }) {
  return;
}

export function Right({ right }) {
  return;
}
```
