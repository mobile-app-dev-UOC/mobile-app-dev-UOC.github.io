---
layout: default
title: 1.2. Hardware 
parent: 1. Introduction
nav_order: 2
---

# 1.2 Hardware in mobile devices

One of the first steps in the development of a mobile app is defining its hardware requirements: the set of hardware features required to run the application. Having many requirements may enable more complex capabilities in the app, but it can also limit its use on older devices (and therefore reduce the target audience).

Moreover, modern mobile operating systems control access to certain hardware elements of the device. This is done to ensure user privacy and security and to prevent potential abuse by malicious applications. When designing an app, we must identify which hardware elements will be used to inform the user appropriately. We should also define how the application will behave if a user does not grant such permissions (or revokes them).

In the following, we list the basic hardware elements that need to be considered when developing a mobile app.

## 	CPU (Central Processing Unit)

Our application may need to execute computationally expensive tasks, for instance, image or video manipulation, artificial intelligence algorithms, or complex video games.  It is necessary to make sure that the target mobile devices can deal with the required processing load. 

In this regard, many devices dedicate specific parts of the processor to accelerate certain operations. For example, Apple Mx processors contain a specific architecture to accelerate operations required by certain types of neural networks and machine learning models.
	

## Screen

Our app will likely run on different devices with very different screen characteristics: **screen size** (usually measured as the length of the diagonal of the screen in centimeters or inches), **pixel density** (number of pixels per centimeter or inch), ... Our app should behave correctly on each type of screen and include multimedia resources tailored to device characteristics (size, resolution, ...). That is, the design of our app should be flexible enough to adapt and make the most of the features of each screen.

## Battery

While devices have an ever-increasing battery life, some hardware features are extremely battery-hungry. For example, apps that use GPS or Bluetooth intensively can drain a battery very quickly. 

## Local storage

We need to determine the amount of storage that our app will need and compare it to the capacity available on our target devices. It is also interesting to determine the level of security of the local file system, in case sensitive information needs to be stored locally.

##	Sound

Mobile devices have increased the quality of their sound systems by including variants such as Dolby Atmos. Dolby Atmos allows you to hear three-dimensional surround sound. To achieve this effect on mobile phones, the two speakers on the phone are usually used next to the two headphones.

## Physical buttons

Many devices have physical buttons that give us quick access to premium features such as the Android “back” button, speaker volume modification, or the “return to home” screen button. Moreover, some smart watches feature a physical auxiliary button. An example is the Digital Crown of the Apple Watch which allows you to return to the main screen, zoom on the screen, ...

>![Physical buttons in a smart watch](/images/01/button.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
>*Example of physical buttons in an Apple 4 Watch Series 4 smart watch*. 
>Source: [Janothan Parker @ Wikimedia](https://commons.wikimedia.org/wiki/File:Apple_Watch_Series_4_Extract.png). License: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

Smart glasses usually have physical buttons on the side for input management. These buttons allow you to move through menus, zoom, ...

Devices without physical buttons perform the above operations using gestures on the screen.

##	Touch screens

There are two basic types of technologies: **capacitive** and **resistive**.

Capacitive displays are more expensive to manufacture, which is why they are present in high- or mid-end devices. Screens of this type allow the tapping of more than one finger at a time. This feature is called multitouch. If our app is going to be installed on devices with multitouch screens, we can use a gesture-based programming technique. One of the most common gestures is a pinch, which is using two fingers to adjust the zoom.

Resistive displays are much cheaper to manufacture. They are present in low-end devices or industrial devices that do not require multitouch.

##	Haptic feedback 

Haptic feedback aims to provide information to the user by employing touch. It is achieved with the device's vibration motor. Its most common use is the vibration the device makes when it receives a call.

High-end devices feature vibration motors that can perform many different vibrations. These vibrations can be associated with different application actions, creating a much more immersive feel.

---

## Communications

### MWWAN (Mobile Wireless Wide Area Network)
	
MWWAN networks are wireless networks that allow the connection of multiple mobile devices across a broad geographic area. Telecom carriers offer coverage using different technologies, referred to by their generation as 2G, 3G, 4G, 5G, ... Each generation offers transmission speed and capacity improvements. Nevertheless, the deployment of these networks can be uneven between different countries or even between different regions of the same country. 
Today, most mobile devices are equipped with 4G technology. Older devices can only reach 3G and more advanced devices have 5G technology. We must keep in mind that if the country where the application is deployed does not have a 5G network, it will be irrelevant that our device supports this type of connectivity.

###	Wi-Fi 

[Wi-Fi](https://www.wi-fi.org/) (or WLAN) is a local area wireless network (*i.e.*, limited range) for data exchange. Understanding the **Wi-Fi version** (802.11 b/g/n/ac/ad) of our target devices is critical as there are big differences in **data transfer speed** among Wi-Fi versions. Transfer rate is measured in Mbps (Megabits per second). 

Another differential factor is the **range** of Wi-Fi networks depending on their version. The range is the maximum distance in meters from which the signal can be received.

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

[Bluetooth](https://www.bluetooth.com/) is a standard for wireless communications to exchange voice and data over short distances. For some applications, it is very important to determine the **Bluetooth version** supported by our device. There are large differences in energy consumption and distance reached depending on the Bluetooth version.

### NFC (Near Field Communication). 

[NFC](https://nfc-forum.org/) is a very short-distance communications technology used primarily for electronic payments.

###	RFID (Radio Frequency IDentification)

[RFID](https://www.ieee-rfid.org/) is a remote data storage and retrieval system that uses devices called tags. The main purpose of RFID technology is to transmit the identity of an object (similar to a unique serial number). For this reason, it is primarily used in many industrial mobile devices and the retail sector.

---

## Sensors

### Biometric identification sensors.

The best-known biometric sensor is the **fingerprint sensor**. It is typically used to access a device without entering any type of code or password.      

### Accelerometer and gyroscope.

The most typical use of the accelerometer and gyroscope is to determine whether our device is in a vertical or horizontal position. They are also used in some video games as a video game controller. In addition, indoor positioning attempts have been made using these sensors.

### Microphone

It is very important to determine if our device has any type of noise cancellation system.

###	Camera

Cameras are one of the fastest-evolving sensors in both photography and video. We must determine what features will be needed in our application: resolution, frames per second/fps (in case of video), etc.

Another very important aspect depending on the application we want to develop is whether we can access directly the raw data gathered by the sensor (RAW format). Most devices do not allow access to this format and only provide images that are already processed in JPEG or similar formats. If we are going to develop professional photography applications it is very useful to be able to directly access RAW information.

###	LIDAR

**Light Detection and Ranging** or **Laser Imaging Detection and Ranging**. 

LIDAR is a laser-based sensor that allows accurate distance measurements from our device to an object in our environment. Using this sensor dramatically improves the AR application experience. Another use case of LIDAR is performing a 3D scan of an object.

###	Positioning: GPS, Compass

Global Positioning System (GPS)-based positioning is a critical tool for a multitude of applications. The GPS system gives us the latitude and longitude of the position where our device is located. 

This positioning is complemented by a compass sensor that allows us to determine if we are moving in the right direction more accurately.  Remember that GPS positioning does not work indoors, because the signal is not received correctly from the GPS satellites that provide us with the information. Indoor positioning attempts have been made using only the compass information.

###	Medical sensors

Many smartwatches have a broad array of medical sensors to capture information about heart rate and blood oxygen level. Ultrasound-based blood pressure sensors are currently being worked on. Moreover, some smartwatches carry several heart rate sensors to allow medical-grade electrocardiograms (ECGs) to be performed.



