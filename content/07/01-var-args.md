---
layout: default
title: 7.1. Variable arg lists
parent: 7. Advanced Kotlin features
nav_order: 1
---

# 7.1. Passing an arbitrary number of parameters to a method

We use the keyword `vararg` to indicate that the number of parameters is unspecified.

```kotlin
fun Add(vararg numbers: Int):Int {
   var result = 0
   for (t in numbers)
       result += t
   return result
}

var r = Add(10,20,2)
```

The result of this method will be the sum of its arguments: `32`.
