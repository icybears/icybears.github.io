---
layout: post
title:  "The Factory Method design pattern"
description: "Design Patterns explained: The Factory Method Design Pattern, with example code and diagrams"
categories: 
    - Design Patterns
tags:
    - Object-oriented design
---


The Factory Method design pattern is a creational design pattern that is about dedicating a method of a class for the creation of  objects that this class needs without tightly coupling the class to the objects that it creates. It is done by leaving the choice of what specific object to create to the subclasses.

This is about decoupling a class from the objects that it normally creates using the `new` keyword and becomes tightcoupled to. The process is now encapsulated behind the "factory" method. 

Simply moving the instantiation of a specific kind of object inside the "factory" method will still keep the coupling to the concrete object that is being created.

That is why the class implementing the factory method pattern should let the subclasses determine the specific kind of object they need. This achieves the possibility of new kind of objects being created. We can have as many types of objects as subclasses we create, achieving therefore the Open/Closed Principle.

There are, however, considerations to make that are best explained through an example.

Imagine we have a `CarManufacture` class (*I call it manufacture instead of factory to avoid any confusion with the pattern*), this class is responsible of creating cars and managing car manufacturing operations.
For the creation process of the `Car` objects, the class is simply creating and returning new concrete `Car` instances. 
Additionally, our `CarManufacture` class is used by our client code too, `CarDealership` class which expects the `CarManufacture` to provide it with a `Car` object's price.

![Diagram 1](/images/blog/design-patterns-factory-method/design_patterns_factory_method_diagram_1.png)

Client code is tied to `CarManufacture` which in turn is tied to a specific concrete `Car` object.

Let's fix this.

Step 1: Abstraction

![Diagram 2](/images/blog/design-patterns-factory-method/design_patterns_factory_method_diagram_2.png)

We introduced an `ICar` interface to decouple the `CarManufacture` and the Client code from the concrete `Car`object. We also made the `CarManufacture`'s method abstract, allowing for subclasses to extend the `CarManufacture` for different types of cars.

Step 2: Extension

![Diagram 3](/images/blog/design-patterns-factory-method/design_patterns_factory_method_diagram_3.png)

Note that the only reason we went for extending the `CarManufacture` instead of using an `Interface` is because we assumed in this example that the `CarManufacture` was generic enough and that the only varying part of it was the `Car` object that it used to create. Meaning that all the operations in `CarManufacture` are valid for the subclasses.

In summary, the factory method design pattern is all about encapsulating object creation inside a method of our class and allowing extensibility in regards to what type of object to create. 

In general, the factory method design pattern looks like this:
![Diagram 4](/images/blog/design-patterns-factory-method/design_patterns_factory_method_diagram_4.png)


Note that if we had dedicated an entire class to handle object creation instead of a method then we'd end up with a Simple Factory design. If we designed this class to create several types of related objects through multiple methods then we'd end up with something like the Abstract Factory design.



