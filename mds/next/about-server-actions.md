# More About Server Actions

[Server Actions and Mutations | Documentation](https://nextjs.org/docs/app/building-your-application/data-fetching/server-actions-and-mutations)

- Server Actions are asynchronous functions that are executed on the server. They can be used in Server and Client Components to handle form submissions and data mutations.

- Server actions can be invoked using the `action` attribute in a `<form>` element, in both server and client components.

- Server actions can also be invoked from event handlers, `useEffect`, third-party libraries, and other form elements like `<button>`.

- Behind the scenes, actions use the `POST` method, and only this HTTP method can invoke them.

- Server Actions are functions. This means they can be reused anywhere in your application.

- When a server action is invoked in a form, the action automatically receives the `FormData` object. To extract the data use native [FormData methods](https://developer.mozilla.org/en-US/docs/Web/API/FormData#instance_methods). This means that you don't need state or third-party libraries to manage the form fields.

## Server Actions in Server Components

```js
export default function Page() {
  async function create(formData) {
    "use server";

    console.log(formData);
  }

  return (
    <form action={create}>
      <input type="text" name="fullName" />
    </form>
  );
}
```

## Server Actions in a Separate File

```js
// actions.js

"use server";

export async function create(formData) {
  console.log(formData);
}
```

## Client Components and Server Actions

Client components can only import server actions from files that uses the `'use server'` directive.

```js
import { create } from "actions";

export default function Button() {
  return <Form create={create} />;
}
```

Client components can use the server action directly or pass as a prop to inner client components.
