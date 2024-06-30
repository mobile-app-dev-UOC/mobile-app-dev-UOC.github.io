---
layout: default
title: 13.1. Remote notifications
parent: 13. Notifications
nav_order: 1
---

# 13.1. Remote notifications

As we discussed earlier, remote notifications originate from a back-end.  This back-end must contain two types of components:

- The component sending the notification 
- The business logic component that decides when and what notification to send.

On many occasions, this business logic is maintained on an proprietary backend and a third-party back-end is only used to send the notification. 


> ![Configuration with two back-ends: a proprietary one for the business logic and a third-party back-end for sending remote notifications.](/images/13/double-back-end.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Configuration with two back-ends: a proprietary one for the business logic and a third-party back-end for sending remote notifications.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

There are several reasons to use two different back-ends. On the one hand, the business logic can be very complex and it may have to interact with large volumes of data stored in our corporate databases. On the other hand, the notification is delegated to a third-party provider such as Firebase because that way we can abstract away the type of device, Android or iOS, to which we are sending the notification.

Conversely, if our business logic is simple and we do not want to use proprietary servers, we can use a third-party vendor like Firebase as our only back-end.

> ![Configuration with a single third-party back-end.](/images/13/single-back-end.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Configuration with a single third-party back-end.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

 
In this case Firebase uses Analytics to collect information about application usage and uses Functions to be able to program snippets of code that can trigger notifications about a set of devices.

In this section, we are not going to focus on the business logic, which would be different in each application. We are going to focus only on the process of sending and receiving notifications and we will use Firebase to perform this task. 

---

## Setting up Firebase

As we indicated in the backend section, we need to go to the Firebase home screen and create a new project.  For this project we agree to use Google Analytics, although it is not strictly necessary if we are only going to use Cloud Messaging functionality.  Then, we select the `Cloud Messaging` option.


> ![Using Cloud Messaging.](/images/13/cloud-messaging.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Using Cloud Messaging.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We add the app that will use these notifications with by clicking the platform type icon. In our case, we click on Android.

Then, we enter the unique identifier of our application and optionally an identifier to be used within Firebase.

> ![Setting up the app identifier.](/images/13/app-id.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Setting up the app identifier.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

After this we download the `google-services.json` file that we will leave at the root of our application just like we did in the back-end section ([Section 11](/content/11/)) when we used the database functionality in back-end.

> ![Storing the JSON file.](/images/13/json.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Storing the JSON file.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

The Gradle file must contain the necessary plugins:

```gradle
plugins {
   id 'com.android.application'
   id 'kotlin-android'
   id 'com.google.gms.google-services'
}
```

In the `dependencies` section, there must be the following:

```gradle
dependencies {
   implementation platform('com.google.Firebase:Firebase-bom:29.0.3')
   implementation 'com.google.Firebase:Firebase-analytics'
   implementation 'com.google.Firebase:Firebase-messaging-ktx'
}
```

In the `manifest.xml` file we must create a service that will be responsible for responding to events related to the Firebase messaging system.

```xml
<service
   android:name=".MyFirebaseMessagingService"
   android:exported="false">
   <tent-filter>
       <action android:name="com.google.firebase.MESSAGING_EVENT" />
   </intent-filter>
</service>
```

We declare this class in our code:

```kotlin
class MyFirebaseMessagingService  : FirebaseMessagingService() {
    override fun onNewToken(token: String) {
        Log.d("Messages", "Refreshed token: $token")

	}

    override fun onMessageReceived(remoteMessage: RemoteMessage) {
	
	}
}
```

In the `onCreate` callback of our main `Activity` we use the singleton instance of `FirebaseMessaging` to register our app to receive notifications.
 
```kotlin
FirebaseMessaging.getInstance().token.addOnCompleteListener(OnCompleteListener { task ->
   if (!task.isSuccessful) {
       Log.w("Messages", "Fetching FCM registration token failed", task.exception)
       return@OnCompleteListener
   }

   // Get new FCM registration token
   val token = task.result
})
```

In `token` we receive the unique identifier of our app on that device in Firebase. A token looks like this:

```
cQXY11yGRayDBq_8flcERi:APA91bEBOQeuq4IkUCvp8L07dxFsXOiPpHNN37Bc9-851q5DAshGg3tHho97Boz6M0e5nMIox8zbWePr74Sa_eCwd0N1uVWkp9uFnwFHO-DIaCuj71sMyDoBQ_bVRxoO1AzJZHg_oR7V
```

If this is the first time that we have registered our app, the callback `onNewToken` from the `MyFirebaseMessagingService` will also receive the token. In this way, if we are implementing the business logic in another server, we can send this token to our other back-end. 

Although this token is typically small, Google indicates it can grow up to 4KB in size.

---

## Sending a test message

We can now send our first test message from the Firebase console itself.

> ![Sending a test message (step 1).](/images/13/test-message.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Sending a test message (step 1).*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Click on "`Send your first message`". On the next screen we indicate a title, a text and click on "`Send test message`".

> ![Sending a test message (step 2).](/images/13/test-message2.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Sending a test message (step 2).*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, Firebase requests which tokens we want the test message sent to. The token can be easily known by placing a breakpoint in the previous code that we have placed in the main `Activity`.

> ![Select target token.](/images/13/select-token.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Select target token).*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We tap the `+` button and then the "`Test`"" button. Then, the notification will be sent. The time required to receive the notification may depend on many factors like our network, Google Cloud Messages load, ...

When it is received, if the application is in the foreground, the method `onMessageReceived`will be executed.

```kotlin
override fun onMessageReceived(remoteMessage: RemoteMessage) {
    Log.d(TAG, "From: ${remoteMessage.from}")

   // Check if message contains a data payload.
   if (remoteMessage.data.isNotEmpty()) {
       Log.d(TAG, "Message data payload: ${remoteMessage.data}")

       if (/* Check if data needs to be processed by long running job */ true) {
           // For long-running tasks (10 seconds or more) use WorkManager.
           scheduleJob()
       } else {
           // Handle message within 10 seconds
           handleNow()
       }
   }

   // Check if message contains a notification payload.
   remoteMessage.notification?.let {
       Log.d(TAG, "Message Notification Body: ${it.body}")
   }

   // Also if you intend on generating your own notifications as a result of a received FCM
   // message, here is where that should be initiated. See sendNotification method below.
}
```

The basic components of a notification are: title, text, image, sound indicator, and a set of parameters in the form of (key, value) pairs. These parameters can be entered from the Firebase Cloud Messaging Console.

The `time_to_live` parameter that indicates how long the notification should remain on Firebase servers if you have not been able to deliver it is also important. By default, `time_to_live` is usually one month. This is why our device receives a lot of push notifications when it is off for a while and powers up again.

> ![Customizing a notification in the Firebase Cloud Messaging Console.](/images/13/notification-components.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Customizing a notification in the Firebase Cloud Messaging Console.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

---

## Processing notifications

Processing a notification can be slow or fast, depending on what it means for our app to receive that notification. 

The example code simply states that if the notification carries additional data, this notification must be executed on a separate thread using the `scheduleJob` method used by the `WorkManager` we studied in the concurrency section ([Section 10.5](/content/10/05-workmanager)).

> ![Debugging the code that receives a remote notification.](/images/13/debug-notification.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Debugging the code that receives a remote notification.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

```kotlin
private fun scheduleJob() {
   // [START dispatch_job]
  val work = OneTimeWorkRequest.Builder(MyWorker::class.java).build()
   WorkManager.getInstance(this).beginWith(work).enqueue()
   // [END dispatch_job]
}
```

Let us remember that by default notifications are processed on the main thread, so if we anticipate that processing may take some time, a concurrent scheduling mechanism needs to be used.

If the application is turned off or in the background, the above code will not be executed. Instead, the notification appears in the system notification inbox. Clicking on the notification opens the app. 


To find out if the app has started because the user clicked on a notification, in the activity's `onCreate` method we can explore the extras to see if we find the parameters of a notification.

```kotlin

if (attempt.extras != null) {
   for (key in intent.extras!!.keySet()) {
       val value = intent.extras!!.getString(key)
       Log.d(TAG, “Key: $key Value: $value”)
   }
}
```

In this case, in the Logcat window we observe the following:

```
2022-04-21 2:23:53.773 17193-17193/com.uoc.notifications D/MainActivity: Key: google.ttl Value: null
2022-04-21 14:23:53.780 17193-17193/com.uoc.notifications D/MainActivity: Key: google.original_priority Value: high
2022-04-21 2:23:53.787 17193-17193/com.uoc.notifications D/MainActivity: Key: age Value: 23
2022-04-21 2:23:53.796 17193-17193/com.uoc.notifications D/MainActivity: Key: from Value: 854165052084
2022-04-21 2:23:53.804 17193-17193/com.uoc.notifications D/MainActivity: Key: name Value: John
2022-04-21 14:23:53.812 17193-17193/com.uoc.notifications D/MainActivity: Key: google.message_id Value: 0:1650543757735526%7f81b5967f81b596
2022-04-21 14:23:53.821 17193-17193/com.uoc.notifications W/Bundle: Key gcm.n.analytics_data expected String but value was a android.os.Bundle.  The default value <null> was returned.
2022-04-21 14:23:53.881 17193-17193/com.uoc.notifications D/MainActivity: Key: gcm.n.analytics_data Value: null
2022-04-21 14:23:53.890 17193-17193/com.uoc.notifications D/MainActivity: Key: collapse_key Value: com.uoc.notifications
```

A typical example of an alert with parameters is the arrival of a notification of a particular news item from a news application. When we click on the notification, the app should not start in the main index: it should position itself in the particular news item mentioned in the notification. To achieve this, the news identifier goes in one of these  (key, value) parameters.


---

## Channels

From Android 8.0 (API 26), notifications are assigned to a channel. This is done so that the user can configure and even disable notifications by channel, from the menu "`Android Settings \ Apps & Notifications`". Previously, all app notifications had to be turned off, without being able to customize our decision. Currently we can disable only those from a given channel while leaving the others active, providing a finer control to the user.

Within the callback `onCreate` in our `MainActivity`, we can specify the channel of our notifications if the Android version supports it.

```kotlin

if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
   // Create channel to show notifications.
    val channelId = "channelId"
   val channelName = "channelName"
   val notificationManager = getSystemService(NotificationManager::class.java)
   notificationManager?.createNotificationChannel(NotificationChannel(channelId,
       channelName, NotificationManager.IMPORTANCE_LOW))
}
```

From Firebase we can populate the channel identifier:

> ![Defining the channel ID.](/images/13/channel-id.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Defining the channel ID.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


---

## Customizing notifications

Firebase notifications cannot be customized, for example by adding buttons for quick response. In this case, the notification that appears in the inbox is generated by the Firebase library itself. However, Firebase may offer this possibility in the future. In any case, Firebase’s philosophy is to separate the concept of notification from a particular platform, so when they implement it they are sure to provide a way to add certain specific buttons from the Firebase console itself.

What we can choose from Firebase is to send the notification with a small image or a large image.

> ![Notification with small versus large images.](/images/13/image-notification.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Notification with small versus large images.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

> **Learn more:**
> In this section we have been using the Firebase Cloud Messaging console for testing. In architectures with proprietary back-ends, back-end developers should integrate the Firebase library that allows sending notifications into their proprietary back-end.


