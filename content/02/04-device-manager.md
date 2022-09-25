---
layout: default
title: 2.4. Device Manager
parent: 2. Android Studio
nav_order: 4
---

# 2.4. Device Manager

Device Manager allows you to manage multiple **Android Virtual Devices** (AVD). An AVD is an emulator of a hardware and operating system configuration. These emulators will allow us to test our application under different conditions.

> ![Device Manager](/images/02/avd.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Device Manager in Android Studio.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Clicking the `Create Device` button displays a list of available settings. These settings are organized first by device type: TV, phone, watch, tablet or car.

Once one of these categories is selected, we must choose the hardware model we want to emulate. Each device tells us the physical size, resolution, and pixel density (dpi). Pixel density will be explained in detail in [Section 5](/content/05/). The hardware devices that appear in the list are either Google devices or generic devices. If we need to use devices from another manufacturer, the manufacturer should supply them.

> ![Setting up the hardware in an AVD](/images/02/avd-hardware.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Setting up the hardware in an AVD.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

After selecting the hardware model, it is time to select the OS version:

> ![Setting up the operating system in an AVD](/images/02/avd-os.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Setting up the operating system in an AVD.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

In this section we must consider two aspects:

1.	If we choose a very modern version, we may not be testing our app on many older devices that have not yet been updated.

2.	We can choose an Android version with the Google APIs or without them. Google APIs provide services such as notifications, geolocation, ... These services are not part of the Android core and therefore hardware manufacturers must pay Google to include them on their devices.

By default, we should use Android versions with Google APIs. Versions without Google APIs should only be selected when we are completely sure that our app should work on devices without them. In that case, we should also include the equivalent APIs supplied by the device manufacturer. 

If the operating system version is not locally available, it will be turned off in the interface. In order to use it, we need to download it beforehand.

Once the operating system has been selected, we will need to select certain device features that we can modify later:

> ![Basic configuration of an AVD](/images/02/avd-settings.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Basic configuration of an AVD.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We can keep the default values for the first group of settings. For example, we can keep graphical performance on the “Automatic” setting. Nevertheless, it is very important to click the `Show Advanced Settings` button. This button lets us configure important parameters.

> ![Advanced settings of an AVD](/images/02/avd-advanced.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Advanced settings of an AVD.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

First, many devices have a low amount of RAM, and our app can have performance issues if it uses high-resolution images for example. In this case, we need to increase this memory space.  We may also have the same problem with the amount of internal storage of the device.  Remember that all these parameters can be changed later from the device list.

By clicking the pencil icon, we can change the above parameters.

> ![Editing the settings of an AVD](/images/02/avd-edit-params.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Editing the settings of an AVD.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Clicking on the down arrow displays a menu of options where we can delete, duplicate, ... AVDs.

> ![Actions involving AVDs.](/images/02/avd-operations.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Actions involving AVDs.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

The most interesting option in this list is `Wipe Data`. This button clears all user data and restores the device to its default factory settings. It may be necessary to perform this action periodically. Otherwise, after installing many applications, the virtual device may start emitting error messages like `Waiting for target to come online`.

To choose an emulator, we select it in the top bar of Android Studio.Then, we click the play button if we want it to be installed without using the debugger. We can use the green bug button if we want to install and debug the app.

> ![Launching an AVDs.](/images/02/avd-running.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Launching an AVD.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

> ![An example of the execution of an AVD.](/images/02/avd-example.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *An example of the execution of an AVD.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

> ***Learn more:***  
> If we drag an image file from our computer onto the main Android screen running on the emulator, it will be copied to the folder `Media / Downloads`.
