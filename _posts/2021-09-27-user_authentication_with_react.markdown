---
layout: post
title:  "User Authentication with React"
date: '2021-09-27'
categories: rails react
author: Sajan Basnet
tags: react programming
toc: true
permalink: '/blogs/react/user_authentication/01'
summary: We will be using React to create the frontend part of user authentication
uniq_heading_id: '#post10'
uniq_body_id: 'post10'
---

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Hi there, if you have been following the "Building API on Rails Series" then you will have an Rails API only application with user authentication. In this part we are going to integrate that API with the React application that we are going to build.
<img class= "img-fluid img-thumbnail img-space mb-0" src="{{site.baseurl}}/assets/img/post8/security.jpg" alt="securing ruby on rails api">
 <p class= "text-center mt-0">
 <a href='https://www.freepik.com/vectors/business' class= "small">Business vector created by jcomp - www.freepik.com</a>
</p>
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Create React App
Make sure that you have node and npm installed.
```ruby
npm --v
6.14.12
```
Now let's create a React app using `npx`
```ruby
npx create-react-app web_app
```
After its finished `cd` inside the **web_app** folder and run
```ruby
npm start
``` 
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