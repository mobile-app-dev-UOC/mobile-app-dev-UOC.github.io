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

>Activities were concieved for two fundamental reasons. First, the operating system can reclaim memory by destroying activities that are in the background. Second, to enable one application to request services offered by other applications. For example, an application that wants to edit a photo can open a specific editor activity from another application that allows you to modify the image. 
