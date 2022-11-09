---
layout: default
title: 8.3. Propagating and cancelling events
parent: 8. Events
nav_order: 3
---

# 8.3. Propagating and cancelling events

On Android, events spread from child to parent: from the bottom layer to the top.  Along this path, we can declare the same event handler to behave differently: it can take some action and cancel the event, so it no longer reaches the parent.

That is why `onKeyUp` events do not reach the Activity when you are using the soft keyboard in an EditText view: the EditText view processes the event and cancels it. The only way to make this event reach the Activity is to create our own EditText class and rewrite the behavior of the event `onKeyUp`.

> ![Event propagation.](/images/08/event-propagation.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Event propagation.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Let us do this through an example: we create a MyView class, the red rectangle, that inherits from View and assign it the same event handler from the previous section.

> ![App for experimenting with event propagation.](/images/08/listener-experiment.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *App for experimenting with event propagation.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

```kotlin
class MyView : View {

   override fun onTouchEvent(e: MotionEvent): Boolean {
      val x: Float = e.x
      val y: Float = e.y

      when (e.action) {
         MotionEvent.ACTION_MOVE -> {
            Log.i("onTouchEvent View", "ACTION_MOVE")
         }
         MotionEvent.ACTION_UP -> {
            Log.i("onTouchEvent View", "ACTION_UP")
         }
         MotionEvent.ACTION_DOWN -> {
            Log.i("onTouchEvent View", "ACTION_DOWN")
         }
      }

      return true
   }
}
```

Returning true indicates that the view has consumed the event and no longer reaches the activity. In Logcat we have the following messages from the view:

```
2022-05-04 12:47:05.499 22324-22324/com.uoc.listeners I/onTouchEvent View: ACTION_MOVE
2022-05-04 12:47:05.799 22324-22324/com.uoc.listeners I/onTouchEvent View: ACTION_MOVE
2022-05-04 12:47:05.854 22324-22324/com.uoc.listeners I/onTouchEvent View: ACTION_MOVE
2022-05-04 12:47:05.888 22324-22324/com.uoc.listeners I/onTouchEvent View: ACTION_MOVE
2022-05-04 12:47:05.898 22324-22324/com.uoc.listeners I/onTouchEvent View: ACTION_UP
```

On the other hand, returning false indicates that we have not consumed the event and it reaches the activity. In this case, we have the following messages from the Activity:

```
2022-05-04 12:49:04.094 22480-22480/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:49:04.148 22480-22480/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:49:04.457 22480-22480/com.uoc.listeners I/onTouchEvent: ACTION_MOVE
2022-05-04 12:49:04.489 22480-22480/com.uoc.listeners I/onTouchEvent: ACTION_UP
```