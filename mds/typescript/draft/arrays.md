[&larr; Back](./README.md)

# Arrays

## Table of Contents

- [Array Type Annotations](#array-type-annotations)
- [Multi-dimensional Arrays](#multi-dimensional-arrays)
- [Tuples](#tuples)
- [Array Type Inference](#array-type-inference)
- [Rest Parameters](#rest-parameters)
- [Spread Syntax](#spread-syntax)

<br>

## Array Type Annotations

```ts
const nums: number[] = [1, 2, 3];
const onOff: boolean[] = [true, true, false];
const friends: Array<string> = ["Robert", "Andrew"];
```

The `nums` array can only contain numbers, `onOff` only booleans, and `friends` only strings.

<br>

## Multi-dimensional Arrays

We can declare multidimensional arrays: arrays of arrays of arrays (of some type).

```ts
const numbers: number[] = [1, 2, 3];
const multiNumbers: number[][] = [nums, 4, 5];
const letters: string[][][] = ["a", ["b"], [["c"]]];
```

<br>

## Tuples

Arrays with elements of different types.

```ts
let ourTuple: [string, number, string, boolean] = ["Hello", 7, "World", false];
```

In TypeScript, when an array is typed with elements of specific types, it’s called a **tuple**.

Tuple types specify both the lengths and the orders of compatible tuples, and will cause an error if either of these conditions are not met, so each value must match its type and index.

When we want tuples, we need to use explicit type annotations. Tuples have fixed lengths.

<br>

## Array Type Inference

```ts
let arr = [true, true, false];
arr[3] = true; // expanding
```

The type of `arr` is `boolean[]` (and not tuple).

If the type annotation was not defined, then TypeScript infer types from initial values.

<br>

## Rest Parameters

Type annotations for a rest parameter are identical to type annotations for arrays.

```ts
function merge(str, ...letters: string[]) {
  // <code>
}
```

TypeScript will treat `letters` as an array of strings.

<br>

## Spread Syntax

```ts
let words: [string, string] = ["Hello", "World"];
let greet = (...words: string[]) => console.log(...words);
greet(...words);
```

<br>
