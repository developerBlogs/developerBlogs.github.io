---
layout: post
title: All about Classes and Objects in Dart
date: 2021-04-08 01:25
category: flutter
author: Sajan Basnet
tags: flutter
permalink: '/blogs/flutter/day4'
summary: All about classes and objects in Dart.
toc: false
uniq_heading_id: '#post8'
uniq_body_id: 'post8'
---


Suppose, there's a company with more than 40 employees. Each employee will have their own properties like their name, job position, age, and so on. 

Let’s create an Employee class in Dart.

```dart
class Employee {
  String name;
  int age;
  String position;
}
```
<br>

Now let’s say among all employees we have an employee whose name is James, age 25 and has the job position as a software engineer.

Let’s create an object as employee1 from the **Employee** class

```dart
class Employee {
  String name;
  int age;
  String position;
}

void main() {
  var employee1 = Employee();
  employee1.name = 'James';
  employee1.age = 25;
  employee1.position = 'Software Engineer';

  print("${employee1.name} is ${employee1.age} years old and is a ${employee1.position}");
}

//output
//James is 25 years old and is a Software Engineer
```

<br>

### Constructors

We generally use constructors if we want  to initialize a object when its created.

**1. Standard Constructor**

```dart
class Employee {
  String name;
  int age;
  String position;

  Employee (this.name, this.age, this.position);
}

void main() {
  var employee1 = Employee('James', 25, 'Software Engineer');
  print("${employee1.name} is ${employee1.age} years old and is a ${employee1.position}");
}
```



**2. Named Constructor**

```dart
class Employee {
  String name;
  int age;
  String position;
  String email;

  Employee (this.name, this.age, this.position);

  //named parameters
  Employee.email(this.email);
}

void main() {
  var employee1 = Employee('James', 25, 'Software Engineer');
  print("${employee1.name} is ${employee1.age} years old and is a ${employee1.position}");
  print(Employee.email('sth@gmail.com').email);
}
```

<hr>
*Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a> Have a great time :smiley_cat:*

