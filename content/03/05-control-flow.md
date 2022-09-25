---
layout: default
title: 3.5. Control flow
parent: 3. The Kotlin programming language
nav_order: 5
---

# 3.5. Control-flow statements

## 3.5.1. Conditional statements

Kotlin provides the typical `if...else` conditional statement available in all programming languages. Moreover, we also have the statement `when`, a multi-branch conditional statement with multiple branches, similar to the `switch...case` of other languages:

```kotlin
var input:String = "help"
when (input) {
    "help" -> {
        Log.i("info","help text")
    }
    "exec" -> {
        Log.i("info","exec text")
    }
    else -> {
        print("command error")
    }
}
```

## 3.5.2. Loop statements

Kotlin offers the classic `while` loop statement, which executes a sequence of statements while a condition is met:

```kotlin
Factorial var = 1; 
while (i > 1) {
    factorial *= i
    i--
}
```

If we want to traverse all the elements of a data structure, it is preferable to use a `for` loop:

```kotlin
val arrayList = ArrayList<String>()
arrayList.add("Hello")
arrayList.add("Bye")
for (item: String in arrayList) {
    Log.i("info",item)
}
```

We can also traverse a numerical range using the following syntax:

```kotlin
for (i in 1..10) {
}
```

It is also possible to define more complex traversals, such as traversing a range in reverse (`downTo`) skipping two values at a time (`step 2`):

```kotlin
for (i in 10 downTo 0 step 2) {
    Log.i("info",i.toString())
}
```

Finally, we can exit a loop by using a `break` statement. If this is necessary, then it is preferable to use a `while` loop instead.
