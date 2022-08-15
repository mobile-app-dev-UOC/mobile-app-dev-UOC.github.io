---
layout: default
title: 3.2. Operators
parent: 3. The Kotlin programming language
nav_order: 2
---

# 3.2. Operators


## 3.2.1. Behavior of typical operators

**Division**

```kotlin
val x = 7 / 2
```

In this expression, Kotlin infers that the two numbers are of type `Int` and disregards the decimal part of the result to provide an `Int` result: x == 3.

```kotlin
val x:Double = 7 / 2
```
This is a compile-time error.  Kotlin continues to assume that this is an integer division (because both operands are integer constants). Then, it cannot assign the integer result to a value of type `Double`.

Considering that all types in Kotlin are classes, we can invoke a method from one of the two operands to convert it into a `Double`.

```kotlin
val x:Double = 7.toDouble() / 2
```

Now the output value is `x == 3.5` as expected.


## 3.2.2. Range Membership Operator

We can determine if the value of a variable is inside an interval with the operator `in`.

```kotlin
var a = 3 
var min = 2 
var max = 10 
if (a in min..max) 
{ 
	print("ok")
}
```