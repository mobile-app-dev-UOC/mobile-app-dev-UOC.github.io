---
layout: default
title: 10.5. WorkManager
parent: 10. Concurrency
nav_order: 5
---

# 10.5. WorkManager

WorkManager allows us to run scheduled tasks. Once scheduled, these tasks are executed even if the application is closed. If they are already running, they will continue running even if the application is closed.


These scheduled tasks are often used to perform cache cleanup, performance improvements, backups, ... There are different types of tasks depending on their duration, time on which they start to run, periodicity, ... A task may be given execution requirements such as the need to be connected to a network, have enough battery, ...

To schedule tasks, the first setp would be creating a class that inherits from Worker and  implements our task. In our example, we would simply write from 1 to 1000 on the Logcat waiting one second between each write.

```kotlin
class MyWorker(val context: Context,
              workerParams: WorkerParameters
) : Worker(context, workerParams) {

   override fun doWork(): Result {
      for(i in 1..1000){
           Log.d("WorkManager",i.toString())
           thread.sleep(1000)
      }
       return Result.success()
   }
}
```


From our app we indicate that we want to schedule the task performed by this class performs. We set a delay (in this example, 60 seconds) before this scheduled task begins:

```kotlin
val work =
   OneTimeWorkRequestBuilder<MyWorker>()
       .setInitialDelay(60, java.util.concurrent.TimeUnit.SECONDS)
       .addTag("worker_test")
       .build()

WorkManager.getInstance(this).enqueue(work)
```

Now we can close the app in the emulator and, using the `attach` button, connect the debugger to our app again. Then, we will see that the code of our class continues to run even after we closed our app.

> ![The scheduled task continues to run after the original app was closed.](/images/10/workmanager.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *The scheduled task continues to run after the original app was closed.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)