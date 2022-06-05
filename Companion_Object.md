## Companion object
An object declaration inside a class can be marked with the companion keyword:

```kotlin
class Player(val id: Int) {
companion object Properties {
/* Default player speed in playing field - 7 cells per turn */
val defaultSpeed = 7

        fun calcMovePenalty(cell: Int): Int {
            /* calc move speed penalty */
        }
    }
}

/* prints 7 */
println(Player.Properties.defaultSpeed)
```
A companion object is a singleton attached to an outer class, and hence it cannot be accessed without accessing the outer class. It allows us to understand that the current object is somehow connected with the outer class. For example, we can store the default speed for all players in the Player companion object. That also means that every Player instance contains a reference to the companion object and will return its instance every time.

Until now, we've worked with a named companion object. However, unlike the case of a nested object, the name can be omitted. Let's try it:

```kotlin
class Player(val id: Int) {
companion object {
/* Default player speed in playing field - 7 cells per turn */
val defaultSpeed = 7

        fun calcMovePenalty(cell: Int): Int {
            /* calc move speed penalty */
        }
    }
}

/* prints 7 */
println(Player.defaultSpeed)
```
As you can see, if we omit the name of a companion object, we can still access it through the outer class declaration. If we want to somehow use it, we can define it manually. If a companion object doesn't have a name, we can also use the default name Companion:

```kotlin
/* prints 7 too */
println(Player.Companion.defaultSpeed)
```
### Companion object and outer class
A companion is really closely associated with the outer class. You may freely use properties and functions from the companion object in the outer class. For example:

```kotlin
class Deck {
companion object {
val size = 10
val height = 2
fun volume(bottom: Int, height: Int) = bottom * height
}

    val square = size * size             //100
    val volume = volume(square, height)  //200
}
```
But what happens if the outer class has a property with the same name as the companion object? Well, in this case, the properties from the class will shadow the properties of the companion.

```kotlin
class Deck {
companion object {
val size = 10
}
val size = 2
val square = size * size // 4
}
```
In this case, if you want to use a property from the companion, you must use the companion's name, or, if it wasn't named, the default name Companion:

```kotlin
class Deck {
companion object {
val size = 10
}
val size = 2
val square = Companion.size * Companion.size // 100
}
```
Similar to the case of nested objects, you cannot use the properties and the functions of the outer class in the inner one. For example, you cannot do something like that:

```kotlin
class Deck() {    
val size = 2
object Properties {
val defaultSize = size // you cannot get this variable
}
}
```
### Limitations of companion objects
Only one companion object is available for each class. That means you can't create multiple companion objects for a class because Kotlin does not support this behavior, even if they have different names. If we try to do it, an error will occur:

```kotlin
class BadClass {
companion object Properties {

    }

    companion object Factory {
    
    }
}

// Compilation error
// Error: Only one companion object is allowed per class
```
However, we can create one companion object and several nested objects:

```kotlin
class Player(val id: Int) {
companion object Properties {
/* Default player speed in playing field - 7 cells per turn */
val defaultSpeed = 7

        fun calcMovePenalty(cell: Int): Int {
            /* calc move speed penalty */
        }
    }

    /* creates a new instance of Player */
    object Factory {
        fun create(playerId: Int): Player {
            return Player(playerId)
        }
    }
}

/* prints 7 */
println(Player.Properties.defaultSpeed)

/* also prints 7 */
println(Player.defaultSpeed)

/* prints 13 */
println(Player.Factory.create(13).id)
```
There is one more limitation: we cannot create a companion object inside another singleton (or a companion object) because that violates the global access principle.

```kotlin
object OuterSingleton {
companion object InnerSingleton { // Modifier 'companion' is not applicable inside 'object'

    }
}
```
### Analogue in other languages
If you come from another programming language, you may be a little confused by companion objects. The closest concept to it is static members. The keyword static means that fields and methods with this modifier are common for all objects of the class and can be used without creating an instance of the class. You may encounter this keyword in several languages.

For example, in Java, the usage of static will look like this:

```
class Dog {
public static int numOfPaws = 4;

    public static String createSound() {
        return "WUF-WUF";
    }
}
/*prints WUF-WUF*/
System.out.println(Dog.createSound());
```
As you may know, Kotlin doesn’t have the static keyword. If you need something common to all instances of a class, you can use a companion object:

```kotlin
class Dog {
companion object {
val numOfPaws: Int = 4
fun createSound(): String = "WUF-WUF"
}
}
/*prints WUF-WUF*/
println(Dog.createSound())
```
As you can see, when you use a companion object you also don't need to create a class instance to get this function! Remember, a companion object is not equal to a Java static initializer. In Kotlin, it is a single nested object that wraps all methods and fields which are common for the whole class.

### Conclusion
Let's recap! Declaring a companion object is a good way to organize your data. It is like a nested object, but more closely related to the class. You can freely work with its properties from the outer class as if they are the companion's own properties. Use a companion object when you need one singleton associated with a class: it is preferable to use a companion object rather than a nested class.