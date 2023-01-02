---
layout: default
title: 9.3. JSON and XML
parent: 9. Local data persistence
nav_order: 3
---

# 9.3. Data storage and exchange formats: JSON and XML

We have already seen that our app operates with objects. However, we may need these items to be stored in permanent storage. Moreover, we may also need them to be sent using a process-to-process intercom mechanism. Thus, we may need to convert these objects into text strings that can be saved or sent.

To do this, we can use two alternative textual formats: JSON or XML.

## 9.3.1. JSON (JavaScript Object Notation)

JSON is highly tied to JavaScript. JavaScript provides methods that allow conversion of JavaScript objects to JSON strings (serialization) and conversion of JSON strings to JavaScript objects (deserialization). Its simplicity allowed it to spread quickly. 

In Kotlin, the process is just as simple if we add the following to our Gradle file:

```gradle
plugins {
     id 'org.jetbrains.kotlin.plugin.serialization' version "1.6.10" apply false

}

dependencies {
    implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.3.2")
}
```


Let us look at an example of how to do this. To start, we create the Kotlin class we want to serialize and deserialize in JSON. We import serialization and above the class we add the annotation `@Serializable`.

```kotlin
import kotlinx.serialization.Serializable
import kotlinx.serialization.json.Json
import kotlinx.serialization.encodeToString


@Serializable
data class Item(val id: Long,
                val name: String,
                val description: String,
                val phones:List<String>)

```

Now we can convert our instances of the `Item` class to JSON. To do it, we import the following packages:

```kotlin
import kotlinx.serialization.json.Json
import kotlinx.serialization.encodeToString
import kotlinx.serialization.decodeFromString
```

We can now create the instance and convert it into a string:

```kotlin
var it:Item = Item(1234,"Adam","Scottish economist and philosopher who was a pioneer of political economy",listOf("999999999","55555545","4343242342"))
val json_str:String = Json.encodeToString(it)
```

The result is this:

```json
{"id":1234,"name":"Adam","description":"Scottish economist and philosopher who was a pioneer of political economy","phones":["999999999","55555545","4343242342"]}
```

To convert the string back to a Kotlin object, simply type the following:

```kotlin
var it2:Item = Json.decodeFromString<Item>(json_str)
```

---

## 9.3.2. XML (Extensible Markup Language)

XML is a mark-up language that is very similar to HTML but where the user decides the set of tags are and their attributes. The user may also decide the hierarchical structure of the tags: for a particular node, which is the set of children that is supported. All these decisions are described in a **Document Type Definition (DTD)**.


The Android development process is full of XML files used by the environment itself: the `manifest.xml` file, the layout files, the files found in the `res/values` folder, ... In addition to these files, we may create or use XML files specific to our application. These files can be read from a web server or as a file within the `res/raw` folder.

```xml
<?xml version="1.0" encoding="utf-8"?>
<doc>
   <item id='1' type='draft'>
       <link>https://www.uoc.edu/portal/en/index1.html</link>
       <description><![CDATA[description1]]></description>
   </item>
   <item id='2' type='draft'>
       <link>https://www.uoc.edu/portal/en/index2.html</link>
       <description><![CDATA[description2]]></description>
   </item>
   <item id='3' type='publish'>
       <link>https://www.uoc.edu/portal/en/index3.html</link>
       <description><![CDATA[description3]]></description>
   </item>
   <item id='4' type='draft'>
       <link>https://www.uoc.edu/portal/en/index4.html</link>
       <description><![CDATA[description4]]></description>
   </item>
</doc>
```

In Android there are three ways to analyze XML files:
- Using `XMLPullParser` or SAX analyzers. Both are event-driven analyzers. They are fast and use a low amount of memory, so it is the right way when we want to analyze very large XML files. 
- The `XMLPullParser` is more modern and user-friendly than the SAX analyzer. 
- The third method involves using an analyzer that loads the entire DOM (Document Object Model) into memory and allows us to traverse XML file in any way we want. This DOM is a tree that represents the entire XML file in memory.

First, we will see how the `XMLPullParser` parser would work with the previous XML.
To this end, we load the file found in `res/raw`  using:

```kotlin
istream value: InputStream = this.getResources().openRawResource(R.raw.document)
```
We create the parser and assign the `istream` to the parser:

```kotlin
val parserFactory = XmlPullParserFactory.newInstance()
val parser = parserFactory.newPullParser()
parser.setFeature(XmlPullParser.FEATURE_PROCESS_NAMESPACES, false)
parser.setInput(istream, null)
```

We declare some variables that we will use during the analysis:

```kotlin
var tag: String? = ""
var text: String? = ""
var link_str : String? = ""
var id_str : String? = ""
var description_str : String? = ""
var event = parser.eventType
```

We will go through the entire document until the `XmlPullParser.END_DOCUMENT` event is received:

```kotlin
while (event != XmlPullParser.END_DOCUMENT) {
   tag = parser.name
   when (event) {
       XmlPullParser.START_TAG -> if (tag == "item") {
           id_str = parser.getAttributeValue(null, "id")
       }
       XmlPullParser.TEXT -> text = parser.text
       XmlPullParser.END_TAG -> when (tag) {
           "item" -> Log.d("msg",link_str + " " + description_str + " "+ id_str)
           "link" -> link_str = text
           "description" -> description_str = text
       }
   }
   event = parser.next()
}
```


The core idea behind the loop can be summarized as follows:

1. When we receive an `XmlPullParser.TEXT` event we store the text in an auxiliary variable (`text`).

2. With the `XmlPullParser.START_TAG` event, we request the name of the tag, `tag = parser.name`. If the tag is relevant to our analysis, we save the value of its attributes  temporary variables. 

3. When the `XmlPullParser.END_TAG` event is received, we can now save our object to a data structure in our application because we have all the necessary data in the temporary variables. 

If we are now performing the same analysis using the DOM analyzer, we will need to perform the following steps:

Again, we load the file found in `res/raw` using:

```kotlin
istream value: InputStream =this.getResources().openRawResource(R.raw.document)
```

We create the parser and assigned the `istream` to the parser:

```kotlin
val builderFactory: DocumentBuilderFactory = DocumentBuilderFactory.newInstance()
wow docBuilder: DocumentBuilder = builderFactory.newDocumentBuilder()
wow doc: Document = docBuilder.parse(istream)
```

DOM-type parsers load the entire document into memory and we can query them to access the nodes we want. For example, to list the nodes of type `item` we would use the following call:

```kotlin
val nList: NodeList = doc.getElementsByTagName("item")
```

The power of this type of analyzer is that it is able to perform complex queries using a query language called XPath.
We can define an XPath query that returns only those nodes of type `item` where the `type` attribute has the value `draft`:

```kotlin
val xpFactory = XPathFactory.newInstance()
var xPath = xpFactory.newXPath()
var xpath = "//doc/item[@type='draft']"
val nList2: NodeList = xPath.evaluate(xpath, doc, XPathConstants.NODESET) as NodeList
```

In `nList2` we will have the list of nodes that meet the condition. We can now navigate through these nodes:

```kotlin

for (i in 0..nList2.length - 1) {

   val node = nList2.item(i)

   val attributeValue = node.attributes.getNamedItem("id")
   val value = attributeValue.nodeValue

   xPath = xpFactory.newXPath()
   xpath = "./link"
   val child1 = xPath.evaluate(xpath, node, XPathConstants.NODE) as Node
   val child1_value = child1.firstChild.nodeValue


   xPath = xpFactory.newXPath()
   xpath = "./description"
   val child2 = xPath.evaluate(xpath, node, XPathConstants.NODE) as Node
   val child2_value = child2.firstChild.nodeValue

}
```

To access the attributes of a node we use:

```kotlin
val attributeValue = node.attributes.getNamedItem("id")
val value = attributeValue.nodeValue
```

For each node we declare two more XPaths to access the internal `link` and `description` nodes. The text is in the `firstChild.nodeValue` of these nodes.

> **Learn more:**
> It is not correct to access nodes using the node position because this would cause the analysis to fail if new nodes are added in between. Another reason not to use the node position is because the spaces and line breaks between XML nodes on Android count as text nodes and this could make us use a wrong index for the node we want to access.








