---
layout: default
title: 10.3. Using async and wait
parent: 10. Concurrency
nav_order: 3
---

# 10.3. Using async and wait

Sometimes, we want a thread to stop running concurrently. For instance, our current thread may need to wait for the result of a coroutine before continuing to run. Obviously, the main thread should not do that because our application would stop, but it may be necessary to do it in other threads.

In the example below we create a coroutine that will then stop to wait for another result:

```kotlin
val coroutineContext: coroutineContext =  Dispatchers.Default + SupervisorJob()
val scope: coroutinescope = coroutinescope(coroutineContext)

scope.launch() {
   val job_deferred: Deferred<Int> = async {
delay(1000L)
   	(..100).random()
   }

   val r = job_deferred.await()
   Log.i("coroutine await",r.toString())
}
```

First, `r = job_deferred.await()` is executed on thread `worker1`:

> ![Executing await.](/images/10/async1.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Executing await.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


This causes the thread `worker1` to stop until the coroutine started with `await` finishes and returns its result. Now we can see that the coroutine that has been launched is running on the thread called `worker2`.

> ![The coroutine is running while the original thread waits.](/images/10/async2.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *The coroutine is running while the original thread waits.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Finally, when `job_deferred` completes its execution, the coroutine running it stops and `worker1` resumes its execution having the result in variable `r`:

> ![After await.](/images/10/async3.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *After await.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

