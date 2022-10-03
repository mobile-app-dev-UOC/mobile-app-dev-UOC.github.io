---
layout: default
title: 4.4. 1-N vs N-N architectures
parent: 4. Android app architecture
nav_order: 4
---

# 4.4. 1 Activity - N fragments vs N Activities - N fragments

> ![Architecture of an Android app: Activities, Fragments and Models.](/images/04/architecture.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Architecture of an Android app: Activities, Fragments and Models.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

App development on Android should be tailored to two basics:
1. The increase in power of mobile devices, both in terms of memory capacity and computing power.
2. Customer demands in terms of efficiency, usability and even spectacularity.

These reasons have led Google to recommend the move from the traditional architecture of an application that contains many activities to an application with few activities and many fragments. 

One of the main reasons for having activities was allowing the operating system to destroy activities in the background to provide more memory to the activity in the foreground. As we have indicated earlier, given the rapid increase in memory of the devices this is no longer necessary.

App development now looks more like app development for Windows, macOS, Linux, or even the same iPhone iOS system. Google often bets on few activities, even reaching a single activity and a set of fragments. This provides several advantages:

- *Information sharing:*
    -	We can use the ViewModel because it can be shared between all fragments and the activity.
    -	We can use the Singleton design pattern without problems.

- *Design benefits:* Interface designers often request that the brand graphic image takes precedence over Google’s design guides. They also ask for smooth effects and transitions.

    -	The use of a single full-screen activity facilitates complete control over the design.
    -	Fragment transitions are smoother, more varied, and more spectacular than activity transitions. Activity transitions are controlled by the system, while fragment transitions can be controlled by the developer.

An example of this architecture is the Navigation Drawer Activity example that is provided as one of the available skeletons of apps in Android Studio.

> ![Navigation Drawer Activity.](/images/04/navigation-drawer.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Navigation Drawer Activity.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

## The problem of the "fat activity"

Some developers do not like the single-activity model as it tends to produce activities that are very long (many lines of code) and which use a lot of memory.

- Regarding the amount of code that is added to the parent activity:
This problem is related to the fact that the design of application components and the distribution of responsibilities between them has not been done well rather than using a single activity.  After all, this problem does not occur in iOS and the rest of the systems that have what we might be called “unique activity” architecture.

- Regarding memory usage:
Fewer activities require the developer to exercise greater control over memory allocation and release. The ViewModel has included several aids in this regard.

In conclusion, when should we use N activities? Current trends indicate that it makes sense to use several activities in specific cases:

a)	When it makes sense that the activity coexists in parallel on an Android Multiwindow system with another activity. As typical examples, we can mention: writing two documents from a word processor or note system, or writing or viewing two emails in an email client application. Each document or email would be a different activity.

b)	When we want our activity to be invoked from an activity in another application. For example: to register our activity to send a message through a specific messaging system of our company.

