---
layout: default
title: 9.1. User preferences
parent: 9. Local data persistence
nav_order: 1
---

# 9.1. User preferences 

Preferences are an easy way to save data on Android. This is data saved as a (key,value). Pair. They are similar to those stored in the cookie system used for the world wide web.

This approach should only be used to store specific items that are not intensively queried or updated during the execution of our application. The reason is that preferences use expensive access protocols at the CPU level, and if we use them intensively our app may run. 

As the name implies, preferences are the ideal place to save application settings and preferences. We can load them into memory once (at startup) and then access them from a ViewModel or Singleton. 

The data types that can be saved are: boolean, int, long, float, string and stringSet. We can even convert any object to JSON and store it here.

To manage user preferences, we use the class `SharedPreferences` and the `SharedPreferences.Editor`. To create or modify the values of preferences, we write the following code:

```kotlin
var user_id = "user@user.com"
var pwd = "Fi12_4of36"
val sharedPref:SharedPreferences  = getSharedPreferences("application", Context.MODE_PRIVATE)
val editor: SharedPreferences.Editor = sharedPref.edit()
editor.putString("user_id", user_id)
editor.putString("pwd", pwd)
editor.apply()
```

When we create the `sharedPref` instance we specity the **preference package** we want. An application can have multiple preference packages. In the case of our example we call it `application` and we open it in private mode (`MODE_PRIVATE`). This `MODE_PRIVATE` indicates that this preference package can only be used by the application that created it. In the early versions of Android, there were ways to share preferences among other apps but they have become obsolete due to security issues.

To access the value of a key in the preferences, you must type:

```kotlin
val sharedPref:SharedPreferences = getSharedPreferences("application", Context.MODE_PRIVATE)
val user = sharedPref.getString("user_id",null)
val pwd = sharedPref.getString("pwd",null)
```
The second parameter of `getString` is the value that should be returned if the key is not defined. In this case, we use `null`.

There are other method such as:

- Test if a given key has been defined:

```kotlin
val contains:Boolean = sharedPref.contains("user_id")
```

- Delete a key:

```kotlin
editor.remove("user_id")
editor.apply()
```

- Clear all keys:

```kotlin
editor.clear()
editor.apply()
```

Instead of `apply()` we can use `commit()` to confirm the changes: it returns true if the changes could be made and false if it could not. `commit()` is a synchronous method and therefore it is slower than `apply()`. 

Values stored as user preferences are not encrypted. Thus, you should take care when storing sensitive information such as passwords. These values are stored as unencrypted file in XML format in the following path:
`/data/data/YOUR_PACKAGE_NAME/shared_prefs/PREFERENCES_NAME.xml`
`PREFERENCES_NAME` is the name of the preference package. In this case, the `PREFERENCES_NAME` we have chosen is  `application`. 

> ![Location of user preferences.](/images/09/preferences.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Location of user preferences.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

```xml
<map>
  <string name="user_id">user@user.com</string>
  <string name="pwd">Fi12_4de36</string>
</map>
```

We can easily encrypt this file using:

```kotlin
val  masterKey:MasterKey = MasterKey.Builder(this, MasterKey.DEFAULT_MASTER_KEY_ALIAS)
   .setKeyScheme(MasterKey.KeyScheme.AES256_GCM)
   .build();

val sharedPreferences: SharedPreferences = EncryptedSharedPreferences.create(this,
   "secret_shared_prefs",
   masterKey,
   EncryptedSharedPreferences.PrefKeyEncryptionScheme.AES256_SIV,
   EncryptedSharedPreferences.PrefValueEncryptionScheme.AES256_GCM
)

val editor = sharedPreferences.edit()
```

After encrypting the preferences, we can use the editor as usual. We can also read preferences as usual. The contents of the XML file would look like the following:

```xml
<map>
  <string name="__androidx_security_crypto_encrypted_prefs_key_keyset__">12a9018c4ee16c404ac688f449da5ec8fd947afec933725aa34d976084d7654cc4b383ad6ffe6aae9c59fce0617d3753784dd106b1f43739203a9c7f295207c30b542aafc913eccea82658300ec6b978221a8c0eb3f6de178fc439509a678483a0fbafba129d3fff8302db414b2ebb82d5191c83ab68917639516a0a26214d71c7dc2ff235d08fa09c47beb0f04a01d12336e62bab987cceeb2d2d32f7de4015573eb83bc3a39e850175f4861a4408ced9af8107123c0a30747970652e676f6f676c65617069732e636f6d2f676f6f676c652e63727970746f2e74696e6b2e4165735369764b6579100118ced9af81072001</string>
  <string name="AXAr7M6CvDmbNrhbyugYy3lgSg2jjLFkRfzJLA==">AVeJ+YTn/Tr6PKpURgZIpI7bUHp9Vh+kwJhW96b3Pw8henzyPctXuSfyYpy5vS+XQfJrO3kk</string>
  <string name="__androidx_security_crypto_encrypted_prefs_value_keyset__">1288011b9493199aadccefb1a25fa54c132c0f26cae42c3b20fb94a0091a45d29d4194e2d711b1f6b282031907ed3d63d4bb9338ae89d28db94870403d144f54fdee04bd5ad09fd6c7448b2dfa3e59d8e1942d99879f09ce3fe6a4adc952d249b98f326fb5cec7fd2b8f3ce957fdff973128308136621bbde64976032ff8ffa2a62a2baf767f8226e6a0381a440884f3a7bc05123c0a30747970652e676f6f676c65617069732e636f6d2f676f6f676c652e63727970746f2e74696e6b2e41657347636d4b657910011884f3a7bc052001</string>
  <string name="AXAr7M5AswgnuqSt2o/2x167f0K71oji">AVeJ+YRQpgbZPMg0pM27fRF6o9Pvopapcoo8+b42F+0C4FpzlgCzRiDBNcNphR8I4Xw6</string>
</map>

```



