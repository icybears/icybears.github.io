---
layout: post
title:  "The Template Method design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

The Template Method design pattern is a fairly simple behavioral pattern that is useful when we have a process or an algorithm that can be divided into several small steps that needs to be done in a specific order, and we want the flexibility of being able to vary each of these small steps individually.

We encapsulate the whole algorithm inside its own class and encapsulate each part or step in its own method, and finally we define the overall process inside the "template" method by making calls to the various small steps. Now varying each small step of the process comes down to overriding method definitions through inheritance. 

Note that the process is defined in the template method and cannot be modified, the individual implementations of the different parts of the process that are encapsulated in their own methods is what can be implemented or modified by subclasses. 

What must be implemented by subclasses is represented by an `abstract` method.
What is optionally modifiable is implemented by the superclass (e.g default behavior) and can be overriden by subclasses and is represented by `protected` methods. 
What is extra and totally optional is defined in the superclass by empty methods and can be overriden by subclasses. For example, hooks that allow subclasses to "hook into" the process/algorithm at various points to do some extra stuff if desired.

Let's take as a quick example a `Process` that we manage to divide into a series of parts: `taskA()` followed by `taskB()`, then `taskC()`. And for convenience we'll add one extra step, `after()` ,that will hook after the process is achieved.

```java
abstract public class Process{

	protected void TaskA(){
		//contains default behavior, subclasses can still override it
	} 

	protected abstract void TaskB(); // for subclasses to specify

	protected abstract void TaskC(); // for subclasses to specify

	protected void after(){
		//empty method that will hook at the end of the process.
		//left for convenience if subclasses want to do something after
	}

	public final void templateMethod(){
		taskA();
		taskB();
		taskC();
		after();
	}
}
```

