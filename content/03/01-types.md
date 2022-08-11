---
layout: default
title: 3.1. Types
parent: 3. The Kotlin programming language
nav_order: 1
---

# 3.1. Types

As we have already discussed, Kotlin performs type checking at compile time. However, this does not imply that the types must always be declared explicitly: Kotlin can automatically infer types at compile time by analyzing how variables are initialized. When it is not possible to deduce the type of a variable, the compiler will emit an error message requesting an explicit type declaration. The following subsections present several examples.

## 3.2 Basic types

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

We can explicitly state the type of a variable:

```kotlin
var one: Int = 1
```

On the other hand, we can also let the Kotlin compiler infer the type automatically from the initialization. To infer the type, Kotlin chooses the smallest type (starting from `Int`) that can contain the value of the variable.

```kotlin
var one = 1 // Int 
val three = 3000000000 // Long
```

In this case, variable `one` is of type `Int`. Meanwhile, variable `three` is of type `Long` because the value is too large fit in an Int variable


