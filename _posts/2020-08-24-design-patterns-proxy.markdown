---
layout: post
title:  "The Proxy design pattern"
description: "Design Patterns explained: The Proxy Design Pattern, with example code and diagrams"
categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

The Proxy design pattern is a structural design whereby we introduce an intermediary object (the proxy) between a client and an object of interest (the subject) so that when the client seeks to make calls to the subject, it is the proxy object that receives those calls and based on its internal logic, can  choose to delegate the calls to the actual object or not, or perform some task first, then delegate to the actual object, or delegate to the actual object then perform some task before returning to the client.

Both the proxy and the subject share a common interface. It is through this interface that the client makes calls, and because the client only knows about that interface, our program can substitute the intended object with the proxy object and the client wouldn't care. Additionally, the proxy object wraps around the subject which allows the proxy object to delegate to the subject before or after performing some logic.

The following is a UML diagram of the pattern:

![Proxy Design Diagram](/images/blog/design-patterns-proxy/design_patterns_proxy_diagram_1.png)

To summarize, a proxy is an object that can substitute some other object of interest AND wraps around it all at the same time.  The substitutability is achieved by having both the proxy and the object of interest share the same interface, and by wrapping around this object, the proxy can perform some "additional" logic (e.g access control, lazy loading, caching, remote access) before delegating to the actual object to do its job.
