---
layout: post
title:  "The Observer design pattern"
description: "Design patterns explained: The Observer Design Pattern, with example code and diagrams"
categories: 
    - Design Patterns
tags:
    - Object-oriented design
---


The Observer pattern is a Behavioral pattern that is aimed at solving the case where one or multiple objects depend on an object of interest (the subject) to change or execute an operation in order for them to do their job. 
The idea is that when a change or an operation happens, the subject notifies all the observers so they can do what they are supposed to do. For that, a subject must keep track of all the objects that wish to be notified. Since we're talking about one or multiple objects the subject will have to track them in a collection. Any object that wish to be notified should be able to "subscribe" to the subject (and get added to the collection), or unsubscribe from the subject (and get removed from the collection). When the "trigger" event happens (Again, it could be a change of state or a method call), the subject will then notify all the observers. This behavior simply consists in looping over the collection of subscribed objects and calling a specific method on them (e.g `update()`). Typically, an `Observer` interface defines the contract that observers should adhere to and it also allows to decouple the `Subject` from concrete observers.

The code snippet below shows an implementation of a `Subject` class.

```java
public class Subject{

    private Set<Observer> observers = new HashSet<>();

    public void operation(){
        // do something
        
        //and then

        //notify the observers
        notifyAllObservers();
    }

   public void notifyAllObservers() {
		for(Observer o: observers){
			o.update();
		}
	}
    
    public void attach(Observer observer){
        observers.add(observer);
    }

    public void detach(Observer observer){
        observers.remove(observer);
    }
}
```
This is a simple `Observer` interface. 
```java
public interface Observer{
    void update();
}
```
_Note that in Java there is a `java.util.Observer` provided out of the box and that has been deprecated since Java 9._

Here is the UML diagram of the Observer pattern that shows the `Subject` in relation with the observers using the `Observer` interface:

![Observer Design Pattern Diagram](/images/blog/design-patterns-observer/design_patterns_observer_diagram_1.png)

It's worth noting that when the event of interest is a change of state and the subject is about to notify or "broadcast" the event to the observers, there are two ways that this can happen in:
- The subject only notifies the observers of the change and lets them "pull" the updated data that they're interested in. This is called the "pull model".
- The subject automatically sends the changed state to the observers as soon as it happens. This is the "push model".

Another way to look at the Observer pattern is of a decoupled collaboration between a core stateful component and multiple state dependent components. For example you can think of a Model as the stateful component and Views as state dependent components. When a change happens to the Model the views should know about it and reflect it. So instead of coupling the two together, we encapsulate the stateful component and let state dependent components observe it and thereby get notified when a change happens. 

The following UML class diagram is an example of GUI components that are observing a stateful component so that if a change happens to its internal state they will get notified and update themselves accordingly.

![Observer design pattern example diagram](/images/blog/design-patterns-observer/design_patterns_observer_diagram_2.png)
