[&larr; Back](./README.md)

# Common TypeScript Configurations

[**TypeScript Configuration Reference**](https://www.typescriptlang.org/tsconfig)

[**compilerOptions reference**](https://www.typescriptlang.org/tsconfig#compilerOptions)

<br>

## Using Community Created Configurations

Figuring out all the options we should include for every project can add a lot of setup time when starting a project. Luckily, the TypeScript community maintains a variety of configuration [bases](https://github.com/tsconfig/bases/).

A configuration base is a shared TypeScript configuration that we can install as a dependency and then use in our `tsconfig.json`.

To install a base, we can run the following command at the root of our TypeScript project:

```
npm install --save-dev @tsconfig/recommended
```

Then, in our `tsconfig.json` file, we can add this base as the value of the `extends` option:

```json
{
  "extends": "@tsconfig/recommended/tsconfig.json",
  "compilerOptions": { ... }
}
```

The `@tsconfig/recommended` base applies [this configuration](https://github.com/tsconfig/bases/blob/main/bases/recommended.json). While this will set up our `tsconfig.json` with a variety of options, we can override any option defined by the base configuration by defining options in our `tsconfig.json`.

<br>

## Creating a Shared Configuration

Creating a configuration that fits our exact needs that we can share across projects.

To create a custom TS base, we'll need a JSON file with our TS configuration that we'd like to use as a base configuration.

```json
// custom-tsconfig.json
{
  "compilerOptions": {
    "target": "esnext",
    "strict": true,
    "noUnusedParameters": true,
    "noUnusedLocals": true
  }
}
```

Then, inside `tsconfig.json`, we add our `custom-tsconfig.json` as the base configuration by adding its path to the `extends` options.

```json
{
  "extends": "./custom-tsconfig.json",
  "compilerOptions": { ... }
}
```

<br>
