## Types
One of the most important parts of Kotlin is the type system, a mechanism for detecting and preventing illegal program States. It imposes structure on our program. Without structure, programs are wildly complex, ready to "do damage" at the slightest mistake of the programmer. With the help of types, we can describe and give meaning to the relationships between components in our program, making it concise and readable.

### Subtype and supertype
Types in Kotlin are organized into a hierarchy of subtype-supertype relationships. So what are the subtype and supertype? Let's find it out by looking at an example.

Do you like coffee or tea? Well, both are drinks. We can state the fact that coffee and tea are related to a specific type: drinks. In other words, coffee and tea are subtypes of drink, while drink is the supertype for these two types

So, the subtype is a datatype that is related to another datatype (supertype) and shares common characteristics and rules of behavior with it. Note that the rules of behavior of different subtypes may vary, just like all kinds of drinks have some sort of color, but it's different for coffee and tea.

Logically, supertype is a type that specifies the characteristics and rules of behavior that every subtype will follow.

For example, Number is a supertype for all types that represent numeric value; Int and Double are subtypes of Number type.

### Root type Any
In the previous topics, you got familiar with the concept of nullable and non-nullable types. Now it's time to look deeper and understand what represents these types in Kotlin.

In Kotlin, type Any is a supertype for all types that don't support null. This means that any non-nullable type is a subtype of Any. For example, you can assign non-null String to Any type:

`val message: Any = "Important message"
`However, you cannot assign a null value to the Any type:

`val message: Any = null  // Error: Null can not be a value of a non-null type Any`
Type Any is also a supertype for primitives such as Boolean:

`val isNull: Any = false`
Type Any is at the top of the Kotlin type hierarchy for types that cannot be null. For example, the type Number is a subtype of type Any

Note that type Any does not support null. When we talk about a type as a subtype of Any, we can be sure that when we try to access this type, we will not get a NullPointerException. In other words, Kotlin guarantees that the subtype of type Any can never be null, which means that null checks become useless when we're dealing with type Any.

Let's consider an example:

```kotlin
fun stringify(any: Any) {
any?.toString()  // '?' can be omitted
any!!.toString() // '!!' can be omitted
}
```
### Root type Any?
If you want a variable to store any value including null, use type Any?.

As you can see, to access a nullable variable you need to use a special suffix '?', otherwise, an error will occur. Also, the suffix '?' is used when declaring a variable that can be null. Note that you can't assign null to non-null variables. Let's see an example:

```kotlin
val number1: Number = null // Error: Null can not be a value of a non-null type Number
val number2: Number? = null // OK
```
While type Any is a supertype for all types that do not support null, Any? is a supertype for types that can be either null or not.
From this fact, it follows that type Any? is a supertype for type Any.

Non-null type is a subtype of its nullable equivalents, for example, type Number is a subtype of type Number?, and type Int is a subtype of type Int?. Let's see what it looks like.

This is why you can store a non-null Number value in a nullable Number? variable, but you cannot store a nullable Number? value in a non-null Number variable.

### Unit
Unit type can be used as the return type of a function that does not return any meaningful value:

```kotlin
fun logCurrentState(): Unit {
println("Current state of a program: $state")
}
```
If you write a function and the return type is not specified, the compiler will treat it as a Unit function:

```kotlin
fun updateState(state: State) {
logCurrentState()
this.state = state
logCurrentState()
}

val result: Unit = logCurrentState()
```


Like any other type, Unit is a subtype of Any. It can also be a nullable Unit?, which is a subtype of Any?.

Unit? is a type that can be two values: the Unit value and null.

### Nothing
At the very bottom of the Kotlin type hierarchy is the type Nothing.

Nothing is a type that has no instances. For some functions in Kotlin, the concept of a return value doesn't make sense, since they never return controls. This means that any code following an expression of type Nothing is unreachable.

Sometimes it is useful to know that some function does not return control: for example, the fail function that returns an error:

```kotlin
fun fail(): Nothing {
throw Exception("Fail!")
}
```
Therefore, a throw is an expression of type Nothing.

With the Nothing type, which can be a subtype of Any type, the type system allows any expression to fail while doing some work:

```kotlin
fun throwIfNull(name: String?) {
    if (name == null){
        throw Exception("Name can't be null!")
    }
}
```


