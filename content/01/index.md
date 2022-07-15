---
layout: default
title: 1. Introduction
nav_order: 1
---

{:toc:}

# 1. Introduction

This course presents the fundamentals of mobile application (app) development. Before focusing on the development process, it is necessary to become familiar with mobile devices. To this end, this section describes the features of a mobile device that are relevant from a development perspective, such as its hardware or its operating system.

## 1.1. Mobile devices

```kotlin
class Element(name: String, value: Int) {

 companion object ElementsFactory {
    var counter:Int = 0
    public fun GetId():Int
    {
        counter++
        return counter
    }
  }
}

var elem2:Element = Element("cat", Element.ElementsFactory.GetId())
```

