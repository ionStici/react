[&larr; Back](./README.md)

# Union Types

TypeScript allows us to be flexible with how specific our types are by combining different types. When we combine types, it is called a union.

<br>

## Table of Contents

- [Defining Unions](#defining-unions)
- [Type Narrowing](#type-narrowing)
- [Unions and Arrays](#unions-and-arrays)
- [Common Key Value Pairs](#common-key-value-pairs)
- [Unions with Literal Types](#unions-with-literal-types)

<br>

## Defining Unions

Unions allow us to define multiple allowed type members by separating each type member with a vertical line character `|`

```ts
let ID: string | number;

ID = 1;
ID = "001";
```

Here, `string | number` is a union that allows `ID` to be a `string` or a `number`.

Like this, it’s more flexible than a single primitive type, but much more specific than the `any` type.

Unions can be written anywhere a type value is defined, including function parameters:

```ts
function getMarginLeft(margin: string | number) {
  return { marginLeft: margin };
}
```

<br>

## Type Narrowing

**Type narrowing** is when TypeScript can figure out what type a variable can be at a given point in our code.

```ts
function getMarginLeft(margin: string | number) {
  if (typeof margin === "string") {
    //
  }

  if (typeof margin === "number") {
    //
  }
}
```

Since `margin` can be a `string` or a `number`, we may want to perform different logic for different scenarios.

To do this, we could implement a **type guard**.

A _type guard_ is a conditional that checks if a variable is a certain type.

_p.s._ TypeScript is able to infer types in many convenient situations. A great example is a function's return type. TypeScript will look at the contents of a function and infer which types the function can return. If there are multiple possible return types, TypeScript will infer the return type as a union.

<br>

## Unions and Arrays

To create a union that supports multiple types for an array’s values, wrap the union in parentheses `(string | number)`, then use array notation `[]`.

```ts
const dateNumber = new Date().getTime(); // number
const dateString = new Date().toString(); // string

const timesList: (string | number)[] = [dateNumber, dateString];
```

p.s. The parentheses are vitally important to type arrays correctly.

<br>

## Common Key Value Pairs

When we put type members in a union, TypeScript will only allow us to use the common methods and properties that all members of the union share.

```ts
const batteryStatus: boolean | number = false;

batteryStatus.toString(); // No TypeScript error
batteryStatus.toFixed(2); // TypeScript error
```

From the above code example, TypeScript will only allow us to call methods that both `number` and `boolean` share, for example the `toString()` method. But, since only `number` has the `toFixed()` method, TS will return an error.

Rule: Any properties or methods that are not shared by all of the union’s members won’t be allowed and will produce a TypeScript error.

<br>

## Unions with Literal Types

Literal type unions are useful when we want to create distinct states within a program.

```ts
type Color = "green" | "yellow" | "red";

function changeLight(color: Color) {
  // ...
}
```

With the code above, we could ensure that wherever `changeLight()` is called, that it gets passed only allowed stoplight colors. If we tried to call `changeLight('purple')`, TypeScript would return an error.

<br>
