---
layout: post
title:  A Basic Program in Dart
date: 2021-04-06 01:25
category: flutter
author: Sajan Basnet
tags: flutter
permalink: '/blogs/flutter/day2'
summary: Basic of Dart and its fundamentals.
toc: false
uniq_heading_id: '#post6'
uniq_body_id: 'post6'
---

<img class= "img-fluid img-thumbnail img-space" src="{{site.baseurl}}/assets/img/2021.png">
 

Let's write a program to sort numbers in ascending orders. 

```dart

//A program to sort numbers in ascending order.

sortNumbers(list) {
  int n = list.length;
  for (int i = 0; i < n - 1; i++) {
    for (int j = 0; j < n - 1; j++) {
      if (list[j] > list[j+1]){
        list[j] = list[j] + list[j+1];
        list[j+1] = list[j] - list[j+1];
        list[j] = list[j] - list[j+1];
      }
    }
  }
  return list;
}

void main() {
  List<int> list = [6, 9, 2, 1, 8];
  List<int> sortedNumbers = sortNumbers(list);
  print(sortedNumbers);
}

//Output
//[1,2,6,8,9]

```

**Notes:**

- Dart is a **true object-oriented language** i.e. every thing is an object.

- Dart is **static type** i.e. if we declare a variable as `int n` then the variable can only have an integer value.  If we want a dynamic variable then we can use the keyword **`dynamic`** .

-  **`main()`** function, serves as the entry-point to the app i.e. the the app execution starts from the  `main()` function.

- Dart supports generic types, like `List<int>` (a list of integers) or `List<dynamic>` (a list of objects of any type).

- Uninitialized variables have an initial value of `null`.

- `var`can be used to define a variable with out specifying its type and it can be changed anytime. If we want an unchangeable variable we can use `final`or `const` .

-  We can use single or double quote to define a string. `${`*`expression`*`}`inside the string  quotes is used for **interpolation** 

- In Dart, **arrays are list**. `var list = [1,2,4,6]` where 0 is the index of the first value and `list.length - 1` is the index of the last value

- **Spread operator (`...`)** can be used to insert all the values of a list into another list. 

  ```dart
  var list1 = [1,2,3,4,5]
  var list2 = [0. ...list1]
  ```

- In Dart, **Map** is an object that associates keys and values normally known as hash or dictionary. 

  ```dart
  var nobleGases = {
    2: 'helium',
    10: 'neon',
    18: 'argon',
  };
  ```

<hr>
*Please feel free to give your feedback on the comment section below or ping me at <a aria-label="Send email" href="mailto:sajanbasnet75@gmail.com"><i class="icon fa fa-envelope" style="font-size:32px; margin: 0px 3px;"></i></a> or  <a aria-label="My LinkedIn" rel="noreferrer" target="_blank" href="https://www.linkedin.com/in/sajan-basnet-b4b1b0148/"><i class="icon fa fa-linkedin-square" style="font-size:32px; margin: 0px 3px;" aria-hidden="true"></i></a>. Have a great time :smiley_cat:*

