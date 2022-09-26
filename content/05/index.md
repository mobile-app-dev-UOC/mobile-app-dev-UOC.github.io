---
layout: default
title: 5. User interfaces
nav_order: 5 
has_children: true
---

# 5. User interfaces

Android builds its user interfaces using **views** and **view groups**. 
- A view is a basic element such as: a text, an image, a button, etc.
- A view group (ViewGroup) is a set of views that are visually organized in a certain way. A ViewGroup can in turn contain groups of views. The organization of the views is hierarchical: all views have a single parent view. That is, all views belong directly to at most a single view group. That’s why we use the term **tree of views** to refer to the set of descendants of a given view.

On top of this basic structure, Google publishes [Material Design](https://material.io/). Material Design is a comprehensive design guide that takes into account both visual appearance and user interaction. Material Design provides a set of design guidelines, components, and patterns studied by the Android team. However, it is important to note that it is not mandatory to use Material Design. A very common situation in the industry is to use some of the components it provides but adapting their graphic appearance to the design selected by our application design team. In this section, we will discuss some of the most usual components.


> ![View and ViewGroup class hierarchy.](/images/05/uml-view-viewgroup.png){:style="display:block; margin-left:auto; margin-right:auto"}
> *View and ViewGroup class hierarchy.*  
> Source: Javier Salvador (Original image) License: [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

Interfaces are typically deployed declaratively using XML files called layouts. These layouts can be used to create view groups or a particular view. In addition, it is also possible to create items dynamically using Kotlin.

{:toc:}