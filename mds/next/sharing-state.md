# Sharing State Between Client and Server: The URL

## searchParams in Server Components

Server components get access to `searchParams` through props.

```js
// page.js
export default function Page({ searchParams }) {
  console.log(searchParams); // { sort: "all" }
}
```

Consider that the page that uses `searchParams` will no longer be statically rendered.

## Using searchParams in Client Components

```js
"use client";

import { usePathname, useRouter, useSearchParams } from "next/navigation";

export default function Text() {
  const searchParams = useSearchParams();
  const activeSort = searchParams.get("sort") ?? "all";

  const router = useRouter();
  const pathname = usePathname();

  function handleFilter(filter) {
    const params = new URLSearchParams();
    params.set("sort", filter);
    router.replace(`${pathname}?${params.toString()}`);
  }

  return <button onClick={() => handleFilter("surprise")}>{activeSort}</button>;
}
```
