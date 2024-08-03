# Creating API Endpoints with Route Handlers

[Documentation](https://nextjs.org/docs/app/api-reference/file-conventions/route)

Route Handlers allow you to create custom request handlers for a given route using the Web Request and Response APIs.

To create a route handler we use another convention file called `route.js` inside any folder that doesn't have a `page.js` file.

Inside `route.js` we can create one or more functions that each correspond to one of the HTTP verbs. Note that these functions need to be called by the name of the HTTP verb.

```js
// api/route.js

export async function GET(request, { params }) {
  try {
    return Response.json({ message: "Success" });
  } catch (err) {
    return Response.json({ message: "Not found" });
  }
}

export async function HEAD(request) {}

export async function POST(request) {}

export async function PUT(request) {}

export async function DELETE(request) {}

export async function PATCH(request) {}
```

Now we can make GET requests to the `api` endpoint thanks to this route handler.

```js
async function fetchData() {
  const response = await fetch("api");
  const data = await response.json();
  console.log(data); // { message: 'Success' }
}
```

The following HTTP methods are supported: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`.

**Note:** Creating API endpoints in Next.js is not so important as it was before in pages router, because now we use server actions to mutate data.
