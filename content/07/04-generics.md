---
layout: default
title: 7.4. Generics
parent: 7. Advanced Kotlin features
nav_order: 4
---

# 7.4. Generic classes and methods


Kotlin allows you to define generic classes and functions that operate on a class that is passed as a parameter. For classes, they are often used to represent data structures that store any class of data in a certain way. Some examples of generic containers can be found in the collections provided by the Kotlin Standard Library: List, Set, Map, ... Its syntax is very similar to that used in other languages that support generic types such as Java or C++.

Generic functions coupled with lambda functions are often used to encode generic algorithms that can be applied to any class. For example, data collections use generic sorting methods that are receive as parameter a lambda function that compares two specific elements.

For instance, we can create our own generic test class like this:

```kotlin
class MyList<T>(vararg t: T) {
   var inner_list:List<T> = t.toList()

   fun GetFirst():T
   {
       return inner_list.first()
   }
}
```

And we can use it like this:

```kotlin
var m:MyList<Student> = MyList(Student("Tom",40),Student("Ada",19),Student("Hugo",15),Student("Adam",14))
var s1 = m.GetFirst()
```

We indicate to the generic type `MyList` that it will be created using elements of type `Student`:

```kotlin
var m:MyList<Student>
```

The difference between using generics and using parameters of type `Any` is that, in the case of generics, all the elements of the collection have the same type: the type defined when the collection is created (in our example, `Student`). Therefore, Kotlin can look for errors in our code by checking types, method definitions, and properties. With Any, no checks can be performed (errors are detected at run-time rather than compile-time) and casts have to be performed in many cases (making code more verbose).
