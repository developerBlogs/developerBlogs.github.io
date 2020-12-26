---
layout: post
title: LEARNING FLUTTER (DAY 1)
date: 2020-07-26 11:25
category: flutter
author: Sajan Basnet
tags: flutter
permalink: '/blogs/flutter/day1'
summary: Journey of learning Flutter.
toc: false
uniq_heading_id: '#post5'
uniq_body_id: 'post5'
---

Yes, after so much laziness and procrastination I have finally decided to learn Flutter. This blog is just a note of my journey of learning Flutter from Day 1


<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/programming.jpeg">


### DART PROGRAMMING LANGUAGE

Before directly diving into Flutter I wanted to start from the basic. Since Dart is what Flutter uses it’s obviously better to learn it first.
Like most programming languages out there **Dart** is also one of them and it was owned and maintained by Google. It’s easy to learn Dart if you have experience working with Java, JavaScript, Ruby, or Python. 

*In a nutshell, Dart is an object-oriented, class defined, single inheritance language using a C-style syntax that transcompiles optionally into JavaScript. It supports interfaces, mixins, abstract classes, reified generics, optional typing, and a sound type system.* [*Source*](https://flutterbyexample.com/lesson/about-dart#why-does-flutter-use-dart)

I guess that all the details I need for now about Dart.


### I was curious why did flutter use Dart anyway?

And the first reason I found was 
**Dart supports multi-platform (ios, android, and web)**. 

Besides this **Dart supports Just in Time(JIT) compiling which allows hot-reload making Flutter apps faster.****

**For more curiosity :**

[Why Flutter Uses Dart by @wmleler](https://hackernoon.com/why-flutter-uses-dart-dd635a054ebf)

[Why flutter uses Dart? (youtube)](https://www.youtube.com/watch?v=5F-6n_2XWR8&feature=emb_logo)




### Getting Started With Dart.

**Installing Dart**

According to official docs of [Dart](https://dart.dev/get-dart), we don't need to install Dart SDK if we install the **Flutter SDK**. 

In my linux I installed it using  using snapd.

```ruby
$ sudo apt-get update
$ sudo snap install flutter --classic
$ flutter sdk-path
```

[For other OS ](https://flutter.dev/docs/get-started/install)


```ruby
$ flutter --version
```
<img class= "img-fluid img-thumbnail" src="{{site.baseurl}}/assets/img/flutter_version.png">

In order to get dart command working anywhere in the terminal

```ruby
$ export PATH="$PATH:path_to_flutter/bin/cache/dart-sdk/bin/"
$dart --version
Dart SDK version: 2.10.4 (stable) (Wed Nov 11 13:35:58 2020 +0100) on "linux_x64"
```


### HELLO WORLD !!!

Yes I started with the classic hello world program with Dart as well. 

In my vs code text editor I created a file as `hello_world.dart` 

```dart
void main(){
  print("HEllO, WORLD!!!")
}
```

And in the terminal 

```ruby
$ dart run hello_world.dart
HELLO, WORLD!!!
```

<hr>
*Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:*

