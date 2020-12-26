# Devise in Rails 6 Application

Most of the website usually needs an authentication system so that user can securely register, login and logout. Besides that there can be other features needed such as password recovery or reset , inviting users through mail, confirmation  and verification and so on . Instead of building all this from scratch we can make use of one of the popular gem 'devise' for this. 

So, in this topic we are going implement devise in our rails application for the authentication. But before that its better if you look at the **[Devise Docs](http://devise.plataformatec.com.br/#controller-filters-and-helpers)** and check out all the modules, utilities and its recommendation to understand the basic authentication strategy.

## Setting Up Rails Application 

Obviously the first thing we need to have is a rails application and I hope you have already set it up. If not you can follow this topic **[Ouick React On Rails 6 SetUp ](link)**for setting it up which I will be using as well for this tutorial. Remove the `--webpack=react` if react is not required.



## Installing and Configuring Devise 

1. Add the gem `gem 'devise', '~> 4.7'` in the  Gemfile.

2. Run bundle install

3. Execute the devise install command 

   ```ruby
    $rails g devise:install
   
     create  config/initializers/devise.rb
     create  config/locales/devise.en.yml
   ```

4.  Set up the default URL options for the Devise mailer in each environment

   ```ruby
   config/environments/development.rb
   
   # frozen_string_literal: true
   
   Rails.application.configure do
     # ...
     config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
     # ...
   end
   ```

   Remember to configure for production and staging environment as well if required

5. Generate a User model by using the devise. 

   ```
   $rails g devise User
   
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

   After creating the User model with the devise we will see that our **routes.rb**  also is upadted with a devise_for :users line. This line adds routes for User resource that provides sign in, sign out, update password, and CRUD operations. in the user model we can see that different devise modules and some are enabled by default by devise.

6. Run Migration with `rake db:migrate`

7. Generate Views by using devise

   ```bash
$ rails generate devise:views
   ```

8. Start rails server with `rails s` and go to http://localhost:3000/users/sign_in

   We can see the login form and other links such as sign up and forgot password.



## Adding Bootstrap, FontAwesome,  JQuery and Toastr.

Lets install font-awesome, bootstrap and jquery in our application with yarn. If you already have it installed skip this step else go to terminal and run

```bash
$ yarn add @fortawesome/fontawesome-free bootstrap jquery toastr
```

We can check **package.json** file to make sure that all the dependences above are installed. 

After that we need to Import these dependencies into our application. Open the file  **javascript > packs>application.js** and add these code.

```javascript
//application.js 

require("jquery")
require('bootstrap')
global.toastr = require("toastr")

import 'bootstrap/dist/css/bootstrap'
import "@fortawesome/fontawesome-free/js/all";
import "@fortawesome/fontawesome-free/css/all.css";
import "../stylesheets/application"
```

 

For jquey yo work open the `environment.js` file and add this

```javascript
//environment.js

const { environment } = require('@rails/webpacker')
const webpack = require('webpack')

environment.plugins.prepend(
  "Provide",
  new webpack.ProvidePlugin({
    $: "jquery",
    jQuery: "jquery",
    jquery: "jquery",
    "window.Tether": "tether",
    Popper: ["popper.js", "default"] // for Bootstrap 4i
  })
);
module.exports = environment
```

### 2. Adding the views navigation bar, footer and flash message partials.

Before we start adding users to our model lets first tweak some designs by adding a navigation bar, flash message and a footer in our application. Go to the **views** folder and add a folder as **includes**. Inside the includes folder create a three partial templates as **_navbar.html.erb**  ,**_footer.html.erb** and **_flash.html.erb**.

Copy these code as fallows in those files.

1. **For navbar partials (_navbasr.html.erb)**

   ```html
   # nabbar.html.erb
   
   <nav class="navbar navbar-expand-sm navbar-dark" style="background-color: black;">
     <div class="container">
       <b><%= link_to "My Rails App", root_path, class: "navbar-brand" %></b>
       <button class="navbar-toggler d-lg-none" type="button" data-toggle="collapse" data-target="#collapsibleNavId"
         aria-controls="collapsibleNavId" aria-expanded="false" aria-label="Toggle navigation">
         <span class="navbar-toggler-icon"></span>
       </button>
       <div class="collapse navbar-collapse justify-content-end" id="collapsibleNavId">
         <ul class="navbar-nav">
           <li class="nav-item">
             <% if user_signed_in? %>
             <%= link_to('Logout', destroy_user_session_path, method: :delete, class: "nav-link text-white") %>
             <% else %>
             <%= link_to('Login', new_user_session_path, {class: "nav-link text-white login-btn"}) %>
             <% end %>
           </li>
         </ul>
       </div>
     </div>
   </nav>
   ```

   

2. **For flash message partial (_flash.html.erb)**

   ```html
   #flash.html.erb
   
   <% flash.each do |key, value| %>
   <div class="container mt-2">
     <div class="alert <%= key == 'notice' ? 'alert-primary' : 'alert-danger' %>" role="alert">
       <%= value %>
       <button type="button" class="close" data-dismiss="alert" aria-label="Close">
         <span aria-hidden="true">&times;</span>
       </button>
     </div>
   </div>
   <% end %>
   ```

   

3. **For footer partial (_footer.html.erb)**

   ```
   <footer class="myfooter">
     <p class="text-center text-white pt-3">Copyright <i class="fa fa-copyright" aria-hidden="true"></i> Your Name</p>
   </footer>
   ```

   

4. Create user_form.scss and footer.scss files inside **assets>stylesheets**.

   ```scss
   # user_form.scss
   
   .email-input,
   .pwd-input,
   .login-btn {
     border-radius: 100px !important;
     max-width: 100%;
   }
   
   ```

   ```scss
   # footer.scss
   
   .myfooter {
     position: absolute;
     width: 100%;
     height: 60px;
     bottom: 0;
     background-color: black;
   }
   ```



## Client-Side Validation with Parsley Js

[Parsley](https://parsleyjs.org/doc/index.html) is a javascript form validation library. It helps to provide the users with feedback on their form submission before sending it to your server. Lets go ahead and install this. I tried using **yarn add parsley.js** and and required it in the application.js file. And similarly like adding Jquery in the **environment.js** file i added parsley as well. But for some reason when I run **$('form').parsley().validate()** it was not working. So, eventually I use their cdn :weary:. If any of you were able to add this through yarn  with no any sort of errors please ping me in the comments.

In the `application.html.erb` add the cdn inside the head tag. So, our file will look like this 

```html
<!DOCTYPE html>
<html>

<head>
  <title>ReactRailsCrudApp</title>
  <%= csrf_meta_tags %>
  <%= csp_meta_tag %>

  <%= stylesheet_link_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>
  <%= javascript_pack_tag 'application', 'data-turbolinks-track': 'reload' %>
  <script src="http://parsleyjs.org/dist/parsley.min.js"></script>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>

<body>
  <%= render "includes/navbar" %>
  <%= render "includes/flash" %>
  <div class="container">
    <%= yield %>
  </div>
  <%= render "includes/footer" %>
</body>

</html>
```

Lets create  a custom JavaScript file inside the `javascript > packs > user_form.js`.

```javascript
// user_form.js

function validate_form() {
  $('form').parsley().validate();
}

document.addEventListener('turbolinks:load', () => {
  const clickButton = document.getElementById("submit-btn");
  if (clickButton) {
    clickButton.addEventListener("click", validate_form);
  }
});
```

In the application.js file require this custom js file as

```javascript
//application.js
...
require("packs/user_form")
...
```



## Editing Devise Form Views

Replace the code as below for the devise form views.

1. **devise > sessions> new.html.erb**

   ```html
   <div class="container">
     <div class="row">
       <div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
         <div class="card card-signin my-5">
           <div class="card-body pr-5 pl-5">
             <h4 class="card-title text-center pb-2 mb-4">Welcome Back!</h4>
             <div class="form-signin">
               <%= form_for(resource, as: resource_name, url: session_path(resource_name), html: {'data-parsley-validate' => true, novalidate: true} ) do |f| %>
               <%= bootstrap_devise_error_messages! %>
               <div class="form-group ">
                 <i class="fa fa-user icon position-absolute m-2 ml-4 text-primary d-none d-sm-block"></i>
                 <%= f.email_field :email, class: 'form-control email-input text-center', placeholder: "Email address", required: true,  'data-parsley-trigger' => 'change'  %>
               </div>
   
               <div class="form-group ">
                 <i class="fa fa-key icon position-absolute m-2 ml-4 text-primary d-none d-sm-block"></i>
                 <%= f.password_field :password, class: 'form-control pwd-input text-center', placeholder: 'Password', required: true %>
               </div>
   
               <% if devise_mapping.rememberable? %>
               <div class="form-group form-check">
                 <%= f.check_box :remember_me, class: 'form-check-input' %>
                 <%= f.label :remember_me, class: 'form-check-label' do %>
                 <%= resource.class.human_attribute_name('remember_me') %>
                 <% end %>
               </div>
               <% end %>
   
               <div class="form-group text-center mt-4">
                 <%= f.submit  t('.sign_in'), class: 'btn btn-primary login-btn btn-block', id:'submit-btn' %>
               </div>
               <% end %>
               <div class="form-group text-center small">
                 <%- if devise_mapping.recoverable? && controller_name != 'passwords' && controller_name != 'registrations' %>
                 <%= link_to t(".forgot_your_password?"), new_password_path(resource_name) %><br />
                 <% end -%>
               </div>
               <hr>
               <br>
               <div class="form-group text-center">
                 <%- if devise_mapping.registerable? && controller_name != 'registrations' %>
                 <p>Dont have a account?
                   <strong><%= link_to t(".sign_up"), new_registration_path(resource_name) %></strong>
                 </p>
                 <% end -%>
               </div>
             </div>
           </div>
         </div>
       </div>
     </div>
   </div>
   ```

2. **devise > registrations > new.html.erb**

   ```html
   <div class="container">
     <div class="row">
       <div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
         <div class="card card-signin my-5">
           <div class="card-body pr-5 pl-5">
             <h4 class="card-title text-center pb-2 mb-4">Create an Account</h4>
             <div class="form-signup">
               <%= form_for(resource, as: resource_name, url: registration_path(resource_name), html: {'data-parsley-validate' => true, novalidate: true }) do |f| %>
               <%= bootstrap_devise_error_messages! %>
               <div class="form-group">
                 <i class="fa fa-user icon position-absolute m-2 ml-4 text-primary d-none d-sm-block"></i>
                 <%= f.email_field :email, class: 'form-control email-input text-center', placeholder: "Email address", required: true,  'data-parsley-trigger' => 'change' %>
               </div>
   
               <div class="form-group">
                 <i class="fa fa-key icon position-absolute m-2 ml-4 text-primary d-none d-sm-block"></i>
                 <%= f.password_field :password, class: 'form-control pwd-input text-center', placeholder: 'Password', required: true,  'data-parsley-minlength' => '6',  'data-parsley-trigger' => 'change'  %>
               </div>
   
               <div class="form-group">
                 <i class="fa fa-key icon position-absolute m-2 ml-4 text-primary d-none d-sm-block"></i>
                 <%= f.password_field :password_confirmation, class: 'form-control pwd-input text-center', placeholder: 'Confirm Password', required: true, 'data-parsley-equalto'=>"#user_password", 'data-parsley-trigger' => 'change' %>
               </div>
   
               <div class="form-group text-center mt-4">
                 <%= f.submit  t('.sign_up'), class: 'btn btn-primary login-btn btn-block', id:'submit-btn' %>
               </div>
               <% end %>
               <hr>
               <br>
               <div class="form-group text-center">
                 <%- if controller_name != 'sessions' %>
                 <p>Already a Member?
                   <strong><%= link_to t(".sign_in"), new_session_path(resource_name) %></strong>
                 </p>
                 <% end -%>
               </div>
             </div>
           </div>
         </div>
       </div>
     </div>
   </div>
   ```

3. **devise > passwords > new.html.erb**

   ```html
   <div class="container">
     <div class="row">
       <div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
         <div class="card card-signin my-5">
           <div class="card-body pr-5 pl-5">
             <h4 class="card-title text-center pb-2 mb-4">Forgot Password?</h4>
             <div class="form-signin">
   
               <%= form_for(resource, as: resource_name, url: password_path(resource_name), html: { method: :post, 'data- parsley-validate' => true, novalidate: true  }) do |f| %>
   
               <div class="form-group">
                 <%= f.email_field :email, required: true, class: 'form-control email-input text-center', placeholder: "Email address",  'data-parsley-trigger' => 'change' %>
               </div>
   
               <div class="form-group">
                 <%= f.submit 'Submit', class: 'btn btn-primary login-btn btn-block submit-btn', id:'submit-btn' %>
               </div>
               <% end %>
   
               <div class="form-group text-center">
                 <%- if controller_name != 'sessions' %>
                 <p>Already a Member?
                   <strong><%= link_to t(".sign_in"), new_session_path(resource_name) %></strong>
                 </p>
                 <% end -%>
               </div>
   
               <div class="form-group text-center">
                 <%- if devise_mapping.registerable? && controller_name != 'registrations' %>
                 <p>Dont have a account?
                   <strong><%= link_to t(".sign_up"), new_registration_path(resource_name) %></strong>
                 </p>
                 <% end -%>
               </div>
             </div>
           </div>
         </div>
       </div>
     </div>
   </div>
   ```