---
layout: post
title:  "The Chain of Responsibility design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

The Chain of Reponsibility design pattern is a pattern that comes in mind when a request can be handled by one or more receivers. And the idea of the pattern is to chain all potential receivers together by simply having each receiver point to the next receiver in line.

From a handler in the chain of handlers, there are two distinguishable behaviors:
- The chaining behavior, it consists of having each handler be able to delegate to the next handler in line. This behavior is true for all handlers, which is why we can encapsulate it in a parent handler class and have the subclass inherit it. 
- The behavior pertaining to each individual handler. It's the business logic of the handler and is subjective to each handler and is therefore left for individual subclasses to define.

```java
public abstract class Handler {

	private Handler next;
	
	public abstract void operation();
	
	public void handle() {
		
		operation();
		
		if(next != null)
			next.handle();
	}
	
	public void setNext(Handler handler) {
		this.next = handler;
	}
}
``` 
So I wrote a simple `Handler` class that contains a field to hold the next handler, an abstract `operation()` method to encapsulate the handler's internal logic that is left for subclasses to define and a `handle()` method that will call the current handler's `operation()` method and then delegate to the next handler if there is any. 
And of course if there are any parameters they will simply be passed down the chain as arguments.

Here are a couple of concrete handlers that define a simple `operation()` (and of course inherit the default chaining behavior enforced by the `handle()` template method):

```java
public class FirstHandler extends Handler{

	@Override
	public void operation() {
		
		System.out.println("first handler called");
	}
}

public class SecondHandler extends Handler{

	@Override
	public void operation() {
		
		System.out.println("second handler called");
	}
}

public class ThirdHandler extends Handler{

	@Override
	public void operation() {
		
		System.out.println("third handler called");
	}
}
```
And finally in our main method:

```java
public static void main(String[] args) {
		
		Handler handler1 = new FirstHandler();
		Handler handler2 = new SecondHandler();
		Handler handler3 = new ThirdHandler();
		
		//chaining handler1 to handler2
		handler1.setNext(handler2);
		
		//chaining handler2 to handler3
		handler2.setNext(handler3);
		
		//launching the request to the first handler (the entry point)
		handler1.handle();
	}
```

The result of course is:
>first handler called
>second handler called
>third handler called

Note that a request can propagate through every handler in the chain, or it can propagate until it meets a specific handler that meets some kind of requirement to handle. The pattern simply enforces the creation of an ordered list of handlers that are able to handle the request and delegate to the next handler in line. 

Here is the general diagram for this pattern:
![Chain of Reponsibility design pattern diagram](/images/blog/design-patterns-chain-of-responsibility/design_patterns_chain_of_responsibility_diagram.png)

Note that in the code examples above, I used a template method to encapsulate the common "chaining" behavior and an abstract method to encapsulate the specific handler logic. Though we can simply enforce a single `handle()` method as is shown in the UML diagram.