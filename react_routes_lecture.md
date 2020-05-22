## JavaScript History API

Access the browser inside the console: `window` represents the browser window object.

`window.history` keeps track of where you've visited.

`window.history.back` navigates to the previous page you were on. Same thing as the back button in the browser.

The History API allows us to change the browser URL without firing off a new request / response cycle.

Change the URL without refreshing the page:
```
window.history.pushState({}, "", "[URL]")
```

Frontend routing can make your application more compartmentalized.

## Building a Single Page Application (SPA)
1) Run `npm install react-router-dom` inside your react application.
2) Go to `index.js` and import the new package. 
```
import {BrowserRouter} from 'react-router-dom'
```
3) Wrap the App component with the BrowserRouter component. This gives us the functionality to have URLs in our app.
```
ReactDOM.render(
    <BrowserRouter>
        <App />
    </BrowserRouter>,
document.getElementById('root'))
```
4) Go to App.js and import the Route component. Then render the Route component above the display components.
```
import {Route} from 'react-router-dom'
...
<Route path="/cat" component={Cat} />
<Horton />
<Lorax />
<Thing1 />
<Thing2 />
```
This displays the Cat component at the '/cat' URL, whereas the other display components show up no matter what.

Another way of writing the Route component. Allows us to pass in props.
```
<Route path="/horton">
	<Horton />
</Route>
```

### Route keywords
`<Route exact strict path ="/cat"`
This doesn't allow the user to render a page that has any letters after the given URL.

### Home page URL
Displaying the home page with "/" URl, and no other URL that includes /.
```
<Route path="/" exact component={Home} />

```

### How to load a 404 page
Import the Switch component. It will only show one Route component at a time.
```
import {Route, Switch} from 'react-router-dom'
```
Wrap all the other Route components with the Switch component.
```
<Switch>
	<Route path="/" exact component={Home} />
    ...
    <Route component={NotFound} />
</Switch>
```
URLs that don't exist will automatically render the NotFound component.

Put the Home component and 404 component at the bottom of the other routes.

### Link component
Import the Link component. Acts like an `<a>` tag, but doesn't refresh the page.
```
import {Route, Switch, Link} from 'react-router-dom'
```
    
Render the Link component for each Route component.
```
<ul>
    <li>
    	<NavLink to="/" exact>Home</Link>
    </li>
    <li>
    	<Link to="/cat">The Cat in the Hat</Link>
    </li>
    <li>
    	<Link to="/lorax">Lorax</Link>
    </li>
</ul>
```

## Time to refactor!
Render a component if the path slug matches the Link component.

Use a single Route path to conditionally show all the Dr. Seuss characters. Pass in a helper method that matches the path slug to existing Dr. Seuss' character slugs.
```
decideCharacter = (routerProps) => {
	let URL = routerProps.match.params.slug
    let character = this.state.characters.find((character) => {
    	return character.slug === URL
    })
    if (character) {
    	return <Character name={character.name}>
    } else {
    	return <NotFound />
    }
}
```

Dynamically render the Link components. 
```
render() {
	let componentArray = this.state.characters.map((character) => {
    return
    	<li key={character.id}
            <NavLink to={`/characters/${character.slug}`}>
            </NavLink>
        </li>
    })
	return (
    ...
    <ul>
    	<li>
        	<NavLink to="/" exact >Home</NavLink>
        </li>
        { componentArray }
    </ul>
    ...
    )
}
  
...

<Switch>
	<Route path="/characters/:slug" render={this.decideCharacter} />
    ...
</Switch>
```