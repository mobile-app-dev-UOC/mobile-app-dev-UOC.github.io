---
layout: default
title: 2.5. Hardware devices
parent: 2. Android Studio
nav_order: 5
---

# 2.5. Hardware devices

In addition to using AVDs, we can debug our app on a physical device. To do this we have two options: USB or Wi-Fi. USB is the most commonly used approach. 

First, we must allow the Android Studio debugger to connect to our physical device. Achieving this may require a different method for each device or device manufacturer. In most devices, the method involves going to `Device Settings` and tapping the interface or system version identifier five times.

> ![Activating the developer mode on an Android device.](/images/02/developer-mode.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Activating the developer mode on an Android physical device.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Sometimes a countdown of the times we need to press for the developer mode to be activated is displayed.

Once the developer mode is activated, we must access your settings and activate: `Developer`, `USB installation` and `USB debugging options`.

> ![Developer mode settings.](/images/02/developer-mode-settings.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Developer mode settings.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Now if we connect our Android device via USB, it will appear in the list that previously only included the AVDs that we had defined.

If we use this device outside of the development environment, we must remember to disable developer options when we are not developing apps. Activating the developer mode is a security risk as malicious software can take advantage of the features that it offers.
