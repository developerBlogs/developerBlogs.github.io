---
layout: post
title: Getting Started With Git - Chapter Two
date: 2020-07-18 17:51
categories: blog
author: Sajan Basnet
tags: git programming
toc: true
permalink: '/blogs/git/02'
summary: In this chapter we will learn a step by step approach to know how to initialize Git in our project, use different git commands and how to push the our project into the Github.
uniq_heading_id: '#post2'
uniq_body_id: 'post2'
til: false
---

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/workflow.jpg">

In this chapter we will learn step by step, how to use git in our repository(project). So, if you are new to git, please follow these steps so that you will be able to manage the many versions of your project, handle the changes in your code base, create branches for new features, open a pull request (PR) of your branch, push your project to Github and more.

</div>
</div>
<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
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
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## 2. Create A Project and Initialize Git In It

Before using git lets configure it first 

```ruby
$ git config --global user.name "Your Name"
$ git config --global user.email you@example.com
```

Done, lets create a folder. I named mine as **LearningGit** .

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
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
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
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
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
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
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
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## 6. Import The Repository To Github

Lets push our repository to the **[Github](https://www.github.com)**. 

Goto  **[Github](https://www.github.com)** and create a repository from your Github account. Create a Github account first if you haven't.

Create a Repository and name it. Since we are importing the existing local repo to github 
lets not create a Readme file and skip it.

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/github.png">

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

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
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

After doing this we need to check our project status using `git status` , it shows that two files are changed. The **index.html** and the new **README.md** file. 

In order to know what changes were made we can use the command **`git diff`** 

We are sure about the changes so we will add and commit them like we did above by using **git add** and **git commit** command. 

I hope you added and committed the changes. If we run this **index.html** we will see that the output in the browser has a header text **"I am adding an awesome feature to this project"**.

Lets switch to the master branch  **`git checkout master`**. Now if we reload the browser we will not be seeing that header text **"I am adding an awesome feature to this project"**. This is because we had added this feature to our **develop** branch and right now we are in the **master** branch.

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## 8. Open a Pull Request For Review And Discussion 

If you are opening a Pull Request(PR) for your work then what it basically means is that you have added some awesome feature, changes to this project or solved a bug and you are notifying all the involved team members, reviewer about the changes before merging it to the master branch.

So, by opening a pull request it initiates discussion about the commits. The person reviewing the PR can raise questions and comments about the code you have wrote and tell you to improve it.

Once whole team is satisfied with the pull request you opened the reviewer or owner of the project can merge this feature to the master branch.

Before creating PR we need to push the feature branch up to the remote repo. Lets push our develop branch to the remote repository.

`**git push origin develop**`


```ruby
$ git push origin develop

Username for 'https://github.com': your_github_username
Password for 'https:// your_github_username@github.com': 
Enumerating objects: 13, done.
Counting objects: 100% (13/13), done.
Delta compression using up to 8 threads
Compressing objects: 100% (9/9), done.
Writing objects: 100% (11/11), 1.08 KiB | 551.00 KiB/s, done.
Total 11 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), done.
remote: 
remote: Create a pull request for 'develop' on GitHub by visiting:
remote:      https://github.com/your_github_username/Repo-name/pull/new/develop
remote: 
To https://github.com/your_github_username/Repo-name.git
 * [new branch]      develop -> develop
```
Goto to the link given in the console, this will open github and create a Pull Request to master branch.Don't forgot to add some reviewers if there are any.

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/request_pr.png">

Good job :clap:, you just created a PR now you and your teammates can discuss on this before merging it to master.

</div>
</div>
<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## 9. Merging Branches

If you are a reviewer and merging a Pull Request it can be done easily by the click of a button in Githhub or Bitbucket. Just goto github and the pull-request tab and merge the request to master.

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/merging.png">

But if you want to merge two branches together we can do this by two ways **[git rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)** and **[git merge](https://www.atlassian.com/git/tutorials/using-branches/git-merge)**. We will be using **git merge** .

You can skip this part if you dont want to merge anything for now.

Lets say we want to merge a branch **feature/new_feature** to **develop** branch. Then we can do this by switching to the develop branch (i.e destination branch) and using 

`git merge source_branch_name`

``` ruby
$ git checkout develop
Already on 'develop'
$ git merge feature/new_feature

Updating 125ae10..77d3f5a
Fast-forward
 README.md  | 1 +
 index.html | 2 ++
 2 files changed, 3 insertions(+)
 create mode 100644 README.md
```
</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## 10. Pulling the latest changes from remote repository.

After the PR have been merged we will not have the lates changes to our master branch so we need to pull the latest changes from the remote to our local by using

`**git pull origin branch_name**` 

Or simply `*git pull*` by switching to the branch we want to pull


*Congratulations :tada: :tada::clap:  ​you just finished the all the steps of starting with git. I hope you learned how to work with git on your project. The above steps all just explain one of the simple and general  **git flow** out there.*

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Some Important Notes

### Note 1: Check if you have latest changes in local repo.

Before pushing the feature branch to remote or before creating a pull request. Its better to check if we have the all the latest changes in the branch where we want to merge our feature branch. Here in our case is **master** and so to check this 
1. Switch to master branch.
2. Pull the latest changes using `git pull`

```ruby
$ git pull origin master
```

3. Then switch to the feature branch lets say **develop** for now.
4. Merge the **master** branch to  the  **develop**.

```ruby
$ git merge master
```

5. And finally push this **develop** branch to remote and create a PR against **master** branch 

### Note 2: Its good to create separate develop branch.

Its even good practice if you create a separate branch from **master** like **develop** branch and push this develop branch to the remote repository. Now in your local system switch to the **develop** branch  and create new branches from it for adding new feature. Lets say i added a feature branch as **feature/Login**. Now we will open the pull request of this feature branch against the **develop** branch and not the **master** branch. And the PR can be merged into the develop branch.

This is good way to follow because now you can deploy your **develop** branch and perform testing on it. If everything seems OK! then you can send a PR of this **develop**  to the **master**. Finally after merging **develop** to the **master** you can deploy your master to production.

### Note 3: Creating HotFix 

What if even after all testing and QA session you found a bug in the production. In this case you can 

1. Switch to the master branch 
2. Create a **hotfix** branch   to fix or solve the bug 
3. After finishing just send a PR directly against the master branch. 
4. Merge the **hotfix** to the master and deploy to production.  

### Note 4: Tags

Its better to tag your repository after every release to the production by using `git tag`

1. After releasing your master branch to production switch to master branch
2. Create a tag with version 
   ```ruby
   $ git tag v1.0.0
   ```
3. Push the tags to remote 
   ```ruby
   $ git push --tags 
   ```
4. Best practice for Naming Tags 
   When developing a software product there can be many tags. So, its better to name the tags in a proper way. 

   Usually I use the semantic versioning where tag version are increased according to the types of changes which can be `major`, `minor` and `patch/bug fix`.

   ```ruby
   v[major].[minor].[patch]
   ```
   Since this is our first release we named our tag as v1.0.0. 

   But next time if we did some major changes or we added a heavy feature to our software and released it, then our tag will be v2.0.0. 

   Also, let's say our version 2.0.0 had a bug. So, we created a patch/fix for it and pushed it to production then this time our tag version will be v2.0.1 

### Note 5: .gitignore

There will be files and folders that we may not want to push to the remote repo or that we dont want the git to keep track of its changes. In such case we can put them in the 
.gitignore file.


</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Cloning a Repository Using `git clone`

We can easily clone (copy) a remote repository available in the github site into our local sytem by using the command 

**`git clone url_of_the_repo`**

Go to the desired repository in github you want to clone and copy the url. 

``` ruby
$ git clone https://github.com/ruby/ruby.git 
```

This will clone the repository to your local system. If you want to get all the branches also you can do **`get fetch`**.

</div>
</div>

<div class="row article-container mb-4">
<div class="col-lg-9 col-md-9 mx-auto pt-3">
## Open Source Contribution

What if we have cloned some some one else repository and created a branch **`feature/awesome_feature`** . Now we are hoping that this feature gets merged in the master branch of the repository and is publicly available to people to use it. Since this time we are not the owner of the repository we cannot just merge and push the changed like we did in **step 8 & step 9**. 

So, how can we do this? We can do this by **sending a pull request** to the owner of the repo for the feature you have created. 

Before sending a PR we need to first fork the repo to our github.

You can fork my [**repository**](https://github.com/DevloperBlogger/LearnGit) for this.

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/fork.png">

Then clone this repository to our local sytem like in **Step 10**

Now let's create a branch **feature/awesome_feature** and change the  **index.html** file and save it.

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

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/compare.png">

Click the button and you will be send to the page where you can create a **PR**.

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/pr.png">

All you need to do now is create a pull request by clicking the Create Pull Request button. Once the PR is created the owner of the repo will be able yo view your changes and merge if its really awesome or declined it.

Congratulations you just lean how to make a **Open Source Contribution**


Kudos :clap: :tada:,  You have come to the end of this chapter. I hope this helped you to get the base knowledge of using Git in your project.

**More Learning Sources**

[Git Atlassian](https://www.atlassian.com/git/tutorials/learn-git-with-bitbucket-cloud)

[A successfull git  branching model](https://nvie.com/posts/a-successful-git-branching-model/)

[Understanding the GitHub Flow](https://guides.github.com/introduction/flow/)


</div>
</div>
<div class="row article-container">
<div class="col-lg-9 col-md-9 mx-auto pt-3">

*Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:*
