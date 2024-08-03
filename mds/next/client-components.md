## Client Components

The `'use client'` directive designates a component as a client component.

```jsx
"use client";

import { useState } from "react";

export default function Counter({ c }) {
  const [count, setCount] = useState(c);

  return <p>{count}</p>;
}
```

**Props:** Client components can receive props from server components.

**Initial Render:** Initially, all components, including client components, are rendered on the server.

**Hydration:** Once the components are sent to the client, client components are hydrated to add interactivity.

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

## Using the Context API for State Management

Place the context provider as deep as possible in the component tree to prevent unnecessary re-renders.
