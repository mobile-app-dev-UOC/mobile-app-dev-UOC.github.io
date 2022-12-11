---
layout: default
title: 12.1. Foreground services
parent: 12. Services
nav_order: 1
---

# 12.1. Foreground services

As previously mentioned, foreground services can send notifications. This is the main difference with respect to other types of services.

In all the examples of services that we are going to perform the objective will be for the service to perform the playback of an audio .mp3 file

First, we define a class `MyServiceForeground` that inherits from Service.

```kotlin
class MyServiceForeground : Service() {

   protected lateinit var mediaPlayer: MediaPlayer

   companion object {
       fun startService(context: Context, message: String) {
           val startIntent = Intent(context, MyServiceForeground::class.java)
           startIntent.putExtra("inputExtra", message)
           ContextCompat.startForegroundService(context, startIntent)
       }
       fun stopService(context: Context) {
           val stopIntent = Intent(context, MyServiceForeground::class.java)
           context.stopService(stopIntent)
       }
   }

   override fun onBind(intent: Intent): IBinder? {
       return null
   }

   override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {

       val pendingIntent: PendingIntent =
           Intent(this, MainActivity::class.java).let { notificationIntent ->
               PendingIntent.getActivity(this, 0, notificationIntent, 0)
           }

       val notification: Notification = Notification.Builder(this, "alert_channel")
           .setContentTitle("Play new track")
           .setContentText("")
           .setSmallIcon(R.drawable.ic_launcher_foreground)
           .setContentIntent(pendingIntent)
           .setTicker("")
           .build()

       startForeground(1, notification)

       mediaPlayer = MediaPlayer.create(this, R.raw.sound1)
       mediaPlayer?.start()

       return START_NOT_STICKY
   }
}
```

In this class we define two static methods (`companion object`) `startService` and `stopService` that will allow us to create and stop instances of this service in the foreground.

In the `manifest.xml` file we must declare the service:

```xml
<service
   android:name=".MyServiceForeground"
   android:enabled="true"
   android:exported="true" />
```

We also indicate that we will need permission to run foreground services.

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
```


Now from the main activity we can create the service using the static method `startService`  of our class `MyServiceForeground`:

```kotlin
MyServiceForeground.startService(this, "Foreground Service is running...")
```

Inside the static method `startService`, the service instance will be created. We can pass parameters to the service using the Intent. In this case we pass the message as a parameter.

```kotlin
       fun startService(context: Context, message: String) {
           val startIntent = Intent(context, MyServiceForeground::class.java)
           startIntent.putExtra("inputExtra", message)
           ContextCompat.startForegroundService(context, startIntent)
       }
```

As a result of running `startForegroundService`, the callback `onStartCommand` will be invoked. First, this call callback creates a notification to indicate that an audio file will start playing. The notification channels to be used must have been created earlier from the main activity.

The following code creates the notification:

```kotlin
val pendingIntent: PendingIntent =
   Intent(this, MainActivity::class.java).let { notificationIntent ->
       PendingIntent.getActivity(this, 0, notificationIntent, 0)
   }

val notification: Notification = Notification.Builder(this, "alert_channel")
   .setContentTitle("Play new track")
   .setContentText("")
   .setSmallIcon(R.drawable.ic_launcher_foreground)
   .setContentIntent(pendingIntent)
   .setTicker("")
   .build()

startForeground(1, notification)
```

Now we can add the code to start playing the audio file:

```kotlin
mediaPlayer = MediaPlayer.create(this, R.raw.sound1)
mediaPlayer?.start()
```

Since it is running on the same thread and process as the main activity, when we close the application the service is destroyed and playback is stopped.

