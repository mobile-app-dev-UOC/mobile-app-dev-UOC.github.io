---
layout: default
title: 14.1. Permissions
parent: 14. Geolocation and maps
nav_order: 1
---

# 14.1. Requesting permissions

Geolocation requires an explicit request of permissions from the user. This check is usually done in the `onCreate` callback of the main activity.

The first block of code indicates whether we already have the permissions or need to request them:

```kotlin
if(isPermissionsGranted()){
	// we have permissions 
} else {
	requestLocationPermission()
}
```

In the code above, the functions are defined as follows:

```kotlin
private fun isPermissionsGranted() = ContextCompat.checkSelfPermission(
   this,
   Manifest.permission.ACCESS_FINE_LOCATION
) == PackageManager.PERMISSION_GRANTED

private fun requestLocationPermission() {
   if (ActivityCompat.shouldShowRequestPermissionRationale(this,
           Manifest.permission.ACCESS_FINE_LOCATION)) {
       Toast.makeText(this, "Go to settings", Toast.LENGTH_SHORT).show()
   } else {
       ActivityCompat.requestPermissions(this,
           arrayOf(Manifest.permission.ACCESS_FINE_LOCATION),
           0)
   }
}
```

Finally, if it has been necessary to request permissions and to receive the user's response, we would do the following:

```kotlin
override fun onRequestPermissionsResult(
   requestCode: Int,
   permissions: Array<out String>,
   grantResults: IntArray
) {
   when(requestCode){
       0 -> if(grantResults.isNotEmpty() && grantResults[0]== PackageManager.PERMISSION_GRANTED){
          // user accepts permission
       }else{
           Toast.makeText(this, “To activate the location go to settings and accept the permissions”, Toast.LENGTH_SHORT).show()
       }
       else -> {}
   }
}
```


