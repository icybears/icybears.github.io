---
layout: post
title:  "The Decorator design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

Imagine we want to model a system that deals with cars of different types, and of different "characteristics".
A car can be a fuel car or an electric car, and such cars can be enhanced with tinted windows or paint jobs. In our system, we don't want to simply have a field called "enhancements" and keep track of a car enhancements but instead an "enhanced car" is treated as a special type of car of its own.

The resulting types of cars are the following:

- A Fuel Car
- An Electric Car
- A Repainted Fuel Car
- A Repainted Electric Car
- A Tinted Windows Fuel Car
- A Tinted Windows Electric Car
- A Repainted Tinted Windows Fuel Car
- A Repainted Tinted Windows Electric Car
- ...

Using inheritance, A `RepaintedFuelCar` will subclass a `FuelCar` and so on, the following is a UML class diagram of what it would look like for all the types:
![Messy Inheritance diagram](/images/blog/design-patterns-decorator/design_patterns_decorator_diagram_1.png)

The main danger here is that we have tight coupling between a lot of classes. Another issue is that adding more types will increase this inheritance tree in size pretty rapidly. Imagine if we added a "Steam Car" and all the subsequent enhanced car types in the same manner that we did to the fuel car and the electric car...

Note that for the sake of simplicity, the only business logic we have is the `getDescription()` method that is supposed to return the appropriate description for each type.

## Introducing the Decorator Pattern

The Decorator pattern allows to build on top of an existing object and enhance/extend its behavior without having to resort to inheritance. Because inheriting from a class by default gives the child access to the parent's behavior but also allows it to extend such behavior by overriding the parent's methods which creates tight coupling between classes. Also, for every "enhanced type" we need, we must first create its class.

The Decorator pattern presents a flexible way of dynamically creating different types of objects by making use of composition instead of inheritance. 
A Decorator object is composed of the object we want to enhance, which makes the Decorator able to delegate to the encapsulated object when needed but also add state and/or behavior to it. So far this is just a typical wrapper object. However, the Decorator object also shares the same interface as the encapsulated object. This results in the ability of a decorator object to encapsulate not just the object we want to enhance, but also an already enhanced object, adding even more enhancement to it. 

For our car example, `Car` is the component that we wish to enhance and extend, while `EnhancedCar` is the Decorator object that will both encapsulate a `Car` object and share its interface. See the following diagram:

![Decorator diagram part 1](/images/blog/design-patterns-decorator/design_patterns_decorator_diagram_2.png)

In code, this is what the `Car` component looks like:
```java
public interface Car {

	public String getDescription();
}

```

And the decorator object `EnhancedCar`:

```java
public class EnhancedCar implements Car{

	private Car car;

	public EnhancedCar(Car car) {
		this.car = car;
	}
	
	@Override
	public String getDescription() {
		
		/*
		 * defines a generic behavior by simply delegating 
		 * to the encapsulated car's getDescription()
		 */
		
		return car.getDescription();
	}

}

```
Now from the previous diagram we can have emerge the other types of cars (FuelCar, ElectricCar,...) and enhanced cars (TintedWindowsCar, RepaintedCar,...).

![Decorator diagram part 2](/images/blog/design-patterns-decorator/design_patterns_decorator_diagram_3.png)

Here is what the `FuelCar` class looks like:
```java
public class FuelCar implements Car{

	@Override
	public String getDescription() {
		
		return "Fuel Car";
	}

}
```
The `ElectricCar` class looks the same as above with the obvious exception that it returns "Electric Car" from its `getDescription()` method.

Here is what the `RepaintedCar` decorator class looks like:
```java
public class RepaintedCar extends EnhancedCar {

	private Car car;
	
	public RepaintedCar(Car car) {
		super(car);
	}
	
	@Override
	public String getDescription() {
		return "Repainted "+super.getDescription();
	}
}
```
`RepaintedCar` is a concrete decorator class that takes in an object of type `Car` at construction time, and extends the `getDescription()` logic with its own by concatenating "Repainted " to the encapsulated `Car` object's `getDescription()` content. `TintedWindowsCar` class follows the same idea.

Earlier I talked about how the Decorator pattern allows to create new types of objects dynamically, well, this is what it looks like:

```java		
/*
* An ElectricCar object inside a TintedWindowsCar decorator object
*/
Car tintedWindowsElectricCar = new TintedWindowsCar(new ElectricCar());

// prints: "Tinted Windows Electric Car"
System.out.println(tintedWindowsElectricCar.getDescription());

/*
* A FuelCar object inside a TintedWindowsCar decorator object 
* inside a RepaintedCar decorator object
*/
Car repaintedTintedWindowsFuelCar = new RepaintedCar(new TintedWindowsCar(new FuelCar()));

// prints: "Repainted Tinted Windows Fuel Car"
System.out.println(repaintedTintedWindowsFuelCar.getDescription());
		
```

By composing together these related objects, we were able to produce new objects of different configurations.

And finally, here is the class diagram of the decorator pattern in general:
![General decorator diagram ](/images/blog/design-patterns-decorator/design_patterns_decorator_diagram_4.png)



