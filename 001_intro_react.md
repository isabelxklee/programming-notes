### Note from Eric
If you can build out the mock application from `reactjs.org/docs/thinking-in-react.html`, you'll be ready for the mod 4 code challenge.

### What is a component?
* Fundamental building block for React applications
* Collection of HTML
* Example: Experiences component on Airbnb (image, h1, p)
* Components can be functions or classes

## Building a React app
* Download React `npm i -g create-react-app`
* Create a new React app `create-react-app [APPNAME]`
* Navigate to the new directory `cd [APPNAME]`

### How to create HTML elements in `index.js`
Better to do it this way...
```
let bawkBawk = <h1>Bawk bawk</h1>
ReactDOM.render(bawkBawk, document.getElementById('root'))
```

...than this way.
```
let chickenObj = React.createElement("h1", {id: "chicken"}, "Bawk bawk")
```

### Declarative programming vs. Imperative programming
It's not efficient to use imperative programming because you have to tell the program exactly what to do to make it work.

### Semantic HTML
Try to use fragments instead of divs, to avoid creating too many nested divs.
```
function App() {
    return (
        <>
        	<h1>Hello, world!</h1>
        </>
    )
}
```
If you use divs:
```
...
        <div>
            <h1>Hello, world!</h1>
        </div>
	)
}
    
```

## Using JSX
Create a .js or .jsx file that contains a specific function.
```
ListContainer.jsx
```
Include this line at the top of the file:
```
import React from 'react'
```
Write a function:
```
let ListContainer = () => {
	return(
    	<>
        	[HTML ELEMENTS]
        </>
    )
}
```

At the end of the file, write:
```
export default ListContainer
```
Then go back to `App.js` and call on that function. Otherwise, `ListContainer` will not return anything.
```
App.js

import React from 'react'
import ListContainer from './ListContainer'

function App() {
	return (
    	<h1>Hello, world!</h1>
    	<ListContainer />
    )
}

export default App
```