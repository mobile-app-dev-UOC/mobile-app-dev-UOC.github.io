---
layout: default
title: 3.4. Basic data structures
parent: 3. The Kotlin programming language
nav_order: 4
---

# 3.4. Basic data structures: Array, ArrayList and Collections

## 3.4.1. Array

An array is a sequence of elements with a common type. It is declared by specifying a fixed size and a type for its elements. To do this, we will use the keyword `array`.

The first parameter is the array size and the second parameter indicates how each element has to be initialized in position `i`.

```kotlin
val array:Array<Element?> = Array(2, { i -> null }) 
```

If we want to declare an array of elements of any type we can always use:

```kotlin
val array:Array<Any> = Array(2, { i -> "" })
array[0] = "Hello"
array[1] =  1
```

It is important to note that, in Kotlin, positions in an array range from `0` to `size-1`. We can always access an array size using the array `size` property (`<array-name>.size`).

There are other ways to create an Array. For example, we can create an Array from a list of constant elements:

```kotlin
val array_months = arrayOf("January", "February", "March")
```

In the following example an object array will be created with Int and Float numbers. This strategy is similar to `Array<Any>`.

```kotlin
val array_numbers = arrayOf(1, 3.4, 2.3)
```

Finally, we can create an Array using specific methods depending on the type:

```kotlin
val array = intArrayOf(1, 2, 3, 4)
val array = arrayOfNulls<Number>(5)
```

## 3.4.2. ArrayList


