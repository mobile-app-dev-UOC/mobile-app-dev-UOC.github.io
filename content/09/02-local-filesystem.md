---
layout: default
title: 9.2. Local filesytem
parent: 9. Local data persistence
nav_order: 2
---

# 9.2. Local filesystem

The file system allows us to save large files of any kind. We need to distinguish whether we want to store those files on the internal storage system or the external storage system (memory cards). This is typically added to our application settings.

If we are only going to access these files from our application, we do not need any special permissions, whether we are going to store them in internal or external storage.  Deleting the application deletes all contents of the two storage types. If we use the Device File Explorer and continue to see the files, we must use the `Synchronize` context menu option to see that they have actually been deleted.

## 9.2.1. Basic operations

To implement basic file operations, we use the class `File`, `filesDir` or `getExternalFilesDir`. `filesDir` provides us with the root path to the file area in the internal memory of our application. Finally, `getExternalFilesDir` provides us with the root path to the file area of our application in the external memory.

```kotlin
val directory = filesDir

val fileName = "data1.txt"
var file = File(directory, fileName)

if(!file.exists()) {
   val isNewFileCreated: Boolean = file.createNewFile()
}

file.writeText("this is the text")
```

By default, method `writeText()` uses UTF-8 encoding. 

We can see that the method `fileDir` returns the value:
`/data/user/0/com.uoc.localstorage/files`


> ![Root of the internal memory filesystem.](/images/09-filesystem.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Root of the internal memory filesystem.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

If we want to write to external storage, we would do the same but using `getExternalFilesDir` instead of `filesDir`. In that case, in the emulator, we will see that it returns a path similar to this one:
`/storage/emulated/0/Android/data/com.uoc.localstorage/files`.

> ![Root of the external memory filesystem.](/images/09-external-filesystem.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Root of the external memory filesystem.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We can use the `File` class to write binary files using the method `writeBytes`. We can also associate a `FileOutputStream`.

```kotlin
var phos: FileOutputStream = FileOutputStream(file)
var text:String = "this is the text"
fos.write(text.toByteArray(Charsets.UTF_8))
fos.close()
```

If instead of `filesDir` or `getExternalFilesDir` we use `cacheDir` or `externalCacheDir`, we will be creating the files in a temporary file area. On internal storage, the cache is created in this directory:
`/data/user/0/com.uoc.localstorage/cache`

Temporary files can be destroyed by the user from the system application management area. This option can be found in the followiing path: `Settings/Apps/<our app>/Storage/Clear Cache`

>**Learn more
> The `File` class has methods that allow us to perform all the operations we need with files, directories, path comparison, ... You can find more information about this class here:
[https://developer.android.com/reference/java/io/File](https://developer.android.com/reference/java/io/File).



## 9.2.2. Encrypting text files

