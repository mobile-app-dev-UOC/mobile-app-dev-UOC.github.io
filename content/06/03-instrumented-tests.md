---
layout: default
title: 6.3. Instrumented tests
parent: 6. Debugging and testing
nav_order: 3
---

# 6.3. Instrumented tests 

Instrumented tests do depend on Android and run on the emulator or a physical device. These types of tests typically incorporate **interface tests**. Moreover, in addition to the correction of the tests, we may be interested in their performance.

The [Espresso library](https://developer.android.com/training/testing/espresso) is used to perform interface tests. Espresso allows us to automatically interact with interface elements.

To perform Instrumented tests using Espresso, the dependencies are:

```gradle
dependencies {
  testImplementation 'junit:junit:4.13.2'

  androidTestImplementation 'androidx.test.ext:junit:1.1.3'
  androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'


  androidTestImplementation "androidx.test.espresso:espresso-intents:3.4.0"
  androidTestImplementation 'androidx.test:runner:1.2.0'
  androidTestImplementation 'androidx.test:rules:1.2.0'
  implementation "androidx.test.ext:junit-ktx:1.1.3"
}
```

Within the `androidTest` folder we create a Kotlin file with a class that will represent our test.

```kotlin
@RunWith(AndroidJUnit4::class)
class MainActivityTest{

   @get:Rule var activityScenarioRule = activityScenarioRule<MainActivity>()

   @Test
   fun Test1() {
       onView(withId(R.id.username))
           .perform(typeText("user@mail.com"), closeSoftKeyboard())

       onView(withId(R.id.password))
           .perform(typeText("cat"), closeSoftKeyboard())

       onView(withId(R.id.login)).perform(click())

       onView(withId(R.id.error)).check(matches(withText("ERROR")))
   }
}
```

The Espresso library allows us to perform actions such as:

- Write a text in an EditText:

```kotlin
  onView(withId(R.id.username))
           .perform(typeText("user@mail.com"), closeSoftKeyboard())
```

- Press a button:
 
```kotlin
  onView(withId(R.id.login)).perform(click())
```
- Verify that a text appears in a given EditText:
 
```kotlin
  onView(withId(R.id.error)).check(matches(withText(“ERROR”)))
```

The following [video](/images/06/test-video.mp4) shows a testing session. We can see how at the bottom of the screen, in the Cover browser, it reports the test result and the time that the run has lasted.

> [Result of a Espresso interface test.](/images/06/espresso-test.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Result of a Espresso interface test.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


> **Learn more:**
> Following the following link to To try Espresso snippets: [https://developer.android.com/guide/fragments/test](https://developer.android.com/guide/fragments/test)
