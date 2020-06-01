# Review Redux

Send information from the global state to the DOM.

1. Import the `connect` component from React/Redux. We're going to use it in our export default statement.

2. Write an arrow function `mapStateToProps` that has access to the global state. Pass it into the connect component.

What information do we want to get from the global state?

Return this in `mapStateToProps`. It will become props in our component.

```
import {connect} from 'react-redux'

...

render() {
  return (
    <div>
      {this.props.username}
    </div>
  )
}

const mapStateToProps = (globalState) => {
  return {
    username: globalState.userInfo.username,
    token: globalState.userInfo.token
  }
}

export default connect(mapStateToProps)(ProfileContainer)
```

3. Write keys for the values that we want to access from the global state. `mapStateToProps` takes in a JavaScript object, so we can destructure it in the argument. The keys that are returned will become the name of the props.

```
const mapStateToProps = ({userInfo, petInfo}) => {
  return {
    username: userInfo.username,
    token: userInfo.token,
    pet_id: petInfo.selectedPetId
  }
}
```

## Sending info back to global state
Navigate to thet pet form component. Import `connect` from React/Redux. Update the export default statement. Write a `mapStateToProps` function that returns a JavaScript object. The keys should reflect the information that we want from the global state for this component.

For example, if we want to access the user's token, we can specify that in `mapStateToProps`.

In the submit event handler, write a POST request.

```
// PetForm.jsx

state = {
  name: "",
  age: "",
  breed: ""
}

...

handleSubmit = (event) => {
  ...
  fetch(URL/pets, {
    method: "POST",
    ...
    body: JSON.stringify(this.state)
  })
  .then(r => r.json())
  .then(console.log)
}
```

Go to the backend. Update the routes to include a `create` action for pets. Write a `create` action in the pets controller. If we want access to the user that's logged in, we need to write a before action.

```
// PetsController.rb

before_action :authorized, only: [:create]

...

def create
  @pet = Pet.create(params.permit(:name, :age, :breed))
  render json: @pet
end
```

`authorized` is a method from our users controller that checks if a user has a valid token or not.

## Displaying new info without refreshing the page
Write a function that dispatches an action.
* Let's assume that a reducer has already been created in `index.js`.

```
const addOnePet = (createdPet) => {
  return {
    type: "ADD_ONE_PET",
    payload: createdPet
  }
}

let mapDispatchToProps = {
  addOnePet: addOnePet
}
```

Update the fetch request to call this function.
```
.then(r => r.json())
.then((createdPet) => {
  this.props.addOnePet(createdPet)
})
```

## Redux Thunk
Install the Redux Thunk package and import `thunk` in `index.js`. Also, import `applyMiddleware` from Redux.

```
import thunk from 'redux-thunk'
import {createStore, combineReducers, applyMiddleware} from 'redux'
```

`applyMiddleware` is going to become the second argument in the variable that creates the store.

```
let store = createStore(rootReducer, applyMiddleware(thunk))
```

How can we access our Redux dev tools AND thunk in one line?

We need to import another component from Redux.

```
import {createStore, combineReducers, applyMiddleware, compose} from 'redux'

let store = createStore(rootReducer, 
  composeEnhancer(applyMiddleware(thunk))
)
```

Thunk logs all our state changes and actions. Take advantage of the Redux dev tools.

## Benefits of using Redux Thunk
* Handles asynchronous bits of code
* Allows us to move our fetches outside of our component

Example:
Create an action in our pet form that returns a fetch request.

```
// PetForm.jsx

const addOnePet = (createdPet) => {
  return {
    // type, payload
  }
}

const fetchAndAddPet = () => {
  return () => {
    // write a fetch request here
  }
}

```