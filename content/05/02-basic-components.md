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

## TextView

The TextView is the component that allows displaying text on the screen.  When the text exceeds the width of the TextView, if we have vertical space available, the text jumps to the next line. However, if we have not indicated `wrap_content` in the `height` property, the text will be cut off when we run out of space. 

Text color:
```xml
	android:textColor="#ffffff"
```

Horizontal text alignment: centered, left, right
```xml
android:gravity="center_horizontal"
android:gravity="left"
android:gravity="right"
```

Vertical alignment
```xml
	android:gravity="center_vertical"
```

Assigning horizontal and vertical alignment at the same time
```xml
	android:gravity="center"
```

Justified text: 
```xml
	android:justificationMode="inter_word"
```
Font selection:
```xml
	android:fontFamily="sans-serif-black"
```

Font size:
```xml
	android:textSize="14dp"
```
Line size:
```xml
	android:lineSpacingMultiplier="1.5"
	android:lineSpacingExtra="10dp"
```

In Android, the concept of “line size” does not exist as such. The line height can be calculated as: `Line Size = android:textSize*android:lineSpacingMultiplier + android:lineSpacingExtra`

Adding an extra space is most commonly done using only `android:lineSpacingExtra`.


Adding fonts:
To add a typeface, we have to create an Android resource folder within the `res` folder of our project and select `font` as the type of resource. Then, we can add our fonts by copying them into this folder. 

Leaving a margin around the text:
```xml
	android:padding="10dp"
```

Using formatted text:
We can include different styles in a text using  SpannableString and classes like ForegroundColorSpan or BackgroundColorSpan classes. These classes affect a range of characters within the SpannableString.
