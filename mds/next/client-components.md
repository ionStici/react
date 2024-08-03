## Client Components

The `'use client'` directive designates a component as a client component.

```js
"use client";

import { useState } from "react";

export default function Counter({ c }) {
  const [count, setCount] = useState(c);
  return <button onClick={() => setCount((p) => p + 1)}>{count}</button>;
}
```

**Props:** Client components can receive data from server components through props.

**Initial Render:** Initially, all components, including client components, are rendered on the server.

**Hydration:** Once the components are sent to the client, client components are hydrated to add interactivity.

**Context API Note:** Place the context provider in the Root Layout or as deep as possible in the component tree to prevent unnecessary re-renders.

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
