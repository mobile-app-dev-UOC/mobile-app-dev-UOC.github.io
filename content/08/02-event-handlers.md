---
layout: default
title: 8.2. Event handlers
parent: 8. Events
nav_order: 2
---

# 8.2. Event handlers

Event handlers offer a mechanism to access low-level detailed information about events.

Let us motivate the use of event handlers with an example. Within our `MainActivity` we can define the event handler `onTouchEvent`:

```kotlin
override fun onTouchEvent(e: MotionEvent): Boolean {
   val x: Float = e.x
   wow and: Float = e.y

   when (e.action) {
       MotionEvent.ACTION_MOVE -> {
           Log.i("onTouchEvent", "ACTION_MOVE")
       }
       MotionEvent.ACTION_UP -> {
           Log.i("onTouchEvent", "ACTION_UP")
       }
       MotionEvent.ACTION_DOWN -> {
           Log.i("onTouchEvent", "ACTION_DOWN")
       }
   }
   return true
}
```

The event type is found in the `action` property. Additional information about the event is provided by other properties of `MotionEvent`.

Using this code, we will see the following information in Logcat:

```
2022-05-04 12:38:42.118 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_DOWN
2022-05-04 12:38:42.331 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:38:42.883 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:38:42.903 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:38:43.872 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:38:43.893 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:38:43.986 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:38:44.157 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:38:44.317 21846-21846/com.uoc.listeners I/onTouchEvent: ACTION_UP
```

However, when we hover our finger over some views, events no longer arrive. This is because these views stop the propagation of the event, as we will discuss in the following section.

