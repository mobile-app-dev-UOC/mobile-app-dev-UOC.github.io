---
layout: default
title: 14.2. Geolocation
parent: 14. Geolocation and maps
nav_order: 2
---

# 14.2. Geolocation

As we have already said, geolocation consists of obtaining the position information of a user even if the application is in the background. Because it should work in the background, we will create a service to perform this task. The techniques we will use can also be used within an app, but then we will not receive signals when the app is in the background.

In the `manifest.xml` file, we need to add the following:

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />

<service
   android:name=".MyServiceForeground"
   android:enabled="true"
   android:exported="true" />
```

And the following dependency needs to be added in Gradle:

```gradle
dependencies {
  implementation 'com.google.android.gms:play-services-location:19.0.1'
}
```

We create a new Foreground service:

```kotlin
class MyServiceForeground : Service() {

   private lateinit var fusedLocationClient: FusedLocationProviderClient
   private lateinit var locationCallback: LocationCallback

   companion object {
       fun startService(context: Context, message: String) {
           val startIntent = Intent(context, MyServiceForeground::class.java)
           startIntent.putExtra("inputExtra", message)
           ContextCompat.startForegroundService(context, startIntent)
       }
       fun stopService(context: Context) {
           val stopIntent = Intent(context, MyServiceForeground::class.java)
           context.stopService(stopIntent)
       }
   }

   override fun onBind(intent: Intent): IBinder? {
       return null
   }

   override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {

       val pendingIntent: PendingIntent =
           Intent(this, MapsActivity::class.java).let { notificationIntent ->
               PendingIntent.getActivity(this, 0, notificationIntent, 0)
           }

       val notification: Notification = Notification.Builder(this, "alert_channel")
           .setContentTitle("Start Track")
           .setContentText("")
           .setSmallIcon(R.drawable.ic_launcher_foreground)
           .setContentIntent(pendingIntent)
           .setTicker("")
           .build()

       startForeground(1, notification)

       fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)

       locationCallback = object : LocationCallback() {
           override fun onLocationResult(p0: LocationResult) {
               super.onLocationResult(p0)
               for (location in p0.locations){
                   Log.d("position",location.latitude.toString()+","+location.longitude.toString())
               }
           }
       }


       val locationRequest: LocationRequest = LocationRequest.create()

       locationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
       locationRequest.setInterval(200);
       locationRequest.setFastestInterval(200);

       fusedLocationClient.requestLocationUpdates(locationRequest,
           locationCallback,
           Looper.getMainLooper())

       return START_NOT_STICKY
   }
}
```

In the main activity we can now launch the service with `startService`:

```kotlin
MyServiceForeground.startService(this, "Foreground Service is running...")
```

The `onStartCommand` callback of the service performs all the monitoring. First, an instance of FusedLocationProviderClient is created. This class uses a Google service that attempts to use all available techniques to adjust positioning using the lowest energy consumption.

```kotlin
fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
```

Then we create the object that will receive the signals:

```kotlin
locationCallback = object : LocationCallback() {
   override fun onLocationResult(p0: LocationResult) {
       super.onLocationResult(p0)
       for (location in p0.locations){
           Log.d("position",location.latitude.toString()+","+location.longitude.toString())
       }
   }
}
```

We only need to create the request to the `fusedLocationClient` to perform the monitoring according to the parameters that we are interested in. In our case, as we want to update in real-time, we indicate `LocationRequest.PRIORITY_HIGH_ACCURACY` and an interval equal to five seconds. It is necessary to remember that heavy use of precise geolocation can drain the battery quickly and therefore we should use it only when it is essential.

```kotlin
val locationRequest: LocationRequest = LocationRequest.create()

locationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);
locationRequest.setInterval(5000);
locationRequest.setFastestInterval(200);
```

>**Learn more:**
>You can see more combinations of these parameters in the following link: [LocationRequest parameters](https://developers.google.com/android/reference/com/google/android/gms/location/LocationRequest)

Finally, we start monitoring with:

```kotlin
fusedLocationClient.requestLocationUpdates(locationRequest,
   locationCallback,
   Looper.getMainLooper())

```

While continuous monitoring is usually done from a service, requesting the current position is usually done from an activity. To this end, we need to create an instance of `FusedLocationProviderClient` in the activity.

```kotlin
fusedLocationClient = LocationServices.getFusedLocationProviderClient(this)
```

Then, we need to request the current position:

```kotlin
fusedLocationClient.getLastLocation().addOnSuccessListener(this) { location ->
   if (location != null) {
       val wayLatitude = location.getLatitude()
       val wayLongitude = location.getLongitude()
   }
}
```

