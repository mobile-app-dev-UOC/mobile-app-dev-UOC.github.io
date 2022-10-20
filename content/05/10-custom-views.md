---
layout: default
title: 5.10. Custom Views
parent: 5. User interfaces
nav_order: 10
---

# 5.10. Custom Views 

To customise a View, we must create a component of type: **New \ UiComponent \ Custom View**.
If we call our view `“MyView”`, a sample layout file will be created with the name `sample_my_view.xml`. This file shows us how to use this view in our layout.

Meanwhile, the view is implemented in the Kotin file `MyView`.

In the MyView class four properties will be created for us:

```kotlin

private var _exampleString: String? = null 
private var _exampleColor: Int = Color.RED 
private var _exampleDimension: Float = 0f 
var exampleDrawable: Drawable? = null

```

These properties can be assigned from the layout that uses this class, as follows:

```xml
<com.uoc.layouts3.MyView
   android:layout_width="200dp"
   android:layout_height="200dp"
   app:layout_constraintBottom_toBottomOf="parent"
   app:layout_constraintLeft_toLeftOf="parent"
   app:layout_constraintRight_toRightOf="parent"
   app:layout_constraintTop_toTopOf="parent"
   app:exampleColor="#00FF00"
   app:exampleDimension="24sp"
   app:exampleDrawable="@android:drawable/ic_menu_add"
   app:exampleString="Hello, MyView" />
```

These properties are loaded into one of the class builders using the parameter `attrs` and the `obtainStyledAttributes` context method:

```kotlin
private fun init(attrs: AttributeSet?, defStyle: Int) {
   // Load attributes
   val  = context.obtainStyledAttributes(
       attrs, R.styleable.MyView, defStyle, 0
   )

   _exampleString = a.getString(
       R.styleable.MyView_exampleString
   )
   _exampleColor = a.getColor(
       R.styleable.MyView_exampleColor,
       exampleColor
   )
}
```
	
The other method to be rewritten is the `onDraw` callback. `onDraw` receives the Canvas, the area that the layout has allocated. Coordinates refer to that area. This method uses canvas primitives that allow painting text, images, rectangles, lines, ...

```kotlin
override fun onDraw(canvas: Canvas) {
   super.onDraw(canvas)
}
```

For example, to paint a rectangle we would use the following code:

```kotlin
val myPaint = Paint()
myPaint.setStyle(Paint.Style.FILL_AND_STROKE)
myPaint.setColor(Color.RED)
canvas.drawRect(0f, 0f, width.toFloat(), height.toFloat(),myPaint)
```

To paint a text, we would use:


```kotlin
textPaint = TextPaint().apply {
   flags = Paint.ANTI_ALIAS_FLAG
   textAlign = Paint.Align.LEFT
}

canvas.drawText(
   it,
   paddingLeft + (contentWidth - textWidth) / 2,
   paddingTop + (contentHeight + textHeight) / 2,
   textPaint
)
```

To paint an image, we would use:

```kotlin
var exampleDrawable: Drawable? = null
exampleDrawable = a.getDrawable(
   R.styleable.MyView_exampleDrawable
)
exampleDrawable?.callback = this


exampleDrawable?.let {
   it.setBounds(
       paddingLeft, paddingTop,
       paddingLeft + contentWidth, paddingTop + contentHeight
   )
   it.draw(canvas)
}
```

