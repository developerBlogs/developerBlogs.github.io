---
layout: post
title:  "Building API in Rails - Part 4"
date: '2021-09-16'
categories: rails
author: Sajan Basnet
tags: rails programming
toc: true
permalink: '/blogs/rails_api_series/04'
summary: This is the fourth part of Building Rails API series where we will be talking about securing our api.
uniq_heading_id: '#post8'
uniq_body_id: 'post8'
---

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Hi there, welcome to the fourth part of Building API with Rails series. In this part we are going to discuss how we can secure our API.
<a href='https://www.freepik.com/vectors/business' rel="noreferrer" target="_blank"><img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/post8/security.jpg"> </a>
Normally when it comes to securing API or web application its done through the process of <a href="https://developerblogs.github.io/blogs/security/01#authentication-and-authorization" target="_blank noreferrer">authentication and authoriztion.</a>. 

There are a mainly two ways for doing the authentication part which is **Session based Authentication** and **Token Based Authentication** and one of the best and renown one in Rails for session based authentication is <a href="https://github.com/heartcombo/devise" target="_blank" noreferrer> Devise</a>. 

For token based authentication we can also ues <a href="https://github.com/lynndylanhurley/devise_token_auth" target="_blank" noreferrer>Devise Token Auth</a>. Simillary, other ones are <a href="https://github.com/nsarno/knock" target="_blank" noreferrer>Knock</a>, <a href="https://github.com/doorkeeper-gem/doorkeeper" target="_blank" noreferrer>Doorkeeper</a> as Oauth provider and so on. I will be using <a href="https://github.com/jwt/ruby-jwt" target="_blank" noreferrer>JWT</a> for the token based authentication for this application.
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Now if you are not familiar with what session and token based authentication is I will try to describe the process of both.
## Session Based Authentication
In the session based authentication the processes is as follows: 
1. User fills the login form with their credentials and submits the request to the server.
2. Server then validates the request and then creates a **session** and stores it in the database.
3. The **session id** is then send as a response to the browser.
4. The browser then needs to store this `session_id` somewhere. We know that HTML is a stateless protocol so how can the browser store it and the answer is with **cookies** :cookie:
5. Now when the logged in user needs to make another request to the server to get some protected resources let's say fetch all the super cats then it attaches the `session_id` in the request header and sends the request.
6. Finally, server matches the `session_id` with the one stored in the database and response backs with all the super cats :smile_cat: ​:smirk_cat: :smiley_cat:.
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Token Based Authentication
The authentication process for token based authentication is somewhat similar to that of session based authentication 
1. User fills the login form with their credentials and submits the request to the server.
2. Server then validates the request and then creates a encoded token with the help of private key and some hashing algorithim and this token is known as **JWT**.
3. The encoded `jwt` is then send back as a response.
4. This endocoded `jwt` is generally stored in the local storage.
5. Now when the logged in user needs to make another request to the server to get some protected resources let's say fetch all the super cats available in the server then it attaches the encoded `jwt` in the request header with Authorization as key and also adds a Bearer prefix to the token.
```ruby
Request Headers
Authorization: Bearer <token>
```
6. Finally, server matches the encoded `jwt` by decoding it and response backs with all the available cats :smile_cat: ​:smirk_cat: :smiley_cat:.
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Using BCrypt for password encryption

If you have noticed in the previous part we had stored the password as a plain text in the databse which is not secure. We can use the <a href="https://github.com/bcrypt-ruby/bcrypt-ruby" target="_blank" noreferrer>bcrypt</a> gem to secure the password.

*Note: If we had used the `devise` gem then we didn't have to worry about this, as devise itself can handle this.*

The first thing we need to do is uncomment `gem 'bcrypt', '~> 3.1.7'` this guy in the gem file and then `bundle install`.

In the `User` model we then need to add `has_secure_password` 

```ruby
class User < ApplicationRecord
  has_secure_password
  validates :email, :fullname, :password, presence: true
  validates :email, uniqueness: true
end
```

*Note: Your column name in users table must be `password_digest` and not `password`.*

Now when ever a new user is created their password is saved as encrypted hash instead of plain text. Also one advantage of using bcrypt is that we can easily authenticate the user as well. Let's check this in the `rails c`.
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/post8/bcrypt1.png">
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/post8/bcrypt2.png">
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/post8/bcrypt3.png">
For more information on how Bcrypt works you can read <a href="https://emmanuelhayford.com/understanding-the-bcrypt-hashing-function-and-its-role-in-rails/" target="_blank" noreferrer>here</a>
</div> 
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">

Thanks for reading till the end we have come to the end of **part 4** of the **Building API with Rails series**. In the next part, we will continue the process of securing our API by using **JWT**.

The code base is available [here](https://github.com/sajanbasnet75/rails_api_series). :beers:

</div>
</div>

<div class="row article-container">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:

<div>
<strong>Related Topic</strong>
  <a href="https://developerblogs.github.io/blogs/rails_api_series/02" rel="noreferrer" target="_blank">**Building API in Rails Part 3** </a>
</div>
</div>
</div>