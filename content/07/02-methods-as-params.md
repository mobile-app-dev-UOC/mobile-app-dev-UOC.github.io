---
layout: default
title: 7.2. Methods as params
parent: 7. Advanced Kotlin features
nav_order: 2
---

# 7.2. Passing a method as a parameter to another method

The typical example of methods as parameters is passing a method that compares two elements to a method that allows an Array to be ordered. In this way, the sorting algorithm becomes generic: all you need is a function that compares two elements depending on their type. That, we will not use the same function to compare integers or to compare Strings.

Let us consider a simpler example of passing a method as a parameter: 

```kotlin
fun TestFunArg(
   v: Int,
   f: (param1: Int, param2: Int) -> Int
): Int {

   val v = 2 * f(v,2)
   return v
}
```

The `TestFunArg` method requires two parameters. The second parameter should be a function with two integer type parameters that returns an integer result. This function received as a parameter will be applied within the body of the `TestFunArg` method.

We now declare a function according to this specification:

```kotlin
fun multiplication(p1: Int,p2: Int):Int
{
   val result = p1 * p2
   return result;
}
```

Now we can call the original method passing this function as its second parameter. To pass the function as a parameter, we use the operator `::` in front of the name of the method.

```kotlin
var r = TestFunArg(5, ::multiplication)

```

In this case, the result of the method `TestFunArg` is `20.
