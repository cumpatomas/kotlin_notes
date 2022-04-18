## Functional Decomposition

As your programming task are becoming more complex,  so are your functions. Though you can create a complex program that is wrapped in one solid function or even in **main** function, 
it is better to divide a program into more specific parts that are easy to read and understand. The approach of dividing a complex program
into a number of function is called **_functional decomposition_**.

In this topic, we'll see how to decompose the solution of a particular problem into separate functions. 

Let's consider an example. Think of a program that simulates the Smart Home app. This app is used to control home devices remotely. 
Imagine we have an app that can perform three actions: turn the music on or off, switch the light on or off and control the door lock. Let's consider these actions as parts of our computer program.

The general algorithm of operating the Smart Home app can be broken down into the following steps:

1. Parse the input data (password)
2. Check the password is correct
3. Ask the user what they want to do 
4. If the action is supported, perform it

Imagine that you wrapped the program in code, but without a single additional function. That's how its structure would look like:
```kotlin
fun main ()

val password = "76543210"
var SpeakersState: Stringvar 
var lampState: String
var doorState: String
//...

//reading the password
println("Enter password: ")
val passwordInput = readln()

//Checking password
if (passwordInput != password) {
    println("Incorrect password")
} else {
    // asking user what they want to do
    println("Choose the object: 1 – speakers, 2 – lamp, 3 – door")
    val action = readln()

    when (action) {
        "1" -> {
            // asking the user about the speakers
            when (speakersState) {
                "on" -> {
                    // ...
                }
                "off" -> {
                    // ...
                }
                else -> {
                    // ...
                }
            }
        }
        "2" -> {
            // asking the user about the lights...
        }
        "3" -> {
            // asking the user about the door...
        }
        else -> {
            // ...
        }
    }
}

```
Though you see a truncated version of a real program, the code still looks overloaded. At the same time, it works perfectly fine for our problem and we could leave it like that. However, we might want to adjust it for our needs or extend its functionality later.

What if we want this code to work for multiple users? Or to expand the number of actions and make them more complex? Some parts of the code would still be used, and some of them would probably be deleted. To make this code less specific and more flexible, we can use functional decomposition.

###Decomposing a program into functions
Functional decomposition is the process of decomposing a problem into several functions. Each function does a particular task which we can perform in a row to get the results we need. Considering a problem, we need to identify the actions that will be repeated multiple times or, alternatively, performed separately. This is how we get the desired functions that are easier to read, understand, reuse, test, and debug.

Let's look at our Smart home app again and figure out which steps can be turned into separate functions. First of all, we can separate the user actions and create the corresponding functions: one to control the music, another one to turn the lights on and off, and the third one to operate the door lock.

Take a look at the function controlMusic() that controls the music. Functions controlLight() and controlDoor() follow the same algorithm.
```kotlin
// turns the music on and off
fun controlMusic() {
    println("on/off?")
    val tumbler = readln()
    when (tumbler) {
        "on" -> println("The music is on")
        "off" -> println("The music is off")
        else -> println("Invalid operation")
    }
}
```
These controlling functions perform the main actions that our app provides. The actions are greatly simplified and only used to illustrate the functionality revision process.

Another function that can be separated is the password checker:
```kotlin
// verifies the password and gives the access to Smart home actions if the password is correct
fun accessSmartHome() {
    val password = "76543210"
    print("Enter password: ")
    val passwordInput = readln()
    if (passwordInput == password)
        chooseAction()
    else
        println("Incorrect password!")
}
```
We also created a function chooseAction() with the menu where the user can choose the action. This function asks the user what action they want to perform and gives control to the corresponding function.

Finally, we can run our decomposed program in the main function, which is called once our program is started:
```kotlin
fun main() {
    accessSmartHome()
}
```fun transform(color: String): Int { // you can miss one of the returns
    when (color) {
        "Red" -> return 0
        "Green" -> return 1
        "Blue" -> return 2
        else -> return -1
    }

    fun transform(color: String): Int { // you can accidentally change the variable `colorNumber` 
        var colorNumber = -1
        when (color) {
            "Red" -> colorNumber = 0
            "Green" -> colorNumber = 1
            "Blue" -> colorNumber = 2
        }
        return colorNumber
    }

    fun transform(color: String): Int { // nice and concise code
        return when (color) {
            "Red" -> 0
            "Green" -> 1
            "Blue" -> 2
            else -> -1
        }
    }

This function calls **_accessSmartHome_**, which asks the user to enter a password and, if it o¡is correct, allows them to manage the Smart Home.

### Adding new features
Now, if we want to add another action, all we have to do is define the corresponding function.\
For example, we've got a new Smart Device - an electric kettle. We create a function that switches on and off. To get access to the new function, 
we need to modify the _chooseAction()_ funcion by adding a new available action value:
```kotlin
// controls electric kettle
fun controlKettle() {
    // ...
}

// main menu for choosing the action
fun chooseAction() {
    // ...

    // adding new action 4
    println("Choose the object: 1 – speakers, 2 – lamp, 3 – door, 4 – kettle")
    // ...
        "4" -> controlKettle()
    // ...
}
```

As you can see, we now have a real functioning program that won't fall apart if we decide to change it a bit. We can easily test separate components since they are defined in separate functions. This also makes it easier to support the program in the future.

###Idiom
You already know that _if_ and _when_ can be expressions. So one obvious way of simplifying your code is to use their expression forms. We suggest you use this form in simple functions:
```kotlin
fun transform(color: String): Int { // you can miss one of the returns
    when (color) {
        "Red" -> return 0
        "Green" -> return 1
        "Blue" -> return 2
        else -> return -1
    }

    fun transform(color: String): Int { // you can accidentally change the variable `colorNumber` 
        var colorNumber = -1
        when (color) {
            "Red" -> colorNumber = 0
            "Green" -> colorNumber = 1
            "Blue" -> colorNumber = 2
        }
        return colorNumber
    }

    fun transform(color: String): Int { // nice and concise code
        return when (color) {
            "Red" -> 0
            "Green" -> 1
            "Blue" -> 2
            else -> -1
        }
    }
```
Also, you can use this idiom in single-expression functions:
```kotlin
fun transform(color: String) = when (color) {
    "Red" -> 0
    "Green" -> 1
    "Blue" -> 2
    else -> -1    
}
```
there's also a short form of _if_ expressions. Try writing functions this way:
```kotlin
fun max(a:Int, b: Int) = if (a > b) a else b
```
As you can see, when expressions keep things clear and help you to not lose your branches. Try making use of this idiom when you write code.
