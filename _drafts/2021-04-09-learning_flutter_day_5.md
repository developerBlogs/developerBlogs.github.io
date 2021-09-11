---
layout: post
title: Instance and Class/Static methods in Dart
date: 2021-04-09 01:25
category: flutter
author: Sajan Basnet
tags: flutter
permalink: '/blogs/flutter/day5'
summary: Instance and Class/Static methods in Dart
toc: false
uniq_heading_id: '#post9'
uniq_body_id: 'post9'
---


### Methods

The behavior of an object is defined by the methods which basically are just the functions defined inside the class. Methods can be public, private or protected.  Dart doesn't have the keyword such as public, private or protected.

**1. Instance method**

Instance methods on objects can access instance variables and `this`

```dart
Syntax

retur_type method_name(){
    .....
}

```


```dart
class Employee {
  String name;
  int age;
  String position;
  String email;

  Employee (this.name, this.age, this.position);

  //instance method
  int date_of_birth(){
    var current_year = new DateTime.now().year;
    int dob = current_year - this.age;
    return dob;
  }
}

void main() {
  var employee1 = Employee('James', 25, 'Software Engineer');
  var dob = employee1.date_of_birth();
  print("Date of Birth: ${dob}");
}
```



**2. Class method**

Class methods in dart are the methods which are declared with the `static` keyword. Classes method can be directly called using the class name and these methods have no access to the `this`

```dart
syntax

static return_type method_name(){
    .....
}
```


```dart
class Employee {
  String name;
  int age;
  String position;
  String email;

  Employee (this.name, this.age, this.position);

  static String compareEmployeeAge(Employee first, Employee second) {
    if (first.age > second.age) return "${second.name} is younger than ${first.name}";
    if (first.age < second.age) return "${first.name} is younger than ${second.name}";
    
    return 'Both are of same age';
  }
}

void main() {
  var employee1 = Employee('James', 25, 'Software Engineer');
  var employee2 = Employee('Thom', 26, 'Accountant');
  print(Employee.compareEmployeeAge(employee1, employee2));
}
```
<hr>
*Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a> Have a great time :smiley_cat:*

