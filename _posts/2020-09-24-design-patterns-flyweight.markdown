---
layout: post
title:  "The Flyweight design pattern"
description: "Design Patterns explained: The Flyweight Design Pattern, with example code and diagrams"
categories: 
    - Design Patterns
tags:
    - Object-oriented design
---

The Flyweight design pattern is a structural design pattern that is aimed at achieving a specific goal: Reducing memory usage. This is relevant in specific scenarios too.

When you are in a position where you're making a large number of objects of the same class and you ask yourself: Is there a way to reduce the memory cost for all these objects ?
Well, you can make them "flyweight" and the way to do that is simple:
Look at the state of your objects, figure out which state remains the same across a large number of these objects and which state is particular to each one of them.
- The state that remains the same across a large number of objects is refered to as the intrinsic state, it's the state that is context-independent, and thus is valid for a large number of objects (can be shared!).
- The state that is particular to each object is refered to as the extrinsic state, it's the state that differs from object to object, it depends on the context in which the object is.

The Flyweight pattern suggests that a Flyweight object should only store intrinsic state and move extrinsic state into method parameters where that state is relevant to conduct some operation. The extrinsic state is passed directly when needed into the methods of the Flyweight, and it is the client that handles the storing or computing of this extrinsic state.

So instead of instantiating multiple objects of various 'extrinsic' state, you instantiate just the Flyweight object with its common, context-independent intrinsic state, and whatever particular 'extrinsic' state is needed to conduct an operation is directly passed by the client into the appropriate method on the Flyweight object.

For example, imagine we want to fill up a 2D map that is divided into small tiles with different textures: Grass, water and rock.
If we use the following class for our tiles:

![Example before part 1](/images/blog/design-patterns-flyweight/design_patterns_flyweight_before_1.png)

The following objects are created:

![Example before part 2](/images/blog/design-patterns-flyweight/design_patterns_flyweight_before_2.png)

As you can see, because of the way we designed our Tile class, there is an object for each configuration of coordinates and textures eventhough we have 3 common types of textures repeating themselves per object, we are bound to create as many objects as there are coordinates. So the idea of the Flyweight pattern is to have a class for every intrinsic state, i.e a `GrassTile`, `RockTile` and `WaterTile` each respectively encapsulating a grass, rock and water texture. 
The coordinates (extrinsic state) will be passed from outside directly unto the method that needs them (here for example the `render()` method).

Here is the refactoring of the Tile class following the Flyweight design pattern:

![Flyweight design pattern part 1](/images/blog/design-patterns-flyweight/design_patterns_flyweight_after_1.png)


These classes will be instantiated in a controlled manner, possibly one instance per class, so that they can effectively be shared across multiple operations with varying (extrinsic) configuration provided by the client. In our example, the same instance of a `GrassTile` will be used to render every tile with a grass texture in the map using the different coordinates provided by the client. Same goes for the `RockTile` instance and `WaterTile` instance.
Also note that the intrinsic state on the Flyweight object is set at construction time, and is not to be altered because the Flyweight is an object that may be shared across different contexts.

In order to control the instantiation of these Flyweight objects, the Flyweight design pattern comes with a Flyweight factory class that will handle the creation and managing of the Flyweight objects. See the following UML class diagram:

![Flyweight design pattern part 2](/images/blog/design-patterns-flyweight/design_patterns_flyweight_after_2.png)

In general this is the UML class diagram of the Flyweight pattern:

![Flyweight design pattern general diagram](/images/blog/design-patterns-flyweight/design_patterns_flyweight_general.png)

Notice that we can also have Flyweight objects that don't need to be shared because they have no intrinsic state to share across different contexts, but they can still be used to conduct operations using extrinsic state.

