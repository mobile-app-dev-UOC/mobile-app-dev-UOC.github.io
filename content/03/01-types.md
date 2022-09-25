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

In this case, variable `y` is of type `Int`. Meanwhile, constant `z` is of type `Long` because the value is too large to fit in an `Int` variable. 




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

Type `Any` represents all types in Kotlin. Variables declared as nullable cannot be assigned to variables of type `Any`.

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

### 3.1.3.1. String templates


Kotlin can insert the value of a variable within a string.

```kotlin
var mail_count = 3
var message = “You have $mail_count mails”
```

Moreover, Kotlin can also insert the result of evaluating an expression within a string:

```kotlin
var points = 3
var message = "You have two times points ${2*points}"
```

To use the dollar (`$`) symbol within a string, we must use the escape sequence `\$`.

```kotlin
var msg = "\$t"
```

For more complex formatting, we can use the `format` method from class `String.`

```kotlin
var msg2 = String.format("%.2f",10.708)
```

Floating point numbers are rounded by default to the nearest value. In this example, the `format` method would return the string `“10.71”`.

## 3.1.4. Type casts

Kotlin does not perform automatic type casts. For example, if a method expects to receive a value of type `Long` as a parameter, we cannot call the method with a value of type `Int`. The same is true for floating point numbers: there is no implicit cast from `Float` to `Double`.

Nevertheless, Kotlin offers methods to convert a value of one basic type to a value of another basic type. For example:

```kotlin
val str:String = 4.toString()
// Correct: str <- "4"
```

Like `toString()`, methods such as `toLong()` or `toDouble()` convert numbers between different numeric types.

On the other hand, if we have a variable of type `Any` we may need to recover its value using a more specific type. Unlike methods such as `toString()`, this cast does not transform the stored value, only the type assigned to it. There are two ways to achieve this:

- **The unsafe type cast** `as`: Conversion is always performed and can cause runtime errors.

```kotlin
var tmp1: String? = null
var tmp2: String? = null
var v1: Any = "hello"
var v2: Any = 12
tmp1 =  v1 as String 
// Correct: tmp1 <- "hello"
tmp2 = v2 as String
// Runtime error: 12 is a number, not a String
```

- **The secure type cast** `as?`: If the conversion cannot be performed, the result will be `null`.

```kotlin
tmp1 =  v1 as? String 
// Correct: tmp1 <- "hello"
tmp2 =  v2 as? String
// tmp1 <- null (12 is a number, not a String)
```

## 3.1.5. Finding out the type of a variable

The `is` operator checks if a Kotlin expression has a given type. The `EXP is TYPE` syntax returns `true` if `EXP` has the type `TYPE`, and `false` otherwise. `EXP !is TYPE` can also be used to reverse the test.  We can use this operator within an `if` (single-branch conditional) or `when` (multi-branch conditional) statement.

For example, let us consider the following code:

```kotlin
var tmp2: String = “hello”
var v:Any = 12
TestType(v)
v = tmp2
TestType(v)
```
We define `TestType` as:

```kotlin
fun TestType(v:Any) 
{ 
    when (v) { 
        is Int -> Log.i("info","I'm Int") 
        is String -> Log.i("info","I'm String") 
        is Float -> Log.i("info","I'm Float") 
    } 
}
```

In this example, the first call will return `"I'm Int"` and the second one `"I’m String"`.


## 3.1.6. Not null check

Kotlin variables and expressions can have a `null` value.  As in any object-oriented language, accessing an object with a null value will cause a runtime error. 

Operator `?` (used after the object name) lets us conditionally make method call or access a property. The call or access is only performed if the object is not null.

```kotlin
Var m:Manager? = null
var t1:Element = Element()
m?.Add(t1)
```

Conversely, we can force the method call using operator `!!`:

```kotlin
m!!.Add(t1)
```

With this syntax, we are forcing the method call even if the value of variable `m` is null. In our example this would cause a runtime exception. Thus, before using this operator we need to ensure that the variable is not null.


