---
layout: default
title: 4.1. Activities
parent: 4. Android app architecture
nav_order: 1
---

# 4.1. Activities

Activities are the key element in Android app development. They offer an interface that the user can interact with.

Each Android app has a **primary activity**. This primary activity can open new activities. 

The lifecycle of an activity describes the current state of an activity and how it can evolve. Activities are managed through an **activity stack**. When a new activity is launched, it is placed at the top of the activity stack and becomes the activity that is running in the foreground. Meanwhile, the previous activity falls below the activity that is running in the activity stack, and it will not return to the foreground until the activity above is finished. The activity may end because it decides to terminate itself or if the user performs the "back" operation on her device.

Broadly speaking, an activity can be in four different states:
- **Foreground activity:** it is on the top of the stack, i.e., the activity is running.
- **Activity paused:** the activity has lost focus, but it is still visible.
- **Activity stopped:** the activity has been replaced by another one and below it in the stack.
- **Activity completed or dead:** the activity is paused or stopped, and the system ends or kills it. 

The following image shows a schematic of the lifecycle of an activity:

> ![Lifecycle of an activity](/images/04/activity-lifecycle.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Lifecycle of an activity.*  
> Source: [Android Developers](https://developer.android.com/guide/components/activities/activity-lifecycle) License: [CC BY 2.5](http://creativecommons.org/licenses/by/2.5/)

>**Learn more:**
>The concept of activity is a hallmark of Android over iOS and other operating systems such as Linux, macOS, Windows, etc.
>
>Activities were concieved for two fundamental reasons. First, the operating system can reclaim memory by destroying activities that are in the background. Second, to enable one application to request services offered by other applications. For example, an application that wants to edit a photo can open a specific editor activity from another application that allows you to modify the image. 

## 4.1.1. Primary activity

Each Android app has a primary activity. This activity is the one that runs by default when you tap the application icon. The primary activity is specified in the AndroidManifest.xml file by specifying its `intent-filter` in the following way:

```xml
<intent-filter>
   <action android:name="android.intent.action.MAIN" />
   <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
```

Within the Activity we can distinguish two parts:
-	The **application logic**, a Kotlin file that contains a subclass inheriting from the class AppCompatActivity. We use it as the parent class to offer new functionality on older devices.
-	The **visualization**, a layout file or sets of files describing how information is presented to the user. Layouts will be presented in more detail in [Section 5.3](/content/05/03-layouts).

As previously mentioned, Android activities go through different states. In general, any object that goes through different states offers methods called **callbacks** that are invoked when the activity enters a certain state. If our activity needs to perform any action when reaching a given state, it needs to provide an implementation of the suitable callback.

The first step in the definition of an activity is connecting the application logic to the layout. This is done in the callback `onCreate()` of the activity class:

```kotlin
import com.uoc.activitities1.databinding.ActivityLoginBinding
class LoginActivity : AppCompatActivity() {

    private lateinit var binding: ActivityLoginBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityLoginBinding.inflate(layoutInflater)
        setContentView(binding.root)
    }

}
```

On the Gradle file for the app, we need to add the following: 

```gradle
buildFeatures {
   viewBinding true
}
```

This setting facilitates accessing the layout elements from the class. If we do not use this binding, we must use the `findViewById method` to access the UI controls.

```kotlin
val txtCtrl: EditText = findViewById(R.id.username)
```

We import the class that performs the binding using the following statement:

```kotlin
import com.uoc.activities1.databinding.ActivityLoginBinding
```

In this statement, we use the class name ActivityLoginBinding. This type name is created from the layout name by removing the underscores and adding the word Binding at the end.  For example, from `activity_login.xml` the type `ActivityLoginBinding` would be defined.

Finally, we create the class instance from the layout and assign it as the content for the Activity.

```kotlin
binding = ActivityLoginBinding.inflate(layoutInflater)
setContentView(binding.root)
```

## 4.1.2. Creating new activities from the primary activity 

In certain scenarios, we will need to create new activities (you will see a discussion on when it is appropriate to do so in [Section 4.4.](/content/04/04-1-n-vs-n-n)). To do this, we will create an instance of the Intent class indicating the class that represents the activity we want to use.

To pass parameters from one activity to another we use the `putExtra` method from Intent. 
The first parameter of this method is the key used to query the parameter from the target activity. Some of these identifiers are standard and allow us to use an activity outside of our application by passing parameters to it.

In addition, we may define a set of identifiers used internally in our application. The content of these parameters can be: 
- A basic type: String, Int
- An array  
- A string that stores a class in a given format, for example, JSON.
- A **parcelable class**. A parcelable class can be converted to a format that can be sent by a process communication mechanism and that can be automatically retrieved from that format on the receiver.

This would be an example of how to pass parameters to an activity:

```kotlin
val intent = Intent(this, ListActivity::class.java).apply {
    putExtra(USER_ID_MESSAGE,loginResult.success.userId)
    putExtra(PARAM_INT,12)
    putExtra(PARAM_ARRAY_INT, intArrayOf(1, 3, 6))
    putExtra(PARAM_PARCEL_CLASS, loginResult.success)
}
startActivity(intent)
```

In the `onCreate` method of the target activity, we can retrieve the parameter value using the `get<type>Extra` methods from Intent that correspond to the corresponding type.

```kotlin
class ListActivity : AppCompatActivity() {
    private lateinit var binding: ActivityListBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityListBinding.inflate(layoutInflater)
        setContentView(binding.root)

        val userId =  intent.getStringExtra(USER_ID_MESSAGE)
        val iValue:Int =  intent.getIntExtra(PARAM_INT,0)
        val aiValue: IntArray? = intent.getIntArrayExtra(PARAM_ARRAY_INT)
        val pClass: LoggedInUserView? = intent.getParcelableExtra<LoggedInUserView>(PARAM_PARCEL_CLASS)
   }
}
```

Activities can return results to the activity that created them. These results are treated in the same way as the parameters we use to start running the activity. That is, we use inter-process communication, so we can pass basic types and parcelable classes.

To receive a result, we need to register the activity. Access to the result is granted through a **contract**. Contracts were introduced to avoid having to overwrite the `onActivityResult` method. In the past, in this currently deprecated method, all responses were received from all the different activities that returned results. Then, we had to use conditional statements to determine which activity we were receiving results from, which made the code too complicated. This motivated the use of contracts.

Android provides a [set of standard contracts](https://developer.android.com/reference/androidx/activity/result/contract/ActivityResultContracts) that our apps can use. The most generic one is StartActivityForResult. Here is the code that needs to be added to the class that creates the activity:

```kotlin
var getResult  = registerForActivityResult(
    ActivityResultContracts.StartActivityForResult()) {
    if (it.resultCode == Activity.RESULT_OK) {
        val value = it.data?.getParcelableExtra<AddItemResult>(PARAM_PARCEL_CLASS)
    }
}

val intent = Intent(this, AddItem::class.java)
getResult.launch(intent)
```

In the activity that computes the results, when it is done we need to record the results (using the corresponding data type) and finish the activity.

```kotlin
val l:AddItemResult = AddItemResult("name", "text","content")
val intent = Intent()
intent.putExtra(PARAM_PARCEL_CLASS, l)
this.setResult(Activity.RESULT_OK,intent)
finish()
```

In the code above the AddItemResult statement is implemented as follows:

```kotlin
@Parcelize
data class AddItemResult(val name: String, val text: String, val url_content: String): Parcelable
```

>**Learn more:**
>In a lot of documentation, we talk about the possibility of using the [Singleton design pattern](https://en.wikipedia.org/wiki/Singleton_pattern) to have a single model shared by all activities. The Singleton pattern, for those unfamiliar with it, can be viewed as a kind of global variable. It appears that this approach should work in theory, but it does not always work in practice.
>
>Let us imagine that we have created the variable in the main activity of our app. We then launch other activities that can access this variable seamlessly. 
>
>When the Android operating system runs out of memory, it can decide to destroy activities to free resources. It usually starts with those activities that are not in the foreground. Then, if you destroy the activity that created our global variable, we will no longer be able to access it. Some authors provide solutions to this problem by recording the value of the singleton in permanent storage after each update. Nevertheless, this causes performance issues.
>
>This approach is sometimes combined with the ViewModels that will be introduced in [Section 4.2](/content/04/02-viewmodels). However, this version also suffers from the previously mentioned issues.

## 4.1.3. Creating activities from other applications

In addition to creating our own activities, we may also use a system activity or an activity from another application to perform specific tasks required by our application.

The most common example is accessing photos to use an image provided by the user. To this end, we create an intent but, in the intent, instead of specifying a class of our application, we indicate a type and an action. The activities that have been recorded with that type of action in the system will respond to our request.

```kotlin
val intent = Intent()
intent.type = "image/*"
intent.action = Intent.ACTION_GET_CONTENT
getResult.launch(intent)
```

To collect the result, we will have created the corresponding contract:

```kotlin
ar getResult  = registerForActivityResult(
   ActivityResultContracts.StartActivityForResult()) {
   val value = it.data?.getData()
   val inputStream: InputStream? = this.getContentResolver().openInputStream(value!!)
   val bitmap = BitmapFactory.decodeStream(inputStream)
   binding.itemImageNew.setImageBitmap(bitmap)
}
```

## 4.1.4. Completing activities

Activities are terminated with a call to the `finish` method. If the activity should return results, it is important to remember that the return value must be set before invoking `finish`.

```kotlin
val l:AddItemResult = AddItemResult("name1", "text1","content1")
val intent = Intent()
intent.putExtra(PARAM_PARCEL_CLASS, l)

this.setResult(Activity.RESULT_OK,intent)
finish()
```

## 4.1.5. Opening activities in a new task


Android systems with API version 24 or later that support multitasking allow opening an activity in a new task.

```kotlin
val intent = Intent(this, ScrollingActivity::class.java)
if(this.isInMultiWindowMode()) {
   intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
}
startActivity(intent)
```


