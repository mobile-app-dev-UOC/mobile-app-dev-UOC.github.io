---
layout: default
title: 5.2. Basic components
parent: 5. User interfaces
nav_order: 2
---

# 5.2. Basic components 

Basic components inherit from class View. In this section, we will review the characteristics of the most common components by emphasizing some typical design-related issues.

All components have the properties `layout_width` and `layout_height`, which indicate width and height respectively. Its value can be defined in four different ways: 

1. Specifying a constant value using any unit:

```xml
android:layout_width=”150dp”
android:layout_height=”50dp”
```
2. Using the same size as the GroupView that contains the component:

```xml
android:layout_width=”match_parent”
android:layout_height=”match_parent”
```

3.	Growing its size until its content fits.
```xml
android:layout_height="wrap_content"
```

4. In some layouts, indicating 0dp in the width indicates that it must occupy as much width as is free. The direction of growth (right, left, up, or down) depends on the type of the parent layout.
```xml
android:layout_width="0dp"
```

Moreover, any item that inherits from View can have a background color that is indicated by the `background` attribute. All colors can be indicated in two ways:

1. An absolute value in hexadecimal (RGB format):

```xml
android:background="#ff0000"
```

2. Referring to a style of the theme we are using:

```xml
android:background="@color/design_default_color_background"
```

## TextView


