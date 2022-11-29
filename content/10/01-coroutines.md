---
layout: default
title: 10.1. Coroutines
parent: 10. Concurrency
nav_order: 1
---

# 10.1. Coroutines 

A **coroutine** is executed by a thread belonging to a given **Dispatcher**. A Dispatcher can be viewed as a pool of threads.

Coroutines and Dispatchers appear primarily to solve two problems:

1. Threading used to be abused for tasks such as uploading images from a remote server. If we loaded 1000 images, the developers would create 1000 threads. This caused an overload at the level of the operating system, both in terms of network and CPU usage. The Dispatchers solve that problem because the number of threads in each pool is controlled by the Kotlin coroutine library.
When the number of pool threads is exceeded, Kotlin locks the request until a thread from the pool becomes available.

2. Setting up a thread according to the intended usage is not easy. Different types of threads (complex computations, local input/output operations, network usage) require a different configuration. The Dispatchers perform the configuration of these threads internally in the Kotlin coroutine library.



Thus, we have three types of Dispatchers:
- **Default:** Common pool of Threads used for calculations. If there are multiple cores in the mobile device, it may be possible to distribute these threads among the available cores.
- **I/O:** This is the pool intended for input/output operations. Kotlin knows that the threads in this pool will spend a long time waiting for data and sets them up accordingly.
- **Main:** This is a pool with a single thread that is the main thread running the interface. It is useful for running code from a coroutine on the main thread.


The simplest usage scenario can be two coroutines running in parallel to write a message to the Log.

```kotlin
GlobalScope.launch(Dispatchers.Default) {
   for(i in 1..100){
       delay(1000L)
       Log.i("coroutine 1!",thread.currentthread().name.toString())
   }
}

GlobalScope.launch(Dispatchers.Default) {
   for(i in 1..100){
       delay(1000L)
       Log.i("coroutine 2!",thread.currentthread().name.toString())
   }

   GlobalScope.launch(Dispatchers.Main) {
       FromCoroutine()
   }
}
```

The first coroutine runs on the `Default` dispatcher. It is a loop that performs a hundred iterations by stopping for a second at each iteration and writing an iteration message to the Logcat console. Meanwhile, the second coroutine behaves exactly like the first one but, when it ends, it returns the control to the main thread running the `FromCoroutine` method.

Let us inspect the result of this code in the debug environment.

- The `main` thread is running the interface.

> ![Main thread](/images/10/thread-main.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Main thread.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

- The code of our first coroutine is run by the thread `worker1`.

> ![First coroutine](/images/10/thread1.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *First coroutine.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

- The code of our second coroutine is run by the thread `worker2`.

> ![Second coroutine](/images/10/thread2.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Second coroutine.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

## Scope

The `Scope` indicates the ambit where the coroutine should be executed.  If we use, as in the example, **GlobalScope**, we are using a global scope across the entire application which means that these coroutines will not be destroyed until they are completed, explicitly cancelled or the application is closed.

We can also create local contexts, so we can make the coroutine stop working for example by closing the activity that contains them.

Let us consider an example:
We create properties of our activity that will contain a `scope`, a context and two jobs that are references to two coroutines that we will create.

```kotlin
val coroutineContext: coroutineContext =  Dispatchers.Default + SupervisorJob()
val scope: coroutinescope = coroutinescope(coroutineContext)

lateinit var job1: Job
lateinit var job2: Job
```

Now in the `onCreate` callback of our activity we create the coroutines using the local scope we have previously created and assign them to the jobs.

```kotlin
job1 = scope.launch() {
   for(i in 1..100){
       delay(1000L)
       Log.i("coroutine 1!",thread.currentthread().name.toString())
   }
}

job2 = scope.launch() {
   for(i in 1..100){
       delay(1000L)
       Log.i("coroutine 2!",thread.currentthread().name.toString())
   }
}
```

At any time we can stop one of the coroutines by using `job1.cancel()` or `job2.cancel()`. And if we want to stop both at the same time, we can use `scope.cancel()`.




