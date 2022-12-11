---
layout: default
title: 11. Back-end
nav_order: 11
has_children: true
---

# 11. Back-end

The back-end includes all the services or data offered by the online part of an application. Most applications require back-end functionality. These services or data may be core features or simply auxiliary ones, such as gathering analytics about app usage.

> ![Structure of a back-end.](/images/10/back-end.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Structure of a back-end.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Since a back-end always requires an Internet connection, we must have the following permission in the `manifest.xml` of the app:

```xml
<uses-permission android:name=”android.permission.INTERNET”/>
```

If we use third-party back-ends such as Firebase, it will not be necessary to include this permission in our `manifest.xml` file. Instead, the permission will be included in the library that Gradle downloads so that we can communicate with the back-end.

{:toc:}
