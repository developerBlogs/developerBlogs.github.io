---
layout: post
title: Getting Started With Git - Chapter One
date: 2020-07-18 17:51
categories: git chapter 02
author: Sajan Basnet
tags: [git]
toc: true
summary: In this chapter we will learn a step by step approach to know how to initialize Git in our project, use different git commands and how to push the our project into the Github.
---

In this chapter you will learn step by step how to use git in your project also known as repository. So, if you are new to git, please follow these steps so that you will be able to control your project, handle the changes in your code base, create branches, open a pull request (PR) of your branch, pushing your project to Github and more.

### 1. Install Git 

The first and the foremost step is to install git in our local system.
If you think you already have git install in your system then you can confirm it by going to your terminal or command prompt and type `git --version`

``` ruby
$ git --version
git version 2.20.1
```

For those who don't have git installed 

* **Windows**
[<i class="fa fa-download" style="font-size:32px; margin-left: 5px; "></i>](https://gitforwindows.org/)

* **macOS**
[ <i class="fa fa-download" style="font-size:32px; margin-left: 5px; "></i>](https://git-scm.com/download/mac)

* **Linux**
[ <i class="fa fa-download" style="font-size:32px; margin-left: 5px; "></i>](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

If you’re on a Debian-based distribution, such as Ubuntu, try

``` ruby
$ sudo apt install git-all
```

### 2. Create A Project and Initialize Git In It

Lets create a folder, I named mine as **LearningGit** .

```ruby 
$ mkdir LearningGit
$ cd LearningGit
``` 
Here I am in the **LearningGit** directory. Now in order to initialize git in this project  type  we need to type **`git init`**

```ruby
$ git init
Initialized empty Git repository in /home/sajan/Desktop/LearningGit/.git/
```

Great job !!! You just made your first local git repository.

### 2. Make Your First Change

Open your text editor and create a file **index.html** inside your project folder
Write some code in **index.html**

``` html
<html>
<head></head>
<body>
  <h1>This is the first change</h1>
</body>
</html>
```

You just made your first change. Lets see how we can use git to track this change.

### 4. Add Your Changes Using `git add`

Lets goto to our terminal  and type **git status** to check the current status of our project

``` ruby
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	index.html

nothing added to commit but untracked files present (use "git add" to track)
```

So, what the above response is trying  to say is that we have some untracked files or some changes. So lets add this untracked files to the staging area by typing  **git add index.html** .

``` ruby
$ git add index.html 
```

Lets check the status again 

``` ruby
$ git status  
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   index.html
```

So now we will get a response that there are no any untracked files. 

> By using **git add index.html**  what we basically did  is that we were sure about the changes we made in the project so we got it ready for the second step or  got it ready to be committed.

> If we have more that one file that has been changed and we want to add it all at once we can do **git add .**

``` ruby
$ git add .
```

### 5. Commit Your Changes Using `git commit`

Now we just need to commit the changes that was sent to the staging area so lets do that by typing **git commit -m**

``` ruby
$ git commit
```

This will generally open a **VIM** editor. Press **Esc** and then **"i"**  to go to insert mode and write your commit message.

After you have written your commit message press **Esc** again and **:wq** to save and quit the Vim editor.

> You can also simply use one line commit using **git commit -m "commit message"**. 

Good job !!! You just made your first commit.

Lets check the status once more 

``` ruby
$ git status 
On branch master
nothing to commit, working tree clean
```

### 6. Import The Repository To Github

Lets push our repository to the **[Github](https://www.github.com)**. 

Goto  **[Github](https://www.github.com)** and create a repository from your Github account. Create a Github account first if you haven't.

 Create a Repository and name it. Since we are Importing the existing local repo to github lets not create a Readme file and skip it.

After successfully creating a repo in the Github you will see some instructions about what to do now. Again since we are importing our local repo to github we will follow the second instruction. Add the remote url to your local repo as origin

``` ruby
$ git remote add origin https://github.com/your-github-username/your-repo-name.git 
```

Are you ready? We  are about to push our repo to the github. So, lets do this using **git push origin master**

``` ruby
$ git push origin master
Username for 'https://github.com': your-github-username
Password for 'https://***@github.com': github-password
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 285 bytes | 285.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/your-github-username/your-github-repo.git
 * [new branch]      master -> master

```

Congratulations, you just pushed your repo to github. 
