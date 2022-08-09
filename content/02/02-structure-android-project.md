---
layout: default
title: 2.2. Structure of an Android project
parent: 2. Android Studio
nav_order: 2
---

# 2.2. Structure of an Android project

An Android project is a designated folder structure that contains a set of files. The most important files in terms of project configuration are the `manifest.xml` file and the Gradle files.

## 2.2.1 Overview of folders and files
	
By default, Android Studio displays the project using the `Android` view. This view is the easiest way to showcase the different components included in an Android app.

> ![Android view of a project](/images/02/android-view.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Android view of a project.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

The `manifests` folder contains the `AndroidManifest.xml` file containing the essential information to create the app.

The `java `folder contains the code of the application. Kotlin files are located here, even if the folder is named Java.

By default, there are three packages: the application and two test packages (`androidTest` and `test`).

The `res` folder contains all the resources the application needs: 

- `Drawable`: graphic elements.
- `Layout`: screen layout.
- `Mipmap`: icons
- `Values / Colors.xml`: colors
- `Values / Strings.xml`: list of strings used in the static texts of the application.
- `Values / Themes`: Sets of values related to design settings.
- `Gradle Scripts`: The set of scripts that tell Android Studio how to build the app.  

When we create folders, sometimes the `Android`` view does not automatically refresh and it is necessary to go to the `Project` view, which is always up to date as it is an image of the file system.


> ![Project view](/images/02/project-view.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Project view.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, from the `Project` view, we go to the folder and then we create a file. After creating the file, we can go back to the `Android view. The folder and file will now appear there.


> ***Learn more:***  
> [https://developer.android.com/studio/projects](https://developer.android.com/studio/projects)

## 2.2.2 Manifest

The Manifest is an XML file that contains essential settings for Android Studio, Google Play Store, and the Android operating system.

This is what a minimal `manifest.xml` file looks like:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
   package="edu.uoc.hello">

   <application
       android:allowBackup="true"
       android:icon="@mipmap/ic_launcher"
       android:label="@string/app_name"
       android:roundIcon="@mipmap/ic_launcher_round"
       android:supportsRtl="true"
       android:theme="@style/Theme.Hello">
       <activity
           android:name=".MainActivity"
           android:exported="true">
           <intent-filter>
               <action android:name="android.intent.action.MAIN" />
               <category android:name="android.intent.category.LAUNCHER" />
           </intent-filter>
       </activity>
   </application>

</manifest>
```

In the root element, we indicate the name of our package, which is the unique identifier of our application within the entire Android ecosystem. It is usually created using [reverse internet domain notation](https://en.wikipedia.org/wiki/Reverse_domain_name_notation). In this case, the unique identifier we have chosen is "edu.uoc.hello".

In the attributes of the `application` element we specify settings such as the following:
- `android:allowBackup="true"`  
  The application can participate in system backup and restore processes.

- `android:icon="@mipmap/ic_launcher"`  
  Application icon, referring to the  `res/mipmap/ic_launcher`  folder.

- `android:label="@string/app_name"`  
  Name of the application, referring to the `res/values/strings.xml` file.

- `android:roundIcon="@mipmap/ic_launcher_round"`  
  Icon version. It only works on Android version 7.1.

- `android:supportsRtl="true"`  
  Indicates that the application will support languages that are written from right to left. With this option, the app will be able to have specific user interface designs for these languages.

- `android:theme="@style/Theme.Hello"`  
  Identifier of the design theme to be used by the application. It refers to the file `res/values/themes/themes.xml`, where there should be an XML element where the attribute `name` has the value “Theme.Hello“

Within the application XML element, there will be one XML element for each **activity** in our application. An activity is a way to break down the application into functionalities. We will look at them in detail in later chapters (see [Section 4.1](/content/04/01-activities)).

```xml
<activity
           android:name=".MainActivity"
           android:exported="true">
           <intent-filter>
               <action android:name="android.intent.action.MAIN" />
               <category android:name="android.intent.category.LAUNCHER" />
           </intent-filter>
      </activity>
```

We identify the main activity (the one that is executed when the user runs the app) as follows:

```xml
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
```

The manifest file also lists the **permissions** that the application must be granted.
For example, if we need to connect to the Internet, we must use the permission:
`android.permission.INTERNET`.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
   package="edu.uoc.hello">
   <uses-permission android:name="android.permission.INTERNET" />
   <application ...>
```

Or, for example, if we want to have access to NFC capabilities we must indicate:

```xml
<uses-permission android:name="android.permission.NFC" />
```

We can indicate specific hardware requirements that the app can check before being installed. For example, we can indicate that our app requires a compass with:

```xml
<uses-feature android:name="android.hardware.sensor.compass"
   android:required="true" />
```

Or, for example, if your app requires the device to have NFC hardware you would state:
```xml
<uses-feature android:name="android.hardware.nfc" android:required="true" />
```

In addition to the activities, there are three other types of elements that are declared in the `manifest.xml`: `service`, `provider`, and `receiver`.

- **Service:** A service is a part of the application that performs a background task. This does not mean that you necessarily create your own execution thread. We will look at services in detail in [Section 12](/content/12/). A typical example is declaring a service that handles notifications in the `manifest.xml`:

```xml
<service
   android:name=".MyFirebaseMessagingService"
   android:exported="false">
   <intent-filter>
       <action android:name="com.google.firebase.MESSAGING_EVENT" />
   </intent-filter>
</service>
```

- **Provider:** A provider is a way to expose access to our application data for other applications. We can expose data of all kinds and we can even allow modifications.  

```xml
<provider
   android:name=".MyContentProvider"
   android:authorities="edu.uoc.hello.data"
   android:enabled="true"
   android:exported="true">
</provider>
```

Android stores `android:authorities` to determine which app will provide the content to us. Applications will use that URL to request the data.
	
- **Receiver:** Receivers allow applications to receive intents from the system or other applications, even if the application is not running. For example, receivers are commonly used to monitor hardware health. One example would be creating a receiver to monitor battery status. 

```xml
<receiver
   android:name=".BatteryLevelReceiver"
   android:enabled="true"
   android:exported="true" >
   <action android:name="android.intent.action.BATTERY_LOW"/>
</receiver>
```




