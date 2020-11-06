---
layout: post
title:  "The Facade design pattern"
description: "Design Patterns explained: The Facade Design Pattern, with example code and diagrams"
categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

Sometimes in order to accomplish a certain task or functionality we require a set of calls to low-level methods or perhaps some API calls, or in a sense complex logic that is irrelevant to what we are trying to expose to the client i.e the task or the functionality itself, and therefore it makes sense to encapsulate all that complex logic inside a class and only expose an easy to use "interface" to clients. This will also keep client code from getting entangled with  implementation details of a certain task or feature. The client shouldn't know about all the dependencies needed, all the method calls and in general the process of how the task is getting accomplished.

The Facade design pattern is about exposing purposeful and simple-to-use methods that the client is interested in all while encapsulating the complex logic behind it.

![Facade design diagram 1](/images/blog/design-patterns-facade/design_patterns_facade_diagram_1.png)


