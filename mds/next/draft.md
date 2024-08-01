# draft

# Sharing State Between Client and Server: The URL

## searchParams in Server Components

Server components get access to `searchParams` through props.

```js
// page.js
export default function Page({ searchParams }) {
  console.log(searchParams); // { filter: "all" }
}
```

Consider that the page that uses `searchParams` will no longer be statically rendered.

## Using searchParams in Client Components

```js
"use client";

import { usePathname, useRouter, useSearchParams } from "next/navigation";

export default function Filter() {
  const searchParams = useSearchParams();
  const activeFilter = searchParams.get("capacity") ?? "all";

  const router = useRouter();
  const pathname = usePathname();

  function handleFilter(filter) {
    const params = new URLSearchParams();
    params.set("capacity", filter);
    router.replace(`${pathname}?${params.toString()}`);
  }

  return "jsx";
}
```

## Server Components in Client Components

```js
// page.js

import ClientComponent from "ClientComponent";
import ServerComponent from "ServerComponent";

export default function Page() {
  return (
    <main>
      <ClientComponent>
        <ServerComponent />
      </ClientComponent>
    </main>
  );
}
```

## Data Fetching Strategies

Instead of fetching data from multiple APIs in the same route component, we should delegate the fetching job to inner components (streaming):

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

Try to pass as little data as possible to client components.

## Using the Context API for State Management

Place the context provider as deep as possible in the component tree to prevent unnecessary re-renders.

## Creating an API Endpoint With Route Handlers

Note: Creating API endpoints in Next.js is not so important as it was before in pages router, because now we use server actions to mutate data.

To create a route handler we create another convention file called `route.js`, inside any folder that doesn't have a `page.js` file.

Inside `route.js` we can create one or more functions that each correspond to one of the HTTP verbs. So these functions need to be called the name of the HTTP verb.

```js
// api/route.js

export async function GET(request, { params }) {
  const { dataId } = params;

  try {
    const data = await getData(dataId);
    return Response.json(data);
  } catch () {
    return Response.json({ message: "Data not found" });
  }
}
```
