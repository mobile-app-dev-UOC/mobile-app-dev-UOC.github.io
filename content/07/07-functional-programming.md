---
layout: default
title: 7.7. Functional programming
parent: 7. Advanced Kotlin features
nav_order: 7
---

# 7.7. Functional programming

While the Kotlin language eminently focuses on object-oriented programming, it also supports another programming paradigm: **functional programming**. Functional programming is based on the intensive use of lambda expressions and recursion.  Kotlin supports the definition of functions using a functional programming style. 

For example, it is very common to take advantage of the functional programming aspects offered by the collections of the Kotlin Standard Library.

```kotlin
var l:List<Student> = listOf(Student("Tom",40),Student("Ada",19),Student("Hugo",15),Student("Adam",14))
var l2 = l.filter { it.age < 20 }
   .sortedBy { it.name}
   .take(2) 
```

Here, we create a list of students and get the first two students who are less than 20 years old sorted alphabetically. The result would be: `"Ada"` and `"Adam"`.

We can also apply a function to each item on the list.

```kotlin
var l:List<Student> = listOf(Student("Tom",40),Student("Ada",19),Student("Hugo",15),Student("Adam",14))

val l3 = l.map { it.name.uppercase() }
```

As we can see, functional programming eliminates the need for many loops and can make our code more compact and readable.
