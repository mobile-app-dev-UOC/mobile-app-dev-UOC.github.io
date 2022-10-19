---
layout: default
title: 5.6. Useful components
parent: 5. User interfaces
nav_order: 6
---

# 5.6. Useful components: Toast, Dialog and CardView

## Toast

A Toast is the typical informational message at the bottom of the app. It is very simple to use. The following code shows the message `"System Ready"` at the bottom of the screen. The third parameter `Toast.LENGTH_SHORT` indicates how long the Toast will be on screen. Constants such as `LENGTH_SHORT` can be used to define this period or we can set a given number of  seconds. It has a limit of approximately four seconds that cannot be exceeded.

```kotlin
Toast.makeText(this, "System Ready", Toast.LENGTH_SHORT).show()
```

> ![A Toast showing a "System Ready" message.](/images/05/toast.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *A Toast showing a "System Ready" message.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

>**Learn more**:
>Custom toast designs have been *unavailable* since Android R.

---

## Dialog

Dialogs perform a similar role to the one they in desktop applications: these are components that appear to focus the user’s attention on a particular information or question.

An example may be displaying a text and three buttons with three options:

```kotlin
val builder = AlertDialog.Builder(this)

with(builder)
{
   setTitle("Android Alert")
   setMessage("We have a message")
   setPositiveButton("OK", DialogInterface.OnClickListener(
       { dialogInterface: DialogInterface, i: Int ->
           Toast.makeText(this@MainActivity, "OK", Toast.LENGTH_SHORT).show()
       }))
   setNegativeButton("NO",  DialogInterface.OnClickListener(
       { dialogInterface: DialogInterface, i: Int ->
           Toast.makeText(this@MainActivity, "NO", Toast.LENGTH_SHORT).show()
       }))
   setNeutralButton("MAYBE",  DialogInterface.OnClickListener(
       { dialogInterface: DialogInterface, i: Int ->
           Toast.makeText(this@MainActivity, "MAYBE", Toast.LENGTH_SHORT).show()
       }))
   show()
}
```

> ![A sample Dialog.](/images/05/dialog.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *A sample Dialog.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


There are many other ways to quickly create a dialog.

First, we can provide styles to the dialog from a style resource file:

```kotlin
val items = arrayOf("One", "Two", "Three", "Four")
val builder = AlertDialog.Builder(this)
with(builder)
{
   setTitle("Items")
   setItems(items) { dialog, which ->
       Toast.makeText(applicationContext, items[which] + " is clicked", Toast.LENGTH_SHORT).show()
   }

   setNegativeButton("CANCEL",null)
   show()
}
```

We can also create a dialog with a multi-select list:

```kotlin
val items = arrayOf("One", "Two", "Three", "Four")
val selectedList = ArrayList<Int>()
val builder = AlertDialog.Builder(this)

builder.setTitle("multiple selection")
builder.setMultiChoiceItems(items, null
) { dialog, which, isChecked ->
   if (isChecked) {
       selectedList.add(which)
   } else if (selectedList.contains(which)) {
       selectedList.remove(Integer.valueOf(which))
   }
}

builder.setPositiveButtonIcon(resources.getDrawable(android.R.drawable.arrow_down_float, theme))

builder.setPositiveButton("OK") { dialogInterface, i ->
   val selectedStrings = ArrayList<String>()

   for (i in selectedList.indices) {
       selectedStrings.add(items[selectedList[i]])
   }

   Toast.makeText(applicationContext, "Selected: " + Arrays.toString(selectedStrings.toTypedArray()), Toast.LENGTH_SHORT).show()
```
In the previous case, we also added an icon to the button using: `setPositiveButtonIcon`.

And finally we can create a fully custom dialog using a layout. In this way, we can create the layout in any way we want:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
   android:orientation="vertical"
   android:layout_width="match_parent"
   android:layout_height="match_parent">

   <EditText
       android:id="@+id/textEdit"
       android:layout_width="match_parent"
       android:layout_height="wrap_content"/>

</LinearLayout>
```

Then, we assign the layout to the new dialog:

```kotlin
val builder = AlertDialog.Builder(this)
val inflater = layoutInflater
builder.setTitle("Dialog and EditText")
val dialogLayout = inflater.inflate(R.layout.custom_dialog, null)
val editText  = dialogLayout.findViewById<EditText>(R.id.textEdit)
builder.setView(dialogLayout)
builder.setPositiveButton("OK") {
       dialogInterface, i -> Toast.makeText(applicationContext, "EditText is " + editText.text.toString(), Toast.LENGTH_SHORT).show() }
builder.show()
```

> ![Example of a custom Dialog.](/images/05/custom-dialog.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Example of a custom Dialog.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

---

## CardView

This component, rather than providing functionality, adds a way to create the distinctive design of these types of elements. The design includes rounded corners and a 3D lifting effect.

In order to use the CardVIew class we need to add the dependency:
```gradle
implementation("androidx.cardview:cardview:1.0.0")
```

We can now deploy cards within our layout.

```xml
<androidx.cardview.widget.CardView
   xmlns:card_view="http://schemas.android.com/apk/res-auto"
   android:id="@+id/card_view"
   android:layout_width="match_parent"
   android:layout_height="wrap_content"
   card_view:cardCornerRadius="10dp"
   card_view:cardElevation="10dp"
   card_view:cardBackgroundColor="#eeeeee"
   app:layout_constraintRight_toLeftOf="parent"
   app:layout_constraintRight_toRightOf="parent"
   app:layout_constraintTop_toTopOf="parent"
   android:layout_marginTop="20dp"
   android:layout_marginLeft="10dp"
   android:layout_marginRight="10dp"
   >

   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="wrap_content">

   <TextView
        android:padding="20dp"
       android:id="@+id/info_text"
       android:text="The Sun is the star at the center of the Solar System. "
       android:textSize="20dp"
       android:textColor="#000000"
       android:layout_width="match_parent"
       android:layout_height="match_parent" />

       <ImageView
           android:layout_width="match_parent"
           android:layout_height="300dp"
           android:src="@drawable/sun"
           android:scaleType="centerCrop"
           android:background="#dddddd"
           />

   </LinearLayout>
</androidx.cardview.widget.CardView>
```

> ![Example of a CardView.](/images/05/card-view.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Example of a CardView.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)
