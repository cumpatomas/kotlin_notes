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

