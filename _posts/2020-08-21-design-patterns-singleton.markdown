---
layout: post
title:  "The Singleton design pattern"

categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

Whenever we want to create an instance of a class from anywhere in our code, we simply call the class's constructor.
```java
 ClassA instance = new ClassA();
```
The constructor is by default public and therefore freely accessible by outside users, and on top of that, the constructor guarantees the creation of a new instance of the class in memory.

 It turns out that there are cases where we do not want users of our class to enjoy the privilege of being able to create new objects of a class at will using the publically accessible constructor. In the case of the Singleton pattern, the goal is to make sure only a single instance of a class can be created and accessible to all users.

 For that, we simply prevent users of the class from accessing the class's constructor and provide them with an alternative, a carefully crafted method that will allow them to retrieve the instance of a class all while making sure no more instances will be created and the same one instance will be provided to all users.

 - We make the constructor private, preventing users of the class from creating new instances.
 - We make available a method that will make sure one and the same instance of the class is returned to the callers. Such method cannot be an "instance-level" method i.e a typical public method because users cannot instantiate objects let alone call instance-level methods on them, so it becomes clear that this method has to be on the class-level, i.e static.

{% highlight java %}
public Type {

private static Type instance;

private Type(){}

public static Type getInstance(){
    if(instance == null){
        instance = new Type();
    }
    return instance;
}
}
{% endhighlight %}

The first time the client code calls `getInstance()`, the private instance is null and a new instance is created and returned. Any subsequent calls to `getInstance()` will result in the returning of that same instance. This is the lazy approach, i.e the first time the instance is created is exactly when it is first asked for. This approach is NOT thread-safe.

The eager approach is to have an instance created and available the moment the class is loaded in the JVM. Subsequent calls to `getInstance()` will return the one instance that was already created at class loading time. As a reminder, static/class-level members are loaded in memory the moment a class is loaded into the JVM by the class loader. See the following example. 

{% highlight java %}
public Type {

private static Type instance = new Type();

private Type(){};

public static Type getInstance(){
    return instance;
}
}
{% endhighlight %}

Note that the above approach IS thread-safe however not lazy.

To achieve Thread-safety and lazy-loading there are a couple of known approaches:

### Double-checked locking

{% highlight java %}
public Type {

private static volatile Type instance;

private Type(){}

public static Type getInstance(){
  if (instance == null) {
			synchronized (Type.class) {
				if (instance == null) {
					instance = new Type();
				}
			}
		}
    return instance;
}
}
{% endhighlight %}

You can read about the [double-checked locking mechanism in depth here](https://en.wikipedia.org/wiki/Double-checked_locking#Usage_in_Java)

### Initialization-on-demand holder idiom

{% highlight java %}
public class Type {
    private Type() {}

    private static class LazyHolder {
        static final Type INSTANCE = new Type();
    }

    public static Type getInstance() {
        return LazyHolder.INSTANCE;
    }
}
{% endhighlight %}

You can read about the [initialization-on-demand holder idiom in depth here](https://en.wikipedia.org/wiki/Initialization-on-demand_holder_idiom)