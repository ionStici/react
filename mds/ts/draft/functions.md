[&larr; Back](./README.md)

# Functions

## Table of Contents

- [Parameter Type Annotations](#parameter-type-annotations)
- [Optional Parameters](#optional-parameters)
- [Default Parameters](#default-parameters)
- [Inferring Return Types](#inferring-return-types)
- [Explicit Return Types](#explicit-return-types)
- [Void Return Type](#void-return-type)

<br>

## Parameter Type Annotations

Function parameters syntax rule: place the type annotation after a colon next to the parameter name.

```ts
const displayData = (name: string, age: number, job) => {
  console.log(name, age);
};
```

Parameters that we do not provide type annotations for are assumed to be of type `any` - the same way variables are.

<br>

## Optional Parameters

TypeScript normally gives an error if we don’t provide a value for all arguments in a function.

To indicate that a parameter is intentionally optional, we add a `?` after its name.

```ts
const greet = (name?: string) => console.log(`Hello ${name}`);
```

This tells TypeScript that the parameter is allowed to be `undefined` and doesn’t always have to be provided.

<br>

## Default Parameters

If a parameter is assigned a default value, TypeScript will infer the variable type to be the same as the default value’s type. (This is similar to how TypeScript infers the type of an initialized variable to be the same as the type of its initial value.)

```ts
const greet = (name = "John") => console.log(`Hello ${name}`);
```

The function `greet()` can receive a `string` or `undefined` as its `name` parameter — if any other type is provided as an argument, TypeScript will consider that a type error.

Parameters with default values don’t need a ? after their name, since assigning a default value already implies that they’re optional parameters.

<br>

## Inferring Return Types

TypeScript infers the type of values returned by functions.

```ts
const greet = () => "Hello";
const myVar = greet();
```

It does this based on the value after the return statement.

<br>

## Explicit Return Types

We can add an explicit type annotation after function's closing parenthesis.

```ts
const greet = (name?: string): string => `Hello ${name}`;
const sayHello = greet("John");
```

TypeScript will produce an error for any return statement that doesn’t return the right type of value.

<br>

## Void Return Type

It is preferred to use type annotations for functions even when those functions don't return anything.

```ts
const greet = (name?: string): void => {
  console.log(`Hello ${name}`);
};
```

When there is no returned value, we must treat the return type as `void`.

<br>
