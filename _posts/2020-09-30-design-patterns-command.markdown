---
layout: post
title:  "The Command design pattern"
description: "Design Patterns explained: The Command Design Pattern, with example code and diagrams"
categories: 
    - Design Patterns
tags:
    - Object-oriented design
---
In OOP, objects communicate by sending messages to each other. It is done by having one object invoke a method on another object. This process is often refered to as sending a request, and it involves two participants: The sender (or invoker, the object that calls a method) and the receiver (the object whose method is being called).

Take as an example a button. When a button is clicked we want to do something. Obviously we are not going to write the whole logic of what to do inside the `Button` class but we will instead make a request to an object that can do the work, e.g a `Service` object. 

We can have a `StartButton`, when clicked, the object will invoke the `start()` method of the service object.

```java
public class StartButton {

    //this service will be injected
	private Service service;
	
	public void onClick() {
		service.start();
	}
}
```
Notice how we are defining what method to invoke at compile time.
If we want to have a button that calls the `stop()` method instead, we will have to write another class and hard-code it.

```java
public class StopButton {

    //this service will be injected
	private Service service;
	
	public void onClick() {
		service.stop();
	}
}
```

Also notice how the process of sending a message by default creates tight-coupling between the invoker (the button object) and the receiver(the service object).

The idea of the Command pattern is to turn individual requests into objects. Method invocations like `service.start()` or `service.stop()` will be encapsulated inside command objects, `StartCommand` and `StopCommand`. Anything that a request needs, primarily the object whose method is being called and any arguments will be encapsulated inside the command object. To make the request, the sender will have to call the `execute()` method of the command object.
The command object completely hides the receiver from the invoker and serves as a layer of indirection between the two. The invoker will be working with a generic command interface in order to be able to use all kind of commands.

So now instead of having a `StartButton` and `StopButton` with hard-coded method calls, we can simply have a `Button` class that will take a specific `Command` object and will call its execute method.

```java
public class Button {
	private Command command;

    public Button(Command concreteCommand){
        command = concreteCommand;
    }
	
	public void onClick() {
		command.execute();
	}
}
```
Now we can pass any `Command` object to the `Button` at runtime and the `onClick()` method will end up invoking whatever request/method call is encapsulated inside this command object.

This is the `Command` interface:
```java
public interface Command {

	public void execute();
}
```
We then define concrete commands, `StartCommand` and `StopCommand` each encapsulating a different method call to a `service` object that will be delegated to:
```java
public class StartCommand implements Command{

	private Service service;
	
	public StartCommand(Service service) {
		this.service = service; 
	}
	
	@Override
	public void execute() {
		service.start();
		
	}

}
```
```java
public class StopCommand implements Command {
	
	private Service service;

	public StopCommand(Service service) {
		this.service = service; 
	}

	@Override
	public void execute() {
		service.stop();

	}
}
```
To get a larger view of what we've done so far, here is a UML class diagram of the classes previously mentioned and their relationships:
![Command design pattern example](/images/blog/design-patterns-command/design_patterns_command_diagram_1.png)

The last thing left is for a `Client` to instantiate a concrete `Command` and pass a `Service` object to it, and then instantiate a `Button` object and pass the concrete command to it. Something like:
```java
Service service = new Service();
Command startCommand = new StartCommand(service);
Command stopCommand = new StopCommand(service);

Button startButton = new Button(startCommand);
Button stopButton = new Button(stopCommand);
```


Eventually, the Command pattern is a layer of indirection and flexibility where the object initiating the request (aka the invoker) delegates to a command to encapsulate and hide the receiver of the request from it. Additionally the command will also in turn delegate to a receiver to do the task. 
Because the command is an "objectified" request or method call, you can swap out commands at runtime since it's as simple as passing a different command object to the same invoker and it will result in different requests. This is [Dynamic dispatch](https://en.wikipedia.org/wiki/Dynamic_dispatch) in effect, and it's a recurrent theme in design patterns.

This is the final diagram for our example:
![Command design pattern final example](/images/blog/design-patterns-command/design_patterns_command_diagram_2.png)

And here is the overall diagram for the Command design pattern:
![Command design pattern general diagram](/images/blog/design-patterns-command/design_patterns_command_diagram_3.png)

Note that the command object can have more state and methods that will give it more control over a request, allowing for functionalities such as undoing a request and redoing a request, though not necessarily related to the pattern itself, these features are almost always brought up when people talk about the Command pattern.

Finally, we can briefly compare the Strategy pattern and the Command pattern and notice that the strategy's execute method wraps the algorithm itself whereas the command's execute method wraps the receiver of the request. 