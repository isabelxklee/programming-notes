# Getting started

Create a new rails app.
```
rails new [APPNAME] --database=postgresql
```

Set up the backend as an API. Open the backend directory.
 * Uncomment `cors.rb`.
 * Change `origins` to `'*'`.
 * Add `active_model_serializers` and `bcrypt` to the Gemfile.
 * Run `bunlde install`.
 
Create resources. Installing the active model serializer before creating the models will automatically create serializers for each model.
 ```
 rails g resource User username password_digest
 rails g resource Snack name user:belongs_to
 ```
 
Navigate to the newly-created models. Update the model associations.
```
class User < ApplicationRecord
	has_many :snacks
end
```
 
Create some seed data.
```
isabel = User.create(username: "isabel", password: "abc123")
isabel.snacks.create(name: "Chicken Sandwich")
```

Migrate the database and seed the data.
```
rails db:create
rails db:migrate
rails db:seed
```

Navigate to the frontend directory and run `npm start`. Make sure the backend and frontend can communicate with each other.

## Registering a new user
### Backend
Create the appropriate routes in `config.rb`. 

```
config.rb

post '/users', to: 'users#create'
```

Write the model actions in their controllers.
```
class UsersController < ApplicationController
	def create
    @user = User.create(user_params)
    if @user.valid?
      render json: @user, status: :201
    else
      render json: {message: "Failed to create a new user"}, status: :500
    end
  end
    
  private
  
  def user_params
    params.permit(:username, :password)
  end
end
```

Write validations for models.
```
class User < ApplicationRecord
	has_many :snacks
    validates_uniqueness_of :username
    validates :username, presence true
    validates :password, presence true
end
```

### Frontend
Create state that has a user key. The values should mimic the properties that the backend params can receive for that key.

```
App.js 

state = {
	user: {
    id: 0,
    username: "",
    snacks: []
  }
}
```

Make a POST request when a new user is created. Send the information about the user to the backend.

```
App.js 

handleRegistration = (userInfo) => {
	fetch("/users", {
    method: "POST",
      ...
      body: JSON.stringify(userInfo)
    })
  .then(r => r.json())
  .then((response) => {
    if (response.id) {
      this.setState({
          user: response
      })
    }
  })
}
```

### Serializers
Serializers should be written in a linear fashion, never two-ways. The user serializer can call on the snack serializer, but not vice versa. Write a `has_many` association OR a `belongs_to` association.