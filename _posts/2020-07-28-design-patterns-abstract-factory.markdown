---
layout: post
title:  "The Abstract Factory design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

I personally find the "Abstract Factory" naming a confusing one. I will try to highlight the confusion.
 Intuitively, we think of what a factory is, something that is responsible for providing objects of some kind, and then we deduce what that would look like if we simply made it "abstract". Like a factory class made `Abstract` to allow subclasses to extend the type of objects it can create. 

 But this is not what the Abstract Factory design stands for. The standard definition goes like this :
 >Provide an interface **for creating families of related or dependent objects** without specifying their concrete classes

 The `Abstract` in Abstract Factory  is not about how the factory is designed but instead it is about WHAT the factory is designed to produce. This factory's task is to produce objects based on an "abstract concept" meaning that this factory is free to produce a wide range of objects so long as they fall under that "category" or "general idea". It maintains an "abstract" or high-level criteria for what kind of objects it produces.

 For example, a Hardware Factory can produce all kinds of hardware-related objects, e.g monitors,keyboards and mouses. Any more specific than that is left for the concrete class to define. Maybe Gaming Hardware ? or Low-end Hardware ? And of course, the client knows nothing about these details.  The client only knows about what the Abstract Factory defined for it.

 The following diagram shows the Abstract Factory design for the above mentioned example:
![Abstract Factory Diagram 1](/images/blog/design-patterns-abstract-factory/design_patterns_abstract_factory_diagram_1.png)
