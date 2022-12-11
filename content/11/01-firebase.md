---
layout: default
title: 11.1. Firebase
parent: 11. Back-end
nav_order: 1
---

# 11.1. External back-end: Firebase 


Firebase is the backend provided by Google for easy mobile app development. 

Firebase provides a wide variety of back-end features. The goal of Firebase is to cover all the necessary aspects so that the application developer does not need to create a custom proprietary back-end. However, this goal is not achieved in full, because among other things it does not offer an SQL database. In any case, many applications do not require an SQL database and can operate using one of three storage models provided by Firebase. These three storage models are:

- **Firestore Database**: Flexible and scalable NoSQL database. It is Firebase’s most recent offering and has recently added real-time capabilities.
- **Realtime Database**: Saves and syncs data in real time. It is the original real-time database for Firebase.
- **Storage**: Can store and send large content such as photos or videos.

In this course materials, we will work with the Firestore Database.

From a browser, we access the url [https://firebase.google.com/](https://firebase.google.com/), click on `Start` and create a new project.

> ![Create a new project in Firebase.](/images/11/add-project.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Create a new project in Firebase.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We are going to add two things to this project:
1.	The back-end functionalities we will need.
2.	The applications that will use it.


Once we click `Add`, we will open an assistant. In the first step, the assistant will request
two pieces of information:
1.	 The name of the project.
2.	 Whether we are going to use Google Analytics or not. This step is very important because Google Analytics allows us to gather  metrics that can help us improve the functionality of our app. We may also run small snippets of code when Analytics detects that a condition is met. We can use the default Analytics account suggested by Firebase.

After a few moments, a page opens where we can now add the functionalities and applications:

> ![Add features and apps menu.](/images/11/add-app.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Add features and apps menu.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


## Adding a Firestore database

We will start by adding the functionalities. In this case we are going to add a Cloud Firestore database. We click the left menu for this type of database and click the `"Create database"` button. 

The first step asks us whether we want to put the database into production or if it will be a
test database. This basically affects safety rules. We will start by creating the database in
test mode and we can change this decision later. This will create a single security rule that
will only control the amount of time that the database has been in test mode. If this counter
exceeds one month, we will no longer be able to connect to the database unless we modify
the security rule again.

> ![Select a production or test database.](/images/11/production-test.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Select whether the database is a test or a production system.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

In the next step, we will select the location of the data center where our back-end will store our data.

> ![Select a datacenter.](/images/11/datacenter.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Select a datacenter.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


After this step, the area where we can actually start to set up our database already appears:

> ![Creation of a Firestore database.](/images/11/firestore.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creation of a Firestore database.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


 In Firestore, data is organized into **collections** and within the collections we have **documents**. It is important to note that, unlike relational databases, document instances may have a different number and name of attributes. Both collections and documents have a **unique identifier** and a **path**. Collections and documents are arranged in the form of a tree so the path of each document is unique.

 Let us create our first collection, which we will call "news".

> ![Creating our first collection: news.](/images/11/news.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Creating our first collection: news.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Below we select the properties of the the elements of our collection:

> ![Defining the properties of our collection.](/images/11/properties.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Defining the properties of our collection.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

One of the available datatypes for properties is `byte`, which can store up to 1MB. Nevertheless, as with most databases using this datatype is not a best practice. Instead, what is usually done is to save an identifier that allows us to locate a file later that contains the information in Google Storage.

We can also add other collections as properties of an item. This will be very important later when implementing relationships between entities.

In this step we can create the fields we need and even fill in their content. As identifier, we click on `Auto-ID`:

> ![Assigning values to the properties of our collection.](/images/11/properties2.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Assigning values to the properties of our collection.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Once this is done, we can save the collection and inspect its first element:

> ![Browsing a collection.](/images/11/browse-collection.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Browsing a collection.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

We can now add a one-to-many (1:N) relationship called "related_news" that lists the news related to a given news story. For example, we created a news story titled "Saturn" with id `KFwbJ5VhRpHzHqY4hCX` and in the "related_news" collection of the news titled "Sun" we added the previous id as a reference in "related_news".

> ![Defining a 1:N relationship.](/images/11/one-to-many.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Defining a 1:N relationship.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

To map many-to-many relationships (M:N) such as students enroling in courses in a university:

> ![Students and courses: a M:N relationship.](/images/11/students-courses.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *Students and courses: a M:N relationship.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)


What we can do is create a collection with documents that have two attributes. The first attribute is of type string and contains the path of the first element and the second attribute is the path of the second element of the relationship.


As with traditional databases, indexes increase the performance of queries. By default, Firestore creates individual indexes for each attribute or field that is created in each document. To create indexes that contain multiple fields, you must use the compound index creation option.

## Adding our app

After creating the database, now we can set up our app in Firebase. To do this, we that the `“Add app”` button and select the button with the Android icon.

> ![Adding our app in Firebase.](/images/11/new-app.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Adding our app in Firebase.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

In the following dialog we introducethe unique Android app identifier and optionally an alias that the Firebase environment will use to refer to our app.

> ![Setting up the app id.](/images/11/app-id.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Setting up the app id.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, we need to download a JSON file called `google-services.json` and copy it into the app folder.

> ![Downloading the Google services JSON file.](/images/11/json.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
> *Downloading the Google services JSON file.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Then, we add the proper dependencies to the Gradle file:

```gradle
plugins {
   id 'com.android.application'
   id 'kotlin-android'
   id 'com.google.gms.google-services'
}

dependencies {
   implementation platform('com.google.firebase:firebase-bom:29.0.3')
   implementation 'com.google.firebase:firebase-analytics-ktx'
}
```

## Using our database

Now we can perform basic operations with our database from our Kotlin code. For instance, to add an item to a collection we would do the following:

```kotlin
val db = FirebaseFirestore.getInstance()
val news = db.collection("news")

val hashMap = hashMapOf<String, Any>(
   "title" to "The cat is fine",
   "text" to "Nairobi"
)

db.collection("news")
   .add(hashMap)
   .addOnSuccessListener {
       Log.d("Firestore", "Added story with ID ${it.id}")
   }
   .addOnFailureListener { exception ->
       Log.w("Firestore", "Error adding story $exception")
   }
```

As we can see, the procedure is always asynchronous. In fact, all operations on Firebase are asynchronous. We have two listeners that will run if the operation has been successful or incorrect.

To query the items in a collection, we would do the following:

```kotlin
val db = FirebaseFirestore.getInstance()
db.collection(“news”)
   .get()
   .addOnSuccessListener { querySnapshot ->
       querySnapshot.forEach { document ->
           Log.d(“Firestore”, “Read document with ID ${document.id}”)
       }
   }
   .addOnFailureListener { exception ->
       Log.w(“Firestore”, “Error getting documents $exception”)
   }
```

In this example we return all the elements of the "news" collection. 

To read a specific document using your document path we would do the following:

```kotlin
db.document("/news/BGJHkVMOJp4H6fgjouMk")
  .get()
   .addOnSuccessListener { document ->
           Log.d("Firestore", "Read document with ID ${document.id} ${document.data?.get("title")}")
   }
   .addOnFailureListener { exception ->
       Log.w("Firestore", "Error getting documents $exception")
   }
```

We access the `title` property using the following syntax: `document.data?.get("title")`.

For a more complex example, let us consider how we would access items in a collection related to a given item. For example, to access related news, the “related_news” collection of the news with path `“/news/BGJHkVMOJp4H6fgjouMk”` would need to do the following:

```kotlin
db.document(“/news/BGJHkVMOJp4H6fgjouMk”)
   .get()
   .addOnSuccessListener { document ->
       document.reference.collection(“related_news”).get()
           .addOnSuccessListener { querySnapshot ->

               querySnapshot.forEach { documentr ->

                   Log.d(“Firestore”, “Read document with ID ${documentr.id} ${documentr.data?.get(“doc_id”)}”)

                   val document_reference:DocumentReference = documentr.data?.get(“doc_id”) as DocumentReference
                   val path: String = document_reference.path
                   db.document(path).get().addOnSuccessListener { documentr2 ->
                       Log.d(“Firestore”, “Read document with ID ${documentr2.id} ${documentr2.data?.get(“title”)}”)

                   }


               }

           }

   }
   .addOnFailureListener { exception ->
       Log.w(“Firestore”, “Error getting documents $exception”)
   }
```

To modify the value of attributes in a document we would use the following snippet:

```kotlin

val db = FirebaseFirestore.getInstance()
db.document("/news/Vmq4WlxsIooR0XudkCgu").update("title","updated")
```

The `update` method can have N parameters. These parameters should always consist of an “attribute name” followed by the new value of that attribute.

```kotlin
db.document("/news/Vmq4WlxsIooR0XudkCgu").update("title","updated1","text","updated2")
```

We can delete a document by invoking its `delete` method:

```kotlin
db.document("/news/Vmq4WlxsIooR0XudkCgu").delete()
```

To consult the M:N Student vs Subject relationship, we would use the following snippet:

```kotlin
db.collection("takes").whereEqualTo("takes_student","/student/1")
  .get()
  .addOnSuccessListener { querySnapshot ->
      querySnapshot.forEach { document ->
          Log.d("Firestore", "Read document with ID ${document.data}")
      }
  }
  .addOnFailureListener { exception ->
      Log.w("Firestore", "Error getting documents $exception")
  }
```

In this query, we are requesting the paths of the courses taken by the student with identifier equal to one.

> **Learn more:**
> [Queries in Firebase Cloudstore](https://firebase.google.com/docs/firestore/query-data/queries)

> **Learn more:**
> When we use a third-party backend in your app, it is extremely important to carefully study its limits and costs. You should be aware of the limits of the free versions and how scalability impacts cost. If you are not careful, the back-end costs for your app can be prohibitive.

