---
layout: default
title: 5.2. Basic components
parent: 5. User interfaces
nav_order: 2
---

# 5.2. Basic components 

Basic components inherit from class View. In this section, we will review the characteristics of the most common components by emphasizing some typical design-related issues.

All components have the properties `layout_width` and `layout_height`, which indicate width and height respectively. Its value can be defined in four different ways: 

- Specifying a constant value using any unit:

```xml
android:layout_width=”150dp”
android:layout_height=”50dp”
```
- Using the same size as the GroupView that contains the component:

```xml
android:layout_width=”match_parent”
android:layout_height=”match_parent”
```

-	Growing its size until its content fits.

```xml
android:layout_height="wrap_content"
```

- In some layouts, indicating 0dp in the width indicates that it must occupy as much width as is free. The direction of growth (right, left, up, or down) depends on the type of the parent layout.

```xml
android:layout_width="0dp"
```

Moreover, any item that inherits from View can have a background color that is indicated by the `background` attribute. All colors can be indicated in two ways:

- An absolute value in hexadecimal (RGB format):

```xml
android:background="#ff0000"
```

- Referring to a style of the theme we are using:

```xml
android:background="@color/design_default_color_background"
```

---

## TextView

The TextView is the component that allows displaying text on the screen.  When the text exceeds the width of the TextView, if we have vertical space available, the text jumps to the next line. However, if we have not indicated `wrap_content` in the `height` property, the text will be cut off when we run out of space. 

- Text color:
```xml
	android:textColor="#ffffff"
```

- Horizontal text alignment: centered, left, right
```xml
android:gravity="center_horizontal"
android:gravity="left"
android:gravity="right"
```

- Vertical alignment
```xml
	android:gravity="center_vertical"
```

- Assigning horizontal and vertical alignment at the same time
```xml
	android:gravity="center"
```

- Justified text: 
```xml
	android:justificationMode="inter_word"
```
- Font selection:
```xml
	android:fontFamily="sans-serif-black"
```

- Font size:
```xml
	android:textSize="14dp"
```
- Line size:
```xml
	android:lineSpacingMultiplier="1.5"
	android:lineSpacingExtra="10dp"
```

In Android, the concept of “line size” does not exist as such. The line height can be calculated as: `LineSize = android:textSize*android:lineSpacingMultiplier + android:lineSpacingExtra`

- Adding an extra space is most commonly done using only `android:lineSpacingExtra`.

- Adding fonts:
To add a typeface, we have to create an Android resource folder within the `res` folder of our project and select `font` as the type of resource. Then, we can add our fonts by copying them into this folder. 

- Leaving a margin around the text:
```xml
	android:padding="10dp"
```

- Using formatted text:
We can include different styles in a text using class SpannableString and other classes like ForegroundColorSpan or BackgroundColorSpan. These classes affect a range of characters within the SpannableString.
```kotlin
val spannablecontent: SpannableString = SpannableString("Hello SpannableString")
val foregroundColorSpan = ForegroundColorSpan(Color.GREEN)
spannablecontent.setSpan(foregroundColorSpan, 6, 15, 
	Spanned.SPAN_INCLUSIVE_EXCLUSIVE)
binding.editTextMultiline.setText(spannablecontent)
```

In this example, the text would be set to `HelloSpannableString` with the characters forming the word `Spannable` printed in green).

>**Learn more:**
> The set of classes that represent the available styles can be found here: 
> [https://developer.android.com/reference/kotlin/android/text/style/CharacterStyle](https://developer.android.com/reference/kotlin/android/text/style/CharacterStyle)


Finally, we can upload text in HTML format using `Html.fromHtml`. This method converts from HTML to text with Android styles. Care must be taken because not all HTML tags are transformed:
```kotlin
binding.text1.setText(Html.fromHtml("<h2>Title</h2><br><p>Description here</p>",
	Html.FROM_HTML_MODE_COMPACT))
```

---

## ImageView

ImageView is the class that displays an image. This class can display locally available images on our device but does not know how to download an image located on an external server. To download such external images, additional programming is required.

The `scaleType` attribute is one of the most important attributes of this component. It indicates how the image should be scaled within the defined size.

For example:
```xml
	android:scaleType="fitCenter"
```

Snaps the image into the defined space and centers it, while 

```xml
	android:scaleType="centerCrop"
```
Centers the image in the defined space and cuts out what comes out.

> ![fitCenter vs centerCrop](/images/05/image-view.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *fitCenter vs centerCrop*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

A custom scaling matrix can be used to define a scaling that is not supported by the system.

```xml
	android:scaleType="matrix"
```

There are two ways to upload an image in gif format:


1. Using a library like [Glide](https://github.com/bumptech/glide). To do this, we add a dependency to the library in the Gradle of the app.

```gradle
implementation ("com.github.bumptech.glide:glide:4.11.0") {
   exclude group: "com.android.support"
}
```
We can now upload the gif using an ImageView that we have defined in the layout and acts as a placeholder.

```kotlin
Glide.with(this).load(R.drawable.arrow1).into(binding.image);
```

2. API 28 and above include native support for loading many new image formats. This support is achieved using the AnimatedImageDrawable class.

```kotlin
GlobalScope.launch {
  
   val drawable = ImageDecoder.decodeDrawable(source, gfgListner)
   GlobalScope.launch(Dispatchers.Main) {

       binding.image.setImageDrawable(drawable)
       if (drawable is AnimatedImageDrawable) {
           (drawable as AnimatedImageDrawable).start()
       }
   }

}
```

Decoding should be implemented using Kotlin coroutines, because image decoding can be resource intensive and we should not pause the main thread. We will explain how to use coroutines in [Section 7.7](/content/07/07-functional-programming).

>**Learn more:**
> If we are using AnimatedImageDrawable and we cannot see a GIF image in the Android Studio emulator, the problem may be the lack of hardware acceleration. In this case, we should add the following attribute to the manifest file of our application in the activity where we are going to use the AnimatedImageDrawable:
> ```xml
	android:hardwareAccelerated="false"
```

A library that is often used extensively to download images from a remote server is [Picasso](https://square.github.io/picasso/). Using this library, we can download an image from the Internet with a single line of code:

```kotlin
Picasso.with(this).load(“url_image").into(binding.image);
```

It is important to remember that we must declare that our application should have access to the internet in the manifest file.

```xml
<uses-permission android:name="android.permission.INTERNET" />
```
---

## Button

The Button is the component designed to perform an action when pressed.

```xml
<Button
   android:id="@+id/btn"
   android:text="START"
   android:layout_width="150dp"
   android:layout_height="150dp"
/>
```

To know when a user clicks on the button, we assign the value of the code we want to run to the property `setOnClickListen`.

```kotlin
binding.btn.setOnClickListener {
   Toast.makeText(this, "click.", Toast.LENGTH_SHORT).show()
}
```

Any item derived from View can be pressed, for example, an image:

```kotlin
binding.image.setOnClickListener {
   Toast.makeText(this, "image.", Toast.LENGTH_SHORT).show()
}
```

The only difference is that the button performs the visual tapping effect. To use a button that implements the tapping effect but with images instead of text, we must use the ImageButton component. To define the images of the different statuses of the button, we should create a new resource file of type Drawable and Root element (`selector`).

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
   <item android:state_pressed="true"
       android:drawable="@drawable/moon" /> <!-- pressed -->
   <item android:state_focused="true"
       android:drawable="@drawable/sun" /> <!-- focused -->
   <item android:drawable="@drawable/sun" /> <!-- default -->
</selector>
```

Now we assign the selector to our button using the `src` attribute:

```xml
<ImageButton
   android:id="@+id/btn"
   android:src="@drawable/selector_btn"
   android:layout_width="150dp"
   android:layout_height="150dp"/>
```

---

## Input components

There are many components to allow data entry. In the following, we discuss the most frequently used ones: EditText and Switch. 

EditText has a very important attribute called `inputType`. This attribute allows you to perform a series of validations automatically. It also controls the keyboard displayed by Android:

```xml
	android:inputType="number"
```

With this option, the EditText only allows numeric characters and displays the numeric keypad.

```xml
	android:inputType="textPassword"
```

This option hides the characters that the user enters.

```xml
	android:inputType="textEmailAddress"
```

This option shows the character “@” on the keyboard.

```xml
	android:inputType="textMultiLine"
```

This option allows text that occupies more than one line.  If nothing is indicated, the cursor is placed in the middle of the EditText height.

For the text to start at the top of a view, it is necessary to add `android:gravity="top"`.

More than one type can be assigned to achieve effects such as auto-complete, ... For example, if we indicate

```xml
	android:inputType="textMultiLine|textAutoCorrect"
```

the component will allow multi-line inputs and it will correct spelling errors in real time.

It is important to always assign a background to the EditText:

```xml
	android:background="@android:drawable/edit_text"
```

Regarding the Switch component, it can be in two states: true or false. It is often used to let the user select it a feature is required. With the `isChecked` method we can determine the status of the Switch.

```kotlin
binding.switch1.setOnClickListener {
   Toast.makeText(this, binding.switch1.isChecked().toString(), Toast.LENGTH_SHORT)
   	.show()
}
```

---

## Shapes

We can create effects on the backgrounds of any View by creating a resource file of type `drawable`  with root element `shape` and creating the required shape. In the example below, we select a color and indicate that the corners will be rounded.

```xml
shape xmlns:android="http://schemas.android.com/apk/res/android">
   xmlns:android="http://schemas.android.com/apk/res/android"
   android:shape="rectangle">
   <corners android:radius="20dp" />
   <solid
       android:color="#00eeff" />
</shape>
```

Then we assign this drawable to the background of the item we want:

```xml
<View
   android:layout_width="0dp"
   android:layout_height="50dp"
   android:background="@drawable/demo_shape" />
```
