# Redux

**Redux** is a 3rd-party library used for **global state management**

**It is a Standalone library**, but easy to integrate with React apps using `react-redux` library

All global state is stored in one globally accessible store, which is easy to update using "actions". Global store is updated -> then: All consuming components re-render

Two "versions": (1) Classic Redux, (2) Modern Redux Toolkit

Ideal use case for Redux, when there is lots of UI state that updates frequently, but for remote global state we have better and more specialized tools.

<br>

The mechanism of the Redux hook
