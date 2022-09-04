---
layout: default
title: 3.3. Methods
parent: 3. The Kotlin programming language
nav_order: 3
---

# 3.3. Methods

In object-oriented programming, functions offered by objects are called. A method is declared using the reserved keyword `fun`.

## 3.3.1. Return type and value

A function can return a single value or none. If it does not return any value, no information needs to be added to the method. If the method returns a value, it is necessary to declare the result type using `:` after declaring all parameters. The result is return within the code of the method code using the keyword `return`.

```kotlin
public fun GetText():String 
{ 
	return _name + " " + _num.toString() 
}
```

## 3.3.2. Default arguments

We can indicate default values for parameters in a method declaration. In this way, users do not need to pass these values explicitly when the method is invoked if they are using the default values.

For example, if we declare the following method:

```kotlin
fun voltage(resistance: Double = 1.3)
{
	// Code goes here
}
```

The method can be invoked without arguments if parameter `resistance` has the default value `1.3`:

```kotlin
voltage()
```

## 3.3.3. Named arguments

When we invoke a method, we can explicitly specify the name of the parameters we are passing. This allows us to change the order of the parameters with respect to the definition of the method. This functionality may not seem very useful, but if there are four parameters with default values and we just want to use a non-default value for a fifth one, we can simply invoke the method indicating the name of that parameter.


## 3.3.4. Passing parameters by value or by reference

Class and Array parameters (which we will see below) are always passed **by reference** (that is, changes to these parameters are visible from outside the method).

On the other hand, parameters with basic types are passed **by value**: they are treated as if they were variables declared as `val` within the method. That is, the input value cannot be modified.



