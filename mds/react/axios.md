# Axios

[Axios](https://axios-http.com/) is a promise-based HTTP client for JavaScript that can be used in both the browser and Node.js environments. It simplifies making HTTP requests and handling responses. When used with React, it enables easy integration with RESTful APIs, facilitating data fetching and manipulation.

```
npm install axios
```

```js
import axios from 'axios';
```

## GET Request

```jsx
axios.get('api-url.com');
```

## POST Request

```jsx
axios.post('api-url.com', { name: 'Data', description: 'Data description' });
```

## Async/Await

```jsx
async function fetchData() {
  const response = await axios.get('api-url.com');
}
```

## Axios + React Query

### GET

```jsx
async function fetchData() {
  const { data } = await axios.get('api-url.com');
  return data;
}

function Component() {
  const { data } = useQuery('data', fetchData);
  return null;
}
```

### POST

```jsx
async function postData(newData) {
  const { data } = await axios.post('api-url.com', newData);
  return data;
}

function Component() {
  const { mutate } = useMutation(postData);
  const handleSubmit = newData => mutate(newData);
  return null;
}
```
