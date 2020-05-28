# Intro to Redux Part 1
Redux is an independent JavaScript library from React, but they go hand-in-hand. Redux helps manage application state by creating a global state object.

## Problems with React
* Inverse data flow can get confusing.
* When props and states change, a component re-renders. It can get messy sending function definitions down and invoking functions back up.

## Principles of Redux
* Global state is independent of all other components. Its placed above the App component.
* Pure functions
* Single source of truth
* Read-only

## Changing the state
A `store` is a plain, old JavaScript object that knows about the global state.
* There is only one store in a Redux application.

Read-only means that we can't mutate state directly. But we can change state by dispatching an action to the store, which is similar to `setState()`.

An `action` is a plain, old JavaScript object that has 2 keys: `type` and `payload`.
* `type`: the command that describes the state change
* `payload`: data that's needed to complete the change

A `reducer` is a pure function whose return value determines state.
* Pure functions do not change any of the inputs that it receives. It only returns a new copy of state.
* `reduce()` is just a function that returns a single value.

There's no way to go directly from a component to the store. The component has to dispatch an action first.
* Component => Action => Reducer => Store => State

## Creating a React app with Redux
Run `create-react-app [APP_NAME]` in the terminal.

Install Redux. Run `npm install redux` inside the project directory.

Navigate to `index.js` and import the `createStore()` function from Redux.
```
import { createStore } from 'redux'
```

Save the return value from `createStore()` as a variable. Create a reducer function and pass it into `createStore()` as an argument.
```
let reducer = (state, action) => {
  return {
    counter: 3
  }
}

let store = createStore(reducer)
```

If we run `console.log(store.getState())`, it'll return `{counter: 3}`.

Create an action that will tell the reducer what to do.
```
let action = {
  type: "ADD_TO_COUNTER",
  payload: 7
}
```

Dispatch the action to run the reducer and update the state.
```
store.dispatch(action)
```

## Refactor the reducer
Re-write the reducer function using a switch statement. Create an initial state and set it as a default for the state argument.

```
let initialState = {
  counter: 9
}

let reducer = (state = initialState, action) => {
  switch (action.type) {
    case "ADD_TO_COUNTER":
      let newNumber = state.counter + action.payload
      return {
        ...state,
        counter: newNumber
      }
    default:
      return state
  }
}
```