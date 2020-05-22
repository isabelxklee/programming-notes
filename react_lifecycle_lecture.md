## Class component lifecycle
### Render Phase
* `constructor()` controls the JavaScript
* `render()` controls the HTML

### Commit Phase
* `componentDidMount()` is a method that will automatically fire once it's been mounted to the HTML
* When the component gets slapped on the DOM, the body of the `componentDidMount()` gets executed
* Must be written inside a class component
* Helps the frontend and backend communicate with each other
* Great place to make asynchronous fetch requests, but doesn't have to be the only place
* Inside the state: make sure that there's an empty representation of new data that's going to be populated from the fetch request
```
state = {
	masterList: []
}
```
* If there's no empty representation or array, it will return an error saying that the object is undefined
* `render()` will always run before `componentDidMount()`
* Example of fetch request:
```
componentdidMount() {
	fetch(URL)
    .then(r => r.json())
    .then((allLists) => {
        this.setState({
            masterList: allLists
        })
    })
}
```

## Build a dynamic search bar
Let's see how we can handle events inside a functional component.
* Create a new file just for the search bar component

```
const SearchBar = () => {
	return (
        <div className="searchbar">
        <input type="text" name="searchTerm">
        </div>
    )
}

```

### Controlled components
Even though this input isn't part of a form, it should still be a controlled component. Because state is somehow controlled the input and vice versa.
State is the single source of truth for our application.

```
const SearchBar = () => {
	...
    <input
        type="text"
        name="searchTerm"
        value={}
        onChange={}
    />
}
```
* Inputs always use `onChange` to document the user's input.
* State controls the input through the `value` key
* Input controls state through the `onChange` key
* Having one over the other will not create a fully controlled component
* Issue: We can't have state inside the search bar component, but it doesn't matter because what we type in the search bar is going to affect the list component. Therefore, the communication has to be done through the App component because sibling components cannot communicate with each other.

```
class App extends Component {
    state = {
        masterList: [],
        searchTerm: "Oscar"
	}
}

render() {
	<SearchBar searchTerm = {this.state.searchTerm}
}
```
The keyword `this` cannot be accessed inside a functional component.

```
const SearchBar = () => {
	...
    <input
        type="text"
        name="searchTerm"
        value={props.searchTerm}
        onChange={}
    />
}
```
Find a way to update the search term from `App.js`. Create an event listener for it.
```
handleSearchTerm = (termFromChild) => {
	console.log(termFromChild)
    this.setState({
    	searchTerm: termFromChild
    })
}
```
Create an event listener inside `SearchBar.js`.
```
const handleWhichInfoToPassUp = (event) => {
	props.handleSearchTerm(event.target.value)
}
```

## Questions
* What's the difference a between functional component and a class component?
```
// functional component
let list = () => {}

// class component
class List extends Component {}
```
* Why does `render()` run before commit methods?