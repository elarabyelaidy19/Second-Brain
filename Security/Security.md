
## Authentication

## JWT
JWT is an open standard defined in RFC 75191. It allows the secure
exchange of tokens between several parties. - Wikipedia 

**JWT token is composed of three parts:**
- a **header** structured in JSON contains, for example, the validity date
of the token.
- a **payload** structured in JSON can contain any data. In our case, it
will contain the identifier of the "connected" user.
- A **signature** allows us to verify that our application has encrypted
the token and is therefore valid.

These three parts are each encoded in ``base64`` and then concatenated
using points (.).

#### Authentication Proccess
- the client request a session resource with credintials. 
- the server return the user resource along with auth token. 
- the client send the auth token for each page that require auth. 

### JWT in Rails
- add user with email and password_digest attributes
- add ``Bcrypt`` gem
- include ``has_secure_password`` in the user model
    - it add a ``password`` and ``password_confirmation`` attributes to the model. 
    - when cretae user it hash the password and sae it in ``password_digest`` 
- add the ``jwt`` gem 
    - it has two methods ``JWT.encode(message, key)`` and ``JWT.decode(decodeed_message, key)`` 

- create a JsonWebToken class under lib folder 
```ruby  
# /lib/JswonWebToken.rb
class JsonWebToken 
    # the secrete key configured in rails
    KEY_SECRETE= Rails.application.credentials.secrete_key_base.to_s
    def self.encode(payload, exp=24.hours.from_now)
        payload[:exp] = exp.to_i
        JWT.encode(payload, KEY_SECRETE)
    end 


    def self.decode(token) 
        # return the payload only
        decoded = JWT.decode(token, KEY_SECRETE).first  
        # retrive the value of hash with string or symbol
        HashWithIndifferentAccess.new decoded
    end 

end  

# /config/application.config
# load the file into our application
config.eager_load_paths << Rails.root.join('lib')
```
- then we should have a controller class to manage the creation and retriveing the token we genetrate. 

```ruby 
#app/controller/api/v1/tokens_controller.rb
class TokensController < ApplicationController

    def create 
        # find the user using email 
        @user = User.find_by_email(user_params[:email]) 
        # if the user is presented and the hashed password of the password we get from param match with the password_digest we store in the database 
        # the authenticate method provided with bcrypt do the matching part
        if @user.&authenticate(user_params[:password]) 
            render json: { 
                token: JsonWebToken.encode(user_id: @user.id), 
                email: @user.email
            }
        else 
            head :unauthorized
        end 

    end 

    private 

    def user_params
        params.require(:user).permit(:email, :password)
    end 
end  

# /config/route.rb

Rails.application.routes.draw do
    namespace :api, defaults: { format: :json } do
        namespace :v1 do
            # ...
            resources :tokens, only: [:create]
        end
    end
end 
```

- until this part we can GET an authentication token if the provided credentials are correct. 
- now we need to find the user that corresponding to a auth token given into the http header. 
- we will create a ``current_user`` method that will find the user that makes the request given auth token. 

```ruby 
#/app/controllers/concerns/authenticable.rb
module Authenticable 

    def current_user 
        return @current_user if @current_user 

        header = request.header['Authorization'] 
        return nil if header.nil?

        decoded = JsonWebToken.decode(header) 
        @current_user = User.find(decoded[:user_id]) rescue ActiveRecord::RecordNotFound
    end 

end 
``` 

- include authenticable in application controller

```ruby 
class ApplicationController < ActionController::API
    include Authenticable
end 

``` 

- implementing logging in to prevent unauthorized access.  
- will add the ``check_owner`` method that will check that the user corresponding to jwt is the same as the user who needs to be updated.

```ruby 
class UserController < ApplicationController 
    #... 

    before_action :check_owner 

    #... 

    private 
    def check_owner 
        head :forbidden if @user.id != current_user.&id
    end 


end

```





















