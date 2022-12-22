---
layout: default
title: 14. Geolocation and maps
nav_order: 14
has_children: true
---

# 14. Geolocation and maps

Geolocation is the ability of mobile devices to indicate the geographic coordinates (length and latitude) they are in.

The most common systems for calculating position are: **GPS**, **positioning using WiFi**, and **positioning using mobile antennas**.


Positioning using GPS is based on receiving at least the signal from four satellites in the GPS satellite network. Once the signal is received, the device calculates its distance to the different satellites using reverse [trilateration](https://en.wikipedia.org/wiki/True-range_multilateration). With these distances, the relative position of the device to these satellites is easily calculated. Since the satellite signal includes the absolute position of the satellites, it is possible to calculate the absolute position of the device. It is the most accurate method for geolocation but also the one that consumes the most energy. Typical accuracy is a few meters, although GPS positioning often has problems in indoor locations.

Positionings using WiFi and mobile antennas appear as lower energy-efficiency alternatives to using GPS. 

Positioning via Wi-Fi and mobile antennas is based on the existence of large databases where millions of Wi-Fi and antennas from around the globe are geo-located. Devices access this database using the WiFi router’s Mac address or unique antenna identifier. These databases are owned by companies such as Google, Apple, Skyhook, ...

Indoor positioning is usually done in one of two ways:
-	Using additional hardware, devices that are placed inside and help us position ourselves. An example of this technology would be [Apple AirTags](https://www.apple.com/es/airtag/).
-	Techniques based on mapping the indoor space from the different Wi-Fi signals and other signals received at each point. This technique does not require additional hardware.

## Basics

Basics:

- **Latitude:** The latitude is the angular distance between the equator and a given point on Earth, measured along the meridian at which that point is located. It can be used to specify the distance of a location north or south from the equator. It is expressed in angular units. 

- **Longitude:** The longitude expresses the angular distance between a given point of the earth’s surface and the 0° meridian measured along the parallel in which that point is located. Provides location in the east or west direction. 

- **Geographic distance between two points on Earth.** We must remember that since Earth is (approximately) a sphere, we must use spherical trigonometry to calculate the distance between two points.

We have to distinguish between two concepts of this section: Geolocation and Maps.

**Geolocation:** It consists of determining the position (latitude and longitude) of the user at any given time, even if our application is running in the background.

**Maps:** It consists of displaying a map. This map can optionally contain our position, items we want to highlight, routes, ...




{:toc:}
