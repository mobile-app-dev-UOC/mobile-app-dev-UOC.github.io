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

The second element (`text2`, in lavender) should be aligned with the upper right corner of our parent.

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

> ![A screenshot of the proposed ConstraintLayout.](/images/05/constraint-layout.png){:style="display:block; margin-left:auto; margin-right:auto"}
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

> ![A ConstraintLayout where an ImageView is below other components.](/images/05/constraint-layout2.png){:style="display:block; margin-left:auto; margin-right:auto"}
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

---

## LinearLayout

In this type of layout, components are arranged in a linear order in a vertical or horizontal direction.

```xml
<LinearLayout
   android:layout_width="match_parent"
   android:layout_height="wrap_content"
   android:orientation="vertical"
   android:gravity="top"
   >


   <EditText
       android:id="@+id/editTextNumber"
       android:layout_width="match_parent"
       android:layout_height="40dp"
       android:layout_weight="1"
       android:ems="10"
       android:background="@android:drawable/edit_text"
       android:inputType="number" />

   <EditText
       android:id="@+id/editTextPersonName"
       android:layout_width="match_parent"
       android:layout_height="40dp"
       android:ems="10"
       android:layout_weight="1"
       android:inputType="textEmailAddress"
       android:background="@android:drawable/edit_text"
       android:text="" />

   <EditText
       android:id="@+id/editTextPassword"
       android:layout_width="match_parent"
       android:layout_height="40dp"
       android:layout_weight="1"
       android:ems="10"
       android:background="@android:drawable/edit_text"
       android:inputType="textPassword" />


   <EditText
       android:id="@+id/editTextMultiline"
       android:layout_width="match_parent"
       android:layout_height="460dp"
       android:layout_weight="1"
       android:ems="10"
       android:gravity="top"
       android:lineSpacingExtra="10dp"
       android:background="@android:drawable/edit_text"
       android:inputType="textMultiLine|textAutoCorrect" />

</LinearLayout>
```

If we specity a `match_parent` height, the available height will be distributed proportionally according to the value of the attribute `android:layout_weight` of each element.

The `android:orientation="vertical"` attribute of LinearLayout indicates that in this case the components have to be stacked vertically. 

---

## RelativeLayout

Elements can be positioned with respect to the parent or another element in the layout. The z-order is still defined by the order in which the elements are defined within the layout.

```xml
<RelativeLayout
   android:layout_width="match_parent"
   android:layout_height="match_parent">

   <ImageView
       android:id="@+id/image"
       android:layout_width="match_parent"
       android:layout_height="match_parent"
       android:src="@drawable/moon"
       android:scaleType="centerCrop"
       android:background="#ff0000"
       />


   <TextView
       android:id="@+id/text_rel1"
       android:layout_width="50dp"
       android:layout_height="50dp"
       android:background="#ffffff"
       android:color="#000000"
       android:padding="6dp"
       android:gravity="center"
       android:layout_marginTop="20dp"
       android:layout_alignParentLeft="true"
       android:layout_marginLeft="20dp"
       android:text="23:45"
       />

       <TextView
           android:id="@+id/text_rel2"
           android:layout_width="50dp"
           android:layout_height="50dp"
           android:background="#ffffff"
           android:color="#000000"
           android:padding="6dp"
           android:gravity="center"
           android:layout_alignParentRight="true"
           android:layout_marginTop="20dp"
           android:layout_marginRight="20dp"
           android:text="LIVE"
           />

       <View
           android:layout_below="@+id/text_rel2"
           android:layout_alignParentRight="true"
           android:layout_marginTop="10dp"
           android:layout_marginRight="20dp"
           android:background="#ff0000"
           android:layout_width="50dp"
           android:layout_height="10dp"/>

   <TextView
       android:id="@+id/text_rel3"
       android:layout_width="match_parent"
       android:layout_height="50dp"
       android:background="#ffffff"
       android:color="#000000"
       android:padding="6dp"
       android:layout_alignParentBottom="true"
       android:layout_marginBottom="100dp"
       android:text="The Moon is Earth's only natural satellite. At about one-quarter the diameter of Earth "
       />


</RelativeLayout>
```

In this example we can see how the ImageView is placed below everything as it is defined first.

The TextView with `id="@+id/text_rel1"` is positioned to the left of the parent by means of
`android:layout_alignParentLeft="true"`. The parent in this case is the RelativeLayout itself. By default, elements are aligned to the beginning of the parent. 

We leave a margin of 20dp with respect to the element relative to which it has been positioned (in this case, the parent).

```xml
	android:layout_marginTop="20dp"
	android:layout_marginLeft="20dp"
```

In the View used an example, which paints a rectangle in red, we can see how to position an component below the component with the identifier `"@+id/text_rel2"` and aligned to the right of the parent. We also leave a margin of 10dp with respect to the component `"@+id/text_rel2"` and a margin of 20dp with respect to the right border.

```xml
	android:layout_below="@+id/text_rel2"
	android:layout_alignParentRight="true"
	android:layout_marginTop="10dp"
	android:layout_marginRight="20dp"
```

The TextView with `@+id/text_rel3` is aligned with the bottom edge of the parent. A margin of 100dp with respect to that bottom edge is defined.

```xml
	android:layout_alignParentBottom="true"
	android:layout_marginBottom="100dp"
```

> ![An example of a RelativeLayout.](/images/05/relative-layout.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *An example of a RelativeLayout.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

---

## FrameLayout

The FrameLayout is typically used to design the interface of a view that has a fixed size.

In a FrameLayout, the position of each element is defined with respect to the top lefet. With this type of positioning, it is very easy to build fixed size interfaces.

In the example below, we will position two squares, one on top of the other.

> ![Target FrameLayout we need to design.](/images/05/frame-layout.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *ATarget FrameLayout we need to design.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

```xml
<FrameLayout
   android:layout_width="match_parent"
   android:layout_height="match_parent">

   <View
       android:layout_below="@+id/text_rel2"
       android:layout_alignParentRight="true"
       android:layout_marginTop="20dp"
       android:layout_marginLeft="20dp"
       android:background="#ff0000"
       android:layout_width="100dp"
       android:layout_height="100dp"/>

   <View
       android:layout_below="@+id/text_rel2"
       android:layout_alignParentRight="true"
       android:layout_marginTop="40dp"
       android:layout_marginLeft="40dp"
       android:background="#00ff00"
       android:layout_width="100dp"
       android:layout_height="100dp"/>

</FrameLayout>
```

---

## TableLayout

The TableLayout organizes the contents into rows and columns. Rows are represented by the `TableRow` tag. Meanwhile, columns are usually the Views, as in our TextView example.

To set the same width for all the columns of the table, we must set the same value for  `android:layout_weight`. Columns with a higher `android:layout_weight` will use more space. 


```xml
<TableLayout
   android:layout_width="match_parent"
   android:layout_height="match_parent">
   <TableRow android:layout_width="match_parent">
       <TextView android:text="C1"  android:layout_weight="1"/>
       <TextView android:text="C2"  android:layout_weight="1"/>
       <TextView android:text="C3"  android:layout_weight="1"/>
   </TableRow>
   <TableRow android:layout_width="match_parent">
       <TextView android:text="v1"  android:layout_weight="1"/>
       <TextView android:text="v2"  android:layout_weight="1"/>
       <TextView android:text="v3"  android:layout_weight="1"/>
   </TableRow>
</TableLayout>
```

We can also assign a fixed value to the width of a column using the `width` attribute.

```xml
<TableLayout
   android:layout_width="match_parent"
   android:layout_height="match_parent">
   <TableRow android:layout_width="match_parent">
       <TextView android:text="C1"  android:width="50dp"/>
       <TextView android:text="C2"  android:layout_weight="1"/>
       <TextView android:text="C3"  android:layout_weight="1"/>
   </TableRow>
   <TableRow android:layout_width="match_parent">
       <TextView android:text="v1"  android:width="50dp"/>
       <TextView android:text="v2"  android:layout_weight="1"/>
       <TextView android:text="v3"  android:layout_weight="1"/>
   </TableRow>
</TableLayout>
```

> ![Examples of TableLayouts](/images/05/table-layout.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Examples of TableLayouts.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)
