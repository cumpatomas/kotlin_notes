To structure the data you store on a disk, files can be organized into directories. A parent directory can include other directories, that is, subdirectories, which results in a hierarchy of files. For example, consider the filesystem hierarchy in Linux: the root directory / includes all other files and directories, even if they are stored on different physical devices. Note that we omit null safety for brevity.

### Files and Directories
A file is a named data area on a storage medium used as the basic object of interaction with data in operating systems.

A directory is an entity in a file system that simplifies the organization of files. A typical file system contains a large number of files, and directories help organize it by grouping them together. You can also work with them as File entities.

Kotlin offers a number of methods to operate with directories and files. You may, for example, use the java.io library.

Let's consider the most popular of these methods:

- File.isDirectory checks if the File is a directory.
- File.isFile checks if the File is a file.
- File.exists() checks if the file exists.
- File.resolve(String) returns a file named String in a directory.
- File.resolve(File) returns a file File in a directory.
- File.createNewFile() creates a new file.
- File.mkdir() creates a directory.
- 
Here's an example:
```kotlin

val outDir = File("outDir")
println(outDir.exists())    // false
outDir.mkdir()
println(outDir.exists())    // true
println(outDir.isDirectory) // true

val file = outDir.resolve("newFile.txt") // outDir/newFile.txt
println(file.exists())      // false
file.createNewFile()        
println(file.exists())      // true
println(file.isFile)        // true
```
### Methods for Iterating through file hierarchies
You can iterate through file hierarchy with the java.io.File methods:

- File.getParentFile() returns an instance of java.io.File representing the parent directory of a file or null if this file does not have a parent (which means it is the root).
- File.getParent() returns a string representation of the parent directory of a file or null if this file does not have a parent.
- File.listFiles() returns an array of files located in a given directory or null if this instance is not a directory.
- File.list() returns an array of files and directories in the directory defined by this abstract pathname.
- 
_**kotlin.io**_ provides special methods that allow you to iterate through the entire file hierarchy. Let's look at three basic methods:

- File.walk(direction): FileTreeWalk provides the directories and files we can visit in this directory; we need to specify how exactly we will iterate (up or down the hierarchy structure);
- File.walkBottomUp(): FileTreeWalk provides the directories and files we can visit in this directory. It uses depth-first search, and directories are visited after all their files;
- File.walkTopDown(): FileTreeWalk provides the directories and files we can visit in this directory. We use a depth-first search, and the directories are visited before all their files.
- The FileTreeWalk class is used to iterate through a given file system. It allows you to iterate through all files within a given directory. The iterator method returns an iterator traversing the files. You may iterate over this structure or convert it to a list with the toList() function.

In the following section, we'll find out how to apply these methods when dealing with a simple hierarchy.

### A hierarchy example
Let's consider a file hierarchy with the root directory named Files. It contains two subdirectories: CompletedProjects and Music. They also have subdirectories: the HelloWorld directory contains two files related to the project, the JCalculator directory contains only one file, and theSoundtracks directory is empty.

Take a look at the illustration of this hierarchy below:

https://ucarecdn.com/9aa61bd1-2d05-40a2-8284-f9d2e2992a2f/

In the following paragraphs, we will use this file hierarchy in examples that illustrate methods of working with the file system.

### Getting directory contents and directory's/file's parent
listFiles() prints the contents (files and directories) of a chosen directory.

```kotlin
val helloWorld = File("/Files/CompletedProjects/HelloWorld")
val helloWorldFilesNames = helloWorld.listFiles().map{it.name} // [Doc.pdf, Reviews.txt]

val reviews = File("/Files/CompletedProjects/HelloWorld/Reviews.txt")
val reviewsFiles = reviews.listFiles() // null

val soundtracks = File("/Files/Music/SoundTracks")
val soundtracksFiles = soundtracks.listFiles() // []
```

The reviewsFiles is null because reviews is not a directory at all and cannot include other files or subdirectories. Meanwhile, soundtracks is a directory without files, so the result will be an empty array.

The File.parent property returns the name of a file's or directory's parent.

TheFile.parentFile property returns a file's or directory's parent as File.
```kotlin
val files = File("/Files")
print(files.parent) // the result is "/"
print(files.parentFile.name) // the result is ""

val reviews = File("/Files/CompletedProjects/HelloWorld/Reviews.txt")
print(reviews.parent) // the result is "/Files/CompletedProjects/HelloWorld"
print(reviews.parentFile.name) // the result is "HelloWorld"

val root = File("/")
print(root.parent) // the result is "null"
print(root.parentFile.name) // throws java.lang.NullPointerException

```
As you can see, if a directory is the root of a file hierarchy, you will get null. Be careful to not get exceptions!

### Iterating in both directions
We have already mentioned the File.walk(direction) method for iterating through file hierarchy. The attribute direction describes the way in which we can traverse our file hierarchy; it can take two values: FileWalkDirection.BOTTOM_UP and FileWalkDirection.TOP_DOWN.
```kotlin
val files: File = File("/Files")
println("TOP_DOWN: ")
files.walk(FileWalkDirection.TOP_DOWN).forEach { println(it) }

println("BOTTOM_UP: ")
files.walk(FileWalkDirection.BOTTOM_UP).forEach { println(it) }

```
The results of this program will be the following:

TOP_DOWN:\
/Files\
/Files/Music\
/Files/Music/SoundTracks\
/Files/CompletedProjects\
/Files/CompletedProjects/HelloWorld\
/Files/CompletedProjects/HelloWorld/Doc.pdf\
/Files/CompletedProjects/HelloWorld/Reviews.txt\
/Files/CompletedProjects/JCalculator\
/Files/CompletedProjects/JCalculator/Doc.pdf\
BOTTOM_UP:\
/Files/Music/SoundTracks\
/Files/Music\
/Files/CompletedProjects/HelloWorld/Doc.pdf\
/Files/CompletedProjects/HelloWorld/Reviews.txt\
/Files/CompletedProjects/HelloWorld\
/Files/CompletedProjects/JCalculator/Doc.pdf\
/Files/CompletedProjects/JCalculator\
/Files/CompletedProjects\
/Files\

You can get the same result with the following methods:

File.walkBottomUp() (analogous to File.walk(FileWalkDirection.BOTTOM_UP));\
File.walkTopDown() (analogous to File.walk(FileWalkDirection.TOP_DOWN)).

Thus, these three methods allow us to recursively traverse the entire file structure.

### Working with hierarchies
Let's suppose we have an instance of java.io.File named completedProjects, which corresponds to the CompletedProjects directory. Let's now get both of its subdirectories containing project data.

```kotlin
val completedProjects: File = File("/Files/CompletedProjects")

val projects = completedProjects.walk()
projects.maxDepth(1) // HelloWorld and JCalculator
```
The particular order of files in the array is not guaranteed. To find the HelloWorld project, we will iterate through the file tree:

`val helloWorldProject: File = projects.first { it.name == "HelloWorld" }`

Another way to get the directory HelloWorld is to use the File.listFiles() method:

`val helloWorldProject: File = completedProjects.listFiles().first { it.name == "HelloWorld" }`

We assume that it is not null just to simplify our code for education purposes. Still, it is better to check because the actual hierarchy may be changed.

Now let's try to go to the directory Soundtracks from the Reviews.txt file:

```kotlin
val reviews = File("/Files/CompletedProjects/HelloWorld/Reviews.txt")
var parent = reviews.parentFile
while (parent.name != "Files"){
parent = parent.parentFile
}

val soundTracks: File = parent.walkTopDown().first { it.name == "SoundTracks" }
```
### File copying
If you need to copy a file, you may use function copyTo():

```kotlin
val fileIn = File("newFile.txt")
val fileOut = File("copyNewFile")
fileIn.copyTo(fileOut)
```
Note, that if you need to overwrite the file, you need to add a parameter overwrite:

```kotlin
val fileIn = File("newFile.txt")
val fileOut = File("copyNewFile")
fileIn.copyTo(fileOut, overwrite = true)
```
If you need to copy recursively the whole directory, you may use function copyRecursively():

```kotlin
val fileIn = File("outDir")
val fileOut = File("newDir")
fileIn.copyRecursively(fileOut)
```
Note that if you need to overwrite folders and files, you also need to add a parameter overwrite:

```kotlin
val fileIn = File("outDir")
val fileOut = File("newDir")
fileIn.copyRecursively(fileOut, overwrite = true)
```