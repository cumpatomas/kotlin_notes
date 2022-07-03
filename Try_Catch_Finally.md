### The finally block
As you already know, you can use the try-catch statement to handle exceptions in Kotlin. Actually, there is another possible block called finally. All the statements present in this block will always execute regardless of whether an exception occurs in the try block or not.

```kotlin
try {
// code that may throw an exception
}
catch (e: Exception) {
// exception handler
}
finally {
// code is always executed
}
```
In the example above, the finally block is executed after the catch block. You will find finally helpful in situations when you need to perform some finishing actions regardless of whether an exception is thrown. For example, an exception could occur when you're working with a file: then, in the finally statement, you can write the closing of this file.

### Three magic words
Take a careful look at the example below: it illustrates the order of execution of the try-catch-finally statement.

```kotlin
try {
println("Inside the try block")
println(2 / 0) // throws ArithmeticException
}
catch (e: Exception) {
println("Inside the catch block")
}
finally {
println("Inside the finally block")
}

println("After the try-catch-finally block")
```
The output will be the following:

```kotlin
Inside the try block
Inside the catch block
Inside the finally block
After the try-catch-finally block
```
If we remove the line that throws the ArithmeticException, the finally block will still execute after the try block:

```kotlin
Inside the try block
Inside the finally block
After the try-catch-finally block
```
Perhaps you have a question: why do you need the finally block at all, if you can just write the code after try-catch? For a better understanding, take a look at the following example:

```kotlin
fun main() {
try {
val a = 0/0 // throws ArithmeticException
}
finally {
println("End of the try block") // will be executed
}
println("End of the program") // will not be printed
}
```
After the exception is thrown, the line that prints "End of the program", which is located after try-catch, will not run. By contrast, the finally block will always be executed.

Note that the finally block is executed even if an exception occurs in the catch block.
### Omitting blocks
It is also possible to write try and finally without a catch block:

```kotlin
try {
// code that may throw an exception
}
finally {   
// code will always be executed
}
```
In this template, the finally block is executed right after the try block.

So, the code can contain any number of catch blocks or no such blocks at all, and finally blocks can be omitted. However, at least one catch or finally block must be present. This means that when you handle an exception with try, you should add at least one other block: catch or finally. Otherwise, the statement simply won't work.

### Try is an expression
Unlike in Java and many other languages, try can be an expression in Kotlin. This means that it may have a return value:
```kotlin
val number: Int = try { "abc".toInt() } catch (e: NumberFormatException) { 0 }
println(number) // 0

```
In the try block, we're trying to assign the value "abc" to the variable number.

When we attempt to convert the string "abc" to the Int type, the NumberFormatException is thrown. Then, the catch block is executed, so the number variable takes the value 0.

The returned value of a try expression is either the last expression in the try block or the last expression in the catch block(s). The contents of the finally block do not affect the result of the expression:

```kotlin
val number: Int = try { "2a".toInt() } catch (e: NumberFormatException) { 0 }
finally { println("Inside the finally block") }
println(number)
```
The output is the following:

```kotlin
Inside the finally block
0
```
Another useful technique is to rethrow exceptions to the caller. You need to add a way to handle exceptions in the code snippet where you call the function that can throw an exception. Here is an example of how to do this with an expression-style try:

```kotlin
fun test() {
val result = try {
countSomething()
} catch (e: ArithmeticException) {
throw IllegalStateException(e) // do not forget to deal with it
}

    // Working with result
}


try {
test()
} catch (e: IllegalStateException) {
// ...
}
```
### Idiom
Using try-catch blocks as expressions is an idiomatic way of working with exceptions in Kotlin. You get the result immediately, which is very convenient. Compare this method with a less direct one:

```kotlin
val string = "abc"
val number = try {
string.toInt()
} catch (e: NumberFormatException) {
-1
}

// ...

val string = "abc"
var number = 0 // try to avoid var if possible
try {
number = string.toInt()
} catch (e: NumberFormatException) {
number = -1
}
```
### Conclusion 
Now you know how to use the full try-catch-finally statement by adding a finally block that executes no matter what the exception does. Remember that **you can skip catch or finally when you handle an exception with try, but you can't omit both at the same time**. You can also add syntactic sugar to your code as a try-catch-finally expression.