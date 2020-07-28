---
layout: post
title:  "The Simple Factory design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

We say that a class is a Factory when the entire purpose of the class is to handle object creation , keeping the user of such class from being coupled to a concretely created object. The client code (i.e the user of the class ) does not know about any concrete implementation  and expects any object that fulfills a contract (in the form of an Interface) and then calls the Factory's create method passing it as a parameter the type of object it wants back in a form of a String for example. 

The factory knows about all the objects it can create, therefore when called with a request of "I want that type of object" it returns it with the help of "that type of object's concrete implementation". The goal achieved here is keeping the client from being coupled to a concrete object implementation, by having this be encapsulated inside the Factory class.


![Simple Factory Diagram 1](/images/blog/design-patterns-simple-factory/design_patterns_simple_factory_diagram_1.png)

The following is an example of how a Simple Factory class could be implemented (Java)

{% highlight java %}
public class SomeFactory {
	
	private Map<String, ISomeObject> products = new HashMap<>();
	
	public SomeFactory() {
		
        /*
		 Loading the objects that this factory
         should be able to make instances of.
        */
        
		products.put("Type A", new ConcreteObjectA());
		products.put("Type B", new ConcreteObjectB());
		
		
		/* OR
		 Call some method to load the available
         objects from some configuration file or database!
         */
		LoadProducts();
	}
	
	public ISomeObject create(String type) {
		
		if(products.containsKey(type)) {

			return products.get(type);

		} else {
			// return a NotFoundException ?
			// return a null object ? (Null Object Design pattern)
			return null;
		}
	}

	private void LoadProducts() {
		/* Loading the objects that this factory 
         should be able to make instances of.
         */
		
	}
	

	
}

{% endhighlight %}


Note that a Factory doesn't always have to "create" as in "new up" objects and could instead  retrieve them from somewhere and return them, e.g from an object pool in case we were implementing the Object Pool design.