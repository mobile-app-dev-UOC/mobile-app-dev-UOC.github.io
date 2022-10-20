---
layout: default
title: 5.11. Animations and Transitions
parent: 5. User interfaces
nav_order: 11
---

# 5.11. Animations and Transitions

There are two types of animations: animations at the view-level or object-level; and propertly-level animations.

---

## View-level or object-level animations

This type of animation is not resource-intensive from the CPU point of view. However, only a few properties of views can be animated in this way. Using this method is feasible only if the views we animate only have a visual purpose and we have no constraints related to their position in other elements of the layout.

If our views require user interaction or we have position constraints related to other views, serious problems may occur. For example, if we animate the position and have a `click` listener, the listener continues to observe the previous position. Another anomalous situation occurs when the position of the view is restricted in relation to other items. When you animate a view with these relative constraints, it will stop fulfilling them. 


Taking into account these limitations, let us take a look at some examples:

First, we create a resource folder of type `anim`.

In that folder we create an “Animation Resource Type” with root `set`. Within the file we create a translation and animation of the `alpha`.

```xml
<set xmlns:android="http://schemas.android.com/apk/res/android"
   android:fillAfter="true">
   <translate
       android:fromXDelta="0.0"
       android:toXDelta="200.0"
       android:duration="2000" />

   <alpha
       android:fromAlpha="0.0"
       android:toAlpha="1.0"
       android:duration="2000"
       />
</set>
```

To invoke it from our Kotlin code we use the following:

```kotlin
val animation1: Animation =
   AnimationUtils.loadAnimation(applicationContext,com.uoc.layouts3.R.anim.animation3)

binding.image1?.startAnimation(animation1);

```

where `animation3` is the name of the resource we have created within `anim`; and `image1` is the view on which we want to apply this animation.

Moreover, we can also create an animation directly in Kotlin. For instance, here we create a set consisting of an `alpha` animation and a translation animation:


```kotlin
val set = AnimationSet(true)
val alpha = AlphaAnimation(0.0f, 1.0f)
 trans val = TranslateAnimation(0f,200f,0f,0f)
set.addAnimation(trans)
set.addAnimation(alpha)
set.duration = 3000
set.fillAfter = true
binding.image1?.startAnimation(set)
```

---

## Property-level animation

This type of. animation does respect the constraints and events assigned to the object, but on the other hand it is less smooth.
 
In the following example, we can see how we animate the marginLeft of an image that has a listener of a click event and that in turn has restrictions with another button type component.

```kotlin
val animator = ValueAnimator.ofInt(10, 200)
animator.addUpdateListener { animation ->

   val cons:ConstraintLayout.LayoutParams = binding.image1?.layoutParams as ConstraintLayout.LayoutParams
   cons.leftMargin = animation.animatedValue as Int
   binding.image1?.layoutParams = cons
}
animator.duration = 1000
animator.start()
```

> ![Property-level animation.](/images/05/property-animation.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Property-level animation.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


If we want to simulate that our view appears from one side of the screen, we simply position it initially off the screen, `android:layout_marginLeft="-200dp"`, and the animation will move it inside.

>**Learn more:**
>You cannot directly execute animations in the `onCreate` callback of an Activity. If we want to do this, we must postpone the execution of the animation with a timer or a deferred call to a handler.


---

## Transitions

We can create custom input and ouput transitions for out fragments. They are assigned in the `onCreate` callback of the fragment.

For example, we can define the transition called `transition1` in the `transition` directory.

```xml
<slide xmlns:android="http://schemas.android.com/apk/res/android"
   android:duration="@android:integer/config_shortAnimTime"
   android:slideEdge="left" />
```

In the `onCreate` callback of the fragment, we define that this will be our input transition:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
   super.onCreate(savedInstanceState)
   
   val inflater = TransitionInflater.from(requireContext())
   val enterTransition1  = inflater.inflateTransition(R.transition.transition1)

   enterTransition = enterTransition1

}
```

---

## Transitions between fragments sharing items

These are one of the most spectacular transformations. The idea is that an element of a view of the first fragment moves from its position and size to the position and size of another element in the second fragment. All this is done while the rest of the elements in the first fragment disappear and the rest of the elements in the second fragment appear.

Let us consider a typical example: tapping on one item of a list allows us to view its details. You can see a video of the result we want to achieve [here](/images/05/transition.mp4).

In the layout of the item in the list we define which element we want to move. Then, we assign a `transitionName` to it.

```xml
<ImageView
   android:id="@+id/item_image"
   android:layout_width="match_parent"
   android:layout_height="300dp"
   android:scaleType="centerCrop"
   android:background="#dddddd"
   android:transitionName="itemImage"
   />
```

In the layout of the “details” fragment, we define the view where the item will end up being, also declaring the same `transitionName`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".ui.detail.DetailFragment">

       <LinearLayout
           android:orientation="vertical"
           android:layout_width="match_parent"
           android:layout_height="match_parent">

           <ImageView
               android:id="@+id/item_image_detail"
               android:layout_width="match_parent"
               android:layout_height="500dp"
               android:scaleType="centerCrop"
               android:background="#dddddd"
               android:transitionName="itemImage"
               />

           <TextView
               android:padding="20dp"
               android:id="@+id/item_text_detail"
               android:text=""
               android:textSize="20dp"
               android:textColor="#000000"
               android:layout_width="match_parent"
               android:layout_height="wrap_content" />

       </LinearLayout>

</FrameLayout>
```

When you tap the list item, the adapter of the view recyclerView will run the callback `onClick`. In our case, the callback has been passed to the method `adapterOnClick` defined in the Fragment containing the recyclerView.

```kotlin
private fun adapterOnClick(item: Item) {
    (activity as MainActivity?)!!.openDetail(item)
}
```

As you can see, the Fragment invokes the method `openDetail` from the MainActivity. Inside `openDetail`, we first create the detail fragment and assign the item to it.

```kotlin
var f3:DetailFragment? = 

nullf3 = DetailFragment.newInstance("", "")
f3.item = item
```

Then, we state the type of input and output transition of the element shared by the new fragment:

```kotlin
f3.setSharedElementEnterTransition(DetailsTransition())
f3.setSharedElementReturnTransition(DetailsTransition())
```

This class `DetailsTransition` is defined as folows:
- `setOrdering(ORDERING_TOGETHER)`: Applies all transformations at once. If we use `ORDERING_SEQUENTIAL` instead, then transitions will be applied one after the other.
- `ChangeBounds:` Allows changing the position, width and height of the view.
- `ChangeTransform:` Allows rotation and scaling transformations if needed.
- `ChangeImageTransform:` Allows for image resizing.

```kotlin
class DetailsTransition : TransitionSet() {
   init {
       setOrdering(ORDERING_TOGETHER)
       addTransition(ChangeBounds())
       addTransition(ChangeTransform())
       addTransition(ChangeImageTransform())
   }
}
```

In the supportFragmentManager we add the `addSharedElement` passing the source view as the first parameter and the `transitionName` as the second parameter.

```kotlin
supportFragmentManager.commit {
    addSharedElement( item.view!! , "itemImage")
   	replace(R.id.placeholder1, f3!!)
   	addToBackStack(null)
}
```

If the input element belongs to a recyclerView item, we have a problem with the output transition: it cannot be performed because the views representing the items have been destroyed and are not available at the time of the output animation. The best solution if the recyclerView is part of a fragment is not to use `remove` or `replace` with the fragment containing the recyclerView: we will simply hide it and add the “details” view. In this way, the views wil be available when the user presses the `Back` button.

In this example, f2 is the fragment containing the recyclerView and f3 is the fragment representing the “details” view.

```kotlin

supportFragmentManager.commit {
   addSharedElement( item.view!! , "itemImage")
   hide(f2!!)
   add(R.id.placeholder1, f3!!)
   addToBackStack(null)
}
```

