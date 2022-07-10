## Arrays
### Creating an array with specified elements
Kotlin can handle many types of arrays: IntArray, LongArray, DoubleArray, FloatArray, CharArray, ShortArray, ByteArray, BooleanArray. Each array stores elements of the corresponding type (Int, Long, Double, and so on). Note that Kotlin doesn't have default StringArray type. You can store String in the array, but we talk about it on another topic.

To create an array of a specified type, we need to invoke a special function and pass all elements to store them together:

intArrayOf creates IntArray;

charArrayOf creates CharArray;

doubleArrayOf creates DoubleArray;

and so on.
For example, let's create three arrays:

```kotlin
val numbers = intArrayOf(1, 2, 3, 4, 5) // It stores 5 elements of the Int type
println(numbers.joinToString()) // 1, 2, 3, 4, 5

val characters = charArrayOf('K', 't', 'l') // It stores 3 elements of the Char type
println(characters.joinToString()) // K, t, l

val doubles = doubleArrayOf(1.25, 0.17, 0.4) // It stores 3 elements of the Double type
println(doubles.joinToString()) // 1.15, 0.17, 0.4
```
This code snippet above prints three arrays:

```kotlin
1, 2, 3, 4, 5
K, t, l
1.25, 0.17, 0.4
```
The **joinToString()** function converts an array into a string.

When you initialize an array (or anything else) with a sequence of arguments, you can add a trailing comma. It can be useful if you want to add or change some values:

`val array = intArrayOf(1, 2, 3, 4,) // works since Kotlin 1.4`

### Creating an array of a specified size
To create an array of a certain size, we need to write its type and pass it after the type name in round brackets (the constructor):

```kotlin
val numbers = IntArray(5) // an array of 5 integer numbers
println(numbers.joinToString())

val doubles = DoubleArray(7) // an array of 7 doubles
println(doubles.joinToString())
```
These arrays are going to have a predefined size. <u>**They are filled by the default values of the corresponding types (0 for numeric types):**</u>

0, 0, 0, 0, 0
0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0

### Reading array from input
You don't need to figure out all the snippets right now, just use them as a template in your projects!

**To read an array of a certain size from the console**, we first need to create an array of some type with a known size. Inside the parentheses, we should place readln(), with the help of which we can read input from the console. The readln() function returns a string, so don’t forget to convert the input string into the type of the created array.

```kotlin
val numbers = IntArray(5) { readln().toInt() } // on each line single numbers from 1 to 5
println(numbers.joinToString()) // 1, 2, 3, 4, 5
```
This code allows you to read 5 numbers, each number on a separate line.

If you want to read an array in a single line, use the following approach. You can read the array with the readln() function. You’ll get a string, which you should split.

```kotlin
// here we have an input string "1 2 3 4 5"

val numbers = readln().split(" ").map { it.toInt() }.toTypedArray()
println(numbers.joinToString()) // 1, 2, 3, 4, 5
```
Let’s have a look at this code snippet. We read a string from input and then use split(). We divide our string into smaller ones by space, then every element with map function we convert to Int and we return an integer array. Here you can read more about mapping transformation.

There is also a way that allows you to ignore line breaks and extra spaces in the input string. You can do this with the help of regular expressions, which are often used in text searching and editing.
```kotlin
val regex = "\\s+".toRegex()
val str = "1 2\t\t3  4\t5  6"
val nums = str.split(regex).map { it.toInt() }.toTypedArray()
println(nums.joinToString()) // 1, 2, 3, 4, 5, 6

```
With this regular expression, you can ignore spaces and tabs in the input string. You can learn more about regular expressions in our topics.

### Array size
An array always has a size, that is, the number of elements. To obtain it, we need to take the value of the size property. It is a number of the Int type.

```kotlin
val numbers = intArrayOf(1, 2, 3, 4, 5)
println(numbers.size) // 5

```
You cannot change the size of an array after it has been created.

### Accessing elements
You can change the values of array elements. Use the index to set a value in the array.

Setting the value by the index:

```kotlin
array[index] = elem
```
Getting the value by the index:

```kotlin
val elem = array[index]
```
Array indexes are numbers from 0 (the first element) to array.size-1 (the last element).

Here is an example of a three-element array of integers:

```kotlin
val numbers = IntArray(3) // numbers: 0, 0, 0

numbers[0] = 1 // numbers: 1, 0, 0
numbers[1] = 2 // numbers: 1, 2, 0
numbers[2] = numbers[0] + numbers[1] // numbers: 1, 2, 3

println(numbers[0]) // 1, the first element
println(numbers[2]) // 3, the last element
```
Let's take a closer look at the code above. First, we have an array with three elements. By default, all elements are equal to 0. Then, the value 1 is assigned to the first element of the array by the index of 0. Then, the value 2 is assigned to the second element of the array by the index of 1. After that, the value 3 (the sum of 1 and 2) is assigned to the last element of the array by the index of 2. Then, we print the first and the last element of the three-element array.

If we try to access a non-existing element by the index, the program will throw an exception. Let's try to get the fourth element with index of 3 of the considered numbers array:

`val elem = numbers[3]`

The program will throw _ArrayIndexOutOfBoundsException_.

As you may already know, the last element has an index equal to array.size - 1. Let's access the last element and the one before last:

```kotlin
val alphabet = charArrayOf('a', 'b', 'c', 'd')

val last = alphabet[alphabet.size - 1]    // 'd'
val prelast = alphabet[alphabet.size - 2] // 'c'
```
Kotlin provides several convenient ways to access the first and the last elements of an array as well as to access the last index:

```kotlin
println(alphabet.first())   // 'a'
println(alphabet.last())    // 'd'
println(alphabet.lastIndex) // 3
```
Use this approach to make your code more readable and prevent accessing the non-existent indexes.

### Comparing arrays
To compare two arrays, invoke the contentEquals() function of an array and pass another array as the argument. This function returns true when two arrays contain the same elements in the same order. Otherwise, it returns false:

```kotlin
val numbers1 = intArrayOf(1, 2, 3, 4)
val numbers2 = intArrayOf(1, 2, 3, 4)
val numbers3 = intArrayOf(1, 2, 3)

println(numbers1.contentEquals(numbers2)) // true
println(numbers1.contentEquals(numbers3)) // false
```


**Beware, the operators == and != do not compare the contents of arrays, they compare only the variables that point to the same object:**

```kotlin
val simpleArray = intArrayOf(1, 2, 3, 4)
val similarArray = intArrayOf(1, 2, 3, 4)

println(simpleArray == simpleArray)  // true, simpleArray points to the same object
println(simpleArray == similarArray) // false, similarArray points to another 
```

## String Arrays

We can use arrays to process a bunch of data of the same type, and String is no exception. In this topic, we will take a close look at string arrays and cover the essential basics from initializing an array to changing its contents. Note, you can do the same with any other types.

### Initialization
Kotlin does not provide a special function to create an array of strings, so you need to use the function arrayOf which is already familiar to you. Take a look:

```kotlin
val stringArray = arrayOf("array", "of", "strings")
```
Basically, the function arrayOf is universal: you can use it to collect any type of data in an array, including String.

If you want to achieve strict behavior by explicitly defining the type, you can do it like this:

```kotlin
val stringArray = arrayOf<String>("array", "of", "strings")
```
You can also initialize an empty array to be filled with strings later. For this, you need the function emptyArray:

```kotlin
val newEmptyArray = emptyArray<String>()
```
Note that if you choose to use this function, you have to define the type of elements.

The same actually applies to other types: an array created with emptyArray() can be filled with elements of any type that is defined.
### Accessing elements
Since everything you already know about arrays still applies, you can access a certain element by its index:
```kotlin
val stringArray = arrayOf("sagacity", "and", "bravery")
print(stringArray[2])   // bravery

```
Remember that indexing starts with 0
0
. The index of the last element will equal length - 1
length−1
.

You can set an array element in the same way:

```kotlin
stringArray[0] = "smart"
print(stringArray[0])   // smart
```
### Outputting an array
Now you know how to create and fill a string array and access its elements.

To see what we get as a result, and, for example, print a string array, use the familiar function joinToString():

```kotlin
val southernCross = arrayOf("Acrux", "Gacrux", "Imai", "Mimosa")
println(southernCross.joinToString())   //  Acrux, Gacrux, Imai, Mimosa
```
Keep it in mind that joinToString() processes a single string of elements listed orderly and separated by a comma.

### Working with multiple arrays
Let’s look at a couple of things you might want to know about working with several string arrays.

String arrays can be added
You can concatenate several arrays as shown in the following example:

```kotlin
val southernCross = arrayOf("Acrux", "Gacrux", "Imai", "Mimosa")
val stars = arrayOf("Ginan", "Mu Crucis")

val newArray = southernCross + stars
println(newArray.joinToString())    //  Acrux, Gacrux, Imai, Mimosa, Ginan, Mu Crucis
```
### String arrays can be compared
**You cannot use the operators == and != to compare arrays because they simply do not compare their contents**. With arrays, use the function contentEquals() instead:

```kotlin
val firstArray = arrayOf("result", "is", "true")
val secondArray = arrayOf("result", "is", "true")
val thirdArray = arrayOf("result")

println(firstArray.contentEquals(secondArray))  //  true
println(firstArray.contentEquals(thirdArray))   //  false
```
**Note that it returns true only if the elements of the two arrays match completely and are arranged in the same order**.

### Changing the array contents
No matter if you're using val or var, you can edit the values of the existing elements through their index:

```kotlin
val southernCross = arrayOf("Acrux", "Gacrux", "Imai", "Mimosa")
var stars = arrayOf("Ginan", "Mu Crucis")
southernCross[1] = "star"
stars[1] = "star"

println(southernCross[1]) // star
println(stars[1]) // star
```
However, there is a great difference between val and var when it comes to reassignment. When you have a var array, you can change it by adding new elements to it.

Suppose we created an empty string array:

`var southernCross = emptyArray<String>()`

What good is an empty array? Let's fill it:

```kotlin
southernCross += "Acrux"
southernCross += "Gacrux"
southernCross += "Imai"
println(southernCross.joinToString())   // Acrux, Gacrux, Imai
```
Even if your array is not empty to begin with, you can still add elements in the same way:

```kotlin
var southernCross = arrayOf("Acrux", "Gacrux", "Imai")
southernCross += "Mimosa"
println(southernCross.joinToString())  //  Acrux, Gacrux, Imai, Mimosa
```
Just like that, we’ve changed the content of the array by adding new elements. However, there’s one very important point to be made.

In Kotlin, the arrays are in a way unchangeable. Even if the array is declared with var, it cannot really be edited. In both examples, the array southernCross was actually re-created. In fact, we literally deleted the array and created another one instead.

So, we can add new elements if the array is declared as var. If you're using val, that is not possible:

```kotlin
val southernCross = arrayOf("Acrux", "Gacrux", "Imai", "Mimosa")
southernCross += "Ginan" // will not compile
```
### Conclusion
Let's sum things up! You figured out how to use some familiar functions and techniques for working with string arrays.

You can:

+ Use arrayOf() for initialization;
+ Access strings in the array through their indexes;
+ Use joinToString() to create a single string from the array and output it;
+ Use contentEquals() to compare two arrays;
+ Add new elements to an array; keep in mind that you actually create it again instead of merely editing it.