# Redux vs. Context API

## Context API vs. Redux

**Context API + useReducer**

- Built into React
- Easy to set up a single context
- Additional state "slide" requires new context set up from scratch ("provider hell" in App.js)
- No mechanism for async operations
- Performance optimization is a pain
- Only React DevTools

**Redux**

- Requires additional package (larger bundle size)
- More work to set up initially
- Once set up, it's easy to create additional state "slices"
- Supports middleware for async operations
- Performance is optimized out of the box
- Excellent DevTools

## Where to use Context API or Redux?

**Context API + useReducer**

_"Use the Context API for global state management in small apps"_

- When you just need to share a value that doesn't change often [color theme, preferred language, authenticated user, ...]
- When you need to solve a simple prop drilling problem
- When you need to manage state in a local sub-tree of the app

**Redux**

_"Use Redux for global state management in large apps"_

- When you have lots of global UI state that needs to be updated frequently (because Redux is optimized for this) [shopping cart, current tabs, complex filters or search, ...]
- When you have complex state with nested objects and arrays (because you can mutate state with Redux Toolkit)
