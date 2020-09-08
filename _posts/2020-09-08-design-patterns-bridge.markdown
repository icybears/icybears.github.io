---
layout: post
title:  "The Decorator design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

Among all **structural** design patterns, the Bridge pattern seems to be the most difficult to grasp and easy to misunderstand when you first encounter it.

The common definition of the pattern is this:
>Decouple an abstraction from its implementation so that the two can vary independently

The idea of an abstraction is a pretty vague one, anything can be an abstraction of something else, a class may not need to be an "interface" or an "abstract class" for it to be considered an "abstraction". In fact, a class can very well contain some kind of implementations AND be an abstraction of something specific.

To get a concrete idea of what I mean let's consider the following example:
A Workshop (think of an artisan's workshop) can be either "Old school" or "Modern", and inside this Workshop we can be using material based on wood or material based on metal.
We could model this situation by the following UML class diagram:
![Inheritance diagram](/images/blog/design-patterns-bridge/design_patterns_bridge_diagram_1.png)

In the above diagram we can see that each subtype of workshop is tied to both the type of workshop (old school or modern) and the type of material used (wood or metal)
Now what if I said that we actually want to be able to represent a workshop and work with it independently of what type of material it uses. In other words, we want the Workshop to be abstract in regards to what kind of material it uses i.e we want our client code to be able to work with our Workshop code and its subtypes (Old school or modern) regardless of the type of material used. To make this possible all we have to do is take ALL the "material" related code inside our Workshop class and replace it with calls to an abstract class or interface. This abstract class or interface represents common operations on "whatever material". This is what will eventually allow the Workshop class to work with "whatever material" and therefore exist and evolve independently of what material is used.

To illustrate this:
![Refactoring to the Bridge pattern diagram](/images/blog/design-patterns-bridge/design_patterns_bridge_diagram_2.png)

And that is the Bridge pattern. It works to "shield" some code (the workshop) from depending on other concrete code (the material). In fact the bridge pattern is nothing but an implementation of [the Shield pattern](https://wiki.c2.com/?ShieldPattern)

To make sure we've truly understood what the bridge pattern is about, let's look at another, more practical example. But first, notice that in the previous example we've let the Bridge pattern emerge through refactoring, however the Bridge pattern is not necessarily a refactoring pattern but it is more of a considerate design that is made upfront based on some requirements.
Take the example of a Cross-platform GUI Library, clearly, we want our GUI library to be cross-platform, clients should be able to work with the library  and the library should be able to exist and evolve without being tied to any one platform. Thus the Bridge pattern is convenient here as we want to allow our GUI library (the abstraction i.e abstract of platform related code) to be decoupled from any specific platform-related code (the implementation). 
So the idea of the Bridge pattern is to have the GUI Library make the needed calls to a Platform interface instead of a concrete platform. This will then create two different hierarchies, one for the GUI Library, and the other, for the Platform concrete implementations. See the following UML class diagram:

![the Bridge pattern example 2](/images/blog/design-patterns-bridge/design_patterns_bridge_diagram_3.png)

Finally, here is the general class diagram for the Bridge pattern, you can see that it is consistent with what we've been doing so far.

![the Bridge pattern diagram](/images/blog/design-patterns-bridge/design_patterns_bridge_diagram_4.png)