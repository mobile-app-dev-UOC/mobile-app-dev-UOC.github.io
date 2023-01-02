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

Inside the callback, we can save the reference to a GoogleMap instance that we just received to a class property (in this example, `mMap`).

---

## 14.5.2. Showing our position or the position of bookmarked locations

We can display our position or that of bookmarks on the map, provided the method `onMapReady` method has already been executed (or even from within this method).

### 14.5.2.2 Showing the location of a bookmark: Marker

We need to create one instance of the `Marker` class for each location we want to show on the map. A Marker contains the following information: latitude, longitude, title, and an icon.

```kotlin

override fun onMapReady(googleMap: GoogleMap) {
   mMap = googleMap
   val latLng: LatLng = LatLng(41.404076846062054, 2.189701544333684)
   val markerOptions: MarkerOptions = MarkerOptions()
   markerOptions.position(latLng);
   markerOptions.title("Agbar Tower");
   markerOptions.icon(BitmapDescriptorFactory.defaultMarker(BitmapDescriptorFactory.HUE_GREEN));
   val marker:Marker = mMap.addMarker(markerOptions)!!;
   marker.tag = "extra info"
}
```

> ![Example of a marker.](/images/14/marker.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Example of a marker: In the center of the map we can see the Marker in green and, below, the indicator of our position in blue*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Once the `Marker` is created, we can assign additional information to it using the `tag` property. In this example, we have added a string but tag is of type `Any` so we can add any class we define.

To change the appearance of the `Marker`, you can change the icon and even create a custom icon from a drawable.

To detect a user clicking on a `Marker`, we indicate that our activity implements the interface  `GoogleMap.OnMarkerClickListener`:

```kotlin
class MapsActivity : AppCompatActivity(), OnMapReadyCallback, GoogleMap.OnMarkerClickListener 
```

Then, in the callback `onMapReady` we indicate the following:

```kotlin
override fun onMapReady(googleMap: GoogleMap) {
   mMap.setOnMarkerClickListener(this)
}
```

We will now receive the clicks on any `Marker` in the following callback:

```kotlin
override fun onMarkerClick(p0: Marker): Boolean {
   Log.d("map", p0.title!!)
   val str: String? = p0.tag as String?
   return true
}
```

To move the map to certain coordinates, we can use the method `moveCamera` as follows:

```kotlin
mMap.moveCamera(CameraUpdateFactory.newLatLng(latLng));
```

Moreover, to adjust the zoom level we use the method `animateCamera`:

```kotlin
mMap.animateCamera(CameraUpdateFactory.zoomTo(10.0F))
```

To debug map-related code, we can change our position on the emulator for testing purposes by clicking on the three dots to the right of the top bar.

> ![Testing map-related code by changing your position in the emulator.](/images/14/test-location.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Testing map-related code by changing your position in the emulator.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

When you click this icon, you get an emulator settings screen and the first option, which is active by default, is "`Location`". You can look for the location where you want to position the emulator on the search engine and then click the "`Set Location`" button at the bottom.

> ![Interface used to change the location of the emulator.](/images/14/set-location.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Interface used to change the location of the emulator.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

---

## 14.5.3 Polylines, polygons and routes

It is important to distinguish between these three concepts: polylines, polygons and routes.

Drawing segments or polygons on top of a map is simple. Then, we can define a route as a polyline and an area as a surface. The key problem is determining which polyline should be used as the route between two points. To compute this polyline, we need a route provider, which in this case will be Google.

### 14.5.3.1 Showing polylines and polygons

To add a polyline we simply add an instance of the polyline class to our map. We add the latitude and longitude coordinates that make up the polyline.

```kotlin
val polyline1 = mMap.addPolyline(
   PolylineOptions()
       .clickable(true)
       .add(
           LatLng(41.404076846062054, 2.189701544333684),
           LatLng(41.411018040787106, 2.1868837607895193),
           LatLng(41.41541785168701, 2.1956596549986838)
       )
)
```

> ![Example of a polyline.](/images/14/polyline.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Example of a polyline (created with the previous code)*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Similarly, to add a polygon we add an instance of the `Polygon` class to our map. Then we add the latitude and longitude coordinates of the points that make up the polygon. In this case, we also indicate the background color of the area within the polygon in ARGB format using the method `setFillColor`.

```kotlin
val polygon1 = mMap.addPolygon(
   PolygonOptions()
       .clickable(true)
       .add(
           LatLng(41.404076846062054, 2.189701544333684),
           LatLng( 41.411018040787106, 2.1868837607895193),
           LatLng( 41.41541785168701, 2.1956596549986838)
       )
)
polygon1.setFillColor(0x3381C784.toInt());
```
> ![Example of a polygon.](/images/14/polygon.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Example of a polygon (created with the previous code)*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

There many parameters that can be used to customize the visual appearance of our polylines and polygons. A complete list of these options can be found in the [documentation of the Maps SDK for Android](https://developers.google.com/maps/documentation/android-sdk/overview).  
Like we did with `Marker`, we can add additional information to polylines and polygons using the method `setTag`.

To receive clicking events on a polyline or polygon our activity must implement the interfaces `GoogleMap.OnPolylineClickListener` and `OnPolygonClickListener`, respectively.

```kotlin
override fun onPolylineClick(p0: Polyline) {
   EVERYTHING("Not yet implemented")
}

override fun onPolygonClick(p0: Polygon) {
   EVERYTHING("Not yet implemented")
}
```

---

### 14.5.3.2. Computing a route

To compute routes, we need to connect to the Google Cloud Console and add the Directions API we need to calculate paths to the project where we previously included the Google Maps API.

> ![Directions API.](/images/14/directions-api.jng){:style="display:block; margin-left:auto; margin-right:auto"}
> *Directions API.)*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

To use this API, you must have a billing profile associated with the Google Cloud Console and associate this profile with our Directions API.

> **Important**: It is very important to check the costs associated with using the API before testing it or deploying it in your app!

Associating the profile will provide us with the key we will need to use to make calls to this Google URL:

```
https://maps.googleapis.com/maps/api/directions/json?origin=<origin_latitude>,<origin.longitude>&destination=<destination.latitude>,<destination.longitude>&key=<API_DIRECTIONS_KEY>
```

If you visit this URL in a browser and replace : `<origin_latitude>`, `<origin.longitude>`, `<destination.latitude>`, `<destination.longitude>`, and `<API_DIRECTIONS_KEY>` with the corresponding values you will receive a response in JSON format with the generated route.

---

### 14.5.3.3. Plotting a route


This time, to read the JSON response from the Google Cloud backend we are not going to use the Retrofit library, but we are going to use the Volley library. This library does not map to Kotlin classes, but works directly with JSON objects. We’ll also include the Google `android-maps-utils` library that allows us to make fast format transformations.

We add the corresponding dependencies to the Gradle file:

```gradle
dependencies {
   implementation 'com.google.maps.android:android-maps-utils:1.3.1'
   implementation 'com.android.volley:volley:1.2.0'
}
```

Now once the map has been created, we can request routes from the service as follows:

```kotlin
fun GetRoute(origin:LatLng,destination:LatLng)
{
   val path: MutableList<List<LatLng>> = ArrayList()
   val urlDirections = "https://maps.googleapis.com/maps/api/directions/json?origin=${origin.latitude},${origin.longitude}&destination=${destination.latitude},${destination.longitude}&key=<API_KEY_DIRECTIONS>"

   val directionsRequest = object : StringRequest(Request.Method.GET, urlDirections, Response.Ready<String> {
           response ->
       val jsonResponse = JSONObject(response)

       val routes = jsonResponse.getJSONArray("routes")
       val legs = routes.getJSONObject(0).getJSONArray("legs")
       val steps = legs.getJSONObject(0).getJSONArray("steps")
       for (i in 0 .. steps.length()-1) {
           val points = steps.getJSONObject(i).getJSONObject("polyline").getString("points")
           path.add(PolyUtil.decode(points))
       }
       for (i in 0 .. path.size-1) {
           this.mMap!!.addPolyline(PolylineOptions().addAll(path[i]).color(0xff81C784.toInt()))
       }
   }, Response.ErrorListener {
           _ ->
   }){}
   val requestQueue = Volley.newRequestQueue(this)
   requestQueue.add(directionsRequest)
}
```

The first loop builds the route from different points (`LatLng`) included in the JSON `steps` array. The second loop transforms each route into a polyline with color `0xff81C784` (green). When we plot this polyline on the map, we can see the actual route to get from one point to the other.

> ![Displaying a route in the map.](/images/14/route.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Displaying a route in the map.)*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

