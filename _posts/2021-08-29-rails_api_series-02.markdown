---
layout: post
title:  "Building API in Rails - Part 2"
date: '2021-08-29'
categories: rails
author: Sajan Basnet
tags: rails programming
toc: true
permalink: '/blogs/rails_api_series/02'
summary: This is the second part of Building Rails API series where we will build our first api endpoints while following some TDD.
uniq_heading_id: '#post6'
uniq_body_id: 'post6'
---

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Creating our first API endpoint 

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/programmer.jpg"> 

Let's create our first <a href="https://www.altexsoft.com/blog/rest-api-design/" target="_blank">RESTful API</a> endpoint which will be a CRUD endpoint for the user. Before doing that we need to make sure where our API code should go. I hope you are familiar with the rails MVC pattern and how Rails organizes its codebase. If not go ahead and check <a href="https://hackernoon.com/understanding-your-rails-application-structure-r8w32xj" target="_blank">**this** </a> out first. 
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## API versioning

Initially, the version of our API maybe let's say 'V1' and as our application grows it will have many versions for the API as well(v1, v2, v3....). The clients that use our API may not only use the latest version, what I mean by that is there may be some clients which may only use certain versions of the API let's say v1 even though our latest version is v3. So we must keep in mind the versioning of our API.

Inside the controller folder of our app let's make a new folder as `api` and inside the api folder let's create another folder as `v1`. Now our folder structure looks something like this 

```bash
app/controllers/api/v1
```

So all the API we create will be inside this folder.
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Creating User model

Let's say our **user** consists of attributes like **fullname**, **email**, **gender** and **password**. Go to the terminal and type this to create a user model.

```ruby
rails g model User fullname:string email:string password:string gender:integer
```

This will create all the necessary files such as the migration file, user model file, and test cases files.
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/migration.png"> 
Let's do `rails db:migrate` in the terminal. 

I also like to install a gem called <a href="https://github.com/ctran/annotate_models" target="_blank">**annotate** </a> which is quite useful for reading the Active Record schema.

In the Gemfile add `gem annotate` inside the group development and test and then `bundle install` in the terminal. Now in the terminal, we can do 

```ruby
annotate --models --exclude tests,factories
```

If we go to the `user.rb` model we can see the schema information on the top.
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">

## Creating Users Controller

Before creating any API's controller let's create a controller a `base_controller.rb` first inside the `api/v1` folder which will be inherited from the Application controller.

```ruby
# frozen_string_literal: true

module Api
  module V1
    class BaseController < ApplicationController
    end
  end
end
```

Now again inside the folder `api/v1` let's create a User's controller which will inherit from the Base controller.

So all the rules or code added in the base controller will apply to all our API's controllers globally.
 
In the terminal enter this 

```bash
rails generate controller api/v1/users index show create update delete
```

Our `users_controller.rb` will look like this

```ruby
# frozen_string_literal: true

module Api
  module V1
    class UsersController < BaseController
      def index; end
      def show; end
      def create; end
      def update; end
      def delete; end
    end
  end
end
```
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Setting up Routes

Let's edit the generated `route.rb` file to something like this

```ruby
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :users
    end
  end
end

```

Now if we go to our terminal and hit `rails route` we can see our endpoints url for users. 
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/routes.png">

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Writing Test Cases for User model

Let's write some test cases for the user model first also known as **unit test**. I like using the <a href="https://github.com/thoughtbot/shoulda-matchers" target="_blank">**shoulda_matchers** </a> gem which provides the common test functionality.

In the Gemfile add `gem 'shoulda-matchers', '~> 5.0'`inside test group and hit bundle install, and in the `rails_helpers.rb` add this at the end.

```ruby
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

And in the `user_spec.rb` let's write some test cases

```ruby
require 'rails_helper'

RSpec.describe User, type: :model do
  it { should validate_presence_of(:email) }
  it { should validate_presence_of(:fullname) }
  it { should validate_presence_of(:password) }
  it {should validate_uniqueness_of(:email)}
end
```

Let's run the test cases and check if its pass or fails. In the terminal enter `rspec spec/models`
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/fail.png">
Of course, this test will fail as we haven't added any active record validation in the user model so lets add that

Our `user.rb` model will look something like this now

```ruby
class User < ApplicationRecord
  validates :email, :fullname, :password, presence: true
  validates_uniqueness_of :email
end
```

Let's check the test again with `rspec spec/models`
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/pass.png">

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Writing Test Cases for Users Controller

We have added the test for our user model likewise, let's add the test for our user's controller. If we go and see inside the **spec** folder we will already see a folder as `requests/api/v1` which was auto-generated by rails when we did this

```ruby 
rails generate controller api/v1/users index show create update delete
```

We will be using this request spec for testing our user's controller. As you can see that in the `users_controller.rb` we already have some empty methods which we can also call API and each of them has its unique endpoints.

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">

Let's first write some test cases for our `def index` method which is a GET method to retrieve all the existing users from the system.

What we should expect from the response of this method/api is an [HTTP response status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) of 200 and a list of users. So let's add that in our test case which will look something like this now

```ruby
require 'rails_helper'

RSpec.describe 'Api::V1::Users', type: :request do
  describe 'GET api/v1/users' do
    it 'returns list of available users' do
      create(:user)
      total_users = User.all.count
      get api_v1_users_path
      expect(response).to have_http_status(200)
      expect(JSON.parse(response.body).count).to eq(total_users)
    end
  end
end
```

Above the `create(:user)` is from the `factory_bot_rails` gem that we have installed. 

If we run `rspec spec/requests/api/v1/users_spec.rb` in the terminal then we will see that the test will fail which is obvious cause we currently have an empty method.
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/fail2.png"> 
Let's try to pass this test case by adding code in our def index GET method at `users_controller.rb`

```ruby
def index
  render json: User.all, code: '200', status: :ok
end
```
Here we are sending all the available users in our system as a JSON response. Now let's check the test case again if its passes or fails.
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/pass2.png">  
Noice, our test has passed and we have got our first api :tada: :tada:
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Let's write a test case for the API that creates a new user which will be a **POST** method i.e `def create`

This API is supposed to take the user information as request body params then according to it create a new user and finally give a response with an HTTP code status as **201**. Also if we try to create a user with an existing email in the system then the API should respond with an error message.

So, considering this our test case will look something like

```ruby
RSpec.describe 'Api::V1::Users', type: :request do
  describe 'GET api/v1/users/:id' do
    it 'returns a user' do
      user = create(:user)
      expected_response = { 'email' => user.email }
      get api_v1_user_path(id: user.id)
      expect(response).to have_http_status(200)
      expect(JSON.parse(response.body)).to include(expected_response)
    end
  end

  describe 'POST /api/v1/users/' do
    it 'creates a new user' do
      total_users = User.all.count
      request_body = {
        user: {
          fullname: 'New User',
          email: 'new@gmail.com',
          password: 'asdasd12ad'
        }
      }
      post api_v1_users_path, params: request_body
      expect(response).to have_http_status(201)
      expect(User.all.count).to eq(total_users + 1)
    end
  end
end
```

Our test case like before will obviously fail since we have not added anything to the `def create` method yet. In-order to pass this test case our method now will look something like this

```ruby
def create
    user = User.new(user_params)
    if user.save
        render json: user, status: :created, code: '201'
    else
        render json: user.errors, status: :unprocessable_entity, code: '422'
    end
end

private

def user_params
    params.require(:user).permit(:fullname, :email, :password, :gender)
end
```

Above I have made use of **super params** and now if we run our test it will pass. We can add other examples in the test case as well like if the password is empty, fullname is empty, and so on.

Great 🎉 🎉 we have now another API and we can follow the similar process above to create all the required APIs.
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">


Thanks for reading till the end we have come to the end of **part 2** of the **Building API with Rails series**. In the upcoming parts, we will be adding **active model serializers**, **SWAGGER** for API documentation, and others. 

The code base is available [here](https://github.com/sajanbasnet75/rails_api_series). :beers:

</div>
</div>

<div class="row article-container">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:

<div>
<strong>Related Topic</strong>
  <a href="https://developerblogs.github.io/blogs/rails_api_series/03" target="_blank">**Building API in Rails Part 3** </a> 
</div>
</div>
</div>