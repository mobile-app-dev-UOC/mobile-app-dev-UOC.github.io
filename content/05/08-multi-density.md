---
layout: default
title: 5.8. Multi-density
parent: 5. User interfaces
nav_order: 8
---

# 5.8. Multi-density

By default, an application allows `portrait` and `landscape` orientations.
If we want to support only one orientation, we have to specify it in the `manifest` file. Orientation is configured for each activity separately. If we want the app to always remain in portrait orientation we will use the following:

```xml
<activity
   android:name=".MainActivity"
   android:exported="true"
   android:configChanges="orientation"
   android:screenOrientation="portrait"
   >
```

To query the current orientation programmatically we can use:

```kotlin
val orientation = resources.configuration.orientation
if (orientation == Configuration.ORIENTATION_LANDSCAPE) {
   Log.d("debug","In landscape")
} else {
   Log.d("debug","In portrait")
}
```

>**Learn more:**
>On many occasions the emulator is set by default to ignore rotations. If we want to test rotations, we will need to change the settings on the Android running inside the emulator.

We can define a different layout for each orientation type. The default orientation is `portrait`. We need to create a folder of layout resources with `landscape` orientation.

> ![Landscape orientation.](/images/05/orientation.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Landscape orientation.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We should take into account that rotating the screen destroys the current layout and re-creates the appropriate layout. This may cause the loss of information that the user has written in the data entry components. In particular, this may especially happen in the case of large images and text selection. As we saw in [Section 4.2](/content/04/02-viewmodels), viewModels are a good solution to avoid this.

Furthermore, we can define a different layout for each screen size. When we refer to screen sizes, we will always describe screens using dp units. That is, in device-independent units.

The minimum width values correspond to typical screen sizes:

- 320 dp (**Small**): a typical phone display (240 x 320 ldpi, 320 x 480 mdpi, 480 x 800 hdpi, etc.) 
- 480 dp (**Normal**): a large 5" phone (480 x 800 mdpi) 
- 600 dp (**Large**): one 7" tablet (600 x 1024 mdpi) 
- 720 dp (**X-large**): one 10" tablet (720 x 1280 mdpi, 800 x 1280 mdpi, etc.) 

For instance, if we want to create a layout for tablets that are 7’’ or larger, we create:

> ![Layout for tablets that are 7'' or higher.](/images/05/large-width.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Layout for tablets that are 7'' or higher.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We can create folders for graphical resources for different pixel densities.

> ![Supporting different pixel densities.](/images/05/densities.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Supporting different pixel densities.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

This creates a set of folders called `drawable-<density>`. There are other folders named `mipmap` where icons with different densities are stored.

If we include too many high-density graphics, the application will require a lot space in memory and local. This can quickly become a problem! On the other hand, if an app does not find a graphic with the suitable density, it will look for the same graphic in other densities and use the one that fits best. 

>**Learn more:**
>[https://developer.android.com/guide/topics/large-screens/support-different-screen-sizes](https://developer.android.com/guide/topics/large-screens/support-different-screen-sizes)
