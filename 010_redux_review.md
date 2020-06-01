# Review Redux

Send information from the global state to the DOM.

1. Import the `connect` component from React/Redux. We're going to use it in our export default statement. Write an arrow function `mapStateToProps` that has access to the global state. Pass it into the connect component.

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
    username: 
  }
}

export default connect(mapStateToProps)(ProfileContainer)
```