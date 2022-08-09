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
> [https://developer.android.com/studio/projects](https://developer.android.com/studio/projects )
