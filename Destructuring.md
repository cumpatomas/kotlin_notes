Sometimes it is convenient to destructure an object into a number of variables. For example, to better manipulate it, or to make a piece of code more concise. In this topic, we'll see how this can be done.

### Basic destructuring
Suppose you have a User data class that stores user information. It has fields such as String name, Int age, and Boolean isAdmin.

data class User(val name: String, val age: Int, val isAdmin: Boolean)

`val anonim = User("Anonim", 999, false)`

Well, that's all that we need! Now we can separate all variables from the class and work with them as separate objects:

```kotlin
val (userName, userAge, isAdmin) = anonim
println(userName)  // prints Anonim
println(userAge)   // prints 999
println(isAdmin)   // prints false
```
This feature is called a destructuring declaration. A destructuring declaration creates multiple variables at once. We have declared three new variables: userName, userAge, and isAdmin.

A destructuring declaration uses a componentN() operator, that returns an n-th element from the class. The code above is compiled into the following code:

```kotlin
val userName = anonim.component1()
val userAge = anonim.component2()
val isAdmin = anonim.component3()

```
### Destructuring without data classes
Destructuring can be used without data classes as well. We simply need to define a componentN operator manually. Operators are similar to functions but use special symbols that carry out operations on operands/values. For example, + is an operator that performs addition. That's it! Just think of it as a function. Now let's try to override some operators for destructuring:

```kotlin
class User(val name: String, val age: Int, val isAdmin: Boolean){
operator fun component1(): String = name
operator fun component2(): Int = age
operator fun component3(): Boolean = isAdmin
}

// now we can use default destructuring syntax
fun checkIsAdmin(suspiciousUser: User) {
// destructuring
val (name, age, isAdmin) = suspiciousUser

    if (isAdmin)
        println("Have a nice day!")
    else
        println("Sorry, you should not be here.")
}
```
componentN functions work by relying on the position of each class variable. That is a problem because a class is not meant to be positional, so it is very easy to make a mistake here.

Note that we can't override componentN operator in data classes because Kotlin does it for us:

```kotlin
// Error: Conflicting overloads: public final operator fun component1(): String defined in StoreClass
data class StoreClass(val info: String) {
operator fun component1() = info
}
```
### Destructuring with lists and loops
Destructuring declarations also work with lists and loops because List is a class with the implemented componentN operator. Now, let's extract the first 3 elements:

```kotlin
fun processList(customerInfo: MutableList<String>) {
if (customerInfo.size < 3) return
val (firstName, lastName, city) = customerInfo
showCustomerName(firstName, lastName)
findPopularSellersInCity(city)
}
```
If the list has more than 3 elements, they will not be processed and the program will continue its work. In the same way, if the list has less than 3 elements, there will be an error and the program will crash. To avoid that, we add an if-check.

Note that we can get the first N elements from a list or class with more than N components. This might be useful in some tasks.

Destructuring in for-loops also involves a componentN operator. Now, let's send to the analysts in our company all the data on non-admin users:

```kotlin
fun processAnalytics(usersData: MutableList<User>) {
for ((name, age, isAdmin) in usersData) {
if (!isAdmin)
sendAnalyticsToCompany(name, age)
}
}
```
This way each element in MutableList<User> will be destructured.

### Underscoring for variables
When we start using destructuring declarations, the Kotlin compiler may warn us about unused variables in the destructuring declaration. The default IDE solution is to rename unused variables as "_" (underscore), though it has some drawbacks. For example, let's try to copy-paste some code from somewhere else:

```kotlin
val usersData = mutableListOf<User>()
for ((_, _, isAdmin) in usersData) {
// /~
}
```
Looks familiar, doesn't it? In the example above, the componentN operator skipped name and age properties, so they cannot be used in the cycle.

Another useful feature is the trailing comma. You can add a comma at the end of the parameter list and it will work. It's really convenient because you can copy and paste an additional argument without adding commas.

```kotlin
val usersData = mutableListOf<User>()
for ((name, age, ) in usersData) {
// /~
}
```