There are many different ways to read files in Kotlin, but let's focus on just a few of them for now. The core class for
file processing is File from the java.io package. Kotlin provides additional functionality over Java implementation 
through Extension Functions. This means that by just importing **java.io.File** into your project, you can use 
both Java and Kotlin File implementations. Let's take a closer look at these extension functions.

### readText
First, there is **readText()** which helps you read the whole file in just one String variable.

Say we create a text file reading.txt with the following text:

```kotlin
Kotlin or Java,
Java or C++.
```
We put it in src folder, and now we can read it with our readText() method:

```kotlin
val fileName = "src/reading.txt"
val lines = File(fileName).readText()
print(lines)
```
As, you may have already guessed, the output is this:

```kotlin
Kotlin or Java,
Java or C++.
```
Instead of reading our file right away to a String variable, we can create a reference first and then read the text later:

```kotlin
val fileName = "src/new.txt"
val file = File(fileName)
val lines = file.readText()
print(lines)
```
As you see, we made a mistake and tried to read a different file – new.txt. If there is no such file in the filesystem, then NoSuchFileException is thrown. You can catch and process it as a usual Kotlin's exception, it's totally up to you. Now we can fix our example and continue:

```kotlin
val fileName = "src/reading.txt"
val file = File(fileName)
if (file.exists()) { // checks if file exists
val lines = file.readText()
print(lines)
} else print("No such file")
```
Actually, our file variable didn't open the file, it just provided a reference with the path to it. What's more, readText() automatically opened and closed the file!

We can also provide specific charset for reading the file:

``val line = File(fileName).readText(Charsets.US_ASCII)``

Just keep it in mind that by default readText() has UTF-8 charset encoding, but you can change it whenever you want. Also, Kotlin doesn't recommend using this function with files larger than 2 Gb, because this may cause OutOfMemoryError.

We can also check the existence of a file with exists() method, which will return false in case of a missing file and true if Kotlin found it. Actually, you can use any method from File, for example, length() or delete().

###readLines
Function **readLines()** provides the functionality of reading files for each line and store it in List:

```kotlin
val fileName = "src/reading.txt"
val lines = File(fileName).readLines()
for (line in lines){
println(line)
}
```
The output will be the following:

```kotlin
Kotlin or Java,
Java or C++.
```
This method has the same size limitations and charset specification as readText().

### readBytes
Function **readBytes()** may be helpful if you need to store file contents as an array of bytes:

`val lines = File(fileName).readBytes()`

Note, this function returns the ByteArray. The Array structure is similar to the MutableList, you cannot resize it, but you can modify elements. You can easily convert MutableList to ByteArray and vice versa with toByteArray() and toMutableList() functions.

It's still not recommended using this option with large files (2 Gb or more). This method is used as an implementation of readText() function with conversion to String in Kotlin source files.

### forEachLine
forEachLine() is the recommended way of reading large files:

```kotlin
val fileName = "src/reading.txt"
File(fileName).forEachLine { println(it) }
```
This lambda reading approach provides an action (println() in our case) for each line.

It's always a possibility that the file you're going to read has already been opened in another process, or it might have access restrictions. In such cases, AccessDeniedException is thrown.