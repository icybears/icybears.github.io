---
layout: post
title:  "The State design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

An object is a structure that encapsulates state (the data through fields) and behavior (the methods). Sometimes an object's behavior will depend on a value of one of its fields (the state). As a result we end up with one or more method implementations containing switch statements or conditionals where the value of some field is checked and based on it the method does something different.

Take for example a Ticket System for some customer support. When a customer opens a ticket, the ticket is in a `Pending` state, and the support staff can resolve the ticket and the ticket becomes in a `Solved` state. Solved tickets can be re-opened by staff and they return to a `Pending` state or they can be closed and they become in a definitive `Closed` state.
We can model this example with a Finite-State Machine diagram:

![Finite State Machine example diagram](/images/blog/design-patterns-state/design_patterns_finite_state_machine_diagram.png)

In code, this is what an object whose behavior depends on its state can look like:
```java
public class Ticket {

	private Date createdAt;
    // TicketStatus is an ENUM type holding PENDING,SOLVED, and CLOSED.
	private TicketStatus status;

	public Ticket() {
		createdAt = new Date(System.currentTimeMillis());
        // a ticket is initially in a pending state
		status = TicketStatus.PENDING;
	}

	public void resolve() {
		switch (status) {
			case PENDING:
				System.out.println("Resolving the ticket!");
				status = TicketStatus.SOLVED;
				break;
			case SOLVED:
				System.out.println("Error: Ticket is already resolved!");
				break;
			case CLOSED:
				System.out.println("Error: Ticket is closed!");
				break;
		}
	}

	public void reopen() {
		switch (status) {
			case PENDING:
				System.out.println("Error: Ticket is already open!");
				break;
			case SOLVED:
				System.out.println("Re-opening the ticket!");
				status = TicketStatus.PENDING;
			case CLOSED:
				System.out.println("Error: Cannot reopen a closed ticket!");
				break;
		}

	}

	public void close() {
		switch (status) {
			case PENDING:
				System.out.println("Error: Pending tickets must be resolved first!");
				break;
			case SOLVED:
				System.out.println("Closing the ticket!");
				status = TicketStatus.CLOSED;
			case CLOSED:
				System.out.println("Error: Ticket is already closed!");
				break;
		}
	}
}
```

Because the behavior of the `Ticket` object (its method implementations) depends on its current state (here the value of `TicketStatus`), it makes sense to extract a state object and have it encapsulate the behavior that is tied to it. The only thing left for the `Ticket` object to do is hold a reference to this state object and delegate to it for any state-related behavior.

This is what a `Ticket` class will look like, after extracting state-related behavior into its own state object with the help of a `TicketState` interface:
```java
public class Ticket {
	
	private Date createdAt;
	// TicketState is the interface that every state object must implement
	private TicketState currentState;

	public Ticket() {
		createdAt = new Date(System.currentTimeMillis());
		currentState = new Pending(this);
	}

	public void resolve() {
    // state-related logic is delegated to the state object
		currentState.resolve();
	}

	public void reopen() {
		currentState.reopen();
	}

	public void close() {
		currentState.close();
	}
	
	// a method that allows to explicitly change the state of a ticket 
	public void changeState(TicketState newState) {
		currentState = newState; 
	}
}
```

This is the `TicketState` interface:
```java
public interface TicketState {

	public void resolve();

	public void reopen();

	public void close();
	
}
```

And since a `Ticket` can either be `Pending`, `Solved` or `Closed`, we create a class for each one of these states. Each class will encapsulate the behavior that the ticket is supposed to have when it is in that state. 
This is what the `Pending` concrete state looks like:
```java
public class Pending implements TicketState {

	private Ticket ticket;
	
	public Pending(Ticket owner) {
		ticket = owner;
	}
	@Override
	public void resolve() {
		System.out.println("Resolving the ticket!");
		ticket.changeState(new Solved(ticket));		
	}

	@Override
	public void reopen() {
		System.out.println("Error: Ticket is already open!");
	}

	@Override
	public void close() {
		System.out.println("Error: Pending tickets must be resolved first!");
	}

}
```
The `Solved` concrete state:
```java
public class Solved implements TicketState {

	private Ticket ticket;
	
	public Solved(Ticket owner) {
		this.ticket = owner;
	}

	@Override
	public void resolve() {
		System.out.println("Error: Ticket is already resolved!");
		
	}

	@Override
	public void reopen() {
		System.out.println("Re-opening the ticket!");
		ticket.changeState(new Pending(ticket));
	}

	@Override
	public void close() {
		System.out.println("Closing the ticket!");
		ticket.changeState(new Closed(ticket));
	}

}
```

And finally the `Closed` concrete state:
```java
public class Closed implements TicketState {

	private Ticket ticket;

	public Closed(Ticket owner) {
		this.ticket = owner;
	}

	@Override
	public void resolve() {
		System.out.println("Error: Ticket is closed!");
	}

	@Override
	public void reopen() {
		System.out.println("Error: Cannot reopen a closed ticket!");
	}

	@Override
	public void close() {
		System.out.println("Error: Ticket is already closed!");
	}
}
```

Notice that a State object may hold a reference to the object it belongs to, here is is the `Ticket`, that way we can call methods on the `Ticket` object right from the `State` object. In this example, we are simply calling the `changeState()` method.

To give you an overview of what we've done so far, here is the class diagram of the State design pattern for this example:

![State design example diagram](/images/blog/design-patterns-state/design_patterns_state_diagram_1.png)

The key idea is to simply extract state-related logic from the "context" class into individual state classes. Have the context class delegate to the state object for state-related tasks. Of course your context class only knows about the interface that your state will implement (it could be an Abstract class, or an Interface). Additionally, state objects may hold reference to the context object in order to communicate with it. 

In general, this is the UML class diagram of the state pattern:
![State design pattern general diagram](/images/blog/design-patterns-state/design_patterns_state_diagram_2.png)