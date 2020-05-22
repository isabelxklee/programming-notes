HTTP requests are stateless. Therefore, we aren't storing any information about logged in users in our server. We need a way to uniquely identify our user in the frontend.

When a user logs in to a site, they get a unique token. So the logged in user persists even if the page is refreshed.

## JWT Token
Takes in a hash, which stores user information, and returns a unique token.

```
{
	"id": 1
}

=> eyasdfklIFJDSLIFKDFJ.slkjeiwf.SKDJKlkdfd
```

In our Rails backend, install the JWT gem.
```
Gemfile

gem 'jwt`
```

Open up the rails console. Encode a new id. Returns a string of letters. Save that string to a new variable.
```
JWT.encode({id: 27}, "abc123")
wristband = _

=> "wejlfjieojijwefSKDJFEIOWFJ.wieoqiuedklfjdslKWEJFKDKJ.xcvxcvSDFJSKDFA"
```
You can also decode the string.
```
JWT.decode(wristband, "abc123")
=> [{"id" => 27}, {"alg" => "HS256"}]
```

## Hide the token
Go to the Application controller in the Rails backend. Create a method for encoding a token.
```
def encode_token(payload)
	JWT.encode(payload, "abc123")
end
```

Go to the User controller. Add the encode token method to the login method. Send out a JSON that has a user object.

```
def login
    ...
    wristband = encode_token({user_id: @user.id})
    render json: {
    user: UserSerializer.new(@user),
    token: wristband
    }
end
```

## Updating the frontend
Go to `App.js` and Update the state to include a token key.
```
state = {
	user: {
    id: "",
    username: ""
    snacks: []
    },
    token: ""
}
```

Then create a helper method that checks if there's a message key or not. Validates if the user was able to log in or not.

```
handleResponse = (r) => {
	if (r.message) {
    	alert(r.message)
    } else {
    	this.setState(r, () => {
        	this.props.history.push("/profile")
        })
    }
}
```
The response is an object and setState() takes in an object. Serializers help us accomplish this. If we don't match our state to our response, we're going to have to play a guessing game to see which state matches which response.

Once the new user object is set, change the URL to `/profile`.

## Creating a new user
Our create method in the Rails backend should use the same serializer logic as logging in a user.

## Restrict the user's access depending on their logged in status

Check to see if the token exists by creating a boolean function. This will validate if a user is logged in or not.

```
App.js

renderProfile = (routerProps) => {
	if (this.state.token) {
        return <ProfileContainer />
    } else if (routerProps.location.pathname === "/register") {
    // do something else
    }
}
```

Can use this logic to conditionally render different nav links.

withRouter gives us access to changing the browser's history.
```
import {withRouter} from "react-router-dom"

...

let magicalComponent = withRouter(...)
```

Save the new cookies to localStorage. Once a response comes back in the frontend, update `handleResponse` method to set localStorage.

```
handleResponse = (r) => {
	if (r.message) {
    	alert(r.message)
        localStorage.token = r.token
    } else {
   		...
    }
}
```

## Create a componentDidMount() method
Any time you want to send a token to the backend, it should be done in the headers of the fetch request. 

GET request
```
componentDidMount() {
	if (localStorage.token) {
    	fetch("http://localhost:4000/users/stay_logged_in", {
            headers: {
                "Authorization": localStorage.token
            }
        })
        .then(r => r.json())
        .then(this.handleResponse)
    }
}
```

Build out the route in `config/routes.rb`
```
get "/users/stay_logged_in"
```

Create a method in the User controller.
```
def stay_logged_in
	wristband = encode_token({user_id: @user.id})
    render json: {
    	user: UserSerializer.new(@user),
        token: wristband
    }
end
```

In the Application controller, create methods for authorization.
```
def logged_in_user
	if decoded_token
    	user_id = decoded_token[0].token
		@user = User.find_by(id: user_id)
    end
end 

def authorized
	...
end
```

Before any methods are executed, check to see if a user's logged in.
```
before_action :authorized, only: [:stay_logged_in]
```