---
layout: default
title: 8.4. Grouping events
parent: 8. Events
nav_order: 4
---

# 8.4. Grouping events

Many times, hardware emits events at a much higher rate than our event handler can process them. In that case, Android groups the events and they are received in a single call to the handler. 

The most typical example is the movement of the finger above the screen – a multitude of events are received in a very short period of time. To illustrate this phenomenon, we modify the code from the previous section to visualize these intermediate events in the case of `MotionEvent.ACTION_MOVE`.

```kotlin
when (e.action) {
   MotionEvent.ACTION_MOVE -> {
       Log.i("onTouchEvent View", "ACTION_MOVE")
       showHistory(e)
   }
   ...
```

Now, we will implement the method `showHistory`, where we will access all events that have occurred since the previous call.


To understand its implementation, we must be aware that there is a pointer for each finger that is pressing the screen (because the screen can be multi-touch). The number of pointers is stored in `ev.pointerCount. If we press with a single finger on the screen, the number of pointers will be 1.

On the other hand, there is a history of positions that have been left behind by each of those pointers over time. Each element of that history is called a **sample**.

The following methods from `MotionEvent` provide us detailed information about a sample:
- `ev.getHistoricalEventTime(h)` returns the time when sample `h` was taken. 
- `ev.getHistoricalX(p, h)` returns the X position of pointer `p` in sample `h`. 
- `ev.getHistoricalY(p, h)` returns the Y position of pointer `p` in sample `h`.

The index of each pointer (finger) is **not** preserved between consecutive `MotionEvent.ACTION_MOVE` events. That is, a pointer could be in index 0 in one event and in index 2 in the next. However, the `getPointerId` of each pointer is preserved between different `MotionEvent.ACTION_MOVE`. Thus, we will need to use the `getPointerId` instead of the index to keep track of the position of each finger.

```kotlin
fun showHistory(ev: MotionEvent) {
   val historySize = ev.historySize
   val pointerCount = ev.pointerCount
   for (h in 0 until historySize) {
       Log.i("onTouchEvent printSamples View", ev.getHistoricalEventTime(h).toString())
       for (p in 0 until pointerCount) {

           Log.i("onTouchEvent printSamples View",
               ev.getPointerId(p).toString() + " " +
                       ev.getHistoricalX(p, h).toString() + " " +
                       ev.getHistoricalY(p, h).toString() + " "
           )
       }
   }

   for (p in 0 until pointerCount) {
       Log.i("onTouchEvent printSamples End View",
           ev.getPointerId(p).toString() + " " +
                   ev.getX(p).toString() + " " +
                   ev.getY(p).toString() + " "
       )
   }

}
```

The previous code sends to Logcat the positions of each sample in the history of an `MotionEvent.ACTION_MOVE` event and the current position:

```
2022-05-04 18:29:14.064 26888-26888/com.uoc.listeners I/onTouchEvent View: ACTION_MOVE
2022-05-04 18:29:14.065 26888-26888/com.uoc.listeners I/onTouchEvent printSamples View: 19131231
2022-05-04 18:29:14.065 26888-26888/com.uoc.listeners I/onTouchEvent printSamples View: 0 156.96338 389.9596 
2022-05-04 18:29:14.066 26888-26888/com.uoc.listeners I/onTouchEvent printSamples End View: 0 166.45398 361.44263 
2022-05-04 18:29:14.081 26888-26888/com.uoc.listeners I/onTouchEvent View: ACTION_MOVE
2022-05-04 18:29:14.082 26888-26888/com.uoc.listeners I/onTouchEvent printSamples View: 19131252
2022-05-04 18:29:14.083 26888-26888/com.uoc.listeners I/onTouchEvent printSamples View: 0 171.95972 344.89917 
2022-05-04 18:29:14.083 26888-26888/com.uoc.listeners I/onTouchEvent printSamples End View: 0 186.8164 307.3448 
2022-05-04 18:29:14.098 26888-26888/com.uoc.listeners I/onTouchEvent View: ACTION_MOVE 
```
