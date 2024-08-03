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
