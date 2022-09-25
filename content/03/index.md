---
layout: default
title: 3. The Kotlin programming language
nav_order: 3 
has_children: true
---

# 3. The Kotlin programming language

Kotlin is the preferred programming language for Android app development. It supports both object-oriented programming (OOP) and functional programming. Compared to other languages, Kotlin uses similar techniques to those used in Java and C#.

Kotlin is a language with compile-time type verification, like as C or Java: type errors are detected when the code is compiled (unlike interpreted languages such as Python where types are checked at runtime). Regarding its execution, a Kotlin application can be run on the Java Virtual Machine or on top of JavaScript. 

The following would be an example of a "Hello world" program in Kotlin. This program prints the string `"Hello world!"` to the standard output. Notice that its syntax is similar to Java, in particular with respect to comments or method calls. You should also notice that in certain situations it is possible to avoid type declarations and there is no need to write semicolons (`;`) between statements.


```kotlin
/* Hello world! Program */
// My first program written in Kotlin
fun main() {
    println("Hello world!")
}
```

In this first section dedicated to Kotlin, we will focus on the basics and object-oriented programming. In [Section 7](/content/07/) we will discuss advanced aspects and functional programming.

