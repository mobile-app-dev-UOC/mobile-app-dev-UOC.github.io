---
layout: default
title: 2.1. Android Studio IDE
parent: 2. Android Studio
nav_order: 1
---

# 2.1. Integrated Development Environment: Android Studio

[Android Studio](https://developer.android.com/studio) is the official integrated development environment (IDE) for Android. Among other features, Android Studio allows us to design, program, compile, run, deploy, and debug Android applications. 

Its first stable version appeared in 2014, replacing versions of the [Eclipse](https://eclipseide.org/) environment adapted for development for Android using Java. Android Studio was developed from the development environment for Java [IntelliJ IDEA](https://www.jetbrains.com/idea/) from Czech company [JetBrains](https://www.jetbrains.com/). 

## 2.1.1. Creating our first project

We select the `New Project` button in the initial dialog or the `File / New / New Project` menu option if we are already inside the IDE.

> ![Creating a new project in Android Studio](/images/02/new-project.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creating a new project in Android Studio.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

On the next screen, we are asked to choose a template to create the project. These templates correspond to common project typologies, some of which will be discussed later. They are organized by device type.

To get started, we select `Empty Activity` of type `Phone and Tablet`. This is the base template.

> ![Selecting a project template](/images/02/new-project.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Selecting a project template.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

On the last screen of the creation process, we select the programming language that we are going to use. We can choose Kotlin or Java. Since 2019, the official language recommended by Google for Android app development is Kotlin. 

> ![Selecting the programming language.](/images/02/programming-language.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Selecting the programming language used to develop the app.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Also on this screen, we can choose the minimum version on which our application will run, the `Minimum SDK`. If we choose a very recent version, we will be able to access the latest Android functionality. Nevertheless, this will greatly decrease the number of devices on which it can be installed. On the other hand, if we use an older version, it will be possible to install our app on many devices. However, we will have problems implementing some functionalities and in other devices performance might be an issue.

For instance, if we choose API 28, Android 9.0 (Pie), Android Studio informs us that it can be installed on 69% of the devices.

One way to choose the minimum version is to determine the type of users to whom our application is intended and identify the type of device they own.

Now we click on the `Finish` button and the Gradle tools start downloading all the necessary libraries. This process may take a few moments depending on the speed of our network connection.

In Android Studio, projects built from a template are ready to be executed: they include minimal or demo functionality. 

## 2.1.2 A first look at the environment

The project navigator is located at the top left of the user interface. It is typically used in two perspectives:

> ![Project navigator.](/images/02/project-navigator.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Android Studios' project navigator.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

- The `Android` perspective is the logical perspective of an Android app 
- The `Project` perspective is an image of the directory and file structure of our development environment.

When we click on any file in the left browser, its corresponding editor opens on the right-hand side of the screen.

> ![Sample editor.](/images/02/editor.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Sample editor.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

At the bottom there are several tabs:

> ![Bottom tabs.](/images/02/bottom-tabs.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Bottom tabs.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

- **Problems:** It informs us about syntax problems, missing files, etc.
- **Terminal:** It allows us to invoke operating system commands directly from the development environment.
- **LogCat:** It is important in the Debug process. It shows error messages and alerts.
- **Build:** It reports the progress of the Build process. Errors may also appear on this screen.

> ![Build tab.](/images/02/build-tab.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Build tab.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

- **Profiler:** It offersApplication performance measurements.
- **Event Log:** Window where the environment displays messages from each process being executed.

In [Section 6](/content/06/) (dedicated to debugging and testing) we will describe in more detail the panels used in the debug process.

When we click on a layout-type item (those in the `res/layout` folder of the side browser) the Android Studio interface layout editor is launched. On the left of this editor, there is a library of components that we can use on our user interfaces by dragging the component over the design.  At the top of the editor, there is a drop-down that allows us to change the device type. In this way, we can check how the interface behaves when changing the screen size.

> ![Layout editor.](/images/02/layout-editor.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Layout editor.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

The visual editor is one of the biggest challenges for the developers of an IDE. Typically for issues such as accuracy, and the use of overlapping elements, ... developers prefer to use the code view of the layout editor. To access this view, we must press the `Code` button.

> ![Code view of the layout editor](/images/02/code-layout-editor.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Code view of the layout editor.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We can return to the design view by clicking on the `Design` button at the top.

> ![Back to design view](/images/02/back-to-design-editor.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Returning to the design view of the layout editor.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

## 2.1.3. Other useful project templates


It is worth studying the rest of the templates provided by Android Studio. We can learn some techniques just by looking at the code that is automatically generated for these templates:

- **Login Activity:** It implements all the logic of a login system using the Model-View-ViewModel (MVVM) paradigm. 

- **Navigation Drawer Activity:**  Activity that implements the classic drop-down menu on the right.  Each option is implemented as a fragment (see [Section 4.3](/content/04/03-fragments)). That is, it uses the 1 Activity – N Fragments paradigm  (see [Section 4.4](/content/04/04-1-n-vs-n-n)).

> ![Navigation drawer activity](/images/02/navigation-drawer.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Navigation drawer activity.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

- **Bottom Navigation Activity:** Activity that implements a bottom navigation bar. Again, it uses a 1 Activity - N Fragments paradigm (see [Section 4.4](/content/04/04-1-n-vs-n-n)).  Automatically configures cross-snippet navigation.

> ![Bottom navigation activity](/images/02/bottom-navigation.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Bottom navigation activity.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

- **Fullscreen Activity:** This activity runs full screen, without the title bar of Android apps. In many situations, customers request complete control over the design of the app. Thus, this is one of the most popular templates. It is typically used with the 1 Activity - N Fragments paradigm or as a container in a Web view. We will discuss all of these choices in detail in the following sections.