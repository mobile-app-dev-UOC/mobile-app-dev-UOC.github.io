---
layout: default
title: 13.2. Local notifications
parent: 13. Notifications
nav_order: 2
---

# 13.2. Local notifications

Local notifications are generated from the device itself. There are many situations where they are needed: timers, indicating that a background task has been completed, ...

Just like remote notifications, local notifications are organized into channels. For example, we can define these channels in the `onCreate` callback of our activity.

```kotlin
If (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
   val name = “alert_channel”
   val descriptionText = “error alerts”
   val importance = NotificationManager.IMPORTANCE_DEFAULT
   val mChannel = NotificationChannel(“alert_channel”, name, importance)
   mChannel.description = descriptionText
   val notificationManager = getSystemService(NOTIFICATION_SERVICE) as NotificationManager
   notificationManager.createNotificationChannel(mChannel)
}
```

In this case we have indicated `NotificationManager.IMPORTANCE_DEFAULT` which is the default importance of notifications for this channel. If we had assigned  `NotificationManager.IMPORTANCE_HIGH` we would be indicating that we want this notification to always be on screen above everything else. Despite this, it is the user who has the last word about the importance of notifications since they are also set in "`Settings \ Apps & Notifications`".

The minimum code for launching a notification when the user taps on an interface element would be the following:

```kotlin
binding.txtHello.setOnClickListener {

   val builder = NotificationCompat.Builder(context, "alert_channel")
      .setSmallIcon(R.drawable.ic_launcher_background)
      .setContentTitle("notification 1")
      .setContentText("Hello from the app!.")

   with(NotificationManagerCompat.from(context)) {
     notify(Random.nextInt(), builder.build())
   }
}
```

> ![Example of a simple local notification.](/images/13/local-notification.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Example of a simple local notification.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

For local notifications, we can also add buttons that trigger actions. For example, we are going to create a notification with two buttons: "`ok`" and "`cancel`".  

To do this, we use the `addAction` method that receives a `PendingIntent` as a parameter. This `PendingIntent` contains the action and parameters that we want to be executed when the notification button is pressed.  A `PendingIntent` is created because the call will not be made from our own app, but from the system notification inbox (which is managed by Android).

```kotlin
var okIntent = Intent(this, MainActivity::class.java)
okIntent.setAction("okUser")
val okIntentPending = PendingIntent.getActivity(this, System.currentTimeMillis().toInt(), okIntent, 0)

val cancelIntent = Intent(this, MainActivity::class.java)
cancelIntent.setAction("cancelUser")
val cancelIntentPending = PendingIntent.getActivity(this, System.currentTimeMillis().toInt(), cancelIntent, 0)


val builder = NotificationCompat.Builder(this, "alert_channel")
   .setSmallIcon(R.drawable.ic_launcher_background)
   .setVisibility(VISIBILITY_PUBLIC)
   .setContentTitle("notification 2")
   .setContentText("Hello from the app!.")
   .addAction(NotificationCompat.Action(R.drawable.ic_launcher_background, "Ok",okIntentPending))
    .addAction(NotificationCompat.Action(R.drawable.ic_launcher_background, "Cancel", cancelIntentPending))
   .setPriority(IMPORTANCE_HIGH)
   .setDefaults(Notification.DEFAULT_VIBRATE)

with(NotificationManagerCompat.from(this)) {
   notify(Random.nextInt(), builder.build())
}
```

> ![Example of a local notification with two buttons.](/images/13/local-two-buttons.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Example of a local notification with two buttons.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Pressing one of the buttons will execute the class `Activity` that we have indicated in the  `addAction` method. In the `onCreate` method of that Activity, we must analyze whether it is a call caused by one of these buttons and act accordingly.

```kotlin
if (getIntent() != null && getIntent().getAction() != null) {
   if (getIntent().getAction()=="okUser") {
       okUser()
   }
   else if (getIntent().getAction()=="cancelUser") {
       cancelUser()
   }
}
```

As we discussed, one of the common uses of local notifications is to alert the user that a timer has ended. As we have seen in the concurrency section ([Section 10.5](/content/10/05-workmanager)) we create a `Worker` instructing the `WorkManager` to run after a certain amount of time has passed. In the example shown below, we define a time-out after 5 seconds:

```kotlin
val work =
   OneTimeWorkRequestBuilder<MyWorker>()
       .setInitialDelay(5, java.util.concurrent.TimeUnit.SECONDS)
       .addTag("worker_notification")
       .build()

WorkManager.getInstance(this).enqueue(work)
```

Our `MyWorker` class will launch the notification.


```kotlin
class MyWorker(val context: Context,
              workerParams: WorkerParameters
) : Worker(context, workerParams) {

   override fun doWork(): Result {

       val builder = NotificationCompat.Builder(context, "alert_channel")
           .setSmallIcon(R.drawable.ic_launcher_background)

           .setContentTitle("notification 1")
           .setContentText("Hello from the app!.")


       with(NotificationManagerCompat.from(context)) {
           notify(Random.nextInt(), builder.build())
       }

       return Result.success()
   }
}
```

We need to remember that the `WorkManager` is another process and, therefore, even if we close the application the `WorkManager` will continue running and send the notification.