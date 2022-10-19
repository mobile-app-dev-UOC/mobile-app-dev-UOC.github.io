---
layout: default
title: 5.4. ScrollViews
parent: 5. User interfaces
nav_order: 4
---

# 5.4. ScrollView and HorizontalScrollView

ScrollView allows us to scroll vertically on any item that is included inside the view. In the example below, we place a 1024dpx1024dp image inside a ScrollView that allows you to scroll vertically.

```xml
<ScrollView
   android:layout_width="match_parent"
   android:layout_height="match_parent">
   <ImageView
       android:id="@+id/image"
       android:layout_width="1024dp"
       android:layout_height="1024dp"
       android:src="@drawable/moon"
       android:scaleType="centerCrop"
       android:background="#ff0000"
       />
</ScrollView>
```

Similarly, if we want to scroll horizontally, we must use a HorizontalScrollView. If we need both types of scrolls, we can nest a ScrollView and a HorizontalScrollView.

```xml
<ScrollView
   android:layout_width="match_parent"
   android:layout_height="match_parent">
   <HorizontalScrollView
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <ImageView
           android:id="@+id/image"
           android:layout_width="1024dp"
           android:layout_height="1024dp"
           android:src="@drawable/moon"
           android:scaleType="centerCrop"
           android:background="#ff0000"
           />
   </HorizontalScrollView>
</ScrollView>
```

There is one important issue to keep in mind: we should not combine ScrollView with items that already have vertical scrolls such as the RecyclerView or ListView.
