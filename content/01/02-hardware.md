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