---
layout: post
title:  "The Adapter design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

Imagine for example that we are dealing with a third party library that only works with a specific target object and that we would like to work with our own type of object in our existing codebase which happens to be different than what the library expects. 

![Adapter design diagram 1](/images/blog/design-patterns-adapter/design_patterns_adapter_diagram_1.png)

The Adapter pattern allows to introduce a new type of object that is compatible with the library i.e an object adapted to work with the library but contains in its internal implementation our object that is by design incompatible with the library.

![Adapter design diagram 2](/images/blog/design-patterns-adapter/design_patterns_adapter_diagram_2.png)

From the above diagram we can clearly visualize how the adapter is a compatible type (by means of inheritance) that wraps around an incompatible type.

Otherwise, to resolve this "incompatibility" issue we would have to either modify the library code or our own code which not only breaks the Open/Closed Principle but can also be very costly and even impossible.

The adapter object abides by the compatible type's interface by either subclassing a compatible type or by implementing a compatible interface, and is therefore able to communicate with the library. And because it encapsulates the incompatible type, it can also internally communicate with it, do whatever translations or conversions needed, and respond back to the library in a compatible way.

