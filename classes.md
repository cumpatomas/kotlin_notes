## CLASSES

When you roll dice, they are real objects in your hands. While the code you just wrote works perfectly fine, it's hard to imagine that it's about actual dice. Organizing a program to be more like the things it represents makes it easier to understand. So, it would be cool to have programmatic dice that you can roll!
All dice work essentially the same. They have the same properties, such as sides, and they have the same behavior, such as that they can be rolled. In Kotlin, you can create a programmatic blueprint of a dice that says that dice have sides and can roll a random number. This blueprint is called a class.
From that class, you can then create actual dice objects, called object instances. For example, you can create a 12-sided dice, or a 4-sided dice.
Tip: A class is similar to how an architect's blueprint plans are not the house; they are the instructions of how to build the house. The house is the actual thing, or object instance, created according to the blueprint.
Note: Organizing everything about dice into a class is called encapsulation. Encapsulation is a big fancy word, but all it means is that you can enclose functionality that is logically related into a single place.

Similar to using the fun keyword in Kotlin to create a new function, use the class keyword to create a new class.
You can choose any name for a class, but it is helpful if the name indicates what the class represents. By convention, the class name is written in Upper Camel Case (also called Pascal Casing). For example: Car, ParkingMeter, and CustomerRecord are all valid class names, and you can guess at what they represent.\

So far..\
Variables: lowercaseCamel\
Functions: lowercaseCamel\
Classes: uppercaseCamel\

The class declaration consists of the class name, the class header (specifying its type parameters, the primary constructor etc.) and the class body, surrounded by curly braces. Both the header and the body are optional; if the class has no body, curly braces can be omitted.

```kotlin
class Customer        // 1
class Contact(val id: Int, var email: String)   // 2
fun main() {
    val customer = Customer()                         // 3
    val contact = Contact(1, "mary@gmail.com")  // 4
    println(contact.id)                    // 5
    contact.email = "jane@gmail.com"            // 6
}
```

1.	Declares a class named Customer without any properties or user-defined constructors. A non-parameterized default constructor is created by Kotlin automatically.
2.	Declares a class with two properties: immutable id and mutable email, and a constructor with two parameters id and email.
3.	Creates an instance of the class Customer via the default constructor. Note that there is no new keyword in Kotlin.
4.	Creates an instance of the class Contact using the constructor with two arguments.
5.	Accesses the property id.
6.	Updates the value of the property email.
 
### Default constructor

Constructors are class members that initialize a new object of the class. In other words, constructors set a new object state by defining its properties. So, when you create an object, you invoke a constructor.
For further examples, let's use the class Size:
```kotlin
class Size {
    var width: Int = 1
    var height: Int = 1
}

```
Let's remember for a second how to create objects. We write the class name and empty parentheses after it:\

`val size = Size()`

This is actually a constructor call, and it's like calling a function with no arguments. Every class needs to have a constructor, so if it isn't explicitly defined, the compiler automatically generates a default constructor, which only creates an object and doesn't have any logic inside.
### Primary constructor

Often you know the object's properties before you create it. To make your code more concise, you can set the properties in the constructor: just make the constructor receive the arguments you need.
The primary constructor is the right tool for that. It does not contain any code, it just initializes an instance of a class and its properties. To define a primary constructor, you should put class initialization arguments in the parentheses after the class name.
The primary constructor for the class Size will look like this:
```kotlin
class Size(width: Int, height: Int) {
    val width: Int = width
    val height: Int = height
    val area: Int = width * height
}
```
Usually, to define a constructor, you should put the keyword constructor before its parameters. Kotlin's primary constructor allows you to omit the keyword.
In any way, another legitimate way to define the primary constructor looks like this:
```kotlin
class Size constructor(width: Int, height: Int) {
    val width: Int = width
    val height: Int = height
    val area: Int = width * height
}
```
### Property declarations

You can put simple property declarations inside the primary constructor. To declare a read-only property, put the keyword val in the parentheses before the argument name. For a mutable property, use the keyword var.
For example, let's move the property width from the class body:
```kotlin
class Size(val width: Int, height: Int) {
    val height: Int = height
    val area: Int = width * height
}
```
Now let's put the remaining property height inside the primary constructor:
```kotlin
class Size(val width: Int, val height: Int) {
    val area: Int = width * height
}
```
### Default and named arguments

Default values in primary constructors are set in the same way as in the class body. You declare the property with a keyword val or var and place the default value after the assignment operator:
```kotlin
class Size(var width: Int = 1, var height: Int = 1) {
    val area: Int = width * height
}
```
When creating an object of a class with default values in the primary constructor, you can use the default values by omitting the arguments:
```kotlin
val size = Size() // width == 1, height == 1
You can either provide values without property names or use named arguments when creating instances of a class:
val size1 = Size(3, 5) // width == 3, height == 5
val size2 = Size(width = 3, height = 5) // width == 3, height == 5
val size3 = Size(height = 5, width = 3) // width == 3, height == 5
```
You can also omit some of the properties with default values when creating an object. Keep it in mind though, if you break the order of the arguments in the primary constructor, you should always use named arguments:
```kotlin
val sizeWide = Size(10) // width == 10, height == 1
val sizeHigh = Size(height = 10) // width == 1, height == 10
```
A primary constructor is a handy way to define a class concisely. If you want to avoid redundant lines of code, try to make use of primary constructors and default arguments.
### Single line classes
If there are no other class members left except the ones in the primary constructor, we can omit empty curly braces. Imagine that the area property is missing in our example:
class Size(val width: Int, val height: Int)
You can see such classes frequently in real life. For example, data classes—classes whose main utility is storing data—are defined in this way. You will learn about them later on.
Init
Primary constructors cannot contain any code: they only set the values of class properties based on the passed arguments. Sometimes, we want to set some of our properties based on other properties' values or other sources of information. In such cases, we would use initializer blocks, which are prefixed with the keyword init:
```kotlin
class Size(_width: Int, _height: Int) {
    var width: Int = 0
    var height: Int = 0

    init {
        width = if (_width >= 0) _width else {
            println("Error, the width should be a non-negative value")
            0
        }
        height = if (_height >= 0) _height else {
            println("Error, the height should be a non-negative value")
            0
        }
    }
}
```
The keyword init signifies a block of code that serves **as an extension of the primary constructor**. For example, the code below prints a message after the object properties have been set in the primary constructor:
```kotlin
class Size(val width: Int, val height: Int) {
    init {
        println("Initializer block that prints the width ($width) and the height ($height)")
    }
}
```
There may be several initializer blocks in the class body. In this case, property initializers and init blocks are executed in the order of their appearance:
```kotlin
class Size(_width: Int, _height: Int) {
    // 1: the width property is initialized
    val width = _width

    // 2: 1st init block is executed
    init {
        println("First initializer block that prints the width $width")
    }

    // 3: the height property is initialized
    val height = _height

    // 4: 2nd init block is executed
    init {
        println("Second initializer block that prints the height $height")
    }

    // 5: the area property is initialized
    val area = width * height
}
```
**note:** In the examples above, the parameter names begin with underscores (_width, _height) to distinguish them from class members (width, height). It is a useful coding convention widely accepted in various programming languages.

