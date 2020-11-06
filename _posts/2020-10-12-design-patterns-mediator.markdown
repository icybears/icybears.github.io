---
layout: post
title:  "The Mediator design pattern"
description: "Design Patterns explained: The Mediator Design Pattern, with example code and diagrams"
categories: 
    - Design Patterns
tags:
    - Object-oriented design
---
The Mediator pattern is a behavioral pattern about encapsulating multiple objects' communication  logic inside one single Mediator object. 

The initial problem is the intercommunication between multiple objects creates coupling between these objects. Well it is usually fine when an object needs to communicate with another object,but what if an object needs to communicate with every other object like in a many-to-many fashion ? Each object's internal implementation will become tighly coupled to every other object.
The idea of the Mediator pattern is, instead of having each object directly communicate with (and hence depend on) multiple objects directly, it could simply communicate with the Mediator object alone.  The Mediator will encapsulate the logic of what to do after receiving a message from an object and who to call or notify next, and this for every object.
So in other words, the mediator object will know about all the objects and what to do when it receives a message from a specific object (as opposed to each object knowing about every other one). And thus the objects will now only communicate with the Mediator alone. This is what allows the individual objects to remain loosely coupled and independent from each other. 

Take the example of a product manager in the context of a small traditional team composed of designers, programmers, system administrators, and testers. The manager hides team members from each other and mediates between them. Here is what this mediated communication can look like:
- The designer finishes their task and notifies the manager
- The manager notifies the programmer
- The programmer finishes implementing the new design and notifies the manager
- The manager notifies the tester
- The tester tests the new product and sends feedback to the manager
- The manager then notifies the designer and the programmer 
- Etc.

See the following UML class diagram:
![Mediator design pattern example](/images/blog/design-patterns-mediator/design_patterns_mediator_example_1.png)

Although there are multiple approaches of implementing such an object, the Mediator pattern does however come with a few things:
- a `Mediator` interface defining how a mediator will communicate with the "mediated" objects (referred to as `Colleagues`) aswell as allowing for the possibility of working with different concrete mediator classes.
-  a `Colleague` interface that only exposes the relevant behavior that could be used by the mediator when communicating with each `Colleague`. Because the `Mediator` doesn't need to know everything about each "mediated" objects, only the methods that we expose beforehand through the `Colleague` interface are relevant to the `Mediator`.

Here is the overall class diagram of the Mediator design pattern:
![Mediator design pattern diagram](/images/blog/design-patterns-mediator/design_patterns_mediator_example_2.png)

