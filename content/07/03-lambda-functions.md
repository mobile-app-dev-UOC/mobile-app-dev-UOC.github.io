---
layout: default
title: 7.3. Lambda functions
parent: 7. Advanced Kotlin features
nav_order: 3
---

# 7.3. Lambda functions (anonymous functions)

Lambda functions are functions that are are not given a name. Instead, they are declared where they are needed. They should be functions that are not going to be used in different places in our application, because in that case we are interested in declaring them and assigning them a name so that they can be reused by calling them from different points in our code. That is, the lambda functions allow for a more compact and clear code: if we have a function that is only used in one place, we will define its code exactly where it will be used. In this way, we make it clear that it will only be used there.


For example, in [Section 7.2](content/07/02-methods-as-params) we defined the `TestFunArg` method:


```kotlin
fun TestFunArg(
   v: Int,
   f: (param1: Int, param2: Int) -> Int
): Int {

   val v = 2 * f(v,2)
   return v
}
```

When we invoke the `TestFunArg` method, we can use a lambda function as a parameter instead of referring to a named method:

```kotlin
var factor = 4
r = TestFunArg(5) { p1: Int, p2: Int ->
   var result = p1 * p2 * factor
   result
}
```

In this case the result of the previous code is `80`.

Within a lambda function, we can access local variables of the method where it is defined and member variables from the class that contains it.

For instance, the code that is executed by a coroutine (we will introduce this concept in [Section 10.1](/contents/10/coroutines)), or the listener code when attending an event (see [Section 8.1](/contents/08(1-listeners))) is usually written as a lambda function. An example of such a listener would be the following:
