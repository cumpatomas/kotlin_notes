You have already learned about the benefits of transferring data by serializing it into JSON format. Manually converting classes isn't very convenient, but fortunately, there are plenty of libraries available that simplify the process. One of the most popular and frequently used in Android is called Moshi.
Moshi's API is much smaller than other well-known libraries like Jackson and Gson. Gson has more than a thousand methods, while Moshi has only half that number. Moshi also weighs 2-3 times less (just over 100kB compared to Gson's 300kB).

Moshi is fast and requires less memory, making it a great option to choose. In this topic, you'll learn how to use it and find out more about the structure of JSON along the way.

### Adding a library
To use the Moshi library, we need to add the following dependency to our build.gradle.kts file in the dependencies section:

`implementation("com.squareup.moshi:moshi-kotlin:1.11.0")`

With this in place, our IDE will import the required classes as we write our code.

You can find out more by visiting Moshi's GitHub page.

Serializing Kotlin objects into JSON strings
We'll start by defining a Human class that we can use to perform operations:

`class Human(var name: String, var age: Int, var friends: Array<String>)`

Now let's create an instance of the Human class:

`val human = Human("Mike", 20, arrayOf("Alex", "Valery", "Ann"))`

To work with Moshi, we need to use the Builder pattern to create the following object:

```kotlin
val moshi = Moshi.Builder()
.add(KotlinJsonAdapterFactory())
.build()
```
Next, we need to create an adapter to serialize the Human class and pass it the correct reference. This can be done with the Kotlin Classname::class.java feature:

```kotlin
val humanAdapter = moshi.adapter(Human::class.java)
```
That's it! Now we're all set to quickly and conveniently convert our object into a JSON representation. For simplicity, we'll print the string to the output stream. However, in an actual application, we could save the result to a text file or database.

```kotlin
print(humanAdapter.toJson(human))
// {"name":"Mike","age":20,"friends":["Alex","Valery","Ann"]}
```
### Deserializing JSON into Kotlin objects
We've got our adapter ready for the class, so let's try to use it to recreate a new object from a JSON string:

```kotlin
val newHumanString = """
{"name":"John",
"age":25,
"friends":["Mike","Helen"]}""".trimIndent()

val newHuman = humanAdapter.fromJson(newHumanString)
```
The fromJson method doesn't return a Human, it returns a Human? object. This is because it can be nullable (the fromJson method may not recognize the supplied JSON string), so ?. is used to access it safely.

Now we want to deserialize a more complex structure — a list of class objects.

Because our transformation will involve two classes, Human and List, we need to pass them in a ParameterizedType object. This approach allows us to store information about both classes together so it can be passed to the adapter for use during deserialization:

```kotlin
val humanList = listOf(human, newHuman)

val type = Types.newParameterizedType(List::class.java, Human::class.java)
val humanListAdapter = moshi.adapter<List<Human?>>(type)
print(humanListAdapter.toJson(humanList)) // [{"name":"Mike","age":20,"friends":["Alex","Valery","Ann"]},{"name":"John","age":25,"friends":["Mike","Helen"]}]
```
Having created a suitable adapter by combining the List and Human classes, we can use it to convert a JSON string:

```kotlin
val jsonStr =
"""[{"name":"Nick","age":10,"friends":["Valery"]},
{"name":"John","age":25,"friends":[]},
{"name":"Kate","age":40,"friends":[]}]
""".trimIndent()

val newHumanList = humanListAdapter.fromJson(jsonStr)
```

### Working with JSON
In the previous section, we learned how to retrieve objects from JSON via deserialization. Now we'll look at some examples of how to make use of them:

It's simple to find out the name of the newHuman object we created with our adapter:

`print(newHuman?.name) // John`

We can output more complex data as well. Let's see who John's friends are!

`print(newHuman?.friends.contentToString()) // [Mike, Helen]`

We also created an adapter based on two classes that enabled us to deserialize a list of Human objects. Here's what it contains:

```kotlin
for (h in newHumanList!!) {
print(h?.name + " ")
} // Nick John Kate
```
We are sure our list is definitely not null, so use !! when we refer to it.

For the final example, let's change our class a little by adding a Map that displays subject grades:

`class Human(var name: String, var age: Int, var friends: Array<String>, var grades: Map<String, String>)`

Next, create an appropriate adapter that includes Map:

```kotlin
val type = Types.newParameterizedType(List::class.java, Human::class.java, Map::class.java)
val humanListAdapter = moshi.adapter<List<Human?>>(type)
```
We finish by converting our objects from a JSON string as we did before:

```kotlin
val jsonStr =
"""[{"name":"Ruslan","age":20,"friends":["Victoria","Valery"],
"grades":{"Math":"A","Philosophy":"F","Physics":"D"}},
{"name":"Victoria","age":35,"friends":["Ruslan","Anastasia"],
"grades":{"Math":"B","Philosophy":"C","Physics":"B"}}]""".trimIndent()
val humanList = humanListAdapter.fromJson(jsonStr)
```
But this time, we're also able to display information about people's grades, as shown below:

```kotlin
for (h in humanList!!) {
println(h?.name + " has a grade of ${h!!.grades["Math"]} in maths")
}
// Ruslan has a grade of A in maths
// Victoria has a grade of B in maths
```