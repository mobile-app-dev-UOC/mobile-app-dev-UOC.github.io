---
layout: default
title: 10.4. Actors
parent: 10. Concurrency
nav_order: 4
---

# 10.4. Actors

**Actors** are executed throughout the lifetime of the application. These actors typically need non-blocking communication mechanisms with other threads. This is accomplished using **channels**.

> ![Actors and Channels.](/images/10/actors.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Actors and Channels.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Kotlin’s couroutine library provided a mechanism for creating actors but was marked as deprecated by Google. In this documentation we present you with a simple alternative to implementing Actors. 

We can model an Actor or subsystem with a class that has this structure:

```kotlin
class MySubsystem {
   val channel = Channel<String>()

   public fun sendMessage(msg:String)
   {
       GlobalScope.launch(Dispatchers.Main) {
           channel.send(msg)
       }
   }

   public fun start()
   {
       GlobalScope.launch(Dispatchers.Default) {
           while(isActive){
               val msg = channel.receive()

               if(msg=="quit") {
                   cancel()
               }
               else if(msg=="log") {
                   Log.i("Susbsystem!",thread.currentthread().name.toString())
               }

           }
       }
   }
}
```

The class has a `Start` method that runs an infinite loop within the coroutine while the coroutine is active and is permanently running the `channel.receive()` method.

This method locks the thread until a message is received. Depending on the message received, it runs one piece of code or another: if it receives the “log” message, it writes the message in the Log window; on the other hand, if it receives the “quit” message, the subsystem stops because the coroutine is no longer `Active`.

In this example, we use a string channel. However, we can also use channels for message types defined by us, which can require more complex data structures.

There is also a `send` method that can be used by any thread to queue messages that should be processed by the subsystem.

