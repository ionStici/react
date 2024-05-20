# Json Server

[json-server - npm](https://www.npmjs.com/package/json-server)

```
npm install json-server
```

```json
"scripts": { "server": "json-server data/names.json --port 9500" }
```

```jsx
useEffect(() => {
  async function fetchObjects() {
    const res = await fetch(`http://localhost:9500/objects`);
  }
}, []);
```

```jsx
async function createObjects(newObject) {
  const res = await fetch(`http://localhost:9500/objects`, {
    method: 'POST',
    body: JSON.stringify(newObject),
    'Content-Type': 'application/json',
  });

  const data = res.json();
}
```
