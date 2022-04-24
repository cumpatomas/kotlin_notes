## FUNCTIONS AS OBJECTS
We know how to declare functions, how to invoke them, and what they do. Actually, Kotlin provides a way to work with functions as if they were objects. So, let's learn how to store a function as an object and how to use it.

First-class citizen
First-class citizens in programming are the objects that:

can be stored as variables,\
can be returned by a function,\
can be passed to a function as an argument,\
don't depend on their name,\
can be created at the program runtime (the time when the program is working).

For example, an Int is a first-class citizen in Kotlin. To clarify the fourth requirement, an Int with the ten name doesn't need to be 10. And vice versa: the 10 value doesn't need to be stored under the ten name. You can create as many differently named variables for the same value as you need and the value won't change because of the name changing.

In fact, functions are first-class citizens in the Kotlin language too. Let's prove it! In this topic, we will cover only the first four requirements. We will discuss how to create functions at runtime in the next topic.

###Function types
First of all, Kotlin has built-in support for function types. The syntax of a function type is this:

(parameters' types) -> return value type
In a function type there are arrow symbols (->) in the middle, also, there are parenthesized parameters' types split by commas on the left, and, finally, the return value type is written on the right. Thus the arrow seems to point from what the function takes to what it returns.

Let's recall some functions from previous topics and use them as examples:

```kotlin
fun sum(a: Int, b: Int): Int = a + b
sum has a type of (Int, Int) -> Int.

fun sayHello() {
println("Hello")
}
```
sayHello has a type of () -> Unit (this function takes nothing so parentheses with parameters' types are empty, and, also, it returns nothing so the result type is Unit).

### Function references as objects

Also, Kotlin allows getting references to functions. To get a reference to a top-level function, we simply need to write double colon (::) before its name and we don't write parentheses and arguments after the name. Take a look at the example:\
**::sum gives us an object of the (Int, Int) -> Int type.**

Now we are ready to assign function references to values! We can create values this way:

_**val sumObject = ::sum**_

Don't confuse this assignment with saving function result to a value like this: val sumResult = sum(1, 2). The sumResult value has the Int type because the result of the invoked sum function is just a number. Meanwhile, the sumObject value is initialized with a reference to the sum function (::sum), so it has the type of the sum function.

We can also specify the type of the sumObject value explicitly:

_**val sumObject: (Int, Int) -> Int = ::sum**_

In both cases, we have an opportunity to invoke the original sum function by invoking the object: sumObject(10, 20) returns 30 as if we invoked the original function with these arguments directly.

### Functions returning other functions
Since a function can be stored as an object, why not create a function that returns such an object? Let's do this. Take a look at the example below:

```kotlin
fun getRealGrade(x: Double) = x
fun getGradeWithPenalty(x: Double) = x - 1

fun getScoringFunction(isCheater: Boolean): (Double) -> Double {
if (isCheater) {
return ::getGradeWithPenalty
}

    return ::getRealGrade
}
```
Here we have a real grade function, which returns its argument, and a grade with penalty function, which returns its argument minus one (in other words, the decrement of its argument). Also, we have another function which provides us one of the previous two functions.

So if we do val wantedFunction = getScoringFunction(false), the wantedFunction value will contain a reference to a grade function for an honest student. Seeing the getScoringFunction function implementation, we can say that in this case the wantedFunction value contains a reference to the getRealGrade function. So the result of the wantedFunction(9.0) will be equal to 9.0.

### Function references as function parameters
Also, you can create functions that take other functions as arguments. Let's create such function:

```kotlin
fun applyAndSum(a: Int, b: Int, transformation: (Int) -> Int): Int {
return transformation(a) + transformation(b)
}
```
It receives two integers, transforms them using the received transformation function, and returns the sum of the transformed integers. We can declare some transformation functions:

```kotlin
fun same(x: Int) = x
fun square(x: Int) = x * x
fun triple(x: Int) = 3 * x
```
And then pass them to the former function:

```kotlin
applyAndSum(1, 2, ::same)    // returns 3 = 1 + 2
applyAndSum(1, 2, ::square)  // returns 5 = 1 * 1 + 2 * 2
applyAndSum(1, 2, ::triple)  // returns 9 = 3 * 1 + 3 * 2
```
### Real-world usage
The previous example seems to be a bit synthetic. What about more realistic examples? Well, see for yourself.

The String type has the filter method to filter symbols. How does it know which symbols to remove from the string and which ones to leave in it? The answer is simple: this method takes a predicate as an argument and then uses it for internal computations. _**A predicate is a function that takes an argument and returns a Boolean result**_. So in the filter method, the predicate says if the symbol should be left and has the (Char) -> Boolean type.

Let's try to use this method. If we want to remove dots from a string, we declare this predicate:

_**fun isNotDot(c: Char): Boolean = c != '.'**_

Then we can do something like this:

val originalText = "I don't know... what to say..."
val textWithoutDots = originalText.filter(::isNotDot)
As a result, the textWithoutDots string is equal to "I don't know what to say".

### Conclusion
It was the basis of a huge programming paradigm which is Functional programming. Having function objects, we can create functions that receive other functions as arguments. This paradigm is frequently used in Kotlin standard library. We discussed a few examples, but you will definitely find more in the following topics.