## Collections
###What is a collection?
Collections are containers that support various ways to store and organize different objects and make them easily accessible. A collection usually contains a number of objects (this number may be zero) of the same type. Objects in a collection are called elements or items. Collections are an implementation of abstract data structures that can support the following operations:

- retrieving an element;
- removing an element;
- changing or replacing an element;
- adding a new element.
 
However, it is important to note that operations like adding, removing, and changing elements only apply to mutable collections. Let's try to understand what this means.

###Mutability
Collections can be mutable or immutable.

Immutable collections cannot be changed. That is, they only allow those operations that do not change the elements, such as accessing an element. Immutability can be helpful when you want to store items together without allowing them to be modified in the future. By using immutable collections, you can be sure that the contents of the collection will not change over time. For example, you may have a list of pages in a book that cannot be changed.

Mutable collections also let you access the elements, but in addition, they allow operations that change the content of a collection by adding, removing, or updating the stored items. For example you may have a collection of favorite restaurants that can change over time.

Collections in Kotlin
Kotlin Standard Library provides implementation for the basic types of collections: list, set, and map. All three exist in mutable and immutable variations. Before we can consider each type in detail, let's have a quick overview.

List stores elements in a specified order and provides indexed access to them.
Set stores unique elements whose order is generally undefined.
Map stores key-value pairs (entries); keys are unique, but different keys can be paired with equal values.
List is a container that keeps the elements of a given type in the same order in which they were added. Also, List allows duplicate elements. So, if you want to store items in the order of their insertion, and it doesn't matter to you that they can be repeated, use List. For example, you could store student rating history in a List.

The difference between List and Set is that the latter does not allow duplicates: all elements in a Set are unique. So, if you try to add an already existing element to a set, you will simply overwrite it. Also, Set does not maintain the order. So, if you don't want to have duplicates and the order doesn't really matter, Set is the best option. For example, you could use Set to collect the names of your favorite restaurants or people you invited to your party.

Finally, Map stores elements as key-value pairs. Each key is assigned a specific value that you set. Map does not allow duplicate keys but allows duplicate values. If you try to add a new key-value pair and the key is already present in the current map, you will simply overwrite its value. Similarly to Set, Map does not store elements in the order they were inserted. If you want to store key-value pairs, Map is the best option for you. For example, you could store people's names as keys and their job titles as values.

Last, but not least: you can combine different types in a collection if your task requires it. That's why we refer to Kotlin collections as generic.

Common properties and methods for collections
As you can see, collections are quite different in structure and application. However, there are properties and methods that can be used with any collection:

- **size** returns the size of your collection.
- **contains(element)** checks if the element is in your collection.
- **containsAll(elements)** checks if all elements of the collection elements are in your collection.
- **isEmpty()** shows whether the collection is empty or not.
- **joinToString()** converts the collection to a string.
- **indexOf(element)** returns the index of the first entry of the element, or -1 if the element is not in the collection.

In addition, all mutable collections have some common methods. You can use them with any mutable collection, but they cannot be used with an immutable collection:

- **clear()** removes all elements from the collection.
- **remove(element)** removes the first occurrence of an element from your collection.
- **removeAll(elements)** removes from your collection all elements contained in the collection elements.
###Idiom
Another way to check if an item is contained in a collection is to use the keyword in. You may have already seen this if you've studied ranges. If not, no problem – it works just like the contains() function. Look at the example:

println(elem _**in**_ collection)

println(collection._**contains(elem)**_)