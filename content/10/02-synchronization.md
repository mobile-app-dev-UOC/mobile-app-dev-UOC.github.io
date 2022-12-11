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

Another solution is to create a mutual exclusion critical section that ensures that only one thread can execute that code; other threads trying to enter the mutual exclusion critical section will be stopped until the first one leaves it.

First, we declare a property with type `Mutex`:

```kotlin
private val mutex = Mutex()
```

Now we protect the previous `AddCounter2` method code with the mutex:

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

## 10.2.3. Atomic classes

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

## 10.2.4. Semaphores

Semaphores are another synchronization mechanism between threads. They have two fundamental differences with respect to mutex:

1. They allow us to control the number of threads that can access the critical section at a given point in time. For example, we can establish that at most three threads should be able to execute the critical section, if that makes sense for a particular problem.

2. We may allow or disallow entrance to a critical section from another piece of code.

Taking these differences into account, we can simulate a mutex with a semaphore indicating that only one thread can enter that code zone at a time. This is known as a **binary semaphore**. To build one, we create an instance of the `Semaphore` class and pass a parameter with the maximum number of threads that can access the critical section (in this case 1).

```kotlin
private val semaphore: Semaphore = Semaphore(1)
```

Then, we redefine `AddCounter` as follows, using the methods `acquire` and `release`: 

```kotlin
suspend fun AddToCounter3()
{
   Log.i("Semaphore Pre",thread.currentthread().name.toString())
   semaphore.acquire()
   Log.i("Semaphore In",thread.currentthread().name.toString())
   val rnd = (..100).random()
   counter2 += rnd
   semaphore.release()
   Log.i("Semaphore Post",thread.currentthread().name.toString())
}
```

Method `acquire` from class `Semaphore` checks if the value of the semaphore is not equal to zero. In that case, it allows the execution to continue and subtracts one from the value of the semaphore. Meanwhile, if this value is equal to zero, the thread stops.

On the other hand, method `release` adds one the value of the semaphore, allowing another thread to enter the critical section. If any thread is locked, it will be unlocked so it can continue its execution moving into the critical section.

If we execute this code, we will see in Logcat that there are no two consecutive `"Semaphore In"` messages with different thread names. We can also see that if the `worker-1` wants to enter the critical section, it has to wait until `worker-2` leaves and publishes the `"Semaphore Post"` message.

```

2022-04-29 09:41:06.049 12409-12480/com.uoc.cprogram I/Semaphore Pre: DefaultDispatcher-worker-1
2022-04-29 09:41:06.050 12409-12480/com.uoc.cprogram I/Semaphore In: DefaultDispatcher-worker-1
2022-04-29 09:41:06.050 12409-12480/com.uoc.cprogram I/Semaphore Post: DefaultDispatcher-worker-1
2022-04-29 09:41:07.054 12409-12481/com.uoc.cprogram I/Semaphore Pre: DefaultDispatcher-worker-2
2022-04-29 09:41:07.055 12409-12481/com.uoc.cprogram I/Semaphore In: DefaultDispatcher-worker-2
2022-04-29 09:41:07.055 12409-12480/com.uoc.cprogram I/Semaphore Pre: DefaultDispatcher-worker-1
2022-04-29 09:41:07.059 12409-12481/com.uoc.cprogram I/Semaphore Post: DefaultDispatcher-worker-2
2022-04-29 09:41:07.060 12409-12480/com.uoc.cprogram I/Semaphore In: DefaultDispatcher-worker-1
2022-04-29 09:41:07.060 12409-12480/com.uoc.cprogram I/Semaphore Post: DefaultDispatcher-worker-1
2022-04-29 09:41:08.065 12409-12480/com.uoc.cprogram I/Semaphore Pre: DefaultDispatcher-worker-1
```

If we instead initialize the maximum number of threads to 2:

```kotlin
private val semaphore: Semaphore = Semaphore(2)
``

Then, we will have two workers inside the critical section at the same time: 

```
022-04-29 09:56:46.675 12797-12843/com.uoc.cprogram I/Semaphore Pre: DefaultDispatcher-worker-1
2022-04-29 09:56:46.679 12797-12844/com.uoc.cprogram I/Semaphore Pre: DefaultDispatcher-worker-2
2022-04-29 09:56:46.680 12797-12844/com.uoc.cprogram I/Semaphore In: DefaultDispatcher-worker-2
2022-04-29 09:56:46.692 12797-12843/com.uoc.cprogram I/Semaphore In: DefaultDispatcher-worker-1
2022-04-29 09:56:46.704 12797-12843/com.uoc.cprogram I/Semaphore Post: DefaultDispatcher-worker-1
2022-04-29 09:56:46.710 12797-12844/com.uoc.cprogram I/Semaphore Post: DefaultDispatcher-worker-2
```

Nevertheless, in this particular example our code would not work as intended: the value of counter will not be updated accordingly because more than one thread can be reading and modifying the value of `counter2` at the same time. That is, this critical section is not implemented considering more than one simultaneous thread.

## 10.2.5. Deadlocks

A deadlock is a synchronization problem that occurs in concurrent systems when two or more threads are locked waiting for each other in a cyclic way. This usually happens when two semaphores are used in different snippets of code in reverse order and each snippet runs in a different thread.

For example, let us consider the following two methods:

```kotlin
suspend fun DeadLock1()
{
   semaphore1.acquire()
   semaphore2.acquire()
   val rnd = (..100).random()
   counter2 += rnd
   semaphore2.release()
   semaphore1.release()
}

suspend fun DeadLock2()
{
   semaphore2.acquire()
   semaphore1.acquire()
   val rnd = (..100).random()
   counter2 += rnd
   semaphore1.release()
   semaphore2.release()
}
```

Let us assume that there are two threads and `thread1` runs the method `DeadLock1` and method acquires the `semaphore1`. At the same time, `thread2` runs the method `DeadLock2` and acquires the `semaphore2`. If this happens, a deadlock will occur: `thread1` will be waiting for the `thread2` to release the `semaphore2` and `thread2` will be waiting for `thread1` to release the `semaphore1`. Thus, both threads will stop indefinitely, waiting for each other.



