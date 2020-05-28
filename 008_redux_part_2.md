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