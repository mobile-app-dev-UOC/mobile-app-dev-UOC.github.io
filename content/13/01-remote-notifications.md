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


> ![Using Cloud Messaging.](/images/13/single-back-end.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Using Cloud Messaging.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We add the app that will use these notifications with by tapping the platform type icon. In our case, we click on Android.

