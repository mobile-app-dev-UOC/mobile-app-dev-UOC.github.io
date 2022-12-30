---
layout: default
title: 14.3. Geodecoding
parent: 14. Geolocation and maps
nav_order: 3
---

# 14.3. Geodecoding: querying the name of a location

It is common to want to get the name of a place from its coordinates. To do this we use a **geodecoder** ("geocoding" would be the reverse process: converting a name to latitude + longitude coordinates).

```kotlin
fun DecodeLatLong(MyLat:Double,MyLong:Double)
{
   geocoder val = Geocoder(this, Locale.getDefault())
   val addresses: List<Address> = geocoder.getFromLocation(MyLat, MyLong, 1)
   val address: String = addresses[0].getAddressLine(0)
   val countryName: String = addresses[0].countryName
   val locality: String = addresses[0].locality
}
```


For instance, if we execute this method with the following parameters:

```kotlin
DecodeLatLong(41.404076846062054, 2.189701544333684)
```

we obtain the following values in address, country and locality:

- `address`: Avinguda Diagonal, 274, 08018 Barcelona, Spain
- `countryName`: Spain
- `locality`: Barcelona
