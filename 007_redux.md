# Intro to Redux
Redux is an independent JavaScript library from React, but they go hand-in-hand. Redux helps manage application state by creating a global state object.

## Problems with React
* Inverse data flow can get confusing
* When props and states change, a component re-renders. It can get messy sending function definitions down and invoking functions back up.

## Principles of Redux
* Global state is independent of all other components. Its placed above the App component.
* Pure functions
* Single source of truth
* Store: plain, old JavaScript object that knows about the global state
* There is only one store in a Redux application
* Read-only: we can't mutate state directly
* But we can change state by dispatching an action to the store, which is similar to `setState()`
* An action is a plain, old JavaScript object that has 2 keys: `type` and `payload`
* `type`: the command that describes the state change
* `payload`: data that's needed to complete the change