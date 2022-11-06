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

As we said, we can associate events to any view. Thus, we can define the following TextView

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
