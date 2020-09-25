---
layout: post
title:  "The Strategy design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

The Strategy design pattern is a simple and intuitive design pattern that comes in mind when, in a given context (some object), we are trying to do something like accomplish a task or perform an operation and there happens to be different ways (strategies, algorithms...) to do it, and we want to accomodate all of these strategies, AND we want to do that in a decoupled way !

Simply, our context object will reference a generic strategy interface that will be implemented by all the possible strategies. Given a concrete strategy, the context object will delegate to it to perform the desired task.

For example, imagine we have a `Payment` object (the context) that exposes a `process()` method that allows to process a payment using a `PaymentProcessor`. The problem is that we want to accomodate for different payment processors. And the question is how to implement the `process()` method ?
Since the implementation of this method will vary depending on which payment processor is used, it makes sense to delegate to the chosen `PaymentProcessor` to do the processing for us. And thus emerges the Strategy pattern. See the following diagram:

![Stategy design pattern example 1](/images/blog/design-patterns-strategy/design_patterns_strategy_example_1.png)

This pattern allows us to avoid bloating the `process()` method with an implementation that is hardcoded for the different payment processors using things like conditionals. It also allows us to dynamically change the behavior of the `process()` method by being able to switch from using one payment processor (concrete strategy) to another at runtime by simply calling the `setProcessor()` method. We can also add more strategies without breaking existing code.

The following is a UML class diagram for the Strategy pattern in general:

![Stategy design pattern example 2](/images/blog/design-patterns-strategy/design_patterns_strategy_example_2.png)

Note that it is the client that will provide the `Context` with a specific `ConcreteStrategy`.
If there is any data that the strategy need to operate on, we can simply pass it as a parameter to the strategy's `execute()` method (see previous example)


