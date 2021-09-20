---
layout: post
title:  "Building API in Rails - Part 3"
date: '2021-09-05'
categories: rails
author: Sajan Basnet
tags: rails programming
toc: false
permalink: '/blogs/rails_api_series/03'
summary: This is the third part of Building Rails API series where we will serialize the API resources.
uniq_heading_id: '#post7'
uniq_body_id: 'post7'
---

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Hi there, welcome to the third part of Building API with Rails series. In this part we are going to serialize our API response. 
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/post7/serialize.png" alt="ruby on rails api serializer"> 

Serialization is the process of translating a data structure or object state into a format that can be stored (for example, in a file or memory data buffer) or transmitted (for example, over a computer network) and reconstructed later. <small>[*Source: Wikipedia*]</small>
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Since we are building API's we will mostly deal with how the ruby objects can be serialized into a string in JSON format and transmitted.



In Ruby on Rails there are different available gems that we can use as a JSON serializer such as
1. <a href="https://github.com/rails/jbuilder" rel="noreferrer" target="_blank">Jbuilder</a>
2. <a href="https://github.com/rails-api/active_model_serializers" rel="noreferrer" target="_blank">Active Model Serializers (AMS)</a>
3. <a href="http://jsonapi-rb.org/" rel="noreferrer" target="_blank">JSONAPI-RB</a>
4. <a href="https://github.com/jsonapi-serializer/jsonapi-serializer" rel="noreferrer" target="_blank">jsonapi-serizlizer</a>
4. and others...

I will be using the **jsonapi-serializer** which is forked from **Fast JSON API**. I am using this particular serializer because its fast and follows the <a href="https://jsonapi.org" rel="noreferrer" target="_blank">JSON:API</a> specifications.
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## But why use an external serializer gem anyway?

In the previous part, we have seen that Rails has its own `render json: User.all` that renders the response in JSON format. And to modify the API response we can use Rails own [as json](https://apidock.com/rails/ActiveModel/Serializers/JSON/as_json) method so what's the point of using an external serializer gem?
 
In short, if we have to modify the API response into a complex format such as **JSON:API** specification then doing it manually will increase complexity, our codebase will look dirty and hard to maintain.
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Integrating jsonapi-serializer (FAST JSON API) 

The first thing we need to do is add the required <a href="https://github.com/jsonapi-serializer/jsonapi-serializer" rel="noreferrer" target="_blank">jsonapi-serializer</a> gem in our Gemfile and run bundle install.

```ruby
gem 'jsonapi-serializer'
```

Then let's go ahead and used the rails generator to generate the serializer for our User.

```ruby
rails g serializer User fullname gender email
```

This will create a new serializer in app/serializers/user_serializer.rb

```ruby
class UserSerializer
  include JSONAPI::Serializer
  attributes :fullname, :gender, :email
end
```

And in our users controller let's change the `def show` api as

```ruby
def show
    @user = User.find(params[:id])
    render json: UserSerializer.new(@user)
end
```

Now we can see the API response format using curl in our terminal. 

```ruby
 curl --location --request POST 'http://localhost:3000/api/v1/users' --header "Content-Type: application/json" \
--data-raw '{"user": {"fullname": "Sajan Basnet", "email": "sajan@gmail.com", "password": "something", "gender": "1"}}'
```

and then 

```ruby
curl --location --request GET 'http://localhost:3000/api/v1/users/1'
```

We will get the response as this

```ruby
{
   "data":{
      "id":"3",
      "type":"user",
      "attributes":{
         "fullname":"Sajan Basnet",
         "gender":1,
         "email":"sajan@gmail.com"
      }
   }
}
```

If we go and run the test for controllers some test cases will fail as the response now comes in JSON:API specification so we need to modify our test cases which will be something like this.

```ruby
require 'rails_helper'

RSpec.describe 'Api::V1::Users', type: :request do
  describe 'GET api/v1/users' do
    it 'returns list of available users' do
      create(:user)
      total_users = User.all.count
      get api_v1_users_path
      expect(response).to have_http_status(200)
      resp = JSON.parse(response.body)
      expect(resp['data'].count).to eq(total_users)
    end
  end

  describe 'GET api/v1/users/:id' do
    it 'returns a user' do
      user = create(:user)
      expected_response = { 'email' => user.email }
      get api_v1_user_path(id: user.id)
      expect(response).to have_http_status(200)
      resp = JSON.parse(response.body)
      expect(resp['data']['attributes']).to include(expected_response)
    end
  end
end

```

*Note: I have only put here the updated test cases that had failed.*
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Another thing I also like to do is install another gem known as <a href="https://github.com/stas/jsonapi.rb" rel="noreferrer" target="_blank">jsonapi.rb</a> which works pretty well with the jsonapi-serializer. Let's add it and do bundle install.

```ruby
gem 'jsonapi.rb'
```

Create an initializers as jsonapi.rb inside **config/initializers/jsonapi.rb** and add this

```ruby
require 'jsonapi'

JSONAPI::Rails.install!
```
Finally, in the users controller let's change all the existing API's and we will have something like this

```ruby
# frozen_string_literal: true

module Api
  module V1
    class UsersController < BaseController
      def index
        render jsonapi: User.all
      end

      def show
        @user = User.find(params[:id])
        render jsonapi: @user
      end

      def create
        user = User.new(user_params)
        if user.save
          render jsonapi: user, status: :created, code: '201'
        else
          render jsonapi_errors: user.errors, 
                 status: :unprocessable_entity, code: '422'
        end
      end

      def update
        user = User.find(params[:id])
        if user.update(user_params)
          render jsonapi: user, status: :ok, code: '200'
        else
          render jsonapi_errors: user.errors, 
                 status: :unprocessable_entity, code: '422'
        end
      end

      def destroy
        user = User.find(params[:id])
        if user.destroy
          render jsonapi: user, status: :ok, code: '200'
        else
          render jsonapi_errors: user.errors, 
                 status: :unprocessable_entity, code: '422'
        end
      end

      private

      def user_params
        params.require(:user).permit(:fullname, :email, :password, :gender)
      end
    end
  end
end

```
All this process may not seem useful right now since we haven't added any associations and also we right now don't have to fetch any complex API response. Once the application grows big this process will certainly be very useful.

Great :tada: :tada: we have come to the end of this chapter. I hope it helped whoever is reading this. 
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">

Thanks for reading till the end we have come to the end of **part 3** of the **Building API with Rails series**. In the upcoming parts, we will be securing our API with **JWT**, adding **SWAGGER** for API documentation, and others.

The code base is available [here](https://github.com/sajanbasnet75/rails_api_series). :beers:

</div>
</div>

<div class="row article-container">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:

<div>
<strong>Related Topic</strong>

  <a href="https://developerblogs.github.io/blogs/rails_api_series/04" rel="noreferrer" target="_blank">**Building API in Rails Part 4** </a> 
</div>
</div>
</div>