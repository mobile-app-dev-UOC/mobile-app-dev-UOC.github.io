---
layout: default
title: 14.5. Maps
parent: 14. Geolocation and maps
nav_order: 5
---

# 14.5. Maps

To create a map, we will use Google Maps. To use these maps it is necessary to add a Google Maps usage key to the `manifest.xml` file.

```xml
<meta-data
   android:name="com.google.android.geo.API_KEY"
   android:value="${MAPS_API_KEY}" />
```

To get this key we must sign up in the Google Cloud service console.

We create a new project:

> ![Creating a new project](/images/14/new-project.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creating a new project.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


> ![Setting up our new project](/images/14/project-setup.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Setting up our new project.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, we select the option "`Explore and enable APIs`".

> ![Selecting "Explore and enable APIs" (part 1).](/images/14/explore1.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Selecting "Explore and enable APIs" (part 1).*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

> ![Selecting "Explore and enable APIs" (part 2).](/images/14/explore2.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Selecting "Explore and enable APIs" (part 2).*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We select "`Maps SDK for Android`".

> ![Selecting the Maps SDK for Android API.](/images/14/select-api.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Selecting the Maps SDK for Android API.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, we enable the Google Maps API.

> ![Enabling the Google Maps API.](/images/14/enable-api.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Enabling the Google Maps API.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

After that, we need to create credentials for this API:

> ![Creating credentials for the Google Maps API.](/images/14/credentials.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creating credentials for the Google Maps API.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Clicking on "`Create credentials`" opens a drop-down menu where we should select `API Key`. 
At that time, we will receive the key that we must add in the `Local.properties` file.

> ![Creating an API key.](/images/14/api-key.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creating an API key.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Local.properties:
```
sdk.dir=C\:\\Users\\Salva\\AppData\\Local\\Android\\Sdk
MAPS_API_KEY=xxxxxxxxx_xxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Gradle:

```gradle
plugins {
     id 'com.google.android.libraries.mapsplatform.secrets-gradle-plugin'
}

dependencies {
   implementation 'com.google.android.gms:play-services-maps:18.0.2'
}
```

We need to be careful with the APIs we use from Google Cloud, as we need to check their price. For example, they may be free for a small number of uses and their price will skyrocket if used by many users.

---

## 14.5.1. Creating the Map


We will use the SupportMapFragment class that inherits from the Fragment class but with Google Maps support.

Our activity will have a fragment named `com.google.android.gms.maps.SupportMapFragment` within its layout:

```xml
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:map="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:id="@+id/map"
   android:name="com.google.android.gms.maps.SupportMapFragment"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".MapsActivity" />
```

We declare our Activity indicating that it implements the interface `OnMapReadyCallback`:
```kotlin
class MapsActivity : AppCompatActivity(), OnMapReadyCallback
```


In the `onCreate` callback of our Activity, we request the fragment to load the map and the map appears:

```kotlin
val mapFragment = supportFragmentManager
   .findFragmentById(R.id.map) as SupportMapFragment
mapFragment.getMapAsync(this)
```

When the maps are ready, the following callback will be executed:

```kotlin
override fun onMapReady(googleMap: GoogleMap) {
   mMap = googleMap
}
```

At that point we can save the reference to a GoogleMap instance that we just received to a class property (in this example, mMap).

---

## 14.5.2. Showing our position or the position of bookmarked locations

