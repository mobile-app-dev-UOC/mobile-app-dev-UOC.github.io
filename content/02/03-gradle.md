---
layout: default
title: 2.3. Gradle
parent: 2. Android Studio
nav_order: 3
---

# 2.3. Gradle

Gradle is a generic plugin-based toolkit that is used by Android Studio to compile an app and generate APK files. An APK file is a .zip file that contains a file structure that an Android device knows how to install. That is, APK files package the app so that it can be installed and executed on an Android device. 

> ![An APK file](/images/02/apk.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *An APK file for distribuing an Android application.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


Gradle lets us generate different versions of the application for different types of users or devices: 

- For example, we can target different types of processors when the application has a native component. In this scenario, Google Play decides which version should be downloaded (based on the type of device owned by the user) and only the proper version appears in the app store.

- Another example would be the generation of a free version of the app with limited features and a professional version with full functionality. In this scenario, two versions of the app appear in the store and the user should select which version they want to install.

- A final example is generating versions of an app for different screen types. It should be noted that Google recommends avoiding this practice whenever possible, as this forces us to upload different versions of APK to Google Play.

An Android project usually contains several Gradle files: one that is always used for the entire project; and another one for each app included in the project. This means that by default we should prepare at least two Gradle files: the project and the single application that is generated in the project.

The default Gradle file usually looks like this:

```gradle
// Top-level build file where you can add configuration options common to all sub-projects/modules.
plugins {
   id 'com.android.application' version '7.1.2' apply false
   id 'com.android.library' version '7.1.2' apply false
   id 'org.jetbrains.kotlin.android' version '1.6.10' apply false
}

task clean(type: Delete) {
   delete rootProject.buildDir
}
```

The `plugins` section lists which plugins are required by Gradle to generate the APK. In this case, as we develop the app using the Kotlin language, we will need the plugin that allows us to compile the Kotlin code. This plugin is “org.jetbrains.kotlin.android”. The `apply = false` parameter indicates that it should not be used automatically: Android Studio should check the specific Gradle settings for each module or application.

After the `plugins` section, a task (`clean`) is added to Gradle to delete everything in the build directory. 

After inspecting the project’s Gradle file, let us now take a look at the Gradle file for an application:

```gradle
plugins {
   id 'com.android.application'
   id 'org.jetbrains.kotlin.android'
}

android {
   compileSdk 31

   defaultConfig {
       applicationId "edu.uoc.test"
       minSdk 28
       targetSdk 31
       versionCode 1
       versionName "1.0"

       testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
   }

   buildTypes {
       release {
           minifyEnabled false
           proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
       }
   }
   compileOptions {
       sourceCompatibility JavaVersion.VERSION_1_8
       targetCompatibility JavaVersion.VERSION_1_8
   }
   kotlinOptions {
       jvmTarget = '1.8'
   }
   buildFeatures {
   	viewBinding true
   }

}

dependencies {

   implementation 'androidx.core:core-ktx:1.7.0'
   implementation 'androidx.appcompat:appcompat:1.4.1'
   implementation 'com.google.android.material:material:1.5.0'
   implementation 'androidx.constraintlayout:constraintlayout:2.1.3'
   testImplementation 'junit:junit:4.13.2'
   androidTestImplementation 'androidx.test.ext:junit:1.1.3'
   androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
}
```

The first section, `plugins`, lists the plugins that will actually be used to compile this module. In this case, the "com.android.application" plugin tells Gradle how to generate an Android application and the "org.jetbrains.kotlin.android"   plugin is used so that Kotlin code can be compiled. If instead of an application we were developing a library to be used by Android applications, instead of using the plugin "com.android.application" we would have to use "com.android.library".

The `Android` section is where you configure Android-specific settings, such as the Android version.There are several ways to name an Android version. A developer identifies the Android version using the **API level**.

| Platform version | API level | VERSION_CODE |
|------------------|-----------|--------------|
| Android 13 (beta)|	33	   | TIRAMISU     |
|Android 12		   | 32		   | S_V2		  |
|Android 12		   | 31	       | S            |
|Android 11	       |30         | R            |
|Android 10	       |29         | Q            |

The API level is a unique integer value. For the same Android version name (e.g., “Android 12”) there may be different API levels (e.g., levels 31 and 32) indicating that errors have been corrected or new features have been added. 

To set up our application, it will be necessary to specify a value for the Android API level in three parameters: the `compileSdk`, the `minSdk` and the `targetSdk`.

```gradle
compileSdk 31
```

The `compileSdk` is the Android API level that Gradle must use to compile the application. The application can also use the features offered by older API levels (but not newer ones).

```gradle
  defaultConfig {
       applicationId "edu.uoc.test"
       minSdk 28
       targetSdk 31
       versionCode 1
       versionName "1.0"

       testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
   }
```

The `applicationId` is our application identifier within the Android ecosystem. It is a unique identifier that identifies our app within the Google Play Store. Again, [reverse domain notation](https://en.wikipedia.org/wiki/Reverse_domain_name_notation) is used. 

`minSdk` indicates the minimum Android version where our app can run. Our app will not be installed on any device with an Android version less than `minSdk`.

`targetSdk` is the version of Android for which the app has been designed. It usually matches `compileSdk`, and it will the one where our app offers the best user experience.

The `versionCode` is the version number of our app (not Android), which must be an incremental numeric value and is a private value (not shown to users). It is used internally by the Google Play Store to distinguish between different versions. In this way, it will always offer the latest version to each type of device.

Then, the versionName is the version name of the application that is displayed to users. It only has informational value. Although it is usually used in a format like “1.2” or “1.3.3”, no specific format is required.

`testInstrumentationRunner` instructs Gradle that we need to use JUnit4 test classes. JUnit4 is a testing library that enables us to write and run unit tests. These tests check individual methods in isolation by defining an input and the expected output that should be computed for that input.

```gradle
buildTypes {
   release {
       minifyEnabled false
       proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
   }
}
```

`minifyEnabled` equals true tells the compiler to perform reduction, obfuscation, and code optimization tasks. This would be the desirable option when we are distributing our application to third parties. On the contrary, `minifyEnabled = false` is mandatory when we are debugging our app so that we can place checkpoints in our code. Thus, we should only use `minifyEnabled = true` in the release version of our app.

`proguardFiles` is a file containing rules to guide the code optimization process. They will only be considered if `minifyEnabled` is set to true. For example: the rule `keep public class MainActivity` would indicate that MainActivity class should not be optimized.

```gradle
compileOptions {
       sourceCompatibility JavaVersion.VERSION_1_8
       targetCompatibility JavaVersion.VERSION_1_8
 }
```

These settings indicate that advanced programming features available in Java 8 can be used (remember that Android Studio allows you to mix Kotlin code and Java code within your app). Therefore, this section affects those components that are developed in Java, either in our code or in third-party code used in our app.


```gradle
kotlinOptions {
   jvmTarget = '1.8'
}
```

Like Java, Kotlin is an interpreted language that is compiled into an intermediate bytecode for execution on a virtual machine. The `jvmTarget` setting defines the version of that bytecode. Specifically, 1.8 is the oldest version that is not obsolete.

```gradle
buildFeatures {
   viewBinding true
}
```

The `viewBinding` setting instructs Gradle to create classes so that view properties can be accessed directly. Without `viewBinding`, the following code needs to be used to access a property:

```kotlin
val t:TextView = findViewById<TextView>(R.id.text1)
t.text = "Hello"
```

With `viewBinding` equal to true you can use instead the following shorthand:

```kotlin
binding.text1.text="Bye"
```

The last section of the Gradle file is the dependency section:

```gradle
dependencies {
   implementation 'androidx.core:core-ktx:1.7.0'
   implementation 'androidx.appcompat:appcompat:1.4.1'
   implementation 'com.google.android.material:material:1.5.0'
   implementation 'androidx.constraintlayout:constraintlayout:2.1.3'
   testImplementation 'junit:junit:4.13.2'
   androidTestImplementation 'androidx.test.ext:junit:1.1.3'
   androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
}
```

This section specifies which extensions must be downloaded in order to generate our application. For example: 

- "androidx.core:core-ktx" gives us the ability to use a simplified Kotlin syntax for several tasks that are very common in app programming. This option lets us use lambda expressions, defaults, ... (see [Section 7](/content/07) on advanced Kotlin features).
 
- "androidx.constraintlayout:constraintlayout" lets us use the most recent type of Android interfaces.

The **settings.gradle** file tells Gradle in which repositories it can find the plugins and libraries indicated in the dependencies.

```gradle
pluginManagement {
   repositories {
       gradlePluginPortal()
       google()
       mavenCentral()
   }
}
dependencyResolutionManagement {
   repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
   repositories {
       google()
       mavenCentral()
   }
}
rootProject.name = "Test"
include ':app'
```

In this case, we indicate that the repositories where plugins must be searched: `gradlePluginPortal`, `google`, and `mavenCentral`. Dependencies will be searched in `google` and `mavenCentral`.

If your app uses custom libraries from a specific provider, you will need to add the provider’s repository to this list.


