---
layout: default
title: 1.2. Hardware 
parent: 1. Introduction
nav_order: 2
---

# 1.2 Hardware in mobile devices

One of the first steps in the development of a mobile app is defining its hardware requirements: the set of hardware features required to run the application. Having many requirements may enable more complex capabilities in the app, but it can also limit its use on older devices (and therefore reduce the target audience).

Moreover, modern mobile operating systems control access to certain hardware elements of the device. This is done to ensure user privacy and security and to prevent potential abuse by malicious applications. When designing an app, we must identify which hardware elements will be used and when, inform the user appropriately and define how the application will behave if a user does not grant (or revokes) such permissions.

In the following, we list the basic hardware elements that need to be considered when developing a mobile app.

## 	CPU (Central Processing Unit)

Our application may need to execute tasks that are computationally expensive, for instance, image or video manipulation, artificial intelligence algorithms or complex videogames.  It is necessary to make sure that the target mobile devices can deal with the required processing load. 

In this regard, many devices dedicate specific parts of the processor to accelerate certain operations. For example, Apple Mx processors contain a specific architecture to accelerate operations required by certain types of neural networks and machine learning models.
	

## Screen

It is very likely that our app will run on different devices with very different screen characteristics: **screen size** (usually measured as the length of the diagonal of the screen in centimeters or inches), **pixel density** (number of pixels per centimeter or inch), ... Our app should behave correctly on each type of screen and include multimedia resources tailored to device characteristics (size, resolution, ...). That is, the design of our app should be flexbile enough to adapt and make the most of the features of each screen.

## Battery

While devices have an ever-increasing battery life, some hardware features are extremely battery-hungry. For example, apps that use GPS or Bluetooth intensively can drain a battery very quickly. 

## Communications

### MWWAN (Mobile Wireless Wide Area Network)
	
MWWAN networks are wireless networks that allow the connection of multiple mobile devices across a broad geographic area. Telecom carriers offer coverage using different technologies, referred to by their generation as 2G, 3G, 4G, 5G, ... Each generation offers transmission speed and capacity improvements. Nevertheless, the deployment of these networks can be uneven between different countries or even between different regions of the same country. 
Today, most mobile devices are equipped with 4G technology. Older devices can only reach 3G and more advanced devices have 5G technology. We must keep in mind that if the country where the application is deployed does not have a 5G network, it will be irrelevant that our device supports this type of connectivity.

###	Wi-Fi 
Wi-Fi (or WLAN) is a local area wireless network (*i.e.*, limited range) for data exchange. Understanding the **Wi-Fi version** (802.11 b/g/n/ac/ad) of our target devices is critical as there are big differences in **data transfer speed** among Wi-Fi versions. Transfer rate is measured in Mbps (Megabits per second). 

Another differential factor is the range of Wi-Fi networks depending on their version. The range is the maximum distance in meters from which the signal can be received.

As a result, the Wi-Fi version can affect our choices regarding data transmission formats and strategies. 

|WiFi Standard	 | Maximum Speed	| Distance | 
|----------------|------------------|----------|
|802.11b	     | 11 Mbit/s	    | 140 m    |
|802.11g	     | 54 Mbit/s	    | 140 m    |
|802.11n	     | 600 Mbit/s     	| 250 m    |
|802.11ac	     | 6.93 Gbit/s	    | 100 m    |
|802.11ad	     | 7.13 Gbit/s	    | 10 m     |

In wireless transmissions, both speeds and distances refer to ideal conditions where no interference of any kind occurs. In actual use, both the speed and distance are greatly affected by aspects such as physical obstacles, interference from other signals, ...

### Bluetooth

Bluetooth is a standard for wireless communications to exchange voice and data over short distances. For some applications it is very important to determine the **Bluetooth version** supported by our device. There are large differences in energy consumption and distance reached depending on the Bluetooth version.

### NFC (Near Field Communication). 

NFC is a very short distance communications technology used primarily for electronic payments.

###	RFID (Radio Frequency IDentification)

RFID is a remote data storage and retrieval system that uses devices called tags. The main purpose of RFID technology is to transmit the identity of an object (similar to a unique serial number). For this reason, it is primarily used in many industrial mobile devices and in the retail sector.

## Local storage

We need to determine the amount of storage that our app will need and compare it to the capacity available on our target devices. It is also interesting to determine the level of security of the local file system, in case sensitive information needs to be stored locally.

## Sensors

### Biometric identification sensors.

The best known biometric sensor is the **fingerprint sensor**. It is typically used to access a device without entering any type of code or password.      

### Accelerometer and gyroscope.

The most typical use of the accelerometer and gyroscope is to determine whether our device is in a vertical or horizontal position. They are also used in some video games as a videogame controller. In addition, indoor positioning attempts have been made using these sensors.

### Microphone

It is very important to determine if our device has any type of noise cancellation system.




