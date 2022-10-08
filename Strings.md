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

// or even more simple! :)

"Mississippi".count { it == 's' }
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
### Replace

Returns a new string with all occurrences of oldChar replaced with newChar.
```kotlin
val inputString0 = "Mississippi"

println(inputString0.replace('s', 'z')) // Mizzizzippi
```
If we want to replace multiple symbols from an Array (or erase them) we can use the Regex like this:

`.replace("""[\[\],]""".toRegex(), "")`

This will erase the [, ], and commas symbols: 
```kotlin
val list = arrayOf(1, 2, 3, 4, 5, 6, 7, 8, 9)
val pairs = list.filter { it % 2 == 0 }
println("pair numbers: " + pairs)
println("pair numbers: " + pairs.toString().replace("""[\[\],]""".toRegex(), ""))

// pair numbers: [2, 4, 6, 8]
// pair numbers: 2 4 6 8
```
Otherwise we should have used the **replace()** function 3 times.

## toString()

Sometimes we need to submit information as a string, for example, to a console for debugging. How can we represent non-text objects as a string so we can output them in a readable way? This is where we use the toString() function.

### Introduction
Let’s say we have three boxes each filled with a different kind of berry: raspberry, strawberry, and blueberry. We need information about the weight of each of these boxes. Let’s print it:
```kotlin
val raspberryWeight = 10
val strawberryWeight = 15
val blueberryWeight = 20

println(raspberryWeight) //10
println(strawberryWeight) //15
println(blueberryWeight) //20

```
This seems fine. Now let’s create a BerryHolder class that will store the weight of the boxes. Let's try to print these values again:
```kotlin
class BerryHolder(val weight: Int)

val raspberryWeight = BerryHolder(10)
val strawberryWeight = BerryHolder(15)
val blueberryWeight = BerryHolder(20)

println(raspberryWeight) // BerryHolder@6f496d9f
println(strawberryWeight) // BerryHolder@723279cf
println(blueberryWeight) // BerryHolder@10f87f48

```
Well, this certainly doesn’t look like the result we want to see.

Why did this happen? To figure this out, we need to really understand how fun println(message: Any?) works. If we look at the signature of println(), we'll see that it receives a message of the type Any?. Keep in mind that in Kotlin, Any? is a superclass for any class, both standard and customized. So, println() must accept an object of any type and return text, that is, something of the String type.

If we need to manage the behavior of a function for objects of completely different types, we have to convert the object for printing to the String type before the output. println() implicitly calls the toString() function, which converts message to a string.

The toString() function exists specifically to represent objects as strings. So, why does it work so differently with different types?

### Default behavior
The toString() function is defined for the type Any?. This means that all the classes inherit all of the Any? methods, including toString().

The point is that toString() for Any? returns the class name and memory location as a string. For some classes, the default behavior is adjusted for correct processing. For example, take Int or Double:

```kotlin
val nonString = 1.0

println(nonString.toString())   // 1.0
println(nonString)  // 1.0

/* The output is the same: println just implicitly called toString() for Double object */


```
However, for most classes, by default, toString() still returns the name of the class and the address where the object is located in the memory. Usually, we want to get text information about objects in another way, so it makes sense to override toString() for our data type.

### Overriding toString()
It seems that our problems can be solved by redefining the toString() method. toString() is automatically defined for all the classes you create, and you can override it for any class. This can be done the same way as for any other function. Let’s take our BerryHolder class as an example:
````kotlin
class BerryHolder(val weight: Int) {
override fun toString(): String {
return weight.toString()
}
}
println(BerryHolder(10)) // 10

````
Success! This time, printing objects of our class goes as intended.

Let’s take a look at a more complex example. Say, we’re developing an electronic library. First, we have a class User that contains the user’s ID and their login information. We want to be able to output the information about the objects of this class as a String so that we can see the full information with brief explanations. Something like this: User{id=id_value, login=login_value, email=email_value}.

Let’s override the toString() function for our User class:
```kotlin
class User(val id: Int, val login: String, val email: String) {
override fun toString(): String {
return "User{id=$id, login=$login, email=$email}"
}
}

val user = User(1, "uncle_bob", "rmartin@objectmentor.com")
println(user) // User{id=1, login=uncle_bob, email=rmartin@objectmentor.com}

```
The output is adapted for our purposes in a readable way, and there is no memory addressing. Great!

### Overriding toString(): Inheritance
Another reason for the overriding the toString() method is working with superclasses, or parent classes. Here, the general rules of inheritance continue to apply. If the toString() method is defined in the parent class, the derived class will use this particular override.

Let’s go back to our example. The database of the electronic library may contain data not only about the users but also about the authors. Let’s extend the class User with the class Author, which will contain a list of publications (books):

```kotlin
open class User(val id: Int, val login: String, val email: String) {
override fun toString(): String {
return "User{id=$id, login=$login, email=$email}"
}
}

class Author(id: Int, login: String, email: String, val books: String): User(id, login, email) {

}

val user = User(1, "marys01", "mary0101@gmail.com")
val author = Author(2, "srafael", "rsabatini@gmail.com", "Captain Blood: His Odyssey")

println(user)   // User{id=1, login=marys01, email=mary0101@gmail.com}
println(author) // User{id=2, login=srafael, email=rsabatini@gmail.com}
```
The toString() method is not defined for the class Author. It may seem that the function will work by default. However, since Author is inherited from the parent class User, the override for the parent class will be used.

Now, if we modify the class Author and add a specific override of the toString() method, the following override will happen:

```kotlin
class Author(id: Int, login: String, email: String, val books: String): User(id, login, email) {
override fun toString(): String {
return "Author{id=$id, login=$login, email=$email}, books: $books"
}
}

val user = User(1, "marys01", "mary0101@gmail.com")
val author = Author(2, "ohwilde", "wilde1854@mail.ie", "Someone’s portrait")

println(user)   // User{id=1, login=marys01, email=mary0101@gmail.com}
println(author) // Author{id=2, login=ohwilde, email=wilde1854@mail.ie}, books: Someone’s portrait
```
### Using the superclass definition
It may be necessary to invoke the toString() parent implementation in the child class. It can be done with super, as you remember from inheritance:

```kotlin
class Author(id: Int, login: String, email: String, val books: String): User(id, login, email) {
override fun toString(): String {
return "Author: ${super.toString()};\nBooks: $books"
}
}
```
Here, we used the toString() method of the superclass and complemented it for the derived class.

Let’s see how it works. Insert some values of the Author class and output them:

```kotlin
val author1 = Author(1, "uncle_bob",
"rmartin@objectmentor.com",
"\n1.\"Clean Code: A Handbook of Agile Software Craftsmanship\" \n2.\"Agile Software Development: Principles, Patterns and Practices\"")
val author2 = Author(2, "ltlst",
"leotolstoy@mail.com",
"\n1.\"Anna Karenina\" \n2.\"The Death of Ivan Ilyich\" \n3.\"War and Peace\"")

println(author1)
println()
println(author2)
```
Now, let’s see what will be displayed as a result of our program:

```kotlin
/*  Author: User{id=1, login=uncle_bob, email=rmartin@objectmentor.com};
Books:
1."Clean Code: A Handbook of Agile Software Craftsmanship"
2."Agile Software Development: Principles, Patterns and Practices"

    Author: User{id=2, login=ltlst, email=leotolstoy@mail.com};
    Books: 
    1."Anna Karenina" 
    2."The Death of Ivan Ilyich" 
    3."War and Peace"
*/
```

As you can see, we used the definition of the toString() function for the parent class User, adding it to the class Author. The result was an override of toString() for the class Author using the override for User.

### Remove accents in strings

in GRADLE
implementation  'org.apache.commons:commons-lang3:3.12.0'

in file.Kt
import org.apache.commons.lang3.StringUtils

Use in CODE:
StringUtils.stripAccents("String to unaccent")
