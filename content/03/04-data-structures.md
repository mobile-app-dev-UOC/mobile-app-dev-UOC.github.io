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

Finally, we can create an Array using specific methods depending on the type of its elements:

```kotlin
val array = intArrayOf(1, 2, 3, 4)
val array = arrayOfNulls<Number>(5)
```

## 3.4.2. ArrayList

The ArrayList is a very convenient data structure as there is no need to define a fixed size a priori.

```kotlin
val arrayList = ArrayList<String>()
```

ArrayList provides methods to dynamically add and remove items:

```kotlin
arrayList.add("Hello")
arrayList.add("Bye")
arrayList.removeAt(0)
```

At the end of these operations, our ArrayList has only one element: `"Bye"`.

## 3.4.3. Kotlin Standard Library

Kotlin’s standard library provides three key classes for handling dynamic collections of objects: List, Set and Map. These classes provide much richer functionality than an ArrayList. 

### 3.4.3.1. List

A List is a collection of objects that can be sorted by a given criterion. Each item occupies a position, and an item can appear more than once in the list.

A List can be easily sorted using the `sortedBy` method by describing the criterion used to sort its elements.

```kotlin
val list = listOf("123", "12", "1234")
val sorted = list.sortedBy { it.length }
```

Using `sortedBy` we are always using a numerical order. Nevertheless, we may want to define our sorting criterion and therefore our own ordering. To do this, we must define a class and within it we must implement a static method called `compare`. Static methods are explained in more detail in [Section 3.6.8](/content/03/06-objects).

### 3.4.3.2. Set

A Set is a collection of objects without duplicates where order is not relevant.

### 3.4.3.3. Map

A Map is a dictionary of `<key, value>` pairs. Keys cannot be repeated but values can. 

The typical way to use a Map is by reading/writing the value of a specific key:

```kotlin
val items = HashMap<String, Int>()
items["key1"] = 1
items["key2"] = 2
val item = items["key2"]
```
However, we can also traverse the Map like a list:

```kotlin
for ((k, v) in items) {
    Log.i("$k","$v")
}
```

Nevertheless, there is no information about the order in which `<key, value>` pairs will be visited in this traversal. 

### 3.4.4. Enumerated typs

Enumerations are special types which can only have a finite and predefined set of values. The definition and usage of enumerations is similar to other programming languages.

```kotlin
enum class Direction {
    UP, DOWN, LEFT, RIGHT
}
```

We can assign a variable directly using the name of a constant value from the definition of the enumeration or using a string that contains its name:

```kotlin
var d:Direction = Direction.DOWN
var d_from_string:Direction = Direction.valueOf("DOWN")
```

We can also use a more complex statement and assign a series of values to each item in the enumeration.

```kotlin
enum class Direction2(val v1:Int, val v2:String) {
    UP(1,"0001"),
    DOWN(2,"0010"),
    LEFT(3,"0011"),
    RIGHT (4, "0100")
}

var d2: Direction2 = Direction2.DOWN
var v2: Int = d2.v1
var s2: String = d2.v2
```

A typical use case for this feature is retrieving a value of the enumerated type from the corresponding values in other types (e.g., transform an integer 2 into the enumerated value `DOWN`). For this operation to make sense, the value given to each alternative in the enumeration should not be repeated.

In this case, we create a map where the key is the given value and the content is the enumeration. Now we can retrieve values from our enumerated type by using the value as the key in the map.

```kotlin
val map = Direction2.values().associateBy(Direction2::v1)
var d22: Direction2? = map[2]
```