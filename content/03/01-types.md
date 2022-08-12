---
layout: default
title: 3.1. Types
parent: 3. The Kotlin programming language
nav_order: 1
---

# 3.1. Types

As we have already discussed, Kotlin performs type checking at compile time. However, this does not imply that the types must always be declared explicitly: Kotlin can automatically infer types at compile time by analyzing how variables are initialized. When it is not possible to deduce the type of a variable, the compiler will emit an error message requesting an explicit type declaration. The following subsections present several examples.

## 3.1.1. Basic types

Kotlin can manipulate different data types. The simplest are integers (Byte, Short, Int, Long), floating point numbers (Float, Double), Booleans (Boolean with `true` or `false` values), and characters (Char). 

Each numeric type uses a different number of bits for storing data. That is, each one can encode a different range of values and, in the case of floating-point numbers, achieves a different level of precision:

**Integer numbers**

|Type                  |	Symbol	| Size (bits)	| Minimum	       | Maximum          |
|----------------------|------------|---------------|------------------|------------------|
|Byte	               |            |	8	        | -2^7=128         | 2^7-1 = 127      |
|Short	               |            |	16          | -2^8= -32768     | 2^15-1=32767     |
|Int	(default type) |          	| 32            | -2^31 = -2*10^9  | 2^31-1 = 2*10^9  |
|Long	               | L          |	64          | -2^63 = -9*10^18 | 2^63-1 = 9*10^18 |

**Floating-point numbers**

|Type	               | Symbol | Size (bits) | Precision        |
|----------------------|--------|-------------|------------------|
|Float	               | f / F  |	32	      | Single precision |
|Double	(default type) |        | 	64	      | Double precision |


Keyword `val` in front of a variable declaration indicates that its value cannot be modified. That is, we are indicating that the symbol acts as a constant. On the other hand, keyword `var` indicates that its value can be modified. We can explicitly declare the type of a variable:

```kotlin
var x: Int = 1
```

On the other hand, we can also let the Kotlin compiler infer the type automatically from the initialization. To infer the type, Kotlin chooses the smallest type (starting from `Int`) that can contain the value of the variable.

```kotlin
var y = 1 // Int 
val z = 3000000000 // Long
```

In this case, variable `y` is of type `Int`. Meanwhile, constant `z` is of type `Long` because the value is too large fit in an `Int` variable. 

If we want a variable to have a specific type, it is best to explicitly declare that type.

```kotlin
val n: Byte = 15
```

For numeric variables, the `Long` type can be forced by using the `L` suffix after the constant value.

```kotlin
val j = 1L // Long
```
Symbol `?` in a variable declaration indicates that it can take the null value (it is *nullable*).

```kotlin
var tmp: String? = null
```

The following code produces an error because Kotlin cannot deduce the type within the variable declaration.

```kotlin
var tmp          
tmp = "thing"
```

In Kotlin all basic types are also considered classes (in an object-oriented programming way).  At runtime, these types will become simpler structures to improve performance.

## 3.1.2. Special types

Type `Any` represents all types in Kotlin. Variables declared as nullable it cannot be assigned to variables of type `Any`.

```kotlin
var tmp1: String? = null
var tmp2: String = “help”

var v: Any
```

For example, the following assignment fails because `tmp1` was declared to be nullable.      

```kotlin  
v = tmp1
```

However, the second assignment is correct.

```kotlin
v = tmp2
```

## 3.1.3. The String data type

A string stores a sequence of characters. We can define string literals in two different ways:

- Single-line string literals with escape sequences for special characters for line breaks, tabs, etc.

```kotlin
val s = "My, cat \n"
```

- Multiline string literals without escape sequences.

```kotlin
var text = """
            You have two children
            Tell me about them
""".trimMargin()
```

The blank spaces in front of `You` and `Tell` are included as part of the string. If we don’t want this to happen, we need to remove them explicitly.

```kotlin
var text = """
            |You have two children
            |Tell me about them
""".trimMargin()
```

The `+` operator can be used to concatenate strings.

```kotlin
val text = "hello " + "friend"
```
