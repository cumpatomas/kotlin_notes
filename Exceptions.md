As you already know, an exception interrupts normal execution of a program. Normally, this is not what we want to happen. Good news is, it is possible to write code that will handle exceptions without stopping the whole program.

To do that, Kotlin has the exception handling mechanism. After a line of code throws an exception, Kotlin attempts to find a suitable handler for it. Such a handler can be located in the same method where the exception occurred or in the calling method. As soon as a suitable handler is found and executed, the exception is considered handled and the program runs normally.

In this topic, we will learn about two keywords for handling exceptions: try and catch.

## The try-catch statement
Here is a simple try-catch template for handling exceptions:

```kotlin
try {
// code that may throw an exception
} catch (e: SomeException) {
// code for handling the exception
}
```
The try block is used to wrap the code that may throw an exception. This block can include all lines of code, including method calls.

The catch block is a handler for the specified type of exception and all its subtypes. This block is executed when an exception of the corresponding type occurs in the try block.

Note that the specified type in the catch block must be an exception.
In the presented template, the catch block can handle exceptions of the Exception type and all types derived from it.

The following example demonstrates the execution flow with try and catch:

```kotlin
println("Before the try-catch block") // it will be printed
try {
println("Inside the try block before an exception") // it will be printed
println(2 / 0) // it throws ArithmeticException
println("Inside the try block after the exception") // it won't be printed
}
catch (e: ArithmeticException) {
println("Division by zero!") // it will be printed
}

println("After the try-catch block") // it will be printed
```
The output:

```kotlin
Before the try-catch block
Inside the try block before an exception
Division by zero!
After the try-catch block
```
The program does not print "Inside the try block after the exception" since the ArithmeticException aborted the normal flow of execution. Instead, it executes the print statement in the catch block. After the completion of the catch block, the program executes the next statement (printing "After the try-catch block") without returning to the try block again.

Replacing Exception with ArithmeticException or RuntimeException in the catch statement does not change the execution flow of the program. But replacing it, for instance, with NumberFormatException will make the handler unsuitable for the exception and the program will fail.

Pay attention! The variables announced in the try block will only be available in the block: you can't work with it neither outside nor in the catch block.

If the result of the execution of the try block throws an exception not foreseen in the catch expression, the program will fail despite all precautions.
### Getting info about exceptions
When an exception is caught by the catch block, it is possible to get some information on it. To do this, we use message:

```kotlin
try {
val d = (2 / 0).toDouble()
}
catch (e: Exception) {
println(e.message)
}
```
This code prints:

`/ by zero`
### Catching multiple exceptions
It is always possible to use a single handler for all types of exceptions:

```kotlin
try {
// code that may throw exceptions
}
catch (e: Exception) {
println("Something goes wrong")
}
```
Obviously, this approach does not allow us to perform different actions depending on the type of exception that has occurred. Fortunately, Kotlin supports the use of several handlers inside the same try block:


```kotlin

try {
// code that throws exceptions
}
catch (e: IOException) {
// handling the IOException and its subtypes   
}
catch (e: Exception) {
// handling the Exception and its subtypes
}
```
You can add as many catch blocks as you need. When an exception occurs in the try block, the runtime system determines the first suitable catch block according to the type of the exception; matching goes from top to down.

The catch block with the base type has to be written below all the blocks with subtypes. In other words, more specialized handlers (like IOException) must be written before the more general ones (like Exception). Otherwise, the block with the subtype will be ignored.
### Where and how to handle an exception
Technically, an exception can be handled in the method where it occurs or in the calling method. The best approach to handle an exception is to do it in a method that has sufficient information to make the correct decision based on this exception.

So, why should we throw out specific types of exceptions when there is a general Exception that includes all possible cases and is always appropriate? Well, keep in mind that your colleagues or maybe even yourself in a couple of weeks may not really know what’s going on in the code. It would be best for you to provide as much information as possible. This will make handling exceptions much easier.

Always try to find the exception type that fits best to your exceptional event, for instance, throw a NumberFormatException instead of just an Exception.

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