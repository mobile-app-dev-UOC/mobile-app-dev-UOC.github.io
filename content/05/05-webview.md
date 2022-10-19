---
layout: default
title: 5.5. Layouts
parent: 5. User interfaces
nav_order: 5
---

# 5.5. WebView

A WebView is a view that allows HTML pages to be loaded and viewed within an application.

```xml
<WebView
   android:id="@+id/html"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   />
```

To load the UOC main website we do the following:

```kotlin
binding.html.loadUrl("https://www.uoc.edu")
```

If we want to upload a local website to our application we must create an `assets` folder within our project and create the website there. Here we must also include the images, .css or .js files that are needed. In our example, we create the `index.html` web page that we will load like this:

```kotlin
binding.html.loadUrl("file:///android_asset/index.html");
```

If we want to use JavaScript on the web page we are loading, we must explicitly allow JavaScript to run:

```kotlin
binding.html.settings.javaScriptEnabled = true
```

---

## Communication: Kotlin to HTML

We can run methods written in JavaScript and get the result from our Kotlin code.
In the following example, we invoke the JavaScript Message function with the parameter `'Byeeeeeeeee'`. 

```kotlin
binding.html.evaluateJavascript("Message('Byeeeeeeeee')",
   { result ->
       Log.d("Html", result.toString())
   })
```

The JavaScript function returns the integer 10 and updates its content using the parameter it receives:

```html
<script>
   function Message(msg){
      var obj = document.getElementById("div1");
      obj.innerHTML = msg;
      return 10;
   }
</script>
```

---

Communication: HTML to Kotlin

In Kotlin, we create an interface where we will implement the methods that should be invoked from HTML.

```kotlin
class WebAppInterface(private val mContext: Context) {

	@JavascriptInterface
	fun callKotlin(msg: String) {
    	Log.d("Html", msg)
   	}
}
```

Then, we can add this interface to our WebView:

```kotlin
binding.html.addJavascriptInterface(WebAppInterface(this),"AndroidInterface")
```

We can now use it from the HTML page:

```html
<script>
   AndroidInterface.callKotlin("the message");
</script>
```

>**Learn more:**
>If we want to create a hybrid application that has parts in HTML, we should note that adjusting the HTML layout to the size of the mobile device is done within HTML/CSS.

>**Learn more:**
>By default, the WebView does not implement HTML alerts. To do this, it is necessary to assign a WebChromeClient. 
