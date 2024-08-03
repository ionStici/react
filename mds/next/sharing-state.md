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
