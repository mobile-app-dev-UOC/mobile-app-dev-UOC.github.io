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

- The `Android` Perspective is the logical perspective of an Android app 
- The `Project` perspective is an image of the directory and file structure of our development environment.

When we click on any file in the left browser, its corresponding editor opens on the right-hand side of the screen.

> ![Sample editor.](/images/02/editor.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Sample editor.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

