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

To avoid this, we need to add a second argument when calling `useEffect()`. This time, the method will only run once and `setState` will successfully alter the state.
```
useEffect(() => {
  console.log("hello")
  setState(["Goodbye"])
}, [])
```

Work it into something usable and make a fetch request inside the body. We can't use `componentDidMount()` to make the fetch request becuase we're inside a functional component. We can't use any methods that have the keyword `component` in it.
```
useEffect(() => {
  fetch(URL)
  .then(r => r.json())
  .then((newArr) => {
    setState(newArr)
  })
}, [])
```
The callback gets run every time something in the array of dependencies changes. If the array is empty, it acts as `componentDidMount()`.


### Using hooks with forms
If you have multiple inputs, use multiple instances of `useState()`.

```
let [objState, setObjState] = useState({
  list_name: "Worst Oscar Winning Movies"
})

const handleChange = (event) => {
  setObjState({
    ...objState,
    [event.target.name]: event.target.value
  })
}

return (
  <form onSubmit={handleSubmit}>
    <input
      type="text"
      name="list_name"
      value={objState.list_name}
      onChange={handleChange}
    />
  </form>
)
```

`setObjState()` can also take in a callback function. If we want the function to be asynchronous, we'll have to call `persist()` on the event object.

```
const handleChange = (event) => {
  event.persist()
  setObjState((prevState) => {
    return {...prevState, list_name: event.target.value}
  })
}
```

The juice is not worth the squeeze though. Try to stick to the previous way of setting the state.

### POST request
Create an add list function in `App.js`. Pass it down as props to the form.
```
const addList = (newList) => {
  let copyOfMasterList = [...masterList, newList]
  setMasterList(copyOfMasterList)
}
```

Navigate back to `Form.js` and write a handle submit function. Call the add list function as props in the second `.then` statement to add the new list to an existing master list.
```
const handleSubmit = (event) => {
  event.preventDefault()
  let copyOfList = {
    list_name
  }
  fetch(URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify(copyOfList)
  })
  .then(r => r.json())
  .then(props.addList)
}
```