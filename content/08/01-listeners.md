---
layout: default
title: 8.1. Listeners
parent: 8. Events
nav_order: 1
---

# 8.1. Listeners 

In an event system we must distinguish two elements:

- The element on which the event originates, which is a view or derived class. 
- The item that receives the information about the, which is called a **listener**.

For example, let us assume that we define a button in our layout:

```xml
<Button
   android:id="@+id/btn1"
   android:layout_width="match_parent"
   android:layout_height="wrap_content"
   android:text="Button 1"
   app:layout_constraintTop_toBottomOf="@id/text1" />
```

And then we define that our MainActivity is the listener for the events of type “click” for that button:

```kotlin
binding.btn1.setOnClickListener{
   Log.i("setOnClickListener", (it as Button).text.toString())
}
```

As we said, we can associate events to any view. Thus, we can define the following TextView:

```xml
<TextView
   android:id="@+id/text1"
   android:layout_width="match_parent"
   android:layout_height="wrap_content"
   android:text="Hello World!"
   app:layout_constraintTop_toBottomOf="@id/edit2" />
```

And in our activity we can also indicate that we are the listener of clicks for that TextView.

```kotlin
binding.text1.setOnClickListener{
   Log.i("setOnClickListener", (it as TextView).text.toString())
}
```

We can associate listeners to the following types of events:
-  **onClick:**  It is called when you tap on a view. 
-  **onLongClick:** It is called when a long press occurs over a view. 
-  **onFocusChange:** It is called when the focus enters or exits an element.
-  **onKey:**  It is called when the focus is in an editable field and the user presses a key on the physical keyboard.
-  **onCreateContextMenu:** It is called when a context menu is created.

For example, we can intercept the **onFocusChange** event of a component of type EditText  with id `edit1` using the following code:

```kotlin
binding.edit1.setOnFocusChangeListener  { view: View, b: Boolean ->
Log.i("setOnClickListener", b.toString()+ " "+(view as EditText).text.toString())
}
```

Parameter `b` is true when we receive the focus and false when the focus exits. 

>**Learn more:**
>Editable components usually check whether the data written by the user is correct when they lose the focus.

We need to be careful with the `setOnKeyListener` listener, as it only runs if a key is pressed on a physical keyboard. This means that this event does not trigger if we press a key on the soft keyboard of any smartphone or tablet.

```kotlin
binding.edit1.setOnKeyListener() { view: View, i: Int, keyEvent: KeyEvent ->
   Log.i("setOnClickListener", keyEvent.keyCode.toString() + " "+(view as EditText).text.toString())
   false;
}
```

If we want to intercept key presses on the soft keyboard, we can do so using the event handlers we will present in [Section 8.2](/content/08/02-event-handlers).

Throughout this section, we have used lambda expressions (see [Section 7.3](/content/07/03-lambda-functions)) to create the code associated with the listener. We can also create a listener without using lambda expressions.  First of all, we creat a class that inherits from the listener we want to implement. In this case, we choose `OnClickListener`:

```kotlin
MyClick classListener: View.OnClickListener{
   override fun onClick(v: View?) {
       Log.i("MyClickListener", v?.id.toString())
   }
}
```

Then, we create the listener and assign it to the view we want to follow:

```kotlin
private var myListener = MyClickListener()
binding.text1.setOnClickListener(myListener)
```

As we can see, the lambda syntax is more concise and elegant for listeners that have a unique and simple behavior. However, using a class can be useful when we want to encapsulate a complex behavior or a behavior that we want to replicate in more than one view.
