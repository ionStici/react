[&larr; Back](./README.md)

# Types

**TypeScript** code is a superset of JavaScript code - it has all the features of traditional JavaScript but adds some new features (e.g. types).

TypeScript allows us to write JavaScript with a set of tools called a type system that can spot potential bugs in, clarify the structure of, and help refactor our code.

- TypeScript files extension: `main.ts`
- We run TypeScript code through the TS transpier, and if the code adheres to TypeScript's standards, it will output a JavaScript version of the file (.js)

The `tsconfig.json` file that is placed in the root of the project, contains the enforced rules for the TypeScript compiler.

When the compiler notices an error, it will complain when running `tsc` and also by showing a red squiggly line below the code in the editor.

<br>

## Type Inferences

**Type inference** - TypeScript expects the data type of a variable to match the type of the value initially assigned to it at declaration.

If we try to reassign a variable to a value of a different type, TypeScript will surface an error.

<br>

## Type Shapes

An object's shape describes, among other things, what properties and methods it does or doesn't contain.

Because TypeScript knows what types our objects are, it also knows what shapes our objects adhere to.

e.g. All strings are known to have a `.length` property and `.toLowerCase()` method.

TypeScript will log an error if the code attempts to access members of an object known not to exist.

TypeScript's `tsc` command will let you know if your code tries to access properties and methods that don't exist.

```
tsc index.ts
```

<br>

## Any

In situations where TS isn't able to infer a type, it will consider the variable to be of type `any`.

Variables of type `any` can be assigned to any value and TypeScript won’t give an error if they’re reassigned to a different type later on.

<br>

## Variable Type Annotations

Type annotation (type declaration) - explicitly tell TS what type something is or will be.

We provide a type annotation by appending a variable with a colon `:` and the type (e.g. `string`, `number`, `any`).

```ts
let birthYear: number = 1500;
```

<br>
