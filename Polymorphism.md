## Polymorphism
Polymorphism is the ability of an object or its methods to take on many forms depending on its type and the parameters of this or that method. This definition may seem a little vague, so let's look closer at what polymorphism is in practice.

Assume you're programming robots that move boxes in a smart warehouse. For now, we only have two types of boxes: USUAL_BOX and FRAGILE_BOX but it doesn't mean that there won't be other different types of boxes in the future. To make working with robots easier, we create only one method MOVE:

Not all languages support this kind of polymorphism. For example, you can define such methods in Java or Scala, but not in Python or JavaScript.

Now, when we order a robot to move something, it checks the type of a box and adjusts its actions accordingly. It slows down when moving something fragile compared to a regular box and maybe it places this fragile box onto the designated spot for fragile boxes. The mechanism of defining several methods with the same name but with different parameters is known as overloading.

You can see that depending on the type of a box a robot can perform different tasks. We can call it a polymorphic method. Let's go a step further and look at how we can create polymorphic classes.

### Subtyping
Our warehouse may have high ceilings, so it's possible to make a big rack with boxes on it. Robots can reach a certain height, but we cannot expect them to reach the highest shelves. For this kind of task we can create the DRONE subclass of robots.

Basically, these drones do the same thing as the robots that run on the ground: they MOVE all kinds of boxes. However, we know that they do it differently by moving in the air. We can say that our robots take various forms, so we have a polymorphic object that may be a usual robot, or a drone, and how they perform the task depends on their type.

The technique to redefine methods of the parent class in its subclasses is called overriding. We override methods to extend or change their execution flow. In our example, we change the navigation method of robots to correctly control the drones.

### Duck typing
Another way to make polymorphic objects is called duck typing. Duck typing term comes from the duck test reasoning "If it looks like a duck, swims like a duck, and quacks like a duck, then it probably is a duck."

In programming, if an object A has the same methods or attributes as an object B, we can say that the object A is a form of B. In our example if we encounter an object with the method MOVE, we can use it to move boxes in a warehouse. Although this form of polymorphism is weak because all we know is that an object has a method with the same signature.

Imagine that we make two new kinds of robots HEAVYLIFTER and SCAVENGER with the method MOVE. We can use a HEAVYLIFTER as a usual robot, its behavior doesn't differ much. However, a SCAVENGER has an algorithm that moves boxes only to the waste container, so if we try to use it, we shouldn't be surprised to find our boxes in the garbage. No one is safe from such mistakes, so don't forget to check the work of your code with tests, if you prefer to use duck typing