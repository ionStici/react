# Axios

[Axios](https://axios-http.com/) is a promise-based HTTP client for JavaScript that can be used in both the browser and Node.js environments. It simplifies making HTTP requests and handling responses. When used with React, it enables easy integration with RESTful APIs, facilitating data fetching and manipulation.

```
npm install axios
```

```js
import axios from "axios";
```

## GET Request

```jsx
async function getData() {
  const response = await axios.get("api/data");
}
```

We use `axios.get` to send a GET request to the specified URL.

## POST Request

```jsx
async function postData(data) {
  const response = await axios.post("api/data", data);
}
```

We use `axios.post` to send a POST request to the specified URL, passing the `data` we receive as the second argument.

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
