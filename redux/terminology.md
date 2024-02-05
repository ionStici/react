# Terminology

- **[Redux](https://redux.js.org/)** is a 3rd-party library for managing and updating global application state based on the Flux architecture.

- **State** is the set of data values that describes the application, used to render the UI.

- **State Management:** the process of sharing and updating the state.

- **View** is the user interface displayed to users. Users interact with the UI which dispatch actions to the _store_.

- **Actions** are objects describing an event that a user can take to change the state in the application.

- In a Redux application, _data flows in one direction_: from **state** to **view** to **action**, back to state and so on.

- A **Reducer** is a function that determines how the current state and an action are used in combination to create the application’s next state.

- **Store** is a special object in Redux, it holds and manages the entire state of an application. It provides a way to dispatch actions. It calls the reducer when actions are dispatched.

<br>

**Data flow:**

1. The store initializes the state with a default value.

2. The view displays that state.

3. When a user interacts with the view, like clicking a button, an action is dispatched to the store.

4. The dispatched action and the current state are combined in the store’s reducer to determine the next state.

5. The view is updated to display the new state.

<br>

**How it works:**

1. Users interact with the UI which dispatch actions to the store. An **action** is an object that expresses a desired change to the state.

2. The store generates its next state using a **reducer** function which receives the most recent action and the current state as inputs.

3. Finally, the UI is re-rendered based on the new state of the store and the entire process can begin again.
