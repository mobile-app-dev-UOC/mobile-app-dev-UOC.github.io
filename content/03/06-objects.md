---
layout: default
title: 3.6. Objects
parent: 3. The Kotlin programming language
nav_order: 6
---

# 3.6. Object-oriented programming with Kotlin

As we have already mentioned, all values in Kotlin are considered objects. Below we review how to define classes and operate with objects.

## 3.6.1. Basic syntax

In Kotlin, classes are identified by the keyword `class`. In the following sections we will extend this initial definition with new features.

```kotlin
class Element {

}
```

## 3.6.2. Constructors

A constructor is a special function that constructs objects of a certain type, using received values as parameters to initialize the properties of the object.

First, we have a *primary* constructor that is part of the syntax of the `class` statement.

```kotlin
class Element(name: String) { 
    var _name:String = name 
    var _num:Int = 0
}
```

We can also define secondary constructors with different number of parameters.

To define them, we use the keyword `constructor`. We call the primary constructor first, and then our secondary constructor runs. If we do not want to run the primary constructor, we can leave it empty and run it anyway. 

```kotlin
class Element(name: String){ 
    var _name:String = name 
    var _num:Int = 0

    constructor(name: String, num: Int): this(name) { 
    	_num = num 
    }
}
```

## 3.6.3. Visibility

Classes, properties, and methods have visibility modifiers: `private`, `protected`, `internal`, and `public`. By default, visibility is public.

- **Public:** It is visible from any other class.
- **Private:** It can only be used from a method of the same class.
- **Protected:** It can be used from a method of the same class or any class that inherits from it 
- **Internal:** The element can only be used from within the same module. We define module as a set of files that are compiled together.

## 3.6.4. Getters and setters

Getters are methods that run automatically when accessing the value of an object property and allow us to control the returned value. Setters run automatically when you want to change the value of a property and allow us to perform additional operations.

```kotlin
class Element(name: String){ 
    var _name:String = name 
    var _num:Int = 0 
        get() { 
    	    return field / 2 
        } 
        set(value) { 
    	    field = value * 2 
        }
}
```

Keyword `field` is used to represent the field we want to modify. Notice that the field name cannot be used directly because there would be an infinite recursive call (each mention of the field name would invoke the getter/setter again) that would cause an error.

## 3.6.5. Inheritance

Kotlin enables simple inheritance between classes. That is, a class can have at most one parent class. To allow inheritance, in the parent class you need to add the `open` keyword before the name.

```kotlin
open class Element(name: String)
```

Now we can create a new class that inherits from our Element class.

```kotlin
class AdvancedElement(name:String, age:Int) : Element(name) { 
    var _age:Int = age
}
```

Since the parent class primary constructor has a parameter, the child class primary constructor should have at least that parameter.

### 3.6.5.1. Order of execution of constructors

When there is inheritance, the parent class constructor is executed before the child class constructor.

### 3.6.5.2. Overriding parent-class methods

Subclasses may provide new implementations of methods defined in the parent class. This process is called **method overriding**. In addition to allowing inheritance, it is necessary to explicitly indicate in the parent class which methods can be overriden. To indicate this, we must use the keyword `open` once again.

```kotlin
open fun Info()
{
    Log.i("info", "I'm the parent")
}
```

Now we can override the method in a subclass using the `override` keyword .

### 3.6.5.3. Executing the parent class method

Within an overridden method, we can execute the parent’s class method using the `super` keyword.

```kotlin
override fun Info()
{
    super.Info()
    Log.i("info","I'm the child")
}
```

## 3.6.6. Interfaces

An interface is a set of methods and properties that a class must implmement to comply with it. The interface provides one way in which classes implementing it can communicate with other classes in our system.

Like Java, Kotlin does not allow multiple inheritance: a class cannot inherit from more than one class. But a class can implement more than one Interface.

For instance, let us imagine an interface that will allow a class to store string commands that change the value of their properties.

We define the interface like this:

```kotlin
interface HandleCommand { 
    var listCommands: List<String> 
    fun SendCommand(command:String) 
}
```

Now we indicate that our AdvancedElement class is going to implement this interface. We always place interfaces after the primary constructor and class. We need to implement the properties and methods that the Interface requires. If we do not implement all of them, we will receive an error message when compiling the class.

```kotlin
class AdvancedElement(name:String,age:Int) : Element(name), HandleCommand 
{
    override var listCommands: List<String>
        get() = TODO("Not yet implemented")
        set(value) {}

    override fun SendCommand(command: String) {
        TODO("Not yet implemented")
    }
}
```

## 3.6.7. Polymorphism

Polymorphism allows invoking different methods with the same name simply by changing the type or number of parameters.

```kotlin
public fun Add(v:Int)
{
   _num += v
}

public fun Add(v:Float)
{
    _num += v.toInt()
}
```

In this example, when the parameter is of type Int we can add it directly. When the parameter is of type Float it is necessary to convert to Int before adding it.

## 3.6.8. Class properties and methods

Class methods and properties are methods that do not belong to a particular instance of a class but to the entire class. This concept is what in Java are called *static attributes and methods* (via the `static` keyword).

For instance, assume we want to provide a unique incremental Int type key for each object of a class. We can save the value of the next available key to a property belonging to the entire class. Everything within the `companion object <name>` statement is the static part of the external class. In the following example ElementsFactory contains the class methods and properties part of the Element class.

```kotlin
class Element(name: String, value: Int) {

    companion object ElementsFactory {
        var counter:Int = 0
        public fun GetId():Int
        {
          counter++
          return counter
        }
    }

}

var elem2:Element = Element("cat", Element.ElementsFactory.GetId())
```

Another example is generating a custom comparison method that we can then use in collections so we can sort them according to our criteria. For example, let us define a way to sort objects of type Element, assuming that each object contains a name and a number.

```kotlin
class Element(name: String) {
    var _name:String = name
    var _num:Int = 0
}
```

First, we define the comparison as a class method:

```kotlin
class ElementComparator {
    companion object : Comparator<Element> {
        override fun compare(a: Element, b: Element): Int = when {
            a._num != b._num -> a._num - b._num
            else -> a._name.length - b._name.length
        }
    }
}
```

Now, we can test this comparison by adding two elements to a List:

```kotlin
var elem:Element = Element("dog")
var elem2:Element = Element("cat", 14)
var element_list:List<Element> = listOf(elem2,elem)
```

Then, we invoke method `sortedWith`:

```kotlin
var element_list2:List<Element> = element_list.sortedWith(ElementComparator)
```

## 3.6.9. Data Class

Kotlin offers a very fast way to create data classes. That is, classes that save data from our system.

If we use the keyword `data` in front of `class`, Kotlin will automatically create properties from the default constructor parameters and also create the most common methods in this class type.

In the data class, each parameter of the constructor should specify whether the property to be generated must be modifiable (`var`) or constant (`val`)

We define our data class as follows:

```kotlin
data class Person(var Id:String,var firstName:String, var lastName:String)
```

Now, we can use the data class we just defined:

```kotlin
var p:Person = Person("234234234","John","Williams")

var s:String = p.toString()

// s == "Person(Id=234234234, firstName=John, lastName=Williams)"

Log.i("info",p.firstName)
```
