---
layout: default
title: 1.4. Types of apps
parent: 1. Introduction
nav_order: 4
---

# 1.4 Types of mobile applications

When we develop an app, we can target a single ecosystem, but we will generally want the application to be available in various ecosystems to increase the market size. A potential strategy could be to develop an ecosystem-specific application (native application), but certain technologies allow for unique development that can be deployed across different ecosystems (cross-platform application). In the following, we discuss the available strategies and their advantages and disadvantages.

---

## Native apps

Native applications are specially designed and deployed for the execution context (platform or device) on which they will run. As a result, they can take advantage of all the capabilities of those devices. For this reason, native apps deliver the best user experience. Sometimes,they are also subject to specific standards of device manufacturers or platform owners. 

For instance, some types of applications that are typically implemented natively are applications that use multitasking, specific use of sensors, specific use of Bluetooth, ...


### Developing native apps on Android

To develop Android apps, the **SDK** (software development kit) associated with the version of Android targeted by our application is required.

In 2014, Google released the first stable version of [Android Studio](https://developer.android.com/studio), the official integrated development environment for the Android platform. The environment features an editor based on [JetBrains IntelliJ IDEA](https://www.jetbrains.com/idea/) software and various tools such as emulators, a graphic interface designer, and a code debugger.


> ![Android Studio](/images/01/android-studio.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Android Studio.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)
 
Android Studio integrates with [Android NDK](https://developer.android.com/ndk) (Native Development Kit). NDK is a set of tools that enable you to deploy parts of applications using native code languages such as C and C++. When we use native Android Studio code, it allows us to generate a version of our application for each hardware architecture.

Initially, the programming language of choice to develop apps was Java (for processor-independent developments) and C++ or C (for processor-dependent developments). However, since 2017 the official language for developing Android apps is Kotlin. Kotlin was also developed by JetBrains, the company that develops Android Studio. As a result, Android Studio includes great developer tools such as the fantastic cut-and-paste that automatically converts from Java syntax to Kotlin syntax. 


Kotlin runs on the Java Virtual Machine. Its ability to interoperate with Java has facilitated its rapid adoption. In addition, Kotlin can also be compiled into JavaScript. 


### Developing native apps on the Apple ecosystem

[Xcode](https://developer.apple.com/xcode/) is the development tool provided by Apple to build apps for iOS, iPadOS, and watchOS. Originally, the programming language of choice was Objective-C. Objective-C was a language born in 1980 that had barely been modified since then. That’s why in 2014 Apple launched Swift as a preferred programming language to develop applications. Swift is a modern language with a very fast learning curve. This explains its rapid adoption by the developer community. However, the presence of Objective-C behind the scenes continues to be embedded in certain common libraries.


> ![Xcode and iPhone emulator](/images/01/xcode.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Xcode and iPhone emulator.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Swift is very efficient and can easily communicate with Objective-C code. Swift is a strongly typed language: all operations must be carried out with compatible types, which are either explicit or can be inferred at compile time. That is, there is no need to include runtime type casts, improving the performance of Swift programs. 

In 2019, Apple launched SwiftUI, which replaced the previous interface system and modified the primary application design pattern from Model–View–Controller (MVC) to Model-View-ViewModel (MVVM).

From Xcode we can access the SDKs of the different versions of the operating system, debugging tools, and emulators for the different devices.

---

## Cross-platform applications

### Xamarin

[Xamarin](https://dotnet.microsoft.com/en-us/apps/xamarin) is a cross-platform environment that enables native application generation using .NET (Mono) free code deployment and the C# programming language. In 2016, Microsoft purchased the company developing Xamarin. This purchase further increased its integration with the Microsoft development environment (Visual Studio). Xamarin generates native component-based interfaces, so they perform much better than applications based on HTML and JavaScript encapsulation. 

On the other hand, it has problems interconnecting with libraries in Kotlin or Swift. For this reason, Xamarin developers often have to restrict themselves to the components provided by Xamarin, and that sometimes affects the user experience.

### React Native

[React Native](https://reactnative.dev/) was introduced by Facebook in 2015 and it is currently a very consolidated platform. The idea behind React Native was to enable web developers to develop mobile applications using JavaScript in the same way they used React in the web environment. The code is compiled into native applications. Unlike Xamarin, it has a large number of third-party libraries and it is supported by Facebook. Interface components are transformed into native components. It is necessary to test that these components operate correctly on each platform. 

Here are some applications made with React Native: Instagram, Facebook, Facebook Ads, Skype, and Tesla.

### Flutter

[Flutter](https://flutter.dev/) was introduced by Google in 2017. Since its use has expanded very quickly. Its programming language of choice is Dart and it has a very good performance. Unlike other cross-platform systems that struggle with interface elements, Flutter has its own graphical interface system deployed in C++ that runs smoothly and has many components. This system reduces the need to develop custom native components. Because of this, the default apps are displayed in the same way on Android and iOS devices. Nevertheless, in some cases, small components in native technologies will still need to be developed.  

Some sample applications developed using Flutter include are Google Ads app, Philips Hue, and My BMW.

> ![Flutter architecture](/images/01/flutter.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Flutter architecture.*  
> Source: [Flutter.dev](https://docs.flutter.dev/resources/architectural-overview) License: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

### Kotlin Multiplatform

[Kotlin Multiplatform](https://kotlinlang.org/docs/multiplatform.html), developed by Google, is the latest addition to our cross-platform application development toolkit. There is great competition between Flutter and Kotlin Multiplatform as they have been created by different development groups within Google. Google is still undecided and is backing both projects, so this hinders their option to achieve massive adoption. 

For the Android ecosystem, this is a perfect solution as developers only need to know Kotlin. When deploying the iOS version of the app, it is necessary to develop specific graphic components. The idea behind Kotlin Multiplatform is that native application developers can develop most of the applications in a common language and then make the graphic components they need for different architectures. 

### Unity

[Unity](https://unity.com/) is a multi-platform video game engine that has quickly become widely adopted. Considering mobile development, Unity supports versions for Android and iOS using OpenGL ES in both cases. From version 5.6, it also supports Metal low-level APIs on iOS and Vulcan on Android. Unity’s programming languages are C# and UnityScript, a JavaScript-like language. It offers a well-managed component repository called the [Unity Asset Store](https://assetstore.unity.com/). It generates applications that achieve native performance. 

The problem with this type of system is that the provider, in this case, Unity, has to provide access to the new capabilities introduced by the operating system.  Its version known as “Personal” is free but can only be used if your income is below $100,000.

### Ionic Framework

[Ionic](https://ionicframework.com/) enables the creation of hybrid applications based on web technologies. In Ionic, there is a deep relationship between the native part and the JavaScript framework [Angular](https://angular.io/). Using this framework, Ionic ensures higher performance in JavaScript-based code. Moreover, you can still work with React, Vue, or directly with our own JavaScript framework.






