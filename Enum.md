If you've ever used constants in programming, then you might have asked yourself: "How can I store a bunch of constants in one place and handle them all at the same time?" Well, for this question Kotlin has an answer and we call that answer Enums. Basically, Enums represent a logical set of constants, and they make our code clearer and more readable. Let's take a closer look at them.

### Basic Enums
enum is a keyword, which allows us to create our own enumeration (the word that enum represents) just from a usual class:

```kotlin
enum class Rainbow {
RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET
}
```
As you can see from the example above, here is an enum for 7 colors of the rainbow. Now we have our own storage for all these colors. Each of them is a separate instance of Enum. You'll see how we can modify it further. Although now you can create your own Enum, for instance, for statuses of orders:

```kotlin
enum class Status {
OPEN, PENDING, IN_PROGRESS, RESOLVED, REJECTED, CLOSED
}
```
or for the main types of building materials:

```kotlin
enum class Materials {
GLASS, WOOD, FABRIC, METAL, PLASTIC, CERAMICS, CONCRETE, ROCK
}
```
According to Kotlin Coding Conventions, you can use either uppercase underscore-separated names (as usual Kotlin constants are RED_COLOR) or regular camel-humps names starting with an uppercase letter (RedColor). In our examples, we will omit the second option and use only uppercase Enums for better understanding, but keep in mind that both options are possible.

Let's go back to our first example with colors. Since each color is an instance of Rainbow Enum, you can initialize them by passing names of these colors to the constructor:

```kotlin
enum class Rainbow(val color: String) {
RED("Red"),
ORANGE("Orange"),
YELLOW("Yellow"),
GREEN("Green"),
BLUE("Blue"),
INDIGO("Indigo"),
VIOLET("Violet")
}
```
Now we can use color value wherever we want like this:

```kotlin
val color = Rainbow.RED.color
```
It looks very promising, but you may ask "What are the benefits?" That's the right question. You'll find it out later, but now let's modify our Enum and add one more parameter per color:
```kotlin
enum class Rainbow(val color: String, val rgb: String) {
RED("Red", "#FF0000"),
ORANGE("Orange", "#FF7F00"),
YELLOW("Yellow", "#FFFF00"),
GREEN("Green", "#00FF00"),
BLUE("Blue", "#0000FF"),
INDIGO("Indigo", "#4B0082"),
VIOLET("Violet", "#8B00FF")
}
```
Great! Rainbow enum contains information not only about the name of the color but also its HEX value. It's a widely used approach in web development to store a color value as a hexadecimal-digit form of blue, red, and green colors. You can find out more about web colors here. Now, you can use them like this:

`val rgb = Rainbow.RED.rgb`

As we said before, Enum is still a custom class, so we can add to it our own methods. Let's add a method which will print full information about an instance of our Rainbow:

```kotlin
enum class Rainbow(val color: String, val rgb: String) {
RED("Red", "#FF0000"),
ORANGE("Orange", "#FF7F00"),
YELLOW("Yellow", "#FFFF00"),
GREEN("Green", "#00FF00"),
BLUE("Blue", "#0000FF"),
INDIGO("Indigo", "#4B0082"),
VIOLET("Violet", "#8B00FF");

    fun printFullInfo() {
        println("Color - $color, rgb - $rgb")
    }
}
```
Now, let's call it:

```kotlin
val rgb = Rainbow.RED
rgb.printFullInfo()
```
The output will be the following:

`Color - Red, rgb - #FF0000`
### Inside Enum
Now we know what Enum is and how to create it, but in most situations, that's not enough. It's time to find out which methods and properties Enum provides in Kotlin out of the box:

1. name allows you to get the name of Enum's instance, for example:

```kotlin
val color: Rainbow = Rainbow.RED
println(color.name)
```
with the output:

```kotlin
RED
```
2. ordinal contains a position of Enum's instance, for example:

```kotlin
val color: Rainbow = Rainbow.GREEN
println(color.ordinal)
```
with the following output:

`3`
3. values() returns an array of all instances of Enum. It might be helpful if you want to iterate through Enum instances. Now we can check if any specific color is part of the Rainbow or not:

```kotlin
fun isRainbow(color: String) : Boolean {
for (enum in Rainbow.values()) {
if (color.toUpperCase() == enum.name) return true
}
return false
}
```
and try to call it:

```kotlin
println(isRainbow("black"))
```
with the following output:

`false`
4. valueOf() returns an instance of Enum by its name with String type and case sensitivity:

```kotlin
println(Rainbow.valueOf("RED"))
```
and the output will be:

`RED`
If there isn't any appropriate instance of Enum then IllegalArgumentException will be thrown. Please, note that this method is case sensitive.

If you want to extend your Enum but with a static context, then you need to wrap your method with companion object keywords. Let's modify our Rainbow in order to find an instance by RGB parameter:

```kotlin
enum class Rainbow(val color: String, val rgb: String) {
RED("Red", "#FF0000"),
ORANGE("Orange", "#FF7F00"),
YELLOW("Yellow", "#FFFF00"),
GREEN("Green", "#00FF00"),
BLUE("Blue", "#0000FF"),
INDIGO("Indigo", "#4B0082"),
VIOLET("Violet", "#8B00FF"),
NULL("", "");

    companion object {
        fun findByRgb(rgb: String): Rainbow {
            for (enum in Rainbow.values()) {
                if (rgb == enum.rgb) return enum
            }
            return NULL
        }
    }

    fun printFullInfo() {
        println("Color - $color, rgb - $rgb")
    }
}
```
and now we can use it like this:

```kotlin
println(Rainbow.findByRgb("#FF0001"))
```
and the output will be:

`NULL`
Have you guessed why it is NULL? As you might've noticed, we added one more NULL constant in order to return it if we cannot find a color by its RGB parameter. In our example, there isn't any color associated with "#FF0001" RGB, therefore the output is NULL.

### Conclusion
Let's summarize all the information above: Kotlin's Enum, in a nutshell, is a container for a collection of constants. For your convenience, there are some embedded properties and methods which allow you to get the names and order of the constants. You can get all instances of Enum or just one of them, which, hopefully, will make your life easier. Don't forget that you can extend your Enum whenever you want