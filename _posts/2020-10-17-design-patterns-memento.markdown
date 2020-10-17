---
layout: post
title:  "The Memento design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

The Memento design pattern is a pattern that arises when we have interest in rolling back an object to a previous state, which brings the need for tracking the internal state of an object over time.

The first thing that may come in mind is "Tell, don't ask!", the object that has the data is better fit to operate on it, and therefore we might think that tracking the internal state of an object is better done by the object itself. However, that can be a serious violation of the Single Responsibility principle. The object could have a specific purpose, and tracking its state is better handled by an external component, like a "History" or "Tracker" component. Therefore we do not want to bloat our object with logic that serves to track its own state and potentially break single responsibility.
This brings us to the other issue that the pattern aims to solve, which is how to track the internal state of an object without breaking the encapsulation of said object, or in other words, private state of an object should remain private, even if we're trying to keep track of it by an external component.

So the idea of the pattern is to simply have the object whose state we're trying to track (referred to as the Originator) provide us with a "snapshot" of its internal state. This "snapshot" is itself an object whose sole purpose is to hold the state of the Originator at a given moment, this object is called the Memento. The Originator will feed its own state into the Memento and provide it to the client when asked for, specially to the object that will keep track of all these Memento objects (referred to as the Caretaker).

Other than being able to provide a snapshot of its internal state inside a Memento object, the Originator is also able to restore itself to a previous state given a Memento object.

Now how do we feed the Originator's state into a Memento object ? We obviously need to write specific Memento class that is able to hold the same data that is held in the Originator class. So a common implementation of the pattern in Java is to make the Memento a static inner class of the Originator class. And it makes sense because the Memento is a snapshot of the Originator.
In the Java language it can look like this:

```java
class Originator {

    private State state;

    public Memento createMemento() {
        return new Memento(state);
    }
 
    public void restoreFromMemento(Memento memento) {
        state = memento.getState();
    }

    public static class Memento {

        private final State state;
 
        private Memento(State state) {
            this.state = state;
        }
 
        private String getState() {
            return state;
        }
    }
}
```

The `createMemento()` method returns a snapshot of the internal state of the `Originator` by means of the `Memento` object which has been fed the state at construction time and immediately returned.

The object responsible for keeping track of the different created snapshots is the `Caretaker`. It is also responsible for providing the `Originator` with `Memento` objects when asked, all of this without being able to access or alter the Mementos' internal state. 

The implementation of a `Caretaker` really depends on what we are trying to achieve and is beyond the scope of the pattern itself. Do we want to keep track of all the Mementos (a stack or a list) or just one ? Do we want to be able to move back and forth through Mementos to fulfill both  "undo" and "redo" functionalities ( e.g using multiple stacks) ? Do we want to keep track of the Mementos by timestamp (e.g using a Hashmap ) ? 

For our code snippet, we will simply use a stack, and when asked for a `Memento` we will simply `pop()` the last item to enter the stack.

```java
public class Caretaker {
	
	Stack<Memento> mementos = new Stack<>();
	
	
	public void save(Memento memento) {
		mementos.push(memento);
	}
	
	public Memento getLastSavedMemento() {
		
        return mementos.pop();
	}
	
```

The overall UML class diagram for this pattern is the following:
![Memento design pattern diagram](/images/blog/design-patterns-memento/design_patterns_memento_diagram.png)