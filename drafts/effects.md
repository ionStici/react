# Effects and Data fetching

## The Component Instance Lifecycle

The lifecycle of a component encompasses the different phases that a component instance can go through over time. Phases:

1. **Mount / Initial Render**

2. **Re-render** - Happens when: State changes, Props change, Parent re-renders, Context changes.

3. **Unmount** - the component instance is destroyed and removed along with its state and props.

   Later a new instance of the same component can be mounted later, but this specific instance that was unmounted is gone.

We can define code to be executed at different phases of a component's lifecycle by using the `useEffect` hook.

<!-- Setting state in the render logic will cause the component to re-render itself again and again indefinitely. -->

<br>
