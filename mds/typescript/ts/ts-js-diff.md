[&larr; Back](./README.md)

# TypeScript and JavaScript

**TypeScript** _is a superset of JavaScript._ This means that JavaScript code is valid TypeScript code, but TypeScript provides additional features that make it more reliable.

JavaScript has a vast and mature ecosystem with numerous libraries and frameworks. TypeScript extends JS's ecosystem, having good compatibility with existing JS code, making TS powerful.

TypeScript needs to be transpiled to JavaScript before runtime execution.

TS suppots JS libraries, it is not environment specific, you can use TS with node.

**JavaScript:** Dynamically-typed, variable types are determined at runtime.

**TypeScript:** Statically typed, variable types are determined at compilation.

| Criteria          |    JavaScript     |    TypeScript    |
| ----------------- | :---------------: | :--------------: |
| **First release** |       1995        |       2012       |
| **Created by**    |     Netscape      |    Microsoft     |
| **Typing**        | Dynamically-typed | Statically typed |
| **OOP**           |  Prototype-based  |   Follows OOP    |

### Advantages of using TS over JS

- In TS, errors are caught at compile time, resulting in less runtime errors.

- The TS compiler can compile backwards up to ES3. TS is excellent for large-scale projects

- Provides additional programming features, enhancing code organization and maintainability.

- Due to static typing and additional features, TS code becomes more self-documenting.

### Statically Typed vs. Strongly Typed

- **Statically Typed:** Type checking is done at compile time.

- **Strongly Typed:** Strict type rules are enforced, and implicit type coercion is limited.

**TypeScript:** Statically typed and strongly typed, providing type checking at compile time and enforcing strict type rules.

<br>

## Transpilation vs. Compilation vs. Interpretation

**Compilation:** Entire source code is translated before execution, it produces a standalone executable file. Faster execution. Platform specific.

_Example:_ C is a compiled language. The C source code is compiled by a C compiler before execution. Compilation produces a standalone executable file that can be run on a specific machine architecture.

**Interpretation:** Code is translated and executed line by line at runtime. No separate executable file. Potentially slower execution. Platform-independent.

_Example:_ JavaScript is primarily an interpreted language. The JavaScript engine reads and executes the source code line by line at runtime. No separate compilation step. JavaScript code is executed directly by the browser or another runtime environment.

**Transpilation:** High-level source code is translated to equivalent code in another language. May involve an additional processing step. Enable features, address compatibility.

_Example:_ TypeScript is a superset of JavaScript. TypeScript code is transpiled into JavaScript before execution. The TypeScript compiler `tsc` translates TypeScript code into equivalent JavaScript code.

TypeScript code is transpiled into JavaScript code, and the resulting JavaScript code is executed in a JavaScript runtime environment. The TypeScript compiler removes type annotations and other TypeScript-specific features, producing JavaScript code that is compatible with browsers or other JS runtime environments.

<br>
