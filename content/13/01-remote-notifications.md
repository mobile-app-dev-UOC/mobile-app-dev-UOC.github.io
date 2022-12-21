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

As we indicated in the backend section, we need to go to the Firebase home screen and create a new project.  For this project we agree to use Google Analytics, although it is not strictly necessary if we are only going to use Cloud Messaging functionality.  Then, we select the `Cloud Messaging` option.


> ![Using Cloud Messaging.](/images/13/cloud-messaging.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Using Cloud Messaging.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We add the app that will use these notifications with by tapping the platform type icon. In our case, we click on Android.

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
       <action android:name="com.google.Firebase.MESSAGING_EVENT" />
   </intent-filter>
</service>
```

We declare this class in our code:

```kotlin
class MyFirebaseMessagingService  : FirebaseMessagingService() {
    override fun onNewToken(token: String) {
        Log.d(TAG, "Refreshed token: $token")
        sendRegistrationToServer(token)
	}

    override fun onMessageReceived(remoteMessage: RemoteMessage) {
	
	}
}
```

In the `onCreate` callback of our main `Activity` we use the singleton instance of `FirebaseMessaging` to register our app to receive notifications.
 
```kotlin
FirebaseMessaging.getInstance().token.addOnCompleteListener(OnCompleteListener { task ->
   if (!task.isSuccessful) {
       Log.w(TAG, "Fetching FCM registration token failed", task.exception)
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

