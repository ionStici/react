# Json Server

```
npm install json-server
```

```json
"scripts": { "server": "json-server data/names.json --port 9500" }
```

```jsx
useEffect(() => {
  async function fetchNames() {
    const res = await fetch(`http://localhost:9500/names`);
  }
}, []);
```
