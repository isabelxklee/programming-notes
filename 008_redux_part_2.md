# Intro to Redux Part 2

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

Pass in the Provider component to `React.DOM.render()`.
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