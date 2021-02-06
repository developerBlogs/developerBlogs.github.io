---
layout: post
title: Learning Flutter (Day 3)
date: 
category: flutter
author: Sajan Basnet
tags: flutter
permalink: '/blogs/flutter/day3'
summary: All about functions and parameters in Dart.
toc: false
uniq_heading_id: '#post7'
uniq_body_id: 'post7'
---

### All about Functions and Parameters


Let's write a program that takes in the user name as an input and greets the user with their age.

```dart
//import dart::io libary for taking user input
import 'dart:io';

String greetUser(String name, int dob){
  var currentYear = new DateTime.now();
  return "Hello ${name} you are ${currentYear.year - dob} year's old";
}
void main() {
  stdout.writeln("Enter your name: ");
  String name = stdin.readLineSync();
  stdout.writeln("Enter your birth year: ");
  int year = int.parse(stdin.readLineSync());
  print(greetUser(name, year));
}


//output
Enter your name: 
James
Enter your birth year
1996
Hello James you are 25 year's old.
```

Here, we defined a function with  **greetUser** with a String as return type and just one argument. 


For **functions** that contain just one expression, we can use a shorthand syntax:

```dart
//import dart::io libary for taking user input
import 'dart:io';

String greetUser(String name, int dob) => "Hello ${name} you are ${new DateTime.now().year - dob} year's old";

void main() {
  stdout.writeln("Enter your name: ");
  String name = stdin.readLineSync();
  stdout.writeln("Enter your birth year: ");
  int year = int.parse(stdin.readLineSync());
  print(greetUser(name, year));
}

```


Above function **greetUser** has a ***required positional parameters*** as `(String name, int dob) `. So while calling the function its should be called in the same order i.e. `greetUser('James', 1996)`. If we called it like  `greetUser(1996, 'James')`it will raise an error.

<br>
### Named Parameters

**Defining the function:** 

```dart
String greetUser({String name, int dob })
```
**{}** is used while defining a function with named parametrs.


**Calling the function:** 

``` dart
greetUser(name: 'James', dob: 1996);
```

By default named parameters are optional.

**To make the named parameters required:**

```dart
import 'package:meta/meta.dart'
    
const Scrollbar({Key key, @required Widget child}){
    ....
}
```

<br>
<br>

### Optional positional parameters

**Defining the function:** 

```dart
String greetUser(String name, int dob, [int weight])
```

**Calling the function:** 

``` dart
greetUser('James', 1996);

or 

greetUser('James', 1996, 60);

```

<br>
<br>

### Default Parameter values

Default parameter only works with optional parameters and "=" operator is used to give a default value to  a parameter

```dart
String greetUser(String name, int dob, [int weight = 60])
```

<hr>
*Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:*

