As you may know, there are two types of references in Kotlin: nullable and non-nullable. What you don't really know is how to use them properly and what benefits they may give you, so let's take a closer look at these types.

### Safe Calls
Let's start with an example: there is a city object with plenty of nested objects. We want to access the name of a building from this city like this:

```kotlin
if (city != null &&
city.address != null &&
city.address.street != null &&
city.address.street.building != null
) {
print(city.address.street.building.name)
} else {
print(null)
}
```
Looks pretty bad, doesn't it? You have to call too many if-checks in order to avoid a NullPointerException (NPE). Can we work around it? Yes! For this, Kotlin provides safe calls:

```kotlin
print(city?.address?.street?.building?.name)
```
That's it! Just add a ? sign every time right after a nullable reference. ? will compare its value to null and return null if that reference is null. In other words, these two lines are equal:

```kotlin
city?.address
(if (city == null) null else city.address)
```
As you can see from the example above, it's even more convenient for chains. Here is an example of two safe calls in a row:
```kotlin
city?.address?.street
((if (city == null) null else city.address)?.street)

```
### Elvis Operator
Kotlin has another interesting way of handling nullable variables. Let's take a look at this snippet:

```kotlin
var name: String? = "Kotlin"
val length: Int? = name?.length
print(if (length != null) length else 0)
```
Now we have our own length variable. As you see, it will be null in case of null in the name variable. However, we can simplify the code with the Elvis operator:

```kotlin
var name: String? = "Kotlin"
val length: Int? = name?.length
print(length ?: 0)
```
The Elvis operator works like this: if the left-hand side of the expression (name?.length) is not null, return it; otherwise, the right-hand side (0) is to return. You can also use return and throw expressions in the right part:

```kotlin
val length: Int = name?.length
?: throw Exception("The name is null")
```
### The !! Operator
There is an easy way to invoke an NPE: the !! operator. The code won't crash only if you're a 100% sure that your variable won't be null:

```kotlin
var name: String? = "Kotlin"
print(name!!.length)
```
The code above looks like it's screaming, trying to scare the compiler. The piece of code above is almost equal to the one below:

```kotlin
var name: String? = "Kotlin"
val length: Int = name?.length
?: throw Exception("The name is null")
print(length)
```
This operator is used to stop the program when null is met.

Do you remember that we read an input every time like readln() (before Kotlin 1.6, readLine() )? Now you understand why: the readlnOrNull() returns a nullable string. The null is returned when the user doesn't enter a line. If this happens, we don't want to continue the program, so here we ask to check the input for null using the !! operator.

Example of using readlnOrNull():

```kotlin
fun main() {
println("What is your nickname?")
val nickname = readlnOrNull()
println("Hello, $nickname!")
}
```
The output will be the following for Nick value:

What is your nickname?
> Nick
Hello, Nick!
And the output for null (use Ctrl+D / ⌘+D on Mac):


```kotlin
What is your nickname?
> ^D
Hello, null!
```

### Idioms
At first glance, safe calls and Elvis operators may seem complicated, but these functions allow you to write less code when dealing with nullable variables. It's an idiomatic way to write Kotlin code. Here we demonstrate to you some idioms from the official documentation. Almost everything you've learned in this topic is an idiom, so we'll briefly recall the material.

The first idiom in our list is the safe call (they use the term "if-not-null"):

```kotlin
val nullString: String? = null
println(nullString?.length)    // null
val emptyString: String? = ""
println(emptyString?.length)   // 0
```
The next idiom is the Elvis operator with a safe call ("if-not-null-else") :

```kotlin
val nullString: String? = null
println(nullString?.length ?: -1)   // -1
val emptyString: String? = ""
println(emptyString?.length ?: -1)  // 0
```
The third idiom is the ability to throw exceptions with the Elvis operator. Take the opportunity to try it in your project. Just use the following code in place of readln(). If a program can't read a line, instead of an NPE you will get a more specific exception. You can test this by typing Ctrl+D in Intellij IDEA. When debugging, it's nice to know what's going on in your programs rather than pulling your hair out over NPE's.

`readlnOrNull() ?: error("No lines read")`
The last idiom is working with nullable Boolean. This is how it works:

```kotlin
val b: Boolean? = ...
if (b == true) {
...
} else {
// `b` is false or null
}
```
Seems easy, right? Now, let's discuss a more difficult case. Suppose you want to create an expression that tries to test something in a nullable variable and returns false if the variable is null. Otherwise, it returns the result of checking like normal. It's easy to do this with an Elvis operator:

```kotlin
val nullString: String? = null
println(nullString?.isNotEmpty() ?: false) // false
```
What if we try it without the Elvis operator? This way is still possible, but harder to understand:

```kotlin
val nullString: String? = null
print(nullString?.isNotEmpty() == true) // false
```
In this example, nullString?.isNotEmpty() gives you a nullable Boolean. So, when you check == true, a result of false means either that the expression is false or that the variable is null, which is exactly what we want. You won't need to write in this form; this is just an example so you can recognize it if you come across this structure in the future.