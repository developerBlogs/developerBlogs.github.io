---
layout: post
title:  "Building API in Rails - Part 2"
date: '2021-08-29'
categories: rails
author: Sajan Basnet
tags: rails programming
toc: false
permalink: '/blogs/rails_api_series/02'
summary: This is the seconf part of Building Rails API series where we will build our first api endpoints while following some TDD.
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

Let's say our **user** consists of attributes like **fullname**, **email**, **gender** and **encrypted_password**. Go to the terminal and type this to create a user model.

```ruby
rails g model User fullname:string email:string encrypted_password:string gender:integer
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
## Creating the Users Controller

Before creating any API's controller let's create a controller a `base_controller.rb` first inside the `api/v1` folder which will be inherited from the Application controller. So all the rules or code added in the base controller will apply to all our API's controllers globally.

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

In the terminal enter this 

```bash
rails generate controller api/v1/users index show create update delete
```

Our `users_controller.rb` will look like this

```ruby
# frozen_string_literal: true

module Api
  module V1
    class UsersController < ApplicationController
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
  it { should validate_presence_of(:full_name) }
  it { should validate_presence_of(:encrypted_password) }
end
```

Let's run the test cases and check if its pass or fails. In the terminal enter `rspec spec/models`
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/fail.png">
Of course, this test will fail as we haven't added any active record validation in the user model so lets add that

Our `user.rb` model will look something like this now

```ruby
class User < ApplicationRecord
  validates :email, :fullname, :encrypted_password, presence: true
end
```

Let's check the test again with `rspec spec/models`
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/pass.png">

</div>
</div>

<div class="row article-container">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:

</div>
</div>