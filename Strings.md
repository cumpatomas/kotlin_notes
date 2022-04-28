##__WORKING WITH STRINGS__

The elements of a string are individual characters that can be accessed by their index. The first element of a string has an index of 0.

````kotlin
val greeting = "Hello"
val firset = greeting[0] // 'H'
val second = greeting[1] // 'e'
val five = greeting[4] //'o'

````
The las element has the index that equals the length of the string minus 1. For the string "Hello", the las element is 'o'. Its index is 4 because the string's length is 5.
```kotlin
val last = greeting[greeting.length -1] // 'o'
val prelast = greeting[greeting.length -2] // '1'
```
kotlin provides several convenient ways to access the first and the last character of a string:
````kotlin
println(greeting.first()) // H
println(greeting.last()) // 0
println(greeting.lastIndex) // 4
````
You can use this feature to write more readable code.

###Empty String
sometimes you need a fast way to ceck for existence. Of course, you can use the length and ceck if it's greater than 0. But a much more elegant wau is using the function **isEmpty()**
```kotlin
val emptyString = ""
println(emptyString.length == 0) // true
println(emptyString.isEmpty()) // true
```
###Immutability
Strings are immutale, meaning that once created, the string stays the same. You cannot modify an element of a string. So, the example below <u>would not work</u>:
```kotlin
val valString = "string"
valSring[3] = 'o' // an error here!
var varString = "string"
varString[3] = 'o' // an error here too!!
```
If you need to change the string, you can reasign it:
```kotlin
var varString = "string"
varString = "strong" // legal
val valString = "string"
valString = "strong" // error, you cannot reassign a val!
```
Actually, we do not modify the stored value in the varString variable. Instead, we assign a new value to it. So, it is absolutely legal. This is one of the ways to work with strings. If you need to modify e string, just create a new one.

###Comparing strings
To compare two strings, use == (equal) and != (not equal) operators. Both operators take two strings as their operands and returns a value of the Boolean type (true or false).
```kotlin
val first = "first"
val second = "second"
var str = "first"

println(first == str) // true
println(first == second) // false
println(first != second) // true
```

### Filter Method
The String type has the filter method to filter symbols. How does it know which symbols to remove from the string and which ones to leave in it? The answer is simple: this method takes a predicate as an argument and then uses it for internal computations. <u>A **predicate** is a function that takes an argument and returns a Boolean result</u>. So in the filter method, the predicate says if the symbol should be left and has the **_(Char) -> Boolean_** type.

Let's try to use this method. If we want to remove dots from a string, we declare this predicate:

```kotlin
fun isNotDot(c: Char): Boolean = c != '.'
// Then we can do something like this:

val originalText = "I don't know... what to say..."
val textWithoutDots = originalText.filter(::isNotDot)
```
As a result, the **textWithoutDots** string is equal to _"I don't know what to say"_

Rewrite it to pass a lambda:

```.kotlin
val originalText = "I don't know... what to say..."
val textWithoutDots = originalText.filter({ c: Char -> c != '.' })
```
It works! First of all, Kotlin infers types of many objects, and here specifying the c type isn't necessary:

```kotlin
originalText.filter({ c -> c != '.' })
```
Second, there are situations when the lambda is passed as the last argument. This is the case. Kotlin provides a way to eliminate these bracket sequences ({ }), allowing to write the lambda outside the parentheses:

```kotlin
originalText.filter() { c -> c != '.' }
```
If the parentheses are left empty after this operation, you can remove them:

```kotlin
originalText.filter { c -> c != '.' }
```
Finally, when there is a single parameter in a lambda, there is an opportunity to skip it. The parameter is available under the it name. The final version of the code that removes dots is this:

```kotlin
val originalText = "I don't know... what to say..."
val textWithoutDots = originalText.filter { it != '.' }

```
### Count
By default, **.count()** returns the total
number of characters in a string. But count also has a variation that accepts a function as a parameter. This
function has a single Char parameter and returns a Boolean. This version of the count function calls the
provided function on every character in the String and returns the total number of true results returned
```kotlin
"Mississippi".count({ letter -> letter == 's' })
```
When defining lambdas that accept exactly one argument, the **_it_** identifier is available as a convenient alternative to
specifying the parameter name. Both **it** and a named parameter are valid when you have a lambda that has only <u>one
parameter</u>.

_**it**_ is convenient in that it requires no variable naming, but it is not very descriptive about the data it represents. We
suggest that when you are working with more complex lambda expressions, or with nested lambdas (lambdas
within lambdas), you should name the parameter.
On the other hand, **it** is great for shorter expressions. For example, it would allow the **count()** function call you
saw earlier, to count the s’s in “Mississippi,” to be written more concisely, like this:
```kotlin
"Mississippi".count({ it == 's' })
```
Because of the simplicity of this example, this logic is clear even without an argument name.

### CompareTo
Compares this object with the specified object for string alphabetical order. Returns zero if this object is equal to the specified other object, a negative number if it's less than other, or a positive number if it's greater than other.
```kotlin
fun main() {
println("a".compareTo("z")) // -25
println("a".compareTo("a")) // 0
println("a".compareTo("c")) // -2
println("ace".compareTo("car")) // -2
println("cars".compareTo("car")) // 1
}
```
### get
Returns the character of this string at the specified index.
```kotlin
println("missisippi".get(4)) // i
```

### plus
```kotlin
println("a".plus('f')) // af
println("a".plus(200)) // a200
println("a".plus("lone")) // alone
```
### subSequence
Returns a new character sequence that is a subsequence of this character sequence, starting at the specified startIndex and ending right before the specified endIndex.
```kotlin
println("alone".subSequence(1, 3)) // lo ---- (we must input 2 int for start index and last one)
println("alone".substring(1, 3)) // lo
println("alone".substring(1)) // lone ------(we can omit the last index and it takes the last one as default)
```



