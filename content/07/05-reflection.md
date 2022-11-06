---
layout: default
title: 7.5. Reflection
parent: 7. Advanced Kotlin features
nav_order: 5
---

# 7.5. Passing the type of a variable as a parameter (Reflection)

Kotlin libraries may have to deal with classes that are not known at compile-time. In order to ensure that our library interacts with unknown classes properly, it may want to specify some minimum requirements that those classes should satisfy, such as the existence of properties or methods with a given name. The techniques used to pass types as parameters, express conditions on types, or access methods and properties are called **reflection**.

To use reflection within our project, we have to add the following dependencies:

```gradle
dependencies {
   implementation("org.jetbrains.kotlin:kotlin-reflect:1.6.20")
}
```

Let us assume that the app calling our library includes the following class definition:

```kotlin
class User()
{
   public var name:String = ""
}

```

Then, when the application calls our library method like this:

```kotlin
var u = createClass(User::class)
```

... our library is able to create an instance of class `User` (even though we did not know anything about it when the library was implemented) and change the value of the name property.

```kotlin
fun createClass(c: KClass<*>):Any? {
   val ctor = c.constructors.elementAt(0)
   var r = ctor.call()
   var p = r.javaClass.kotlin.memberProperties.find { it.name == "name" }
   (p as KMutableProperty<*>).setter.call(r, "The name")
   return r
}
```

We can also invoke methods of unknown classes in a similar way.

Reflection is a very flexible and powerful feature. Nevertheless, the reflection library should be added only if our library or application requires those capabilities because it grows the size of the executable.

**Example:** The Retrofit library used for communications with REST APIs (we will see it in the backend section, [Section 11](/content/11/)) receives as a parameter the type of a class in our application, the class that describes the available operations of that API. The class received as parameter is not known by the Retrofit library.

```kotlin
val service = getRetrofit().create(APIService::class.java)
```
