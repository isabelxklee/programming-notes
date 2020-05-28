# Intro to Redux Part 2

Project summary: Build a web application that displays pet information. Include CRUD operations for pet instances.

Note: Download Redux DevTools. Helps view the global state in Chrome. View the github repo to add it to our application.

## Getting started
Create an initial state in `index.js`. This will be a global state that's available in all of `App.js`.
```
import { createStore } from 'redux'

let initialState = {
  pets: [
    {
      id: 29,
      name: "Heihei",
      breed: "chicken"
    }
  ]
}
```

Run `npm install react-redux`. This is different than `npm install redux`. Import the `Provider` component in `index.js`.

```
import { Provider } from 'react-redux'
```

Pass in the Provider component to `React.DOM.render()` and wrap it around the App component.
```
let store = createStore(reducer)

...

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>
  document.getElementById('root')
)
```

## Accessing the global state in other components
Navigate to `PetsContainer.js`.

Import the `connect()` function from React/Redux. We will need to import this for any component that needs to get or set information from the global state.

This function takes in a component and returns a magical component.

```
import { connect } from 'react-redux'
...

const PetsContainer = (props) => {
  return (
    // do something
  )
}

let magicalFunc = connect()
let magicalComp = magicalFunc(PetsContainer)

export default magicalComp
```

We can also write it in one line.
`export default connect()(PetsContainer)`

Create a function that will pull out information from the store. The return value of this function will be merged as props. This function should be written outside of the component declaration.
```
let mapStateToProps = (globalState) => {
  return {
    allPets: globalState.pets
  }
}
```

Update the return value of PetsContainer to include the function.
```
let array = props.allPets.map((pet) => {
  return <Pet key={pet.id} pet={pet}>
})
```

## Sending down the global state as props
Navigate to `Pet.js`.
