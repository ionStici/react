[&larr; Back](./README.md)

# Configurations

<br>

## Installing TypeScript Locally Inside a Project

Install TypeScript as a dev dependency:

```
npm install --save-dev typescript
```

```json
// package.json
"scripts": { "tsc": "tsc" }
```

<br>

## Configuring TypeScript

Running a command with many flags is cumbersome, so TypeScript allows us to encode all of its flags inside a configuration file named `tsconfig.json`. We can generate the default `tsconfig.json` with:

```
npm run tsc -- --init
```

Here’s what this command accomplishes:

- `npm run tsc` This runs the `"tsc"` script in `package.json`, which runs the instance of `tsc` installed as a dependency of our project.
- `--` This allows us to pass flags to the "tsc" script in package.json.
- `--init` This is TypeScript CLI’s initialization flag, which will produce a `tsconfig.json` file.

<br>

## Running TypeScript

```
npm run tsc
```

<br>
 
## Running tsc automatically

```
npm run tsc -- --watch
```
