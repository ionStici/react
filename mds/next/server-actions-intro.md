# Introduction to Server Actions

**Server Actions** are a Next.js feature that enable you to perform server-side operations, such as data mutations, directly within your React components. These actions run exclusively on the server, providing a secure and efficient way to handle sensitive operations like authentication, authorization, and database interactions.

## How It Works in Practice

- **Server-Side Execution:** The server action runs exclusively on the server, ensuring that sensitive operations such as authentication and database mutations are securely handled.
- **Client-Server Interaction:** The function is triggered by client-side events (e.g., form submissions), but the actual processing happens on the server.
- **Automatic API Endpoint:** Next.js automatically creates an API endpoint for this server action, managing the communication between the client and server seamlessly.

## Example: Adding a To-Do Item

The following code demonstrates a typical server action used to add a to-do item.

```js
// actions.js

"use server";

import { auth } from "auth";
import { getTodoList } from "getTodoList";

import { revalidatePath } from "next/cache";
import { redirect } from "next/navigation";

export async function addTodo(formData) {
  // 1. Authentication
  const session = await auth();
  if (!session) throw new Error("You must be logged in");

  // 2. Authorization
  const todoList = await getTodoList(session.user.id);
  if (!todoList.id === session.user.id) throw new Error("Not allowed");

  // 3. Data validation
  const todo = formData.get("todo").slice(0, 50);
  if (!/^[a-zA-Z0-9]{1,20}$/.test(todo)) throw new Error("Invalid format");

  // 4. Preparing data
  const newTodo = { todo, todoID: formData.get("todoID") };

  // 5. Mutation
  const { error } = await supabase.from("todoList").insert([newTodo]);

  // 6. Error handling
  if (error) throw new Error("New todo could not be created");

  // 7. Revalidation
  revalidatePath("/todo");

  // 8. Redirecting
  redirect(`/todo/${newTodo.todoID}`);
}
```

- **`'use server'`** : This directive at the top of the file ensures that the function is treated as a server action and will only run on the server.

- The `addTodo` function is a server action that handles adding a to-do item. It accepts `formData` as a parameter.

- **Authentication:** The function starts by authenticating the user using the `auth` function. If authentication fails, an error is thrown.

- **Authorization:** The function then fetches the to-do list associated with the authenticated user. If the user is not authorized to modify the list, an error is thrown.

- **Data Validation:** The to-do item is extracted from `formData` and validated against a regular expression to ensure it meets the required format.

- **Preparing Data:** The validated to-do item is then prepared for insertion into the database.

- **Mutation:** The new to-do item is inserted into the `todoList` table using Supabase. If the insertion fails, an error is thrown.

- **Revalidation:** The `revalidatePath` function is called to revalidate the cache for the `/todo` path, ensuring the new to-do item appears in the list.

- **Redirecting:** Finally, the `redirect` function navigates the user to the `/todo/${todoId}` page.

## Using Server Actions in a Form

Server actions in Next.js allow you to perform server-side operations directly from client-side components, such as forms.

To use a server action with a form, we need to set the server action to the `action` attribute of the `form` element, which will allow the server action to handle the form submission by passing the `formData` object to the server action function.

```js
import addTodo from "./actions";

export default function TodoList() {
  return (
    <form action={addTodo}>
      <input type="text" name="todo" />
      <Button pendingLabel="Adding...">Add Todo</Button>
    </form>
  );
}
```

## Calling a Server Action with Additional Parameters

```js
import { createItemAction } from "server-action";

export default function UpdateItem({ reset, id }) {
  const createItem = createItemAction.bind(null, id);

  return (
    <form
      action={async (formData) => {
        await createItem(formData);
        reset();
      }}
    >
      <input />
    </form>
  );
}
```
