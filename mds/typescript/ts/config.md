[&larr; Back](./README.md)

# TypeScript Configurations

- [**TSConfig Reference**](https://www.typescriptlang.org/tsconfig)
- [**compilerOptions reference**](https://www.typescriptlang.org/tsconfig#compilerOptions)

```
npm install typescript -D
```

```json
// package.json
"scripts": {
    "tsc": "tsc index.ts -w"
}
```

```json
// tsconfig.json
{
  "compilerOptions": {
    "target": "ESNext",
    "strict": true
  }
}
```

<br>
