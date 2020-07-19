---
layout: post
title: Getting Started With Git - Chapter Two
date: 2020-07-18 17:51
categories: git chapter 02
author: Sajan Basnet
tags: [git]
toc: true
summary: In this chapter we will learn a step by step approach to know how to initialize Git in our project, use different git commands and how to push the our project into the Github.
---

In this chapter you will learn step by step how to use git in your project also known as repository. So, if you are new to git, please follow these steps so that you will be able to control your project, handle the changes in your code base, create branches, open a pull request (PR) of your branch, pushing your project to Github and more.

## 1. Install Git 
The first and the foremost step is to install git in our local system.
If you think you already have git install in your system then you can confirm it by going to your terminal or command prompt and using the command 

`git --version`

``` ruby
$ git --version
git version 2.20.1
```

For those who don't have git installed 

* **Windows** [<i class="fa fa-download" style="font-size:32px; margin-left: 5px; "></i>](https://gitforwindows.org/)

* **macOS** [ <i class="fa fa-download" style="font-size:32px; margin-left: 5px; "></i>](https://git-scm.com/download/mac)

* **Linux** [ <i class="fa fa-download" style="font-size:32px; margin-left: 5px; "></i>](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

If you’re on a Debian-based distribution, such as Ubuntu, try

``` ruby
$ sudo apt install git-all
```

## 2. Create A Project and Initialize Git In It

Lets create a folder, I named mine as **LearningGit** .

```ruby 
$ mkdir LearningGit
$ cd LearningGit

``` 
Here I am inside the **LearningGit** directory. Now in order to initialize git in this project we need to use the git command **`git init`**

```ruby
$ git init
Initialized empty Git repository in /home/sajan/Desktop/LearningGit/.git/
```

Great job !!! You just made your first local git repository.

## 3. Make Your First Change

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

## 4. Add Your Changes Using `git add`

Before adding our changes anywhere lets first check the status of the project using command **git status**.

``` ruby
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	index.html

nothing added to commit but untracked files present (use "git add" to track)
```

So, what the above response is trying  to say is that we have some untracked files or some changes. So lets add this untracked files to the staging area by using **git add index.html** .

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

> If we have more that one file that have been changed and we want to add it all at once we can do **git add .**

``` ruby
$ git add .
```

## 5. Commit Your Changes Using `git commit`

Now we just need to commit the changes that was sent to the staging area so lets do that by using **git commit**

``` ruby
$ git commit
```

This will generally open a **Vim** editor. Press **Esc** and then **"i"**  to go to insert mode and write your commit message.

After you have written your commit message press **Esc** again and **:wq** to save and quit the Vim editor.

> You can also simply use one line commit by doing **git commit -m "commit message"**. 

Good job !!! You just made your first commit.

Lets check the status once more 

``` ruby
$ git status 
On branch master
nothing to commit, working tree clean
```

## 6. Import The Repository To Github

Lets push our repository to the **[Github](https://www.github.com)**. 

Goto  **[Github](https://www.github.com)** and create a repository from your Github account. Create a Github account first if you haven't.

Create a Repository and name it. Since we are importing the existing local repo to github 
lets not create a Readme file and skip it.

<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/github.png">

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

## 7. Creating A New Branch Using `git branch`

Lets say we want to add something new to our project or repo but without affecting the current code or lets say we want to copy the current project and make change to the copied version without affecting the original one.

Then this is where branching feature of git comes handy. No matter how big or small your feature is its always better to create a new branch. By doing this whatever the changes we are making will not hamper the original code base until we **merge** these new branch to the original branch.

Before creating a branch lets first check our available branches in the repo using **`git branch --list`**

``` ruby
$ git branch --list
* master

(END)

```

By default while we initialize a git in our project it sets a **master branch**. And this branch is the one that we use in the production environment i.e when our project will be live to public **master** branch will be the active branch.  So, its better practice to create a develop branch first and add other features branch to it. The * in master sign denotes that the active branch right now is master.

Lets create a new branch using the command **`git branch "branch name"`** 

``` ruby
$ git branch "develop"                    
$ git branch --list

  develop
* master
(END)

```

So now we have two branch **develop** and a **master** branch. So lets make our **develop** branch as active so that we only will make changes in the **develop** branch without affecting the **master** branch by using **`git checkout branch_name`**

``` 
$ git checkout develop
Switched to branch 'develop'
```

Now **develop** branch is the active branch, you can make sure by using **git branch --list** again.

> Note:  If you want to create a new branch and switch to it with one line code you can use **`git checkout -b branch_name`**

Lets make some changes to out code base now. Open up your text-editor and edit the **index.html** file 

``` html
index.html
<html>
<head></head>
<body>
    <h1>This is the first change</h1>
    <h2>I am adding an awesome feature to this project</h2>
</body>
</html>
```

Lets go ahead and add a readme  file as well which basically is used to describe about your project and will also be seen in github site. Create a new file with name **README.md** and right some description about our project.

``` html
README.md
## This will be shown in the github site :)
## I am learning git
```

After doing this if we need to check our project status using `git status` , it shows that two files are changed. The **index.html** and the new **README.md** file. 

In order to know what changes were made we can use the command **`git diff`** which will show you the co

We are sure about the changes so we will add and commit them like we did above by using **git add** and **git commit** command. 

I hope you added and committed the changes. If we run this **index.html** we will see that the output in the browser has a header text **"I am adding an awesome feature to this project"**.

Lets switch to the master branch  **`git checkout master`**. Now if we reload the browser we will not be seeing that header text **"I am adding an awesome feature to this project"**. This is because we had added this feature to our **develop** branch and right now we are in the **master** branch.

## 8. Merging Feature Branch With Master

Let's say we are releasing our project and want every features to be included in it. In order to do this what we need to do now is merge all our feature branches into a single branch which is our master branch.

For merging different branches there are two process **[git rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)** and **[git merge](https://www.atlassian.com/git/tutorials/using-branches/git-merge)**. We will be using **git merge** .

Since we are the owner of this repo  and  we are the one creating the feature branches we can just merge the feature branch to the master branch by switching to the master branch ( i.e the branch where we want to merge the feature branches ) and using

 `git merge develop `

``` ruby
$ git checkout master
Already on 'master'
sajan@Sajans-Machine $ git merge develop

Updating 125ae10..77d3f5a
Fast-forward
 README.md  | 1 +
 index.html | 2 ++
 2 files changed, 3 insertions(+)
 create mode 100644 README.md
```

There can come many cases and errors during the merging process, which i will cover in the next chapter  **Git Help Me !!! What Should I Do?**

## 9 Pushing Your Changes to Github
We can easily push our changes up to the remote i.e Github server using **`git push`** 

``` 
$ git push origin master
Username for 'https://github.com':  github_username
Password for 'https:// github_username@github.com': 
......
```

If you want to push another branch lets say develop to the remote repo just change the branch name like this  **`git push origin develop`**.

## 10. Cloning a Repository Using `git clone`

We can easily clone (copy) a remote repository available in the github site into our local sytem by using the command 

**`git clone url_of_the_repo`**

Go to the desired repository in github you want to clone and copy the url. 

``` ruby
$ git clone https://github.com/ruby/ruby.git 
```

This will clone the repository to your local system. If you want to get all the branches also you can do **`get fetch`**.

## 11. Open Source Contribution with Pull Request

In **step 8**  we merged the develop feature to our master  by just using **git merge develop** and pushed it to the remote. This was possible cause we were the owner of that repo. 

What if we have cloned some some one else repository and created a branch **`feature/added_awesome_feature`** . Now we are hoping that this feature gets merged in the master branch of the repository and is publicly available to people to use it. Since this time we are not the owner of the repository we cannot just merge and push the changed like we did in **step 8 & step 9**. 

So, how can we do this? We can do this by **sending a pull request** to the owner of the repo for the feature you have created. Sending a  Pull Request(PR) basically means that you had added some awesome feature to this project or solved a bug and asking the owner of the repo to merge it.

Before sending a PR we need to first fork the repo to our github.

You can fork my [**repository**](https://github.com/DevloperBlogger/LearnGit) for this.

<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/fork.png">

Then clone this repository to our local sytem like in **Step 10**

Now let's create a branch **feature/added_awesome_feature** and change the  **index.html** file and save it.

``` ruby
$ git checkout -b feature/awesome_feature
Switched to a new branch 'feature/awesome_feature'
```

``` html
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8'>
    <meta http-equiv='X-UA-Compatible' content='IE=edge'>
    <title>Page Title</title>
    <meta name='viewport' content='width=device-width, initial-scale=1'>
    <link rel='stylesheet' type='text/css' media='screen' href='main.css'>
    <script src='main.js'></script>
</head>
<body>
    <h1>Hello world!!!</h1>
    <h2>I am creating a new feature and will send PR</h2>
</body>
</html>
```
Now git add and commit these changes and push it up to your remote repo following the **Step 9**

``` ruby
$ git push origin feature/awesome_feature
Username for 'https://github.com': your_github_user_name
Password for 'https://your_github_user_name@github.com': 
Enumerating objects: 5, done.
.....

```
Once you push the changes to your remote repo, you will see a **Compare & pull request** button for feature/awesome_feature branch  GitHub site.

<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/compare.png">

Click the button and you will be send to the page where you can create a **PR**.

<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/pr.png">

All you need to do now is create a pull request by clicking the Create Pull Request button. Once the PR is created the owner of the repo will be able yo view your changes and merge if its really awesome or declined it.

Congratulations you just lean how to make a **Open Source Contribution**


Kudos :clap: :tada:,  You have come to the end of this chapter. I hope this helped you to get the base knowledge of using Git in your project.

<br>
*Next section **[Git Help Me !!! What Should I Do?]({% link _posts/2020-07-18-git-for-beginners-02.markdown %})** is comming soon* 

Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:
