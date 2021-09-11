---
layout: post
title:  "Building API in Rails - Part 1"
date: '2021-08-28'
categories: rails
author: Sajan Basnet
tags: rails programming
toc: false
permalink: '/blogs/rails_api_series/01'
summary: This is the first part of Rails API series where we will set up our rails application, rspec, and other necessary gems.
uniq_heading_id: '#post5'
uniq_body_id: 'post5'
---

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Getting it ready 
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/api.jpeg">

Hi there, welcome to the first part of the series of building API in rails. Let's just start with what this series will cover 

1. Setting up our Ruby on Rails API only application with all necessary gems and stuff.
2. Adding Rspec for the TDD approach.
3. Building a CRUD RESTful APIs
4. Serializing the API resources
5. Securing our API with JWT
6. Integration SWAGGER for api documentation
7. and many more....
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Setting up our Ruby on Rails API only application with all necessary gems and stuff

It's quite easy to set up a ROR application and since ROR is a battery included framework we don't have to worry about any configuration stuff. 

Make sure that you have
1. Ruby installed
2. For this, we can install first <a href="https://rvm.io/rvm/install" rel="noreferrer" target="_blank">**RVM (Ruby Version Manager)** </a>then install the required ruby version. We will be using the latest ruby version which is 3.0.2 as of now. 
3. Node js, npm, and Yarn installed
   Although we will not need this since it's API only application but it might come in handy later so it's better to install it. Refer to  <a href="https://classic.yarnpkg.com/en/docs/install#windows-stable" rel="noreferrer" target="_blank">**this** </a> for installing Yarn. 
   
   For node, we can install it with <a href="https://github.com/tj/n" rel="noreferrer" target="_blank">**nvm** </a>. I love using <a href="https://github.com/tj/n" rel="noreferrer" target="_blank">**n** </a> as node version manager.

   In Rails 7 <a href="https://github.com/rails/importmap-rails" rel="noreferrer" target="_blank"> Import Map</a> is going to be the new and default way of importing javascript assets into the rails application so we might not even need node ​:man_shrugging
6. Rails installed.
   Open the terminal and enter `gem install rails`
7. Check your rails version with the `rails version` command. For this series, we will be using the **Rails version 6.1.4**

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Now since we have everything needed let us go ahead and set up our application.
Go to terminal and enter
```ruby
rails new api_app --api -d postgresql --skip-system-test --skip-test
```

So, you might have noticed the database we will be using which is Postgresql, and also I have skipped the test since we will be using Rspec for testing.

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Before moving forward to creating an API lets first do the following
1. <a href="https://developerblogs.github.io/blogs/rails/01#setup-database" rel="noreferrer" target="_blank">**Setup Database** </a>
2. <a href="https://developerblogs.github.io/blogs/rails/01#setup-robocop-gem" rel="noreferrer" target="_blank">**Setup Rubocop Gem** </a>
3. <a href="https://developerblogs.github.io/blogs/rails/01#setup-rspec-for-testing" rel="noreferrer" target="_blank">**Setup Rspec** </a>


Great 🎉 🎉 we are done with the setup. Let's run `rails s` in the terminal and visit <a href="http://localhost:3000/" rel="noreferrer" target="_blank">**here** </a>.

</div>
</div>

<div class="row article-container">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:

<div>
<strong>Related Topic</strong>

  <a href="https://developerblogs.github.io/blogs/rails_api_series/02" rel="noreferrer" rel="noreferrer" target="_blank">**Building API in Rails Part 2** </a>
</div>
</div>
</div>
