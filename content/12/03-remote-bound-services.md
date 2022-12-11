---
layout: default
title: 12.3. Remote bound services
parent: 12. Services
nav_order: 3
---

# 12.3. Remote bound services


Remote services can be run from applications other than those containing them. In this case, if they run from the same application that contains them, they do stop when the application is closed. However, if they run from another application, the service still runs even if the application that made the request stops.

We declare a `MyServiceRemote` class that inherits from `Service`. This class has an internal class that inherits from `Handler`. In this class, the messages for the methods we want to run will be received.

```kotlin
class MyServiceRemote : Service() {

   inner class MessagesHandler : Handler(Looper.getMainLooper()) {
    
       protected lateinit var mediaPlayer: MediaPlayer

       override fun handleMessage(msg: Message) {
           when (msg.what) {
               MSG_PLAY_SOUND -> {
                   mediaPlayer = MediaPlayer.create(this@MyServiceRemote, R.raw.sound1)
                   mediaPlayer?.start()

                   val responseMessage:Message = Message.obtain(null,MSG_PLAY_SOUND);
                   responseMessage.arg1 = 100

                   msg.replyTo.send(responseMessage)
               }
               else -> super.handleMessage(msg)
           }
       }
   }


   val mMessenger: Messenger = Messenger(MessagesHandler())

   override fun onBind(p0: Intent?): IBinder? {
       return mMessenger.getBinder();
   }

}
```

We need to declare this service in the `manifest.xml` file:

```xml
<service
   android:name=".MyServiceRemote"
   android:enabled="true"
   android:exported="true" />
```

For remote services, it is very important that the `android:exported` attribute is assigned to true because we want to export the service for use from an external application. 

In the main activity we declare an inner class `ReceiveHandler` where we will receive the answers to our service calls.

```kotlin
inner class ReceiveHandler : Handler(Looper.getMainLooper()) {
   override fun handleMessage(msg: Message) {
      Log.d("Response",msg.arg1.toString())
   }
}
```

We also declare a static object of type `ServiceConnection` that will allow us to create the instances of the Messenger classes: one to send the method execution requests and another to receive the answers.

```kotlin
val scRemote = object: ServiceConnection {

   override fun onServiceConnected(className: ComponentName, service: IBinder) {
       mRemoteService  = Messenger(service)
       mRemoteServiceReceiveMessenger = Messenger(ReceiveHandler())

   }

   override fun onServiceDisconnected(arg0: ComponentName) {

   }
}
```

We can now create the service. To create the service, we must indicate the name of the package containing it and the full name of the service.

```kotlin
intent  = Intent()
intent.setComponent(ComponentName("com.uoc.services", "com.uoc.services.MyServiceRemote"))
bindService(intent, scRemote, BIND_AUTO_CREATE)
```

Once the service has been created, we can invoke its methods. In this case, we indicate the `MSG_PLAY_SOUND` method. In the `Message` we can add additional parameters. We indicate in the `replyTo` the property of the Activity that contains the `Messenger` where we want to receive the responses.

```kotlin
msec value: Message = Message.obtain(null, MSG_PLAY_SOUND, 0, 0)
msg.replyTo = mRemoteServiceReceiveMessenger
mRemoteService?.send(msg)
```

Within the service, the method `handleMessage` is invoked. Depending on the `what` property of `Message`, one code or another is executed. In this case, the following is executed:

```kotlin
override fun handleMessage(msg: Message) {
   when (msg.what) {
       MSG_PLAY_SOUND -> {
           mediaPlayer = MediaPlayer.create(this@MyServiceRemote, R.raw.sound1)
           mediaPlayer?.start()

           val responseMessage:Message = Message.obtain(null,MSG_PLAY_SOUND);
           responseMessage.arg1 = 100

           msg.replyTo.send(responseMessage)
       }
       else -> super.handleMessage(msg)
   }
}
```

After starting to play the audio file, a response message is created with the following snippet:

```kotlin
val responseMessage:Message = Message.obtain(null,MSG_PLAY_SOUND);
  responseMessage.arg1 = 100
  msg.replyTo.send(responseMessage)
```


That response message will reach the `ReceiveHandler` that we declared in our main activity.

```kotlin
inner class ReceiveHandler : Handler(Looper.getMainLooper()) {
   override fun handleMessage(msg: Message) {
      Log.d("Response",msg.arg1.toString())
   }
}
```

Remember that, in this scenario, even if we destroy the application that invoked the service remotely (that is, it is a different application) the service will continue to run. That is, in this case the audio file will continue playing.

>**Learn more:**
>Messages to a remote service are queued to a message queue. These messages run one after the other on a first-come, first-served basis. In the event that multiple applications use the same service or we generally want more than one message to run at a time, we have to use concurrent programming techniques.

> ![Invoking a remote service.](/images/12/remote.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Invoking a remote service.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

