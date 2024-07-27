[&larr; Back](./README.md)

# TypScript Cheetsheet

## Table of Contents

- [Introduction](#introduction)
- [Built-in Types](#built-in-types)
- [Functions](#functions)
- [Arrays](#arrays)
- [Objects](#object-type-annotationtypes)
- [Enums](#enums)
- [Function Types](#function-types)
- [Type Aliases](#type-aliases)
- [Generic Types](#generic-types)
- [Generic Functions](#generic-functions)
- [Union Types](#union-types)
- [OOP with Interface](#oop-with-interface)
- [Type Narrowing](./type-narrowing.md)

<br>

## Introduction

- TypeScript files extension: `main.ts`

- `tsc` command - transpile TypeScript code to JavaScript code.

- `tsconfig.json` file specifies the transpier options required to compile TypeScript.

- **Type Inferences:** when the type is not explicitly declared by the user, TypeScript will infer the type on its own.

- **Type Shapes:** an object's shape describes what properties and methods it does or doesn't contain.

- **Any:** When TS isn't able to infer a type, it will consider the variable of type `any`

  Variables of type `any` can be reassigned to any value later on.

```ts
const my_name: string = "John";
```

- **Variable Type Annotation (declaration):** explicitly tell TS what type a variable is.

<br>

## TypeScript Built-in Types

### Primitive Types

_Basic JavaScript Data Types._

1. `number`: for numeric values
2. `string`: sequence of characters
3. `boolean`: true or false
4. `symbol`: represents a unique value
5. `BigInt`: for large integer values.

### Composite Types

_Types made up of other types._

1. **Object**: collection of key/value pairs
2. **Array**: collection of values of the same type
3. **Tuple**: collection of values of different types
4. **Enum**: represents a set of named constant values
5. **Union**: a value that can be of one of several types
6. **Intersection**: a type that combines two or more types
7. **Type Alias**: a custom type name
8. **Generic Type**: parameterized collections of types
9. Other: Function and Class

### Special Types

_Types with special meanings for TypeScript._

1. `any` - represents any type, for dynamic typing
2. `void` - represents a value that has no type
3. `never` - represents a value that never occurs
4. `unknown` - similar to `any`, but more restrictive
5. `null` - represents the absence of a value
6. `undefined` - represents a value that has not been assigned

<br>

## Functions

```ts
const my_name: string = "John";

const greet = (name: string = "Mike", age?: number): string => {
  return `Hello, ${name}!`;
};

greet(my_name); // Hello, John!
```

- **Function parameter type annotation:** `(name: string)`

  Parameters without a type annotation are assumed to be of type `any`

- **Optional Parameters:** `(age?: number)` -

  Indicate that a parameter is intentionally optional, otherwise TS throws an error if an argument is omitted.

- **Default Parameters:** `(name = "John")`

  If a parameter is assigned a default value, TS will infer the variable type of the default value's type.

- **Inferring Return Types:** based on the value after the return statement, TS infers the type for the return value.

- **Explicit Return Types:** `() : string => {}`

  TS will produce an error for any return statement that doesn't return the right type of value.

- **Void Return Type:** `() : void => {}`

  Recommended practice: when no returned value, use `void`

<br>

## Arrays

```js
// the numb array can only contain numbers
const numb: number[] = [1, 7];
```

```js
// Multi-dimensional Arrays
const letters: string[][] = [["a", "b"], []];
const numbers: number[][][] = [[[13]], [[27]]];
```

```js
// Tuples: array typed with elements of specific types
// Each value must match its type and index
const tuple: [string, number, boolean] = ["Hello", 25, true];
```

```js
// Rest Parameters & Spread Syntax
const friends: [string, string] = ["Robert", "John"];
const log = (...friends: string[]) => console.log(...friends);
log(...friends);
```

<br>

## Object Type Annotation

```ts
const person: { name: string; age: number } = { name: "John", age: 25 };
```

<br>

## Enums

**Enums:** enumerate and limit all the possible values that a variable can take.

```ts
enum Color {
  Red = 1,
  Green = 7,
  Blue = "BLUE",
}
```

Each member is assigned a numeric value by default, starting from 0. We can explicitly set the values of enum members to numeric or string values, like above. String Enums Convention: the string value should be just the capitalized form of the enum member.

```ts
const myColor: Color = Color.Green;
const blue: Color = Color.Blue;
const reverse: string = Color[myColor];
console.log(myColor, reverse, blue); // 2 'Green' 'BLUE'
```

Enums can be used in reverse, where you convert a numeric value to its corresponding enum member.

<br>

## Type Aliases

**Type Aliases** - define a custom name for a more complex type.

```ts
// simple string type
type str = string;
const sayHello: str = "Hello";
```

```ts
// object alias
type Point = { x: number; y: number };
let point: Point = { x: 10, y: 20 };
```

```ts
// Intersection Types with Type Aliases
type Person = { name: string };
type Employee = { employeeID: number };
type EmployeeData = Person & Employee;
let mike: EmployeeData;
```

<br>

## Function Types

Precisely control the argument and return types of a function.

```ts
type funcType = (arg0: string, arg1: string) => number;
let myFunc: funcType = (ar1, ar2) => ar1.length + ar2.length;
```

<br>

## Generic Types

**Generics** - create parameterized collections of types.

```ts
type Person<T, N> = {
  name: [T];
  age: [N];
};

const John: Person<string, number> = {
  name: ["John"],
  age: [25],
};
```

<br>

## Generic Functions

```ts
function multiply<T>(value: T, n: number): T[] {
  return Array(n).fill(value);
}
multiply("cheese", 3); // (3) ['cheese', 'cheese', 'cheese']
```

The `value` argument and the returned array will have the same type `T`

<br>

## Union Types

**Union** - define multiple types that a value can take.

```ts
let id: string | number = 1;
id = "001";
```

`string | number` is a union that allows `id` to be a `string` or a `number`.

Unions make the code more flexible, but at the same time more specific than the `any` type.

Unions can be used anywhere a type value is defined, including function parameters:

```ts
function getMargin(margin: string | number) {}
```

### Unions for Arrays

```ts
const seven: (string | number)[] = [7, "7"];
```

### Unions with Literal Types

**Literal Type Unions** - specific (literal) values that a variable or parameter can take.

Literal type unions provide precise type definition, make our intentions explicit, less errors.

```ts
type Color = "Green" | "Yellow" | "Red";
function changeLight(color: Color) {}
```

Here, the `color` parameter can only take one of the 3 specified string literals.

<br>

## OOP with Interface

**`interface`** technically works like `type`

**`implements`** keyword to apply interfaces

Notes: TypeScript allows to infinitely nest objects

```ts
interface Person {
  name: string;
  data: { birthYear: number };
  greet: (keyword: string) => void;
}

class User implements Person {}
```

### Composed Types

```ts
interface Version {
  versionNumber: number;
}

interface General {
  name: string;
  id: number;
  version: Version;
}

interface About {
  general: General;
}

const myAboutInfo: About = {
  general: {
    id: 1,
    name: "My Thing",
    version: {
      versionNumber: 1.0,
    },
  },
};
```

### Extending Interfaces

Copy type members from one interface to another interface using `extends` keyword.

```ts
interface Person {
  name: string;
}

interface User extends Person {
  age: number;
}

const mike: User = { name: "Mike", age: 27 };
```

### Index Signatures

**Index Signatures** - define types for objects where you don't know all the property names in advance but know their types.

```ts
interface Obj {
  [key: string]: number;
}
```

The `[key: string]` syntax within `Obj` defines the keys type as `string` with a value of type `boolean`.

The `key` name is purely for us the developer, it will show up in potential error messages.

### Optional Type Members

```ts
interface User {
  name: string;
  age?: number;
}
```

We can denote any type member as optional using the `?` operator.

<br>
