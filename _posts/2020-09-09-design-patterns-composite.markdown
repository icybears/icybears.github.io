---
layout: post
title:  "The Composite design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

To know and understand what the Composite is about let's first take a look at the following diagram:
![Example of polymorphism](/images/blog/design-patterns-composite/design_patterns_composite_diagram_1.png)
The above diagram simply illustrates polymorphic behavior using an interface. The client doesn't know about `SpecificTypeA` or `SpecificTypeB` and works with the `Type` interface indiscriminately of what specific type it is dealing with.

Now due to some requirements, We also want `SpecificTypeA` to be a collection of objects of type `SpecificTypeA` or `SpecificTypeB` or both, indiscriminately. Well, since they both are concrete implementations of the `Type` interface, then all we have  to do is have `SpecificTypeA` encapsulate a collection of type `Type`. (I know this is pretty abstract, we will see a concrete example soon) 

This is what it looks like in a UML class diagram:
![Composite design example 1](/images/blog/design-patterns-composite/design_patterns_composite_diagram_2.png)
This is the Composite pattern, and `SpecificTypeA` is a Composite type.

To fully understand the Composite pattern, let's turn this abstract example into a concrete one.
Imagine `SpecificTypeA` is a `Folder`, and `SpecificTypeB` is a `File`, and the `Type` interface is an abstraction that allows to work with both files and folders in an abstract manner, we will call it `FileSystemItem`. A folder is a composite type that can be composed of files and other folders. This is what it looks like:
![Composite design example 2](/images/blog/design-patterns-composite/design_patterns_composite_diagram_3.png)

 Note that the composite type has methods to manage the collection of objects that it encapsulates.

And finally, here is the general UML diagram of the Composite pattern:

![Composite design diagram](/images/blog/design-patterns-composite/design_patterns_composite_diagram_4.png)
