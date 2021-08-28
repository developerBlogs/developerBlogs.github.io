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

## Getting it ready 

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/api.jpeg">

Hi there, welcome to the first part of the series of building API in rails. Let's just start with what this series will cover 

1. Setting up our Ruby on Rails API only application with all necessary gems and stuff.
2. Adding Rspec for the TDD approach.
3. Building a login/session API
4. integrating Google Oauth
5. and many more .....


### Setting up our Ruby on Rails API only application with all necessary gems and stuff

It's quite easy to set up a ROR application and since ROR is a battery included framework we don't have to worry about any configuration stuff. 

Make sure that you have
1. Ruby installed
2. For this, we can install first [RVM (Ruby Version Manager)](https://rvm.io/rvm/install) then install the required ruby version. We will be using the latest ruby version which is 3.0.2 as of now. 
3. Node js, npm, and Yarn installed
   Although we might not need this since it's API only application but it might come in handy later so it's better to install it. Refer [to this](https://classic.yarnpkg.com/en/docs/install#windows-stable) for installing Yarn. 
4. For node, we can install it with [nvm](https://github.com/nvm-sh/nvm#installing-and-updating). I love using [**n**](https://github.com/tj/n) as node version manager. 
5. Rails installed.
   Open the terminal and enter gem install rails 
6. Check your rails version with the `rails version` command. For this series, we will be using the **Rails version 6.1.4**


Now since we have everything needed let us go ahead and set up our application.
Go to terminal and enter
```ruby
rails new api_app --api -d postgresql --skip-system-test --skip-test
```

So, you might have noticed the database we will be using which is Postgresql, and also I have skipped the test since we will be using Rspec for testing.



Before moving forward to creating an API lets first do the following

**1.** [**Setup Database**](https://developerblogs.github.io/blogs/rails/01#3-setup-database) 

**2.** [**Setup Rubocop Gem**](https://developerblogs.github.io/blogs/rails/01#2-setup-robocop-gem)

**3.** [**Setup Rspec**](https://developerblogs.github.io/blogs/rails/01#4-setup-rspec-for-testing)



Great 🎉 🎉 we are done with the setup. Let's run `rails s` in the terminal and visit [here](http://localhost:3000/).

Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat: