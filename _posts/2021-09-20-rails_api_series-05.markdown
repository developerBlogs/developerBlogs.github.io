---
layout: post
title:  "Building API in Rails - Part 5"
date: '2021-09-20'
categories: rails
author: Sajan Basnet
tags: rails programming
toc: true
permalink: '/blogs/rails_api_series/05'
summary: This is the fifth part of Building Rails API series where we will secure our API using JWT.
uniq_heading_id: '#post9'
uniq_body_id: 'post9'
til: false
---

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Hi there, welcome to the fifth part of Building API with Rails series. In this part we are going to secure our API application by adding token based authentication for which we will be using <a href="https://jwt.io/" target="_blank" noreferrer>JWT</a>
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/post9/jwt.gif" alt="jwt in ruby on rails">
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Adding the `jwt` gem

The first thing we need to do is add the <a href="https://github.com/jwt/ruby-jwt" target="_blank" noreferrer>jwt</a> gem and do `bundle install`

```ruby
# for authentication
gem 'jwt'
```
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Adding a `secret key`

In order to decode and encode the `jwt` we will be needing an secret or private key. We can easily generate a random secret key in the terminal by using `rails secret`

```ruby
rails secret
```
Copy the generated secret key and add it to the `.env` file.

```ruby
JWT_SECRET_KEY='paste_your_secret_key'
```
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Adding the decode and encode functionality

Let's create a file called `json_web_token.rb` in our model's directory.

In this file, we need to create a class called as `JsonWebToken` and inside the class, we need to add two class methods one for encoding the JWT and another for decoding it. After adding it our file will look like this.

```ruby
# frozen_string_literal: true

# class for authentication with jwt
class JsonWebToken
  JWT_SECRET_KEY = ENV['JWT_SECRET_KEY']
  class << self
    def encode(payload)
      expiration = 7.days.from_now.to_i
      JWT.encode(payload.merge(exp: expiration), JWT_SECRET_KEY, 'HS256')
    end

    def decode(token)
      JWT.decode(token, JWT_SECRET_KEY, true, algorithm: 'HS256').first
    end
  end
end
```

In order to encode and decode the JWT, we have used the jwt secret key that we had added in the `.env` file. Also, we have added the expiration time as 7 days from the date of creation of the JWT.
</div>
</div>


<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Adding the `sessions controller`

We need a route for users to login in our application. We can create a sessions controller and add the login actions inside the controller.

```ruby
# frozen_string_literal: true

module Api
  module V1
    class SessionsController < BaseController
      # POST /api/v1/login
      def login
      end
    end
  end
end

```

And also we need to add the routes as well.

```ruby
# frozen_string_literal: true

Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :users
      post 'login', to: 'sessions#login'
    end
  end
end
```
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Adding test cases
Let's write the test case for the sessions controller and make it pass. Inside the `request/api/v1` folder we need a file named as `sessions_spec.rb`. For easily creating it we can use the rails generator

```ruby
rails g rspec:request api/v1/sessions
```
Now, when the user login to our application with their correct credentials then the test should expect a 200 status. Let's add that to our test case and try to pass it.

```ruby
require 'rails_helper'

RSpec.describe 'Api::V1::Sessions', type: :request do
  describe 'POST /api/v1/sessions' do
    it 'lets user login with correct credentials' do
      user = create(:user)
      request_body = {
        user: { email: user.email, password: user.password }
      }
      post api_v1_login_path, params: request_body
      expect(response).to have_http_status(200)
    end
  end
end
```

Since we haven't added anything in our login action the test will fail if we run it. So lets add some code that checks the users email and password. You can also add negative test cases as well i.e for invalid email and passwords.

```ruby
# frozen_string_literal: true

module Api
  module V1
    class SessionsController < BaseController
      # POST /api/v1/login
      def login
        user = User.find_by(email: login_params[:email])
        if user.present? && user.authenticate(login_params[:password])
          render jsonapi: user, status: :ok, code: '200'
        else
          render jsonapi_errors: [{ title: 'Invalid Email or Password' }],
                 code: '401', status: :unauthorized
        end
      end

      private
        def login_params
          params.require(:user).permit(:email, :password)
        end
    end
  end
end
```

Here I am using the bcrypt authenticate method to authenticate the password. Now if we go and run the test it will pass :tada: :tada:
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Sending the encoded JWT in the reponse

In the above user login response we need to send the encoded JWT as well. So let's use the encode class method that we had created in `json_web_token.rb`. To encode the token we can simply use this
```ruby
token = JsonWebToken.encode(user_id: user.id)
```
To send this as a response we can add an attribute as `auth_token` in the user serializer
```ruby
class UserSerializer
  include JSONAPI::Serializer
  attributes :fullname, :gender, :email

  attribute :auth_token do |user, params|
    params[:auth_token]
  end
end
```

Finally, we need to change the `login` action in the `sessions_controller.rb` which will look something like this now

```ruby
# POST /api/v1/login
def login
  user = User.find_by(email: login_params[:email])
  if user.present? && user.authenticate(login_params[:password])
    token = JsonWebToken.encode(user_id: user.id)
    render jsonapi: user, params: { auth_token: token },
            status: :ok, code: '200'
  else
    render jsonapi_errors: [{ title: 'Invalid Email or Password' }],
            code: '401', status: :unauthorized
  end
end
```

Let's add another example in test case that expects an auth token after users login.

```ruby
  it 'expects auth token in response' do
    user = create(:user)
    request_body = {
      user: { email: user.email, password: user.password }
    }
    post api_v1_login_path, params: request_body
    result = JSON.parse(response.body)
    token = JsonWebToken.encode(user_id: user.id)
    expect(result["data"]["attributes"]["auth_token"]).to eq(token)
    expect(response).to have_http_status(200)
  end
```

If we run the test cases it will pass :tada: :tada:
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Authenticating the request with `jwt`
Great we have added the `jwt` successfully and send back the `auth token` as a response. But we are still not done yet. We need to add a functionality that authenticate the users who request for the protected API resources.

Let's add a method to change users password in `users_controller.rb`

```ruby
# UPDATE api/v1/change_password
def change_password; end
```

Add the routes as well
```ruby
patch 'change_password', to: 'users#change_password'
```
Obiviously, before user changes their password we must authenticate the request first and then only allow them to change it. Let's add a action callback method `authenticate_request!` that will be called before the `change_password` action.

```ruby
# frozen_string_literal: true

module Api
  module V1
    class UsersController < BaseController
      before_action :authenticate_request!, only: [:change_password]
      ...
```

Add the code for authenticating the user in our parent class `base_controller.rb`

```ruby
# frozen_string_literal: true

module Api
  module V1
    class BaseController < ApplicationController

      def authenticate_request!
        @decoded = JsonWebToken.decode(auth_token).deep_symbolize_keys
        set_current_user
      rescue JWT::ExpiredSignature
        render jsonapi_errors: [{ title: e.message }], code: '401', status: :unauthorized
      rescue JWT::DecodeError => e
        render jsonapi_errors: [{ title: e.message }], code: '401', status: :unauthorized
      end

      def set_current_user
        if @decoded[:user_id].present?
          @current_user = User.find(@decoded[:user_id])
        end
      end

      def auth_token
        @auth_token ||= request.headers.fetch('Authorization', '').split(' ').last
      end
    end
  end
end
```

The first thing we have done here is fetch the `auth_token` from the request headers that is sent by the client.
```ruby
Request Headers
Authorization: Bearer <token>
```
After that we have decode the `auth token` with the `def decode` action that we had created in `json_web_token.rb`. While decoding if the token is invalid or expired it will raise an exception according to it, else it will set a `current_user` and goes to the API action for further process.

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Add test case for `change_password`

We have added the action `change_password` and then `before action callback` to authenticate the user request. Let's check if it works by adding a test case for it and passing it.

In the same `users_spec.rb` we can add an example like this
```ruby
  ...
  describe 'PATCH api/v1/change_password' do
    it 'changes user password' do
      user = create(:user)
      request_body = { user: {password: 'new_password'} }
      token = JsonWebToken.encode(user_id: user.id)
      patch api_v1_change_password_path, params: request_body, headers: {'Authorization': token}
      expect(response).to have_http_status(200)
    end
  end
  ...
```

Then to pass it we can update the `change_password` action as
```ruby
  ...
  # UPDATE api/v1/change_password
  def change_password
    if params[:user][:password] && @current_user.update(password: params[:user][:password])
      render jsonapi: @current_user, status: :ok, code: '200'
    else
      render jsonapi_errors: @current_user.errors, status: :unprocessable_entity, code: '422'
    end
  end
  ...
```

Simillary, lets add a negative test case with invalid auth token.
```ruby
  ...
  it 'doenot change password for invalid token' do
    user = create(:user)
    request_body = { user: {password: 'new_password'} }
    token = JsonWebToken.encode(user_id: user.id)
    token = '123213'
    patch api_v1_change_password_path, params: request_body, headers: {'Authorization': token}
    expect(response).to have_http_status(401)
  end
  ...
```
If we run the test case it will pass. Nice ​:muscle: we are finally done with addding jwt in our application and using it to authenticate a user.

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Great :tada: :tada: we have come to the end of the part 5 and thanks for reading till the end.
The code base is available [here](https://github.com/sajanbasnet75/rails_api_series). :beers:

</div>
</div>

<div class="row article-container">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:

<div>
<strong>Related Topic</strong>

  <a href="https://developerblogs.github.io/blogs/rails_api_series/03" rel="noreferrer" target="_blank">**Building API in Rails Part 3** </a>

  <a href="https://developerblogs.github.io/blogs/rails_api_series/04" rel="noreferrer" target="_blank">**Building API in Rails Part 4** </a>
</div>
</div>
</div>