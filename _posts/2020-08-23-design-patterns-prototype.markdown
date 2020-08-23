---
layout: post
title:  "The Prototype design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

The Prototype design pattern is a creational design pattern that allows to turn an object (with all its internal state) into "the starting point" for the creation of new objects of the same type. Such object is called a `Prototypical instance`. The process by which we create new objects from this prototypical instance is referred to as `Cloning`.

Our goal is not just to create new objects of some type, but also to have these objects be created in the same state as the prototypical instance. 

The idea of the Prototype pattern is to provide objects with an internal mechanism in the form of a `clone()` method, that will allow them to create clones of themselves. This `clone()` method will encapsulate the creation of a new empty object and the copying of the properties from the current object into the new one before returning it back to the caller.

The following is a diagram of the Prototype design pattern.

![Prototype Design Diagram](/images/blog/design-patterns-prototype/design_patterns_prototype_diagram_1.png)

By default, Java supports cloning just by making a class implement Cloneable interface, and then we can implement the `clone()` method by delegating the cloning to `Object.clone()` inside our class's `clone()` method by calling super.clone(). See the following code snippet.

```java
public class MyClass implements Cloneable{
	
    /*
    A bunch of state
    */
    private int prop1;
    private String prop2;
    ....
    private SomeType prop10;

    /*
        A bunch of operations, accessors, mutators...
    */

    public MyClass clone() throws CloneNotSupportedException {
        return (MyClass) super.clone();
    
    }

}
```

One thing to keep in mind though, `Object.clone()` does a shallow copy, meaning that if our class contains properties of complex types like objects and not just primitive types, then all cloned objects's complex properties will reference the same object as the cloned object's complex properties. For a deep copy we'll have to provide our own custom implementation of the `clone()` method which could perform the copying of complex properties properly.

And in case we had multiple prototypical instances, i.e objects of interest that we intend to make copy of in the future, then we can keep track of all our "prototypes" in a prototype registry. 

In the following example, we are defining a registry of prototypes of type `SomeType` containing two prototypical instances each holding different default values that an object of type `SomeType` may be initialized with at creation time.

```java
class SomeTypePrototypeRegistry {

    private static final Map<String, SomeType> prototypes = new HashMap<>();

    static {
        prototypes.put("default v1", new SomeType("value1","value2",...));
        prototypes.put("default v2", new SomeType("valueA","valueB",...));
    }

    public static SomeType getPrototype(String name) {
        try {
            /*
            Cloning the requested prototype using 
            the prototype's internal clone method.
            */
            return prototypes.get(name).clone();
        } catch (NullPointerException ex) {
            System.out.println("Prototype " + name + " does not exist");
            return null;
        }
    }
}
```