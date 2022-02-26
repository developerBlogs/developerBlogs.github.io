---
layout: post
title:  "Building API in Rails - Part 6"
date: '2022-02-26'
categories: blog
author: Sajan Basnet
tags: rails programming
toc: true
permalink: '/blogs/rails_api_series/06'
summary: This is the sixth part of Building Rails API series where we will use rswag to generate a <strong class="text-info">swagger documentation</strong> for our API.
uniq_heading_id: '#post10'
uniq_body_id: 'post10'
til: false
---

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Hi there, welcome to the sixth part of Building API with Rails series. In this part we are going to generate a Swagger documentation for our API by using a gem known as <a href="https://github.com/rswag/rswag" target="_blank" noreferrer>rswag</a>.
<img class= "img-fluid img-thumbnail img-space mb-0" src="{{site.baseurl}}/assets/img/post10/doc.jpg" alt="Swagger doc">
 <p class= "text-center mt-0">
 <a href='https://www.freepik.com/vectors/work' class= "small">Work vector created by storyset - www.freepik.com</a>
</p>
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Adding the `rswag` gem

The first thing we need to do is add the <a href="https://github.com/rswag/rswag" target="_blank" noreferrer>rswag</a> gem along with some other necessary gem and do `bundle install`.

```ruby
# for api documentation
gem 'rswag'
gem 'rswag-api'
gem 'rswag-ui'

group :development, :test do
  gem 'rswag-specs'
end
```
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Setting up `rswag`

Now after we have added the necessary we just need to run the install generator in the terminal.

```ruby
rails g rswag:install
```
This will create some initializers and helper files as

```ruby
1. spec/swagger_helper.rb
2. config/initializers/rswag_api.rb
3. config/initializers/rswag-ui.rb
```
I am going to change the default configuration at `spec/swagger_helper.rb` to something like this

```ruby
  config.swagger_docs = {
    'v1/swagger.json' => {
      swagger: '2.0',
      info: {
        title: 'API V1',
        version: '1.0.0',
        description: 'API Docs'
      },
      paths: {},
      schemes: %w[
        http
        https
      ],
      consumes: ['application/json'],
      servers: [
        {
          url: 'http://{defaultHost}',
          variables: {
            defaultHost: {
              default: 'localhost:3000'
            }
          }
        }
      ]
    }
  }

  ....
  config.swagger_format = :json
```

Also for the initializer `rswag-ui.rb`

```ruby
  ...
  c.swagger_endpoint '/api-docs/v1/swagger.json', 'API V1 Docs'
  ...
```
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Generate `spec` file

In order to generate a swagger documentation we need to write our test cases in rswag-spec format. For this we can generate the spec file easily with a generator.


Let's generate a spec file for our users controller, goto terminal and run

```ruby
rails generate rspec:swagger API::V1::Users_Controller
```

Since we already has a user_spec.rb file it will give us a warning. Just press `y` and enter.
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/post10/force.png"> 

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Updating the `users spec` and making it <strong class="text-success">pass</strong>

The spec needed to generate the Swagger documentation is similar to the rspec just need some parameters defined. The `user_spec.rb` will look something like this now

```ruby
  require 'swagger_helper'

  RSpec.describe 'api/v1/users', type: :request do
    let!(:user1) { create(:user, email: 'test@1.com') }
    let!(:user2) { create(:user, email: 'test@2.com') }
    let!(:total_users) { User.all.count }

    path '/api/v1/users' do
      get('list users') do
        response(200, 'successful') do
          tags 'Users'
          after do |example|
            example.metadata[:response][:content] = {
              'application/json' => {
                example: JSON.parse(response.body, symbolize_names: true)
              }
            }
          end
          run_test! do |response|
            resp = JSON.parse(response.body, symbolize_names: true)
            expect(resp[:data].count).to eq(total_users)
          end
        end
      end

      post('create user') do
        parameter name: :user_params, in: :body, schema: {
          properties: {
            user: {
              type: :object,
              properties: {
                fullname: { type: :string, required: true },
                email: { type: :string, required: true },
                password: { type: :string, required: true }
              }
            }
          }
        }

        response(201, 'successful') do
          tags 'Users'
          request_body = {
            user: {
              fullname: 'New User',
              email: 'new@gmail.com',
              password: 'asdasd12ad'
            }
          }

          let!(:user_params) { request_body }

          after do |example|
            example.metadata[:response][:content] = {
              'application/json' => {
                example: JSON.parse(response.body, symbolize_names: true)
              }
            }
          end
          run_test! do |_response|
            expect(User.all.count).to eq(total_users + 1)
          end
        end
      end
    end

    path '/api/v1/users/{id}' do
      parameter name: 'id', in: :path, type: :string, description: 'User id'

      get('show user') do
        tags 'Users'
        response(200, 'successful') do
          let(:id) { user1.id }

          after do |example|
            example.metadata[:response][:content] = {
              'application/json' => {
                example: JSON.parse(response.body, symbolize_names: true)
              }
            }
          end
          run_test! do |response|
            resp = JSON.parse(response.body, symbolize_names: true)
            expect(resp[:data][:attributes][:email]).to eq(user1.email)
          end
        end
      end

      patch('update user') do
        parameter name: :user_params, in: :body, schema: {
          properties: {
            user: {
              type: :object,
              properties: {
                fullname: { type: :string, required: false }
              }
            }
          }
        }

        response(200, 'successful') do
          tags 'Users'
          request_body = {
            user: {
              fullname: 'Updated User Name'
            }
          }

          let!(:user_params) { request_body }
          let(:id) { user1.id }

          after do |example|
            example.metadata[:response][:content] = {
              'application/json' => {
                example: JSON.parse(response.body, symbolize_names: true)
              }
            }
          end
          run_test! do |_response|
            user1.reload
            expect(user1.fullname).to eq('Updated User Name')
          end
        end
      end

      delete('delete user') do
        response(200, 'successful') do
          tags 'Users'
          let(:id) { user1.id }

          after do |example|
            example.metadata[:response][:content] = {
              'application/json' => {
                example: JSON.parse(response.body, symbolize_names: true)
              }
            }
          end
          run_test! do |_response|
            expect(User.all.count).to eq(total_users - 1)
          end
        end
      end
    end

    path '/api/v1/change_password' do
      parameter name: 'Authorization', in: :header, type: :string

      patch('change_password user') do
        parameter name: :user_params, in: :body, schema: {
          properties: {
            user: {
              type: :object,
              properties: {
                password: { type: :string, required: true }
              }
            }
          }
        }

        response(200, 'successful') do
          tags 'Passwords'
          request_body = {
            user: {
              password: 'updated password'
            }
          }

          let!(:user_params) { request_body }
          let!(:Authorization) { 'Bearer ' + JsonWebToken.encode(user_id: user1.id) }
          after do |example|
            example.metadata[:response][:content] = {
              'application/json' => {
                example: JSON.parse(response.body, symbolize_names: true)
              }
            }
          end
          run_test!
        end
      end
    end
  end
```

Simillary, for `session_spec.rb`

```ruby
  require 'swagger_helper'

  RSpec.describe 'api/v1/sessions', type: :request do
    let!(:user1) { create(:user, email: 'test@1.com') }

    path '/api/v1/login' do
      parameter name: :login_params, in: :body, schema: {
        properties: {
          user: {
            type: :object,
            properties: {
              email: { type: :string, required: true },
              password: { type: :string, required: true }
            }
          }
        }
      }

      post('login session') do
        response(200, 'successful') do
          tags 'Sessions'

          let!(:user1) { create(:user, email: 'test@1.com') }
          request_body = {}

          before do |_example|
            request_body = {
              user: {
                email: user1.email,
                password: user1.password
              }
            }
          end

          let!(:login_params) { request_body }

          after do |example|
            example.metadata[:response][:content] = {
              'application/json' => {
                example: JSON.parse(response.body, symbolize_names: true)
              }
            }
          end
          run_test! do |response|
            resp = JSON.parse(response.body, symbolize_names: true)
            token = JsonWebToken.encode(user_id: user1.id)
            expect(resp[:data][:attributes][:auth_token]).to eq(token)
          end
        end
      end
    end
  end
```

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Generate the Swagger document
Now, the only thing we have left to do is generate the `swagger.json` file. For this also we can just use the genrator.

In the terminal paste this command
```ruby
SWAGGER_DRY_RUN=0 RAILS_ENV=test rails rswag
```

Now if we start the rails server and goto <a href="http://localhost:3000/" rel="noreferrer" target="_blank">**localhost:3000/api-docs** </a> we can see the beautifud Swagger documentation UI.

<img class= "img-fluid img-thumbnail img-space mb-0" src="{{site.baseurl}}/assets/img/post10/ui.png" alt="Swagger doc UI">
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Great :tada: :tada: we have come to the end of the part 6 and thanks for reading till the end.
The code base is available [here](https://github.com/sajanbasnet75/rails_api_series). :beers:

</div>
</div>

<div class="row article-container">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:

<div>
<strong>Related Topic</strong>

  <a href="https://developerblogs.github.io/blogs/rails_api_series/04" rel="noreferrer" target="_blank">**Building API in Rails Part 4** </a>

  <a href="https://developerblogs.github.io/blogs/rails_api_series/05" rel="noreferrer" target="_blank">**Building API in Rails Part 5** </a>
</div>
</div>
</div>