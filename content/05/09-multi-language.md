---
layout: default
title: 5.9. Multi-language
parent: 5. User interfaces
nav_order: 9
---

# 5.9. Multi-language support

Android can automatically choose interface texts depending on the language of the device. To do this, we must create a resource folder of type `value` with the country modifier we want to include.

> ![Selecting language/region for localization.](/images/05/country-selection.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Selecting language/region for localization.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, we add a resource file of type `string` and add the text in the new language with the same identifier. When a language is not found, the contents of the `value` folder with no language modifier are used instead.

For example, we store the default values in `value\strings`:

```xml
<resources>
   <string name="app_name">layouts3</string>
   <string name="str_hello">hello</string>
</resources>
```

Then, we store the Spanish texts in `value-es\strings`:

```xml
<resources>
   <string name="app_name">layouts3</string>
   <string name="str_hello">hi</string>
</resources>
```

Inside the app we refer to each string by name.

```xml
<TextView
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:text="@string/str_hello"
   android:id="@+id/label"
   android:textSize="30sp"
   app:layout_constraintBottom_toBottomOf="parent"
   app:layout_constraintLeft_toLeftOf="parent"
   app:layout_constraintRight_toRightOf="parent"
   app:layout_constraintTop_toTopOf="parent" />
```

Words may have a different number of characters in each language. For this reason, we should not hard-code the widths of any view that contains text. That is, we must use `android:layout_width="wrap_content"`.

To access these strings from our Kotlin code we use R.string:
```kotlin
binding.label.setText(R.string.str_hello)
```

To set a particular language programmatically, we use the following statement: 
```kotlin
val current: Locale = resources.configuration.local[0]
```

Note: Resource files have a different appearance if we are using the Android project view or the file system view.

> ![Appearance of resource files.](/images/05/resource-files.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Appearance of resource files.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

>**Learn more:**
>In addition to the above, some languages will need additional settings because they are written from right to left or have other special features. We will also need specific methods for formatting strings that represent dates or other items. You can find further information about these issues in the following link:
[https://developer.android.com/training/basics/supporting-devices/languages](https://developer.android.com/training/basics/supporting-devices/languages)

