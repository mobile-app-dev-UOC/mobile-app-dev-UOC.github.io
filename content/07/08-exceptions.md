---
layout: default
title: 7.8. Exceptions
parent: 7. Advanced Kotlin features
nav_order: 8
---

# 7.8. Exceptions

Exceptions offer a mechanism to capture run-time errors in our code. Using exceptions in a proper way produces more robust code. Its syntax is very similar to Java:

```kotlin
try {
   	// Code to watch
}
catch (e: Exception Type to catch)
{
 	// if we have an exception then execute this code
}
```

In the following example we try to convert a string that does not contain a number to Int.

```kotlin
var number : Int
try {
   val str =  "hello"
   num = str.toInt()
}
catch (e: NumberFormatException)
{
 	Log.d("error",e.stackTraceToString())
}
```

In this snippet, we use `stackTraceToString` to display the error message and call stack in the Logcat window. In this example, we would see the following information in Logcat:

```
2022-04-08 15:36:15.200 26410-26410/com.uoc.kotlin_advanced D/error: java.lang.NumberFormatException: For input string: “help”
        at java.lang.Integer.parseInt(Integer.java:615)
        at java.lang.Integer.parseInt(Integer.java:650)
        at com.uoc.kotlin_advanced.MainActivity.onCreate(MainActivity.kt:106)
        at android.app.Activity.performCreate(Activity.java:7136)
        at android.app.Activity.performCreate(Activity.java:7127)
        at android.app.Instrumentation.callActivityOnCreate(Instrumentation.java:1271)
        at android.app.ActivityThread.performLaunchActivity(ActivityThread.java:2893)
        at android.app.ActivityThread.handleLaunchActivity(ActivityThread.java:3048)
        at android.app.servertransaction.LaunchActivityItem.execute(LaunchActivityItem.java:78)
        at android.app.servertransaction.TransactionExecutor.executeCallbacks(TransactionExecutor.java:108)
        at android.app.servertransaction.TransactionExecutor.execute(TransactionExecutor.java:68)
        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1808)
        at android.os.Handler.dispatchMessage(Handler.java:106)
        at android.os.Looper.loop(Looper.java:193)
        at android.app.ActivityThread.main(ActivityThread.java:6669)
        at java.lang.reflect.Method.invoke(Native Method)
        at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:493)
        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:858)
```

Since all exceptions inherit from a class `Exception`, we can capture **all** errors by writing a catch statement for class `Exception`.

```kotlin
try {
   val str =  "hello"
   num = str.toInt()
}
catch (e: Exception)
{
   Log.d("error",e.stackTraceToString())
}
```

Multiple catches can be defined to execute different code depending on the type of Exception.

```kotlin
try {
   val str =  "hello"
   num = str.toInt()
}
catch (e: NumberFormatException){
   Log.d("error",e.stackTraceToString())
}
catch (e: Exception)
{
 Log.d("error",e.stackTraceToString())
}
```


We can also create our own exceptions by inheriting from the `Exception` class:

```kotlin
class LowNumberException(message: String) : Exception(message) {

}
```

To launch an exception, we use the `throw` statement. In the following example we launch an exception of type `LowNumberException` if the number is less than 10.

```kotlin
var num: Int
try {
   val str =  "2"
   num = str.toInt()
   if(num<10){
       throw LowNumberException("Low Number")
   }
}
catch (e: LowNumberException){
   Log.d("LowNumberException",e.stackTraceToString())
}
catch (e: NumberFormatException){
   Log.d("error",e.stackTraceToString())
}
catch (e: Exception)
{
 Log.d("error",e.stackTraceToString())
}
```

Finally, we can launch the base exception by doing:

```kotlin
throw Exception("Error!")
```