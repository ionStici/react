# Axios

## Introduction

[**Axios**](https://axios-http.com/) is a popular JavaScript library used to make HTTP requests from both the browser and Node.js environments. It offers a promise-based interface, allowing for easy integration with `async/await` syntax, and comes with numerous features that simplify HTTP interactions.

### Why Axios

- **Promise-based Interface:** Simplifies asynchronous code with `async/await`.
- **Interceptors:** Ability to intercept requests or responses before they are handled.
- **Automatic JSON Data Transformation:** Automatically transforms JSON data.
- **Request Cancellation:** Supports canceling requests.
- **Browser Compatibility:** Works in older browsers without the need for polyfills.

```
npm install axios
```

```js
import axios from "axios";
```

**Essential HTTP methods:** `axios.get()`, `axios.post()`, `axios.put()`, `axios.delete()`, `axios.patch()`

## GET Request

```jsx
async function getData() {
  const response = await axios.get("api/data");
  return response;
}
```

- `axios.get` : send a GET request to the specified endpoint.

## POST Request

```jsx
async function postData() {
  const response = await axios.post(
    "api/data",
    { name: "John", password: "i'm not telling" },
    { headers: { "Content-Type": "application/json" } }
  );
}
```

- `axios.post` : send a POST request.
- The second and third arguments are for the data to be sent and for headers.
- The request body is automatically serialized.

## Axios + React Query

```jsx
async function getData() {
  const { data } = await axios.get("api/data");
  return data;
}

function ReactComponent() {
  const { data } = useQuery("data", getData);
  return <RenderData data={data} />;
}
```

## The Axios Instance

```js
export const api = axios.create({
  baseUrl: "http://localhost:3000", // Base URL for all requests
  timeout: 1000, // Requests timeout after 1 second
  headers: { "X-Custom-Header": "foobar" }, // Include custom header
  withCredentials: true, // Send cookies with cross-origin requests
});
```

`axios.create` accepts a config object that sets default settings for all requests made by that instance.

- `baseUrl` : default base URL that will be prepended to all relative URLs in requests made by this Axios instance, example: `api.get('items')`.

- `timeout` : Specifies the number of milliseconds before the request times out. If the request takes longer than the `timeout`, it will be aborted.

- `headers` : Allows you to set custom HTTP headers that will be sent with every request made by this Axios instance.

- `withCredentials` : Indicates whether or not cross-site Access-Control requests should be made using credentials such as cookies, authorization headers, or TLS client certificates.

## Interceptors

### Request Interceptor

```js
import api from "./api";

api.interceptors.request.use(
  (config) => {
    // Do something before the request is sent
    config.headers.Authorization = `Bearer ${"<Token>"}`;
    return config;
  },
  (error) => {
    // Do something with request error
    return Promise.reject(error);
  }
);
```

### Response Interceptor

```js
import api from "./api";

api.interceptors.response.use(
  (response) => {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data
    return response;
  },
  (error) => {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error

    if (error.response.status === 401) {
      // Handle unauthorized access
    }
    return Promise.reject(error);
  }
);
```
