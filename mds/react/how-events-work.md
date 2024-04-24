# How Events Work in React

When we register an event handler on a JSX element, for example by using the `onClick` prop, behind the scenes React registers only one event handler per event type at the root node of the fiber tree during the render phase.

This means that if we have multiple `onClick` handlers in our code, React will somehow bundle them all together and just add one big `onClick` handler to the `#root` node of the fiber tree.

Behind the scenes, React performs event delegation for all our events to the `#root` DOM container, where they will get handled, and not in the place where we attach the `onClick` prop.

## Synthetic Events

When we declare an event handler like this one:

```jsx
<input onChange={e => setText(e.target.value)} />
```

..react gives us access to the event object that was created, just like in vanilla JS. However, in React this event object is different.

In vanilla JS we get access to the native DOM event object, React on the other hand will give us something called a synthetic event, which is a thin wrapper around the DOM's native event object.

By wrapper, we mean that synthetic events are similar to native event objects, but they add or change some functionalities on top of them.

These synthetic events have the same interface as native event objects, including important methods like `preventDefault`. What's special about synthetic events, and one of the reasons why the React team decided to implement them is the fact that they fix some browser inconsistencies, so that events work in the exact same way in all browsers. Most synthetic events actually bubble (including focus, blur, and change which usually do not bubble), except for scroll.
