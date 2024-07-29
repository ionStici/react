[&larr; Back](./README.md)

# Custom Types

## Table of Contents

- [Enums](#enums)
  - [Enums under the hood](#enums-under-the-hood)
- [String Enums vs. Numeric Enums](#string-enums-vs-numeric-enums)
- [Object Types](#object-types)
- [Type Aliases](#type-aliases)
- [Function Types](#function-types)
- [Generic Types](#generic-types)
- [Generic Functions](#generic-functions)

<br>

## Enums

We use **enums** when we’d like to enumerate all the possible values that a variable could have, or in other words, to limit the possible values of a variable.

For example, a variable of the `string` type can have any string as a value, there are infinitely many possible strings, and it would be impossible to list them all.

```ts
enum Pet {
  Hamster,
  Rat,
  Chinchilla,
  Tarantula,
}

const petOnSale: Pet;
petOnSale = Pet.Chinchilla;
petOnSale = Pet.Snake; // error
```

Above, we define the enum `Pet`, representing four animals. Any other values, are not allowed.

Then, our enum type is used in type annotation like any other type.

```ts
const orders: [Pet, number][] = [
  [Pet.Rat, 2],
  [Pet.Chinchilla, 1],
  [Pet.Hamster, 2],
  [Pet.Chinchilla, 50],
];
```

<br>

### Enums under the hood

Under the hood, enum values are assigned a numerical value according to their listed order, starting with `0`.

```ts
petOnSale = Pet.Chinchilla;
petOnSale === 2; // true
```

Furthermore, we can reassign `petOnSale` to a number value, like `petOnSale = 0`, and it does not raise a type error.

We can change the starting number of the first enum value so the next enums will continue from that number, or even specify all numbers separately.

```ts
enum Pet {
  Hamster = 4,
  Rat = 7,
  Chinchilla = 3,
  Tarantula = 1,
}
```

This type of enums are referred to as _numeric enums_, since they are based on numbers.

<br>

## String Enums vs. Numeric Enums

TypeScript also allows us to use enums based on strings, referred to as _string enums_.

```ts
enum DirectionNumber {
  North,
  South,
  East,
  West,
}

enum DirectionString {
  North = "NORTH",
  South = "SOUTH",
  East = "EAST",
  West = "WEST",
}
```

With numeric enums, the numbers could be assigned automatically, but with string enums we must write the string explicitly.

Technically, any string will work, however, it is much better to use the convention where the string value of the enum variable is just the capitalized form of the variable name.

With numeric enums variables we can assign arbitrary numbers directly without leading to type errors. Because of this, as a general rule, always use string enums because they are much more strict.

<br>

## Object Types

Object Type Annotation.

```ts
let person: { name: string; age: number };
person = { name: "John", age: 25 };
```

Trying to assign a value to `person` that doesn't have `name` and `age` properties of the specified types will lead to a type error.

TypeScript places no restrictions on the types of an object’s properties. They can be enums, arrays, and even other object types.

<br>

## Type Aliases

**Type Aliases** - alternative type names that we choose for convenience.

```ts
type specialString = string;
let sayHello: specialString = "Hello";

type Person = { name: string; age: number };
let company = {
  companyName: string,
  boss: Person,
  employees: Person[],
  employeeOfTheMonth: Person,
};
```

Type aliases are truly useful for referring to complicated types that need to be repeated, especially object types and tuple types.

<br>

## Function Types

Note: In JavaScript, functions can be assigned to variable:

```js
let log = console.log; // lack of parentheses
log("hello"); // Prints: Hello
```

In TypeScript, we can precisely control the kinds of functions we assign to a variable, by using **function types** where we specify the argument types and return type of a function.

Type Function Syntax: arrow notation for functions / instead of the return value we put the return type.

```ts
type funcType = (arg0: string, arg1: string) => number;
let myFunc: funcType = (ar1, ar2) => ar1.length + ar2.length;
```

It doesn’t matter what we name the function parameters or the parameters in the type annotation, so long as they have the correct types.

<br>

## Generic Types

TypeScript's generics - create collections of types that share certain similarities. This collections are parameterized by one or more type variables.

Example of a generic type - `Array<T>` - this is generic because we can substitute any type in the place of `T`.

With generics we can define our own collections of object types.

```ts
type Family<T> = {
  parents: [T, T];
  mate: T;
  children: T[];
};

let family: Family<string> = {
  parents: ["stern string", "nice string"],
  mate: "string next door",
  children: ["stringy", "stringo", "stringina", "stringolio"],
};
```

This code defines a collection of object types. We then substitute `T` with some type of our choosing.

Writing generic types with type `typeName<T>` allows us to use `T` within the type annotation as a type placeholder. Later, when the generic type is used, `T` is replaced with the provided type.

<br>

## Generic Functions

Code Example: a function that returns arrays filled with a certain value:

```ts
function getFilledArray<T>(value: T, n: number): T[] {
  return Array(n).fill(value);
}

getFilledArray<string>("cheese", 3);
```

The above code tells TypeScript to make sure that `value` and the returned array have the same type `T`.

When the function is invoked, we provide the `T`'s value.

In general, writing generic functions with function `functionName<T>` allows us to use `T` within the type annotation as a type placeholder. Later, when the function is invoked, `T` is replaced with the provided type.

<br>
