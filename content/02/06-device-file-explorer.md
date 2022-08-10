---
layout: default
title: 2.6. Device File Explorer
parent: 2. Android Studio
nav_order: 6
---

# 2.6. Device File Explorer

To test and debug our application, we can access the file system of the device that we have connected in debug mode. To do so, we need to access the `View \ Tool Windows \ Device File Explorer` option. In the path: `/data/data/YOUR_PACKAGE_NAME` we find the different folders and files related to our application.

If a file that should be inside a directory is not visible, select the directory and from the context menu select `Sync`.

Furthermore, the context menu in Device File Explorer lets us upload files or download files from the device to the computer where we are developing. 

> ![Uploading files to a device](/images/02/upload-files.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Uploading files to an Android device.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)
