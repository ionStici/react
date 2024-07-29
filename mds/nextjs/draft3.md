# draft

Custom react hook that next.js provides that gives the current url (works in client components only).

```js
"use client";
import { usePathname } from "next/navigation";

function Component() {
  const pathname = usePathname(); // /about
}
```
