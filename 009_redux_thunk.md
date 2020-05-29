# Redux Thunk
Create separate reducers for different models.

Example: pets reducer vs. user reducer

```
let petReducer = (state, action) => {
    // something
}

...

let userReducer = (state = initialState, action) => {
  switch(action.type) {
    // do something
    default:
      return state
  }
}
```

Combine different reducers by importing `combineReducers`.

```
import {combineReducers} from 'redux'

let singleObj = {
  petInfo: petReducer,
  userInfo: userReducer
}

let rootReducer = combineReducers(singleObj)

let store = createStore(rootReducer, ...)
```

Now the structure of the application has changed. It's easier to start this way, rather than building out a single reducer and then realizing later on that we need multiple reducers.

### Getting started
Navigate to `App.js`. Make a fetch request for the pets information inside the `componentDidMount()` function.

```
componentDidMount() {
  fetch("URL/users")
  .then(r => r.json())
  .then( // do something )
  ...

  fetch("URL/pets")
  .then(r => r.json())
  .then( // do something )
}
```
Connect this new information to the global state. Import `connect` from React/Redux. Create a function that returns an action.
```
import {connect} from 'react-redux'

...

export default connect(null, setInformation)(withRouter(App))

let setAllPets = (allPets) => {
  return {
    type: "SET_ALL_PETS",
    payload: allPets
  }
}
```

Update the pet reducer to account for this new action.
```
{
  ...

case "SET_ALL_PETS":
  return {
    ...state,
    pets: action.payload
  }
  default:
    return stat
}
```

Update the fetch request for pets.
```
.then((petsArray) => {
  this.props.setAllpetsArray(pets)
})
```

### Summary
* Create reducers for different models.
* Create a combined reducer.
* Update the store to reflect the root reducer.
* Create a fetch request for the backend information. Whatever comes back is only accessible in the App component.
* Send the backend information to the global state.
* Import connect from React/Redux any time we want to change information / use lifecycle methods.
* Now it's a question of if we're GET-ing information or SET-ing information.
* If we're setting information, we need to create an action for it.
* Then we have to dispatch the action to the reducer.
* The return value of a reducer becomes the new global state.