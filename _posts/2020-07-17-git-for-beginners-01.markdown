---
layout: post
title:  "Getting Started With Git - Chapter Two"
date: '2020-07-17'
categories: git chapter 01
summary: In this chapter we will learn what is a version control system, what the heck is Git and why we should use it in our projects.
---

# What is Git and Why Use it?

You have just started to work on a project, integrated an awesome feature to it and then released it. So, what you have now is a initial version of your project (Version 1.0). Lets say you added a new feature to it, now you have a new version for it (Version 1.1). <!--more--> 

A project can have many versions like this with continous changes to it. So, we will need a system or tools to track the changes made in our project, a sytem which can help us to go back and forth to the different versions of the project. Basically what we need is a sytem that can control many versions of our project and Git is basically that, a **Version Control System**.

## Why Use Git? 

Git is one of many version control system and the most preferable one by many people.

####  1. Easy Distribution of your Project

<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/git01.png">

Thats right, if you dont know anything about Version Control System this might be how you are sharing or distributing the latest version of your project with your friends or team members. 

So, thats why you should use Git right now. Cause your way is frustating and it sucks.

You can use Git in your repository/project and just upload it to the version control repository hosting service such as  [**Github**](https://www.github.com) or  [**BitBucket**](https://www.bitbucket.com).
Then you can share the link of your project to your friends or team so they can now clone(download) your repository into their local system.

Sounds somewhat similar, yes? Just like you uploading your zipped project into Google Drive and sharing the sharable link. But trust me its not the same.

You dont have to go through all the hassle and trouble like zipping your project everytime and getting the sharable link every f**ing time.
You just need to type some few easy commands and you have pushed/uploaded your latest version of the project into Github or Bitbucket.
Simillarly your teammates also just need to write a git command to pull/download this latest version to their local sytem.

### 2. Resolve conflicts

<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/git02.png">

If you have a team working on a same project, then there can come many cases where more than one person are editing the same file.

For example you just created a function in file ***something.js*** and one of your teammate came and created another function with the same name or he changed statement inside your function. After few days only you came to know about these changes and now you are sitting their confused who made the changes and when.

Using Git will resolves such issues. It gives you features where you can keep the log/track of your changes from which you can easily know when, where and who made the changes. If you are the owner of the repository you will have the authority to merge the changes your teammate made or just discard them. 

### 3. You can revert and go back to older version of your code.

<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/git003.png">

Indeed, Git is good. It lets you jump to any version of your project easily with out any problems at all.

### 4. Branches for your features. 

<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/git04.png">

No matter how big or small the changes is you just dont want to mess up the present stable code. So with Git you can just create a new feature branch and start working in it without changing your old code which lies in master branch.

Its just like creating a copy of you project and editing on that copied version without touching or editing the original project.

### 5. You will end up needing Git eventually.

There might be no any software companies out their who will not be using any version control system. So, its better to learn git before being hired to a company and have more chance to get hired.

Head out to the next section where you can learn **"How To Use Git?"**.
