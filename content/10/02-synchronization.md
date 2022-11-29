---
layout: default
title: 10.2. Synchronization
parent: 10. Concurrency
nav_order: 2
---

# 10.2. Synchronization

Synchronization mechanisms are critical when using concurrent programming. Basically, these mechanisms allow us to securely access information shared by various coroutines.

Let us consider the following incorrect code:

```kotlin
var counter2 = 0

GlobalScope.launch(Dispatchers.Default) {
   for(i in 1..100){
       delay(1000L)
       AddToCounter2()
   }
}

GlobalScope.launch(Dispatchers.Default) {
   for(i in 1..100){
       delay(1000L)
       AddToCounter2()
   }
}

fun AddToCounter2()
{
     val rnd = (0..100).random()
     counter2 += rnd   
}
```

In this case `counter2` is a property of the class, so the two coroutines can access it. Both coroutines use the `AddToCounter2` method. This method generates a random number and adds it to `counter2`. However, as both coroutines are adding values to `counter2`concurrently, both may read a value from `counter2` at the same time and then only the slowest coroutine will modify the property. This happens because Kotlin code becomes bytecode and that a simple operation such as `counter++` is not atomic. Instead, it can be transformed into a set of operations in bytecode similar to:

```
iload_1		// Load register one with counter
iconst_1	// Load constant one with one
isum		// Sum previous values
istore_1	// Store register one in counter variable
```

Since threads can be executed in any order, it can happen that a thread is interrupted right after reading the value, `iload_1`. Then, when recording the sum, it may overwrite the other one, so in the end only one addition is performed.

To avoid these types of phenomena, we have to use synchronization mechanisms.

## 10.2.1. Volatile

This mechanism is misleading. Adding `@volatile` indicates that only one thread can read or write to a variable or property at a time.

```kotlin
@volatile 
var counter = 0
```

However, as we previously discussed, an operation as simple as `counter++` becomes a sequence of bytecode operations. So even if we add the annotation `@volatile`, it doesn’t solve the problem for us.

## 10.2.2. Mutex

Another solution is to create a mutual exclusion zone that ensures that only one thread can execute that code; other threads trying to enter the mutual exclusion zone will be stopped until the first one leaves it.

First, we declare a property with type `Mutex`:

```kotlin
private val mutex = Mutex()
```

Now we protect the previous AddCounter2 method code with the mutex

```kotlin
suspend fun AddToCounter2()
{
   mutex.withLock {
       val rnd = (..100).random()
       counter2 += rnd
   }
}
```

>**Learn more:**
> The `suspend` keyword indicates that the method can be suspended and resumed in the middle of its execution. Since it has a mutex inside it is clear that for some threads the method will be suspended and resumed.

If instead of calling a method we were to run directly on the body of the coroutine `counter++` we would also have to protect it with a mutex:

```kotlin
GlobalScope.launch(Dispatchers.Default) {
   for(i in 1..100){
      delay(1000L)
      mutex.withLock {
         counter++
      }
   }
}
```

## 10.2.4. Atomic classes

We can declare basic types ready for use in concurrent environments. For example, we can declare another counter of type AtomicInteger.

```kotlin
var counter3: AtomicInteger = AtomicInteger()
```

We initialize it with a value:

```kotlin
counter3.set(0)
```

And now if we want to increase it safely from the point of view of concurreny, we use one of the following:

```kotlin
counter3.addAndGet(100)
// OR
counter3.incrementAndGet()
```






