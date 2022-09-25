---
layout: default
title: 4.2. ViewModels
parent: 4. Android app architecture
nav_order: 2
---

# 4.2. ViewModels

ViewModels were born to disconnect the model (the data used by our app) from the view (the specific way in which data is displayed on the screen). In this way, if the operating system or user makes changes that affect the visualization, the interface data is not lost because it is stored in the model.	

> ![The lifecycle of a ViewModel](/images/05/viewmodel-lifecycle.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *The lifecycle of a ViewModel.*  
> Source: [Android Developers](https://developer.android.com/topic/libraries/architecture/viewmodel) License: [CC BY 2.5](http://creativecommons.org/licenses/by/2.5/)

A typical example is having a form where the user has selected an image and is previewing it. When the device rotates, the selected image disappears. Sometimes this can also happen with text-based inputs. This occurs because rotating the device recreates the activity, rather than modifying it.

First, we define a class that inherits from ViewModel. In this class we define the properties we want to preserve:

```kotlin
class AddItemViewModel: ViewModel() {
   private var _add_description = MutableLiveData<String>()
   var add_description: MutableLiveData<String> = _add_description

   private var _add_image = MutableLiveData<Bitmap>()
   var add_image: MutableLiveData<Bitmap> = _add_image
}
```

In our activity, we create the instance of our model:

```kotlin
private val addItemViewModel: AddItemViewModel by viewModels()
```

The `viewModels` annotation indicates that the model will be retained while the fragment is alive. If you do not have fragments, you will do so while the activity is alive.

In the callback `onCreate` of the activity, we indicate that we want to monitor changes to the model to adapt the interface.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
   super.onCreate(savedInstanceState)
   binding = ActivityAddItemBinding.inflate(layoutInflater)
   setContentView(binding.root)


   addItemViewModel.add_image.observe(this, Observer<Bitmap> {
       binding.itemImageNew.setImageBitmap(it)
   })
}
```

This way, we no longer directly assign the selected image to the interface but instead assign it to the model. The interface will be modified automatically by the previous code.

```kotlin
var getResult  = registerForActivityResult(
   ActivityResultContracts.StartActivityForResult()) {
   val value = it.data?.getData()
   val inputStream: InputStream? = this.getContentResolver().openInputStream(value!!)
   val bitmap = BitmapFactory.decodeStream(inputStream)
   addItemViewModel.add_image.value = bitmap
}
```

When the lifecycle associated with the model ends, the system automatically deletes the instance.
