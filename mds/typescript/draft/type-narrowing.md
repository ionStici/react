[&larr; Back](./README.md)

# Type Narrowing

Type narrowing is when TypeScript can infer more specific types based on the variable’s surrounding code.

<br>

## Table of Contents

- [Type guards](#type-guards)
- [Using in with Type Guards](#using-in-with-type-guards)
- [Narrowing with else](#narrowing-with-else)
- [Narrowing After a Type Guard](#narrowing-after-a-type-guard)

<br>

## Type guards

One way that TypeScript can narrow a type is with a conditional statement that checks if a variable is a specific type - pattern called _type guards_.

Type guards can use a variety of operators that check for a variable’s type, e.g. `typeof` operator.

```ts
function formatDate(date: string | number) {
  if (typeof date === "string") {
    // date must be a string type
  }
}
```

TS evaluates what our code would do at runtime and then infer a more specific type for `date`.

TypeScript can recognize `typeof` type guards that check for these specific values: `string number boolean symbol`.

<br>

## Using in with Type Guards

The [`in` operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in) checks if a property exists on an object itself or anywhere within its prototype chain.

```ts
type Tennis = {
  serve: () => void;
};

type Soccer = {
  kick: () => void;
};

function play(sport: Tennis | Soccer) {
  if ("serve" in sport) {
    return sport.serve();
  }

  if ("kick" in sport) {
    return sport.kick();
  }
}
```

This type narrowing is possible because TypeScript recognizes `in` as a type guard.

<br>

## Narrowing with else

TypeScript can recognize the `else` block of an `if / else` statement as being the opposite type guard check of the `if` statement's type guard check.

```ts
function formatPadding(padding: string | number) {
  if (typeof padding === "string") {
    return padding.toLowerCase();
  } else {
    return `${padding}px`;
  }
}
```

The type guard `typeof padding === 'string'` tells TypeScript that `padding` within the `if` statement’s block must be a string. Going further, TypeScript is also able to infer that, since the `padding` argument is typed as the union `string | number`, that `padding` must be a `number` type within the `else` block.

<br>

## Narrowing After a Type Guard

```ts
type Tea = {
  steep: () => string;
};

type Coffee = {
  pourOver: () => string;
};

function brew(beverage: Coffee | Tea) {
  if ("steep" in beverage) {
    return beverage.steep();
  }

  return beverage.pourOver();
}
```

In the conditional, we immediately return `beverage.steep()`. Once we encounter a `return` statement, the function execution stops. Any code that was meant to work with a `beverage` of type `Tea` will be executed and returned within the conditional. TypeScript then infers that the code following the conditional must be of type `Coffee`.

<br>
