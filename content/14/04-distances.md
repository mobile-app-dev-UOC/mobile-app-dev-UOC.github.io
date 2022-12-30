---
layout: default
title: 14.4. Distances
parent: 14. Geolocation and maps
nav_order: 4
---

# 14.4. Calculating distances between two locations

To calculate the approximate distance in meters between two `Location`, we can use the method `distanceTo` method. For example, if the variable location contains our current position we can determine the distance in meters to the following coordinates: 
`41.404076846062054, 2.189701544333684`

```kotlin
val endPoint = Location("locationD")
endPoint.latitude = 41.404076846062054
endPoint.longitude = 2.189701544333684

val distance: Float = location.distanceTo(endPoint)
```

Remember that this method calculates the physical distance between two points, without considering possible barriers or the existence of roads or paths between them.
