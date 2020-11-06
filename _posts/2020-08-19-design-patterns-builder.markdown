---
layout: post
title:  "The Builder design pattern"
description: "Design Patterns explained: The Builder Design Pattern, with example code and diagrams"
categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

When we want to construct an object we simply use the object constructor for most cases. Object construction is crucial as it allows to instantiate objecs in a valid state.

However, for more complex objects that need a lot of configuration to be in a valid state it becomes much more intuitive to make the process of constructing such objects a process of its own, detached from the object itself, i.e the construction of an object called the Product is done by another object called the Builder. The builder class specializes in dealing with the various fields that the product needs and then builds the product with all its needed fields already set.

Otherwise, we would have to either create multiple (and possibly large) constructors or resort to creating empty objects initially and then using setter methods to fill up their required fields.

## A simple builder class as a helper for object creation

In the following example, we're creating `Pizza` objects, and to do that we use a helper class that is going to be a static inner class called `Builder`.
The `Builder` class allows to specify the size property of a pizza aswell as the crust, sauce, cheese, topping and whether we want extra cheese on it or not. At the end, the `Builder` is able to build a valid `Pizza` object with the beforementioned field values set.

{% highlight java %}
public class Pizza {

	public static class Builder{
		
		private Size size;
		private Crust crust;
		private Sauce sauce;
		private Cheese cheese;
		private Topping topping;
		private boolean extraCheese = false;
		
		public Builder() {}
		
		public Builder size(Size size) {
			this.size = size;
			return this;
		}
		
		public Builder crust(Crust crust) {
			this.crust = crust;
			return this;
		}
		
		public Builder sauce(Sauce sauce) {
			this.sauce = sauce;
			return this;
		}
		
		public Builder cheese(Cheese cheese) {
			this.cheese = cheese;
			return this;	
		}
		
		public Builder topping(Topping topping) {
			this.topping = topping;
			return this;
		}
		
		public Builder withExtraCheese() {
			this.extraCheese = true;
			return this;
		}
		
		public Pizza build() {
			return new Pizza(this);
		}
	} // end of the builder inner class
	
        private Size size;
        private Crust crust;
        private Sauce sauce;
        private Cheese cheese;
        private Topping topping;
        private boolean extraCheese = false;
	/* 
    The pizza object retrieves its construction data 
    from the builder object.
    */
	public Pizza(Builder builder) {
            this.size = builder.size;
            this.crust = builder.crust;
            this.sauce = builder.sauce;
            this.cheese = builder.cheese;
            this.topping = builder.topping;
            this.extraCheese = builder.extraCheese;
	}
}
{% endhighlight %}

Since the `Builder` object holds the necessary data for the creationg of a `Pizza` object, the `Pizza` class expects a `Builder` object at construction time.

Note that in the above example, we are returning the `this` object to allow the client code to chain method calls which is totally optional and has nothing to do with the pattern itself, client code use of the builder could look like this: 

{% highlight java %}
Pizza.Builder builder = new Pizza.Builder();

Pizza pizza = builder.size(someSize)
                    .crust(someCrust)
                    .cheese(someCheese)
                    .topping(someTopping)
                    .withExtraCheese()
                    .build();
{% endhighlight %}
                    
The flexibility of this builder class consist in that we don't have to call all the builder methods and instead we can call only the methods that we want for our object construction...for example, a small sized pizza consisting of some kind of crust and some kind of cheese and that's it, no topping nor extra cheese!

{% highlight java %}
Pizza crustAndCheesePizza = builder.size(someSize)
                                    .crust(someCrust)
                                    .cheese(someCheese);
{% endhighlight %}

## A more robust Builder

In the previous example, the client code directly uses the `Builder` to create a desired `Product` object. What we could have instead is to have the calls to the builder be encapsulated inside another class called the `Director`.

For example, imagine we want to make a Neapolitan Pizza. The client tells the `Director` to make a Neapolitan Pizza and then the `Director` calls all the necessary methods on the builder to produce a Neapolitan Pizza. The `Director` knows about the steps needed for a specific product and directs the `Builder` on what to do to produce such product.
{% highlight java %}
public class Director {

	private IPizzaBuilder builder;

    public Director(IPizzaBuilder builder){
        this.builder = builder;
    }

	public void setBuilder(IPizzaBuilder builder) {
        // allow the Director to work with different concrete builder objects.
		this.builder = builder;
	}
	
	public void makeNeapolitanPizza() {
            builder.size(...);
            builder.crust(...);
            builder.sauce(...);
            builder.cheese(...);
            builder.topping(...);
	}
	
	public void makeVegetarianPizza() {
		...
	}
}
{% endhighlight %}

In the above code snippet, the `Director` expects to be provided with a concrete `Builder` implementation through the `setBuilder()` method.

The Builder interface can be something like this:
{% highlight java %}
public interface IPizzaBuilder {

	void reset();
	
	void size(Size size);

	void crust(Crust crust);

	void sauce(Sauce sauce);

	void cheese(Cheese cheese);

	void topping(Topping topping);

	void withExtraCheese();
}
{% endhighlight %}

Note that the `reset()` method is a useful method that allows for reusing the Builder object because after producing an object from a specific Builder object, we are stuck with the data of the produced object inside the builder unless we call something like a `reset()` method which will allow to simply new-up a new instance of a Pizza object inside the Builder, allowing for the possibility to reuse the same Builder object to create multiple differing products as can be seen in the following PizzaBuilder concrete class:
{% highlight java %}
public class PizzaBuilder implements IPizzaBuilder {

	private Pizza pizza;
	
	public PizzaBuilder() {
		pizza = new Pizza();
	}

	@Override
	public void reset() {
		pizza = new Pizza();
	}
	
	@Override
	public void size(Size size) {
		pizza.setSize(size);
	}

	@Override
	public void crust(Crust crust) {
		pizza.setCrust(crust);
	}

	@Override
	public void sauce(Sauce sauce) {
		pizza.setSauce(sauce);
	}

	@Override
	public void cheese(Cheese cheese) {
		pizza.setCheese(cheese);
	}

	@Override
	public void topping(Topping topping) {
		pizza.setTopping(topping);
	}

	@Override
	public void withExtraCheese() {
			pizza.setExtraCheese(true);
	}
	
	public Pizza getResult() {
		Pizza result = pizza;
        /*
            cleaning up the internal state of the builder object 
            by calling the reset() method.
        */		
		reset();		
		return result;
	}
}
{% endhighlight %}

And the client code could look like this:

{% highlight java %}
PizzaBuilder builder = new PizzaBuilder();
Director director = new Director(builder);

director.makeNeapolitanPizza();

Pizza neapolitan = builder.getResult();

{% endhighlight %}

We can see from the above snippet that the `director.makeNeapolitanPizza()` simply hides the required steps for the creation of a neapolitan pizza from the client and we still retrieve the pizza object from the builder.

And finally the UML diagram for this example:

![Builder Design Diagram](/images/blog/design-patterns-builder/design_patterns_builder_diagram_1.png)