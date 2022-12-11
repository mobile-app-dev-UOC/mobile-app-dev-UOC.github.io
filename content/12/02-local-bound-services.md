---
layout: default
title: 12.2. Local bound services
parent: 12. Services
nav_order: 2
---

# 12.2. Local bound services

In this scenario, the service cannot communicate with the outside. It is called “local bound” because it is used from the same application that creates it. It also runs on the same thread that creates it, in this case the main thread. If we want the service to perform tasks concurrently, we must use one of the mechanisms detailed in [Section 10](/contents/10/).

First, we declare the `MyService` class that inherits from `Service`. This class contains an internal `MP3ServiceBinder` class that provides the methods that can be used by the service client. In this case, the client will be our activity.

```kotlin
class MyService : Service() {

   inner class MP3ServiceBinder : Binder() {
       protected lateinit var mediaPlayer: MediaPlayer

       public fun startAudio() {
           mediaPlayer = MediaPlayer.create(this@MyService, R.raw.sound1)
           mediaPlayer?.start()
       }
   }

   private val serviceBinder: MP3ServiceBinder = MP3ServiceBinder()

   override fun onBind(intent: Intent): IBinder {
       return serviceBinder
   }
}
```

We declare it in the `manifest.xml` file:

```xml
<service
   android:name=".MyService"
   android:enabled="true"
   android:exported="true" />
```

Now we can use it from our activity. We declare a property in the activity of the type of the internal class that inherits from the binder of our service:

```kotlin
private lateinit var bindService: MyService.MP3ServiceBinder
```

We declare in the activity a static object of type `ServiceConnection` that will allow us to create the instance of the class `MyService.MP3ServiceBinder` declared above:

```kotlin
val sc = object: ServiceConnection {

   override fun onServiceConnected(className: ComponentName, service: IBinder) {
       bindService = service as MyService.MP3ServiceBinder
   }

   override fun onServiceDisconnected(arg0: ComponentName) {

   }
}
```

Now we can run the `bindService` method that will use the static `ServiceConnection` object to instantiate the `bindService` property.

```kotlin
val intent = Intent(this@MainActivity, MyService::class.java)
bindService(intent,sc, Context.BIND_AUTO_CREATE)
```

Once linked, we can starting using our methods:

```kotlin
bindService.startAudio();
```

Like in foreground services, closing the application also shuts down the service.


