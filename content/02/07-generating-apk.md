---
layout: default
title: 2.7. Generating the APK file
parent: 2. Android Studio
nav_order: 7
---

# 2.7. Generating the APK file

The APK file is required to send the app to the Google Play Store or to install it manually on an Android device.

Click on the menu option: `Build \ Generate signed bundle \ APK`
 
> ![Generating an APK](/images/02/create-apk.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Generating an APK file to deploy the app.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

On this screen, we select the `APK` option. The `Android App Bundle` option is very similar, but it performs some optimizations to reduce the size of the final generated file.

On the next screen, we will be asked to choose a key to sign the app. This is necessary because all Android applications must be signed with a developer-owned key. Signing software is a way to ensure that the file uploaded to Google Play is the one we created, and that no external agent has made any modifications to it. The first time, since we do not have a key, we click the `Create new` button.

 
> ![Creating a key to sign the app](/images/02/create-key.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creating a new key to sign the app.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


To create a key, we first created a **Key Store**. A Key Store is an encrypted container that contains multiple keys. 

> ![Creating a key store](/images/02/key-store.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creating a key store.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


Then, we fill in the key details. The fields filled in the previous image are the required ones.

Once the key has been created, we select it to sign our application. 

> ![Signing the app with our key](/images/02/signing.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Signing the app with our key.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Below we select the version we want to sign. In this case we select the `release` version.

> ![Selecting the version to be signed](/images/02/select-version-to-sign.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Selecting the version of the app to be signed.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, we click the `Finish` button and start the generation process. When the process ends in the lower right corner of Android Studio, a message appears allowing us to directly access the folder where it was generated.

> ![APK complete message](/images/02/apk-completed.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Message indicating that our APK is ready.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)
