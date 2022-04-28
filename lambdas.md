## Lambda Expressions
We already know how to declare functions with fixed names. Now let's find out the last factor of being a first-class citizen: the opportunity to create a function at runtime and without a predefined name.

### Functions without names
To create a function that isn't bound to its name in Kotlin you can create an anonymous one or a lambda expression:

`fun(arguments): ReturnType { body }` – this one is commonly called an **"anonymous function"**.\
`{ arguments -> body }` – this one is commonly called a **"lambda expression"**.

To make it clearer, take a look at the example below. Here two functions are declared; they are declared in different ways but they do the same thing:

```kotlin
fun(a: Int, b: Int): Int {
return a * b
}

{ a: Int, b: Int -> a * b }
```
As you see, they compute the multiplication of two numbers.

Both these functions have a reasonable type: _**(Int, Int) ->_** Int. So types work just like they do for top-level functions discussed in previous topics.

We need to add that if you want to declare a lambda without arguments, you do not need to write the "arrow symbols". So, a lambda without argument definition looks like this: { body }.

You may ask: how to use a function without a known name? The answer is: there are several options.

For example, you can assign the function to a variable and then invoke it by invoking the variable:

```kotlin
val mul1 = fun(a: Int, b: Int): Int {
return a * b
}

val mul2 = { a: Int, b: Int -> a * b }

println(mul1(2, 3))  // prints "6"
println(mul2(2, 3))  // prints "6" too
```
Also, you can pass such a function as an argument or return such a function from another function.

Finally, you can place parentheses with desired arguments right after the function definition to invoke it in place. However, that doesn't make much sense. So, mostly the three first described options are used.

The process of creating these two functions is quite similar, but lambdas have a more concise and convenient syntax. Therefore, almost always lambdas are used to create a function at runtime in real life. Moreover, there are programmers that don't listen to Kotlin official naming rules, so they can say "an anonymous function" instead of "a lambda". Despite the fact that everybody understands them, we suggest you to call them as they are.

For the same reason of convenience, now we will talk only about lambdas.

### Lambdas and syntactic sugar
There are ways to make the code more readable for human beings without changing the code logic. If there is such a way in a programming language and it relates to syntax, its name is syntactic sugar. Kotlin promotes Functional Programming so there is syntactic sugar for it.

Let's recall this example of passing the function as an argument:

```kotlin
fun isNotDot(c: Char): Boolean = c != '.'
val originalText = "I don't know... what to say..."
val textWithoutDots = originalText.filter(::isNotDot)
```
Rewrite it to pass a lambda:

```kotlin
val originalText = "I don't know... what to say..."
val textWithoutDots = originalText.filter({ c: Char -> c != '.' })
```
It works! First of all, Kotlin infers types of many objects, and here specifying the c type isn't necessary:

`originalText.filter({ c -> c != '.' })`

Second, there are situations when the lambda is passed as the last argument. This is the case. Kotlin provides a way to eliminate these bracket sequences ({ }), allowing to write the lambda outside the parentheses:

`originalText.filter() { c -> c != '.' }`

If the parentheses are left empty after this operation, you can remove them:

`originalText.filter { c -> c != '.' }`

Finally, when there is a single parameter in a lambda, there is an opportunity to skip it. The parameter is available under the it name. The final version of the code that removes dots is this:

```kotlin
val originalText = "I don't know... what to say..."
val textWithoutDots = originalText.filter { it != '.' }
```
Pretty impressive, huh?

### Complex lambdas
Sometimes, the code in a lambda isn't short enough to fit in one line, so you need to split the code into lines. In this case, the last line inside the lambda is treated as the lambda return value:

```kotlin
val textWithoutSmallDigits = originalText.filter {
val isNotDigit = !it.isDigit()
val stringRepresentation = it.toString()

    isNotDigit || stringRepresentation.toInt() >= 5
}
```
Also, a lambda can contain earlier returns. They must be written using the qualified return syntax. This means that after the return keyword the @ symbol and the label name are written. The label name is usually the function name where the lambda was passed. Let's rewrite the previous lambda without changing its result:

```kotlin
val textWithoutSmallDigits = originalText.filter {
if (!it.isDigit()) {
return@filter true
}

    it.toString().toInt() >= 5
}
```
### Capturing variables
Now let's discuss the advantage of function creation at runtime. The point is that all the variables and values which are visible where the lambda is created are visible inside the lambda too. If a lambda uses a variable that is declared outside the lambda, then it's said that the lambda captures the variable.

This works intuitively. In case of a captured value, the lambda can just read it. If a variable is captured, the lambda and the outside code can change it, and these changes will be visible in the lambda and in the outside code.

Take a look at the example below:

```kotlin
var count = 0

val changeAndPrint = {
++count
println(count)
}

println(count)    // 0
changeAndPrint()  // 1
count += 10
changeAndPrint()  // 12
println(count)    // 12
```
Here we declare a lambda and assign it to the changeAndPrint variable. The lambda takes the count variable, increments it (increases it by 1), and prints the new value. Take a look at the printed numbers: they may seem okay but it's vital to understand that the count variable is available for changes from inside and outside the lambda and it changes everywhere.

Here is another example.

```kotlin
fun placeArgument(value: Int, f: (Int, Int) -> Int): (Int) -> Int {
return { i -> f(value, i) }
}
```
The placeArgument transforms the f function that takes two arguments to a function that takes a single argument. We achieve it by creating a lambda that takes only one argument and calls the given function with this argument and the given value. Here the lambda captures the value and the f.

Recall the sum function from previous lessons and the mul2 lambda expression from this lesson:

```kotlin
fun sum(a: Int, b: Int): Int = a + b
val mul2 = { a: Int, b: Int -> a * b }
```
We can create other functions using them. Please note that the sum name refers to a function, so we need to receive the object by writing a doubled colon before the name:

```kotlin
val increment = placeArgument(1, ::sum)
val triple = placeArgument(3, mul2)

println(increment(4))   // 5
println(increment(40))  // 41
println(triple(4))      // 12
println(triple(40))     // 120
```