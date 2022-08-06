---
layout: default
title: 1.3. Ecosystems
parent: 1. Introduction
nav_order: 3
---

# 1.3 Ecosystems

Mobile devices incorporate an operating system that encapsulates hardware features and offers libraries for easy development and APIs to access additional services. We refer to an operating system (or a family of related operating systems) as an **ecosystem**, which includes the set of hardware devices that support it, and the set of available libraries and services (app store, maps, mobile payment, ...). 

In addition to choosing the target ecosystem for an application, it is important to determine the target **version** of the ecosystem. Higher versions will offer more features, more services, and access to the latest hardware capabilities of the latest devices. However, they will also be supported on fewer devices so our app will not be available to certain users.

## Apple (iOS, iPadOS, watchOS)

As often happens with Apple, Apple owns its own proprietary operating systems for its mobile devices. This gives them more control of your products and optimizes their operating system’s use of available hardware.

> ![Apple devices](/images/01/apple-devices.jpg)
> *iPhone 13 Pro and Apple Watch.*  
> Source: [Simon Waldherr @ Wikimedia](https://commons.wikimedia.org/wiki/File:IPhone_13_Pro_and_Apple_Watch.jpg) License: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

[iOS](https://www.apple.com/ios) is the operating system used on iPhone devices. The same system was also initially used on other Apple devices such as the iPad and iPod.

iOS was designed from Mac OS. It included aspects related to mobile interaction and changing the permission framework to fit a device where computer-savvy users install and uninstall many apps, each with different permissions. It highlights the level of security achieved on your devices, which includes file system encryption.
	
In 2019, Apple introduced [iPadOS](https://www.apple.com/ipados) to replace iOS on its tablet-style devices. iPadOS is essentially iOS with the following added capabilities: 

-	Multitasking. Similarly to desktop operating systems, iPadOS devices can run multiple applications at once. These are distributed across the tablet screen and are all visible at the same time.
-	Added keyboard and mouse support.
-	Support for external storage drives is added.
-	Allow the tablet to act as a second monitor on an Apple computer without the need to install additional software ([Universal Control](https://support.apple.com/en-us/HT212757)).

[watchOS](https://www.apple.com/watchos) is the operating system used in Apple smart watches. It was also created from iOS.  At first, watchOS was only a remote desktop and apps were running on the iPhone. Starting with watchOS 4.0 and Apple Watch Series 3 apps already run independently on the watch.  Apple has focused the Apple Watch on e-health to the point of including the ability to perform electrocardiograms. 

---

## Android

In October 2003, Andy Rubin, Rich Miner, and others founded Android Inc. in Palo Alto, California (USA).

In August 2005, Google acquired Android Inc. Key employees of Android Inc., including Andy Rubin, Rich Miner, and Chris White, remained with the company after the acquisition.

In November 2007, the [Open Handset Alliance](https://www.openhandsetalliance.com/) was also announced. It was a consortium of several companies including Texas Instruments, Broadcom Corporation, Google, HTC, Intel, LG, Marvell Technology Group, Motorola, Nvidia, Qualcomm, Samsung Electronics, Sprint Nextel, and T-Mobile. The goal of the Open Handset Alliance is to develop open standards for mobile devices. On the same day, the Open Handset Alliance also announced its first product, Android, a mobile platform built on Linux kernel version 2.6. 

Today we can identify three types of Android systems:

### a) Android with Google API

This is the most common version among devices targeting the general.  This version refers to Android code maintained by Google plus a set of functionalities known as the Google API. There is a wide range of Google APIs that facilitate programming such as: notifications, access to Google Maps, connection to Gmail, authentication, artificial intelligence-related libraries, improved keyboards, …

Lack of access to these APIS will have a severe impact on development time.

### b) Android without Google API

Devices can only access Google APIs if the manufacturer purchases a license from  Google. Thus, the main reason for a manufacturer to ignore Google APIs is to avoid licensing costs or legal issues related to trade wars. In these devices where the Google APIs are not available, the manufacturer usually provides its own version of the most common ones.


### c)	Android Open Source Project  (AOSP) 

[AOSP](https://source.android.com) is the community-maintained version of Android on the Android Open Source Project (AOSP). This is a basic version of Android. All source code is available and is typically used to launch small Android-based proprietary devices. However, the code is not well documented, and developing on top of it is very time-consuming. For instance, the keyboard included in AOSP does not support languages such as Chinese or Japanese and it does not support a gesture writing system.

---

## Huawei (HarmonyOS)

Given the impossibility of using Android due to the trade war with the United States, Chinese giant Huawei was forced to accelerate the development of its own operating system. There is controversy about whether Harmony should be considered a true new operating system or simply an Android version with a different name and Google APIs replaced by Huawei APIs.  
[https://www.harmonyos.com/en/](https://www.harmonyos.com/en/)


##	Samsung (Tizen)
Tizen is a Linux-based mobile operating system supported by the Linux Foundation, developed and used primarily by Samsung Electronics. Tizen is designed to use HTML5 applications. The idea is that it is very easy to carry applications developed using web architectures to mobile environments.  
[https://www.tizen.org/](https://www.tizen.org/)

## Google Fucshia

Currently, Google is creating a replacement for Android. Google Fuchsia is a new operating system based on a new core called Zircon. Thanks to this new OS, Google wants to end Android’s dependency on the Linux core.

Zircon tries to reduce the number of functionalities provided by the operating system so that the device spends as little time as possible running system code. For example, the file system is not within the OS core. This would allow different versions of Fuchsia with different file systems or even different file systems per application.  
[https://fuchsia.dev/](https://fuchsia.dev/)


> ![Zircon](/images/01/zircon.png)
> *Comparing typical core components to Zircon core components*
> Source: [Fuchsia.dev](https://fuchsia.dev/fuchsia-src/get-started/learn/intro/zircon) License: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

---

There are also other ecosystems, mostly based on Android or Linux versions. These systems are not used by many devices.
