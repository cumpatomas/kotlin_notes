## MutableMap
MutableMap is a collection that holds pairs of objects (keys and values) and supports efficient retrieval of the value corresponding to each key. Unlike Map, which is an immutable collection, MutableMap is mutable or modifiable: you can freely add and remove pairs of objects.

Let's imagine that you have a list of staff members with the information about their salaries:

```kotlin
val staff = mapOf(
"John" to 500,
"Mike" to 1000,
"Lara" to 1300
)
println(staff) // output: {John=500, Mike=1000, Lara=1300}
```
Alright, we have a list of staff with salaries, and now we can easily find out the salary of each employee. But what if we hired a new employee?
As we know, Map is an immutable collection: we can't modify its source. So, to add a new employee to Map, we need to create another Map.

```kotlin
var staff = mapOf( // you cannot reassign an immutable reference, so you need to use var
"John" to 500,
"Mike" to 1000,
"Lara" to 1300
)
staff += "Jane" to 700 // reassignment
println(staff) // output: {John=500, Mike=1000, Lara=1300, Jane=700}
```
This is exactly the kind of situation where MutableMap is useful. MutableMap supports addition of elements:

```kotlin
val staff = mutableMapOf(
"John" to 500,
"Mike" to 1000,
"Lara" to 1300
)

staff["Nika"] = 999

println(staff) // output: {John=500, Mike=1000, Lara=1300, Nika=999}
```
This is one of the functions provided to us by MutableMap out of the box, and it allows us to easily modify the map contents.

### Initialization
You can create a MutableMap in a variety of ways:

```kotlin
val students = mutableMapOf<String, Int>("Nika" to 19, "Mike" to 23)
println(students) // output: {Nika=19, Mike=23}
```
The type can also be derived from the context:

```kotlin
val carsPerYear = mutableMapOf(1999 to 30000, 2021 to 202111)
println(carsPerYear) // output: {1999=30000, 2021=202111}
```
You can also transform Map into MutableMap with the help of the function toMutableMap():

```kotlin
val mapCarsPerYear = mapOf(1999 to 30000, 2021 to 202111)
val carsPerYear = mapCarsPerYear.toMutableMap()
carsPerYear[2020] = 2020
println(carsPerYear) // output: {1999=30000, 2021=202111, 2020=2020}}
```
### Adding elements
MutableMap has the same properties and methods as Map: size, isEmpty(), containsKey(key), containsValue(element), and so on.

Also, MutableMap offers additional functionality for changing the contents:

**put(key, value)** associates the specified value with the specified key in the map; the short form of mutableMap[key] = value;

**putAll(Map)** updates the map with key/value pairs from a specified map.

Let's take a look at an example. Imagine a situation where we enroll students in a group and then decide to add students from another city to the same group:

```kotlin
val groupOfStudents = mutableMapOf<String, Int>() // empty mutable map
groupOfStudents.put("John", 4)
groupOfStudents["Mike"] = 5
groupOfStudents["Anastasia"] = 10

val studentsFromOregon = mapOf("Alexa" to 7)

groupOfStudents.putAll(studentsFromOregon)

println(groupOfStudents) // output: {John=4, Mike=5, Anastasia=10, Alexa=7}
```
When you try to associate a specified value in the map with a key that already exists, the existing value will be overwritten. Let's take a look at an example:

```kotlin
val groceries = mutableMapOf<String, Int>()

groceries["Potato"] = 5  
println(groceries)  // output: {Potato=5}

groceries["Potato"] = 10     
println(groceries)  // output: {Potato=10}
```
You can also add an element to a map using the plusAssign operator syntax, like in the example below:

```kotlin
val groceries = mutableMapOf<String, Int>()

groceries += mapOf("Potato" to 5)
groceries += "Sprite" to 1

println(groceries)  // output: {Potato=5, Sprite=1}
```
### Removing elements
You might also need to remove some or all of the elements from your Map. Let's see how this can be done:

```kotlin
remove(key) removes the specified key and its corresponding value from the map;
clear() removes all elements from the map.
val groceries = mutableMapOf(
"Potato" to 10,
"Coke" to 5,
"Chips" to 7
)

groceries.remove("Potato")
println(groceries) // output: {Coke=5, Chips=7}

groceries.clear()
println(groceries) // output: {}

```
You can also remove an element from the map using the minusAssign operator syntax. Take a look at an example:

```kotlin
val cars = mutableMapOf<String, Double>()
cars["Ford"] = 100.500
cars["Kia"] = 500.15

println(cars)  // output: {Ford=100.5, Kia=500.15}

cars -= "Kia"   
println(cars)  // output: {Ford=100.5}
```
