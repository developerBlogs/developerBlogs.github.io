



# Integrating Third Party SignIn in Rails Application

![login](/home/sajan/Desktop/login.jpg)

 Most of the websites now have a third party login such as Facebook or Google login besides just a simple login form. All this third parties they generally follow the same protocol which is **OAuth 2.0** for the authorization and **OpenId Connect** for authentication. If you haven't heard this protocol before I sugguest you to read this topic  **[Understanding Oauth 2.0 and OpenId Connect]({% link _posts/2020-07-26-understanding_oauth2.0_and_openId_connect.markdown%})**. 



We will be integrating  two third party SignIn options in our rails application and for this we will be using the re-known gem **devise**.

1. **Google SignIn**
2. **FaceBook SignIn**



## Setting Up Our Rails Application

Obviously the first step will be  to set up our rails application. If you have already set it up and just looking for integration then move to next step else read this topic **[Quick React On Rails 6 SetUp]({% link _posts/2020-07-22-react-on-rails6.markdown%})**



## Creating Application in Third Party Website

In order to integrate the third parties sing in feature in our application we need to first register an application in their website.  If you haven't register an application I suggest you to follow this   **[Creating an Application On Third Parties Websites]({% link _posts/2020-07-22-react-on-rails6.markdown%})** before integration process.



## Installing Devise for authentication

 As i told before we will be using the well known **devise** gem and configuring it. Lets go ahead and add this gem to our gemfile.

```ruby
gem 'devise', '~> 4.7'
```

Hit bundle install in the terminal. After that we need to now configure the devise in our application. Goto the terminal and run the devise install command.

```bash
$ rails generate devise:install
Running via Spring preloader in process 26493
      create  config/initializers/devise.rb
      create  config/locales/devise.en.yml
===============================================================================

```

Goto development.rb and add this configuration to set the default url for our action mailer for development environment.

```ruby
config/environments/development.rb

# frozen_string_literal: true

Rails.application.configure do
  # ...
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
  # ...
end
```



## Creating User Model Using Devise

Since we have install devise and configured it now we need to generate a user model using this deivse gem. Goto terminal and run

```bash
$ rails generate devise User

  invoke  active_record
  create    db/migrate/20200629151857_devise_create_users.rb
  create    app/models/user.rb
  invoke    rspec
  create      spec/models/user_spec.rb
  invoke      factory_bot
  create        spec/factories/users.rb
  insert    app/models/user.rb
   route  devise_for :users
```

We can see that this command generated  a user model along with spec and factory files. Also a migration file is created which will create a user table. If we look our **routes.rb** we can also see **devise_for:users** line which adds the default routes for the action in devise such as sign in , sign out and others.



Lets go ahead and run the migration to create a user table.

```bash
$ bundle exec rails db:migrate

== 20200629152903 DeviseCreateUsers: migrating ================================
-- create_table(:users)
   -> 0.0156s
-- add_index(:users, :email, {:unique=>true})
   -> 0.0116s
-- add_index(:users, :reset_password_token, {:unique=>true})
   -> 0.0025s
== 20200629152903 DeviseCreateUsers: migrated (0.0298s) =======================
```

Now our user model and table are ready to be used. Lets first create the views of our login page. We can easily generate this views through the devise gem as well. Before that lets add one gem first in our gemfile and hit bundle install

```ruby
gem 'devise-bootstrap-views', '~> 1.0'
```

 This gem is basically used for getting a responsive and good looking devise generated views. Lets goto terminal and generate the views now.

```
$ rails generate devise:views:bootstrap_templates
```



## Adding Necessary Gems

Lets add the necessary gems needed for the third party signin.

| OAuth Service Provider |             gem              |                          Source                           |
| :--------------------: | :--------------------------: | :-------------------------------------------------------: |
|        OmniAuth        |        gem 'omniauth'        |       [Link](https://github.com/omniauth/omniauth)        |
|         Google         | gem 'omniauth-google-oauth2' | [Link](https://github.com/zquestz/omniauth-google-oauth2) |
|        Facebook        |   gem 'omniauth-facebook'    |     [Link](https://github.com/simi/omniauth-facebook)     |

We need to add this in our gemfile and run **bundle install**.



## Adding our Client Id and Secret

In **step 2** we registered applications in the third parties website and each of them gave us a **client id** and **client secret**. So, we need to store this in somewhere in our rails application. For this lets first install a gem called **gem 'dotenv-rails'** and create a **.env** file in the root folder of our application. Don't forget to add this **.env** file in the **gitignore**. In our .env file lets setup our environment variables and add all the credentials we got.

```ruby
.env

GOOGLE_CLIENT_ID=google-client-id
GOOGLE_CLIENT_SECRET=google-client-secret 

FACEBOOK_CLIENT_ID=facebook-app-id
FACEBOOK_CLIENT_SECRET=facebook-app-secret 

```



## Configuring Devise To Use The Environment Variables

We need to configure the devise to use the environment variables that we added above so lets do that.  In our `config/initializers/devise.rb`  lets add the following 

```ruby
Devise.setup do |config|
  ---
  # OmniAuth providers
    
  config.omniauth :facebook, ENV['FACEBOOK_CLIENT_ID'], ENV['FACEBOOK_CLIENT_SECRET'], scope: 'public_profile, email'
  config.omniauth :google_oauth2, ENV['GOOGLE_CLIENT_ID'], ENV['GOOGLE_CLIENT_SECRET'], scope: 'email, profile, calender'
  ---
end
```

We can add other omniauth provider such as **twitter** as well with this same method.

```ruby
config.omniauth :twitter, ENV['TWITTER_CLIENT_ID'], ENV['TWITTER_CLIENT_SECRET']
```

 We can also provide the additional scope provider like above but its optional. Adding this scope parameter can specify what the application is authorized to access of the end users.



## Creating Migration to Update Users Table

In our user table we need to have additional fields such as **provider**,  **uid** to  uniquely identify the user. So we need to create a migration for that and run rake db:migrate.

```bash
$ rails g migration AddOmniauthToUsers provider:string uid:string
```

```bash
$ bundle exec rake db:migrate
== 20200804093916 AddOmniauthToUsers: migrating ===============================
-- add_column(:users, :provider, :string)
   -> 0.0067s
-- add_column(:users, :uid, :string)
   -> 0.0013s
== 20200804093916 AddOmniauthToUsers: migrated (0.0082s) ======================


```



## Adding Omniauthable Module in Devise 

We need to add an additional modules as `:omniauthable` in devise to support the omniauth.  Lets add this inside out **User.rb** file.

```ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable,
         :omniauthable, omniauth_providers: [:facebook, :google_oauth2]
end

```

Similarly if we have other omniauth providers such as twitter or github we can add it in the omniauth_providers array, but the name should be same as that of the gem file.



## Setting Up Omniauth Callbacks Controller

When our application users clicks the **sign in with google** or **facebook** and they are redirected to the authorization server and a consent screen is provided to authorize our application. When user authorizes our application then they gets redirected back to our application through the redirect_uri.

We now need controller to handle this redirects from the third party. So, lets create a controller for this.

```bash
$ rails g controller users/omniauth_callbacks

      create  app/controllers/users/omniauth_callbacks_controller.rb
      invoke  erb
      create    app/views/users/omniauth_callbacks
      invoke  rspec
      create    spec/requests/users/omniauth_callbacks_request_spec.rb
      invoke  helper
      create    app/helpers/users/omniauth_callbacks_helper.rb
      invoke    rspec
      create      spec/helpers/users/omniauth_callbacks_helper_spec.rb
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/users/omniauth_callbacks.scss

```

Now lets add the method that will handle the callback for each third party respectively.

```ruby
class Users::OmniauthCallbacksController < ApplicationController
  # facebook callback
  def facebook
    @user = User.from_omniauth(auth)
    if @user.save
      session[:user_id] = @user.id
      redirect_to controller: 'pages', action: 'home'
    else
      redirect_to new_user_registration_url
    end
  end

  def google_oauth2
    @user = User.from_omniauth(auth)
    if @user.save
      session[:user_id] = @user.id
      redirect_to controller: 'pages', action: 'home'
    else
      redirect_to new_user_registration_url
    end
  end

  def auth
    request.env['omniauth.auth']
  end
end

```



## Setting Up User Model

We have called a method `User.from_omniauth(auth)` in each of our methods in **omniauth_callbacks controller**. So lets create that method in our user controller which will handle the user creation in our application. Open `user.rb` and add this method

```ruby
  def self.from_omniauth(auth)
    where(email: auth.info.email, 
          provider: auth.provider, 
          uid: auth.uid).first_or_initialize do |user|
      user.email = auth.info.email
      user.password = SecureRandom.hex
    end
 end
```



## Setting Up Routes 

```ruby
Rails.application.routes.draw do
  devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }
  # authenticated :user do
  #   root "pages#user_profile", as: :authenticated_root
  # end
  root to: 'pages#home'
  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
end

```

