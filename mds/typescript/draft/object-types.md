[&larr; Back](./README.md)

# Object Types

## Table of Contents

- [Interfaces and Types](#interfaces-and-types)
- [Interfaces and Classes](#interfaces-and-classes)
- [Deep Types](#deep-types)
- [Composed Types](#composed-types)
- [Extending Interfaces](#extending-interfaces)
- [Index Signatures](#index-signatures)
- [Optional Type Members](#optional-type-members)

<br>

## Interfaces and Types

Functionally, `type` and `interface` are identical, both will enforce the typed object at compile time when typing a variable.

The `interface` keyword can only be used to type objects, while `type` can be used to type objects, primitives, and more.

Why? Sometimes, we don’t want a type that can do everything. We’d like our types to be constrained so we’re more likely to write consistent code. `interface` it's a perfect match for OOP.

```ts
type Mail = {
  postagePrice: number;
  address: string;
};

const catalog: Mail;

interface Mail {
  postagePrice: number;
  address: string;
}

const catalog: Mail;
```

The `interface` syntax does not require an equals sign `=` before the typed object.

<br>

## Interfaces and Classes

TS gives us the ability to apply a type to an object / class with the `implements` keyword.

```ts
interface Robot {
  identify: (id: number) => void;
}

class OneSeries implements Robot {
  identify(id: number) {}
}
```

The `implements` keyword is used to apply the type `Robot` to `OneSeries`.

`implements` and `interface` allow us to create types that match a variety of `class` patterns, which makes `interface` a good tool for use on object-oriented programs.

<br>

## Deep Types

```ts
interface Robot {
  about: {
    general: {
      id: number;
      name: string;
    };
  };
  getRobotId: () => string;
}
```

Within the `Robot` `interface`, the `general` typed object is nested inside the `about` typed object. TypeScript allows us to infinitely nest objects so that we can describe data correctly.

<br>

## Composed Types

TypeScript allows us to compose types. We can define multiple types and reference them inside other types.

```ts
interface About {
  general: General;
}

interface General {
  id: number;
  name: string;
  version: Version;
}

interface Version {
  versionNumber: number;
}
```

We can now read our code easier with named types and we can reuse the smaller `interfaces` in other places in our code.

<br>

## Extending Interfaces

Copying type members from one type into another type using `extends`.

```ts
interface Shape {
  color: string;
}

interface Square extends Shape {
  sideLength: number;
}

const mySquare: Square = { sideLength: 10, color: "blue" };
```

The `Square` `interface` uses the `extends` keyword to copy all the type members from `Shape` into `Square`.

<br>

## Index Signatures

Sometimes it's not possible to know the property names for an object, but we may know what the data will look like in general. In that case, it’s useful to write an object type that allows us to include a variable name for the property name. This feature is called **index signatures**.

```ts
{
  '40.712776': true;
  '41.203323': true;
  '40.417286': false;
}
```

e.g. We know that all the property names will be strings, and all their values will be booleans, but we don’t know what the property names will be. To type this object, we can utilize an index signature to type this object.

```ts
interface SolarEclipse {
  [latitude: string]: boolean;
}
```

In the `SolarEclipse` type, there’s an index signature used for defining a variable property name of each type member.

The `[latitude: string]` syntax defines every property name within `SolarEclipse` as a `string` type with a value of type `boolean`.

In the `[latitude: string]` syntax, the `latitude` name is purely for us, the developer, as a human-readable name that will show up in potential error messages later.

<br>

## Optional Type Members

A common scenario in programming is creating a function or class that can take in many arguments, some of which are required, and some that are optional.

```ts
interface OptionsType {
  name: string;
  size?: string;
}

function listFile(options: OptionsType) {
  let fileName = options.name;

  if (options.size) {
    fileName = `${fileName}: ${options.size}`;
  }

  return fileName;
}

listFile({ name: "readme.txt" });
```

`OptionsType` has an optional type member named `size`. We can denote any type member as optional using the `?` operator after the property name and before the colon `size?:`

The optional parameter allows us to call `listFile()` with a parameter that does not include a size property at al.

<br>
