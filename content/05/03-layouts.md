---
layout: default
title: 5.3. Layouts
parent: 5. User interfaces
nav_order: 3
---

# 5.3. Layouts

A layout is an XML file that defines a GroupView. The layout defines the components and their location to create the interface of an activity or fragment. Due to the large number of Android devices and the diversity of their screen characteristics, designing layouts that fit any device correctly is a big challenge.

There are several layout types: ConstraintLayout, LinearLayout, RelativeLayout, FrameLayout and TableLayout.

---

## ConstraintLayout

This is the most recent layout and the default one when we create an activity. It gives us the ability to add constraints to each element. These constraints refer to position and size of the element.

**Position constraints:**

There is one important rule: every view must have at least two constraints, a vertical one and an horizontal one.

Let’s look at the first example:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".MainActivity">

   <TextView
       android:id="@+id/text1"
       android:layout_width="150dp"
       android:layout_height="50dp"
       android:background="#ff0000"
       android:text="International"
       app:layout_constraintLeft_toLeftOf="parent"
       app:layout_constraintTop_toTopOf="parent" />

   <TextView
       android:id="@+id/text2"
       android:layout_width="50dp"
       android:layout_height="50dp"
       android:background="#ff00ff"
       android:text="23:15"
       app:layout_constraintRight_toRightOf="parent"
       app:layout_constraintTop_toTopOf="parent" />

   <View
       android:layout_width="0dp"
       android:layout_height="50dp"
       android:background="#0000ff"
       app:layout_constraintLeft_toRightOf="@id/text1"
       app:layout_constraintRight_toLeftOf="@id/text2"
       app:layout_constraintTop_toTopOf="parent" />


</androidx.constraintlayout.widget.ConstraintLayout>
```

In the first element (`text1`, in red), we specify that the upper left corner should be aligned to the upper left corner of the parent view group. In this case, the parent is directly the ConstraintLayout.

```xml
<TextView
       android:id="@+id/text1"
       android:layout_width="200dp"
       android:layout_height="50dp"
       android:background="#ff0000"
       android:text="International"
       app:layout_constraintLeft_toLeftOf="parent"
       app:layout_constraintTop_toTopOf="parent" />
```

The second element ('text2', in lavender) should be aligned with the upper right corner of our parent.

Finally, the third element should fill all the available space. To achieve this, we align the left corner with the right corner of the first element and then align the right corner with the left corner of the second element.

```xml
<View
  android:layout_width="0dp"
  android:layout_height="50dp"
  android:background="#0000ff"
  app:layout_constraintLeft_toRightOf="@id/text1"
  app:layout_constraintRight_toLeftOf="@id/text2"
  app:layout_constraintTop_toTopOf="parent" />
```

> ![A screenshot of the proposed ConstraintLayout.](/images/05/constraint-layout){:style="display:block; margin-left:auto; margin-right:auto"}
> *A screenshot of the proposed ConstraintLayout.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

The depth level, also known as z-order, is defined by the order in which the element is defined in the layout.

In the example below we have two ConstraintLayout children who inherit from a parent who is also a ConstraintLayout. The item with the identifier `back` has been defined earlier in the layout and appears behind it. The item with the identifier `front` has been defined later, and this is why appears above it.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".MainActivity">

<androidx.constraintlayout.widget.ConstraintLayout
   android:id="@+id/back"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   app:layout_constraintLeft_toLeftOf="parent"
   app:layout_constraintTop_toTopOf="parent">


</androidx.constraintlayout.widget.ConstraintLayout>

<androidx.constraintlayout.widget.ConstraintLayout
   android:id="@+id/front"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   app:layout_constraintLeft_toLeftOf="parent"
   app:layout_constraintTop_toTopOf="parent">

   </androidx.constraintlayout.widget.ConstraintLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

> ![A ConstraintLayout where an ImageView is below other components.](/images/05/constraint-layout2){:style="display:block; margin-left:auto; margin-right:auto"}
> *A ConstraintLayout where an ImageView is below other components.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

If we want to center an element we need to indicate constraints in a somewhat contradictory way: if we align the item to the left and to the right, we center it horizontally.

```xml
	app:layout_constraintLeft_toLeftOf="parent"
	app:layout_constraintRight_toRightOf="parent"
	app:layout_constraintTop_toTopOf="parent"
	app:layout_constraintBottom_toBottomOf="parent"
```

>**Learn more:**
>It is possible to make a component become invisible with a position constraint. For example, consider the following constraint: `app:layout_constraintleft_toRightOf="parent"`.
If the parent is the main container and a component places its left border in the rightmost border of the container, the component will be outside the container (out of bounds to the right) and therefore the component will not be visible.

