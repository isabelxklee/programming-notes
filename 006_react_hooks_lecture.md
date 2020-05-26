# React Hooks Lecture

### File organization
Create a folder just for functional components. Babel JS is a tool that transpiles ES6 code into a previous version of JavaScript. Babel shows us that class components take up more memory than functional components do. 

### Using functional components
We can `console.log` anywhere as long as it's above the return statement.

```
const App = () => {
  const addList = (newList) => {
    console.log(newList, "From App")
  }

  return (
    <div>
      // something
    </div>
  )
}
```

### Getting started with hooks
Go to `index.js`. Change the App component to a functional component. We can't use `setState()` anymore. We aren't importing `{Component}` from React anymore.

Use a different functionality that's built into React. Import `useState` from React.
```
import React, {useState} from 'react'
```

`useState()` takes in `initialState` as an argument. This is the same as:

```
state = {
  key: value
}
```

Call `useState()` and set it as a new variable. We can destructure the state upon declaring it. Hooks are usually structured as the Variable name of the state, and then setting the state. Pass in an empty array to be the initial state.

```
let [state, setState] = useState([])
```

Setting the state will always overwrite the pre-existing value for state. If we want to retain the old values, we can use a spread operator.

Create an event handler that changes state.
```
const handleClick = (event) => {
  setState(["Hello"])
}

return (
  <h1 onClick={handleClick}>Hello</h1>
)
```

### Hooks vs. Class component states
States in functional component hooks can include any data type, whereas state in a class component has to be an object that includes key: value pairs.

### Lifecycle methods
`componentDidMount()` will only run once if there isn't a `componentWillUnmount()` in the application.

Import `useEffect` from React. This will allow us to have lifecycle methods in a functional component.
```
import React, {useState, useEffect} from 'react'
```

Create a `useEffect()` method. If you try to change the state inside this method, it will run forever. This is because it runs whenever the app updates and changing the state is an update.

Example:
```
useEffect(() => {
  console.log("hello")
  setState(["Goodbye"])
})
```

This will log `"hello"` to the console a million times.

To avoid this, we need to add a second argument when calling `useEffect()`.