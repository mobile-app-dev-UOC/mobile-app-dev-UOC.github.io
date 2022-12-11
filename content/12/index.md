---
layout: default
title: 12. Services
nav_order: 12
has_children: true
---

# 12. Services

A service is a component of an application that performs background operations and does not have its own user interface.

There are two types of services:

-	**Foreground Services**

Foreground services need to make local notifications to the user. For example, if they are playing a song, it may be necessary to send a local notification indicating that it has started playing or finished.


-	**Bound Services**

A bound service implements a client-server pattern, where the service is the server. The application that contains it (or another if this is allowed in the configuration) can invoke it to perform certain operations. If used from an application other than the one containing it, inter-process communication (IPC) is used to execute the operations.  

Services run on the same thread as the application except for linked services used from a different application. If we perform operations that can block the interface, we must use concurrent programming techniques.

A service is not running indefinitely: it runs while the application that is using it is still open. An exception to this rule is a bound service running from another application. In this case, the service becomes active until it is explicitly closed by the activity that uses it or the operating system decides to close it because it needs resources.

> **Learn more:**
>The `IntentService` and `JobIntentService` classes are deprecated. We must use one of the service models listed above or the WorkManager discussed in [Section 10.5](/content/10/05-workmanager).


{:toc:}
