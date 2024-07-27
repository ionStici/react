[&larr; Back](./README.md)

# Configurations

<br>

## Installing TypeScript Globally

```
npm install --global typescript
```

Now, we can run `tsc` from any directory.

<br>

## Using tsc

```
tsc index.ts
```

This command will transpile `index.ts` file into a new file named `index.js`

<br>

## Configuring tsc

- [**TSConfig Reference**](https://www.typescriptlang.org/tsconfig)
- [**tsc CLI Options**](https://www.typescriptlang.org/docs/handbook/compiler-options.html)

By default, `target` config is set to transpile TS code into ES3 JS code. ES3 was released in Dec 1999.

One disadvantage of transpiling to ES3 is that it’s more verbose than modern JavaScript specifications. This verbosity results in larger JavaScript file sizes. If we send these JavaScript files to users, like when they visit a website, we’d send them more bytes of code than they need to run our program. As a result, our website would be slower than it needs to be.

To change this:

```
tsc index.ts --target esnext
```

<br>

## `tsc` in watch mode

```json
"scripts": { "tsc": "tsc" }
```

```
npm run tsc -- --watch
```

<br>
