## Pseudocode

### Relational and logical operators
You can also use these relational operators in your pseudocode:

```kotlin
a == b // a equal to b
a != b // a is not equal to b
a < b  // a is less than b
a <= b // a is less or equal to b
a > b  // a is more than b
a >= b // a is more or equal to b
```
All these operations return true or false.
In case of a complex condition, you can use logical operators. The and returns true only if both conditions are true. The or returns false only if both conditions are false. The not just reverses a value. It works this way:

```kotlin
true and true == true
true and false == false
false and true == false
false and false == false

true or true == true
true or false == true
false or true == true
false or false == false

not true == false
not false == true
```
### Conditional operators
Another commonly used type of construction is conditional operators. Let's have a look at an example:

```kotlin
a = 3

if a < 5 then
print(a)
```
Here, we create a variable a and initialize it with a number 3. Then, we check if a is less than 5 and if it is true, we print it to the screen. The syntax is clear: the if keyword is followed by a condition, and the next line gets executed only if the condition is true. If you need to combine several conditions, you can use and, or, and not operators:
```kotlin

a = 10
b = 20

if (a == 10 and b == 20) or not (a == 20 and b == 10) then
print(a)
print(b)
```
To avoid ambiguity, we may need to wrap the conditions into parentheses, like in the example above.

Now, you can put an else branch after the if condition. This branch gets executed if the condition is false. Below you can see an example with the if-else construction:

```kotlin
a = -3

if a > 0 then
print("positive")
else:
print("negative or zero") // prints this
```
Besides, you can use an elif branch. The operator elif is just an abbreviation for else if. The program checks this condition if the first one is false:

```kotlin
a = -5

if a > 0 then            // false
print("positive")
elif a == 0 then        // checks this
print("zero")
else:
print("negative")  // output
```
Here, we check whether a is more than 0, then we check whether it equals 0 using the elif branch, and finally we execute the last else branch. Below you can see the same code without the elif branch:

```kotlin
a = -5

if a > 0 then              
print("positive")
else:
if a == 0 then          
print("zero")
else:
print("negative") 
```

### Loops
Loops serve to perform repeated calculations. We will use two kinds of loops: the while and the for loops. A while loop looks like this:

```kotlin
i = 0
while i < 5:
print(i)
i = i + 1
```
The syntax is the following: the while keyword followed by a condition, colon, and a loop body. This code means "execute a body while the condition is true". In this case, the snippet prints numbers from 0 to 4.

Here is what the for loop looks like:

```kotlin
sum = 0

for i in [1, 9]:
sum = sum + i

print(sum) // 45, sum of numbers from 1 to 9
```
The [1, 9] construction denotes a range of numbers from 1 to 9. The last number is included in the range: we use a closed interval that includes all its limit points. In general, the for i in [a, b] means that the variable i is sequentially assigned to all numbers from the range [a, b].

### Arrays
Arrays serve to store a collection of objects of the same type. If we need an array and want to initialize its elements later, we will write the following construction :

`array[1, 10] // 10-element array with indices from 1 to 10`

Here, the variable array denotes an array of 10 elements. We can also initialize an array with some data explicitly:

`fib = [0, 1, 1, 2, 3, 5, 8] // array with the name fib`

The two most commonly-used operations for arrays are learning the length and accessing elements. Enumeration of elements starts with 1. As you may know, array indices in programming often start with 0, but we will use a common pseudocode approach. Let's have a look at how it works:

```kotlin
x = fib[4] // x is 2
length = len(fib) // length is 7

for i in [1, len(fib)]:
print(fib[i])
```
The last for loop iterates through the numbers in the fib array and prints all of them to the screen.

Another useful operation is getting a subarray of an array. It functions as follows:

```kotlin
array = [0, 3, 2, 4, 1]
subarray = array[2, 4]
print(subarray) # 3, 2, 4
```
To get a subarray, we just specify the desired range in square brackets. Remember that the last number is included in the range.

### Functions
We will often work with functions, since they suit well our goal of ignoring the input format and cutting down on the code size. Now, let's learn how to write a function using pseudocode. Below is a function that calculates the mean value of numbers in an array:

```kotlin
function calc_mean(array):
mean = 0

    for i in [1, len(array)]:
        mean = mean + array[i]
    
    return mean / len(array)
```
First, we put a function's name, then arguments in round brackets separated by spaces, after that an indent and a body. If we need to return something from a function, we use the return keyword, like in the example above.

### Implementing simple algorithms in pseudocode

Let's see how we can implement some simple algorithms using the described pseudocode. The first example is a function that takes an array of numbers as input and returns either zero (if the array is empty) or the maximum number in the array:

```kotlin
function find_max(array):
if len(array) == 0 then
return 0

    max = array[1]
    
    for i in [2, len(array)]:
        if array[i] > max then
            max = array[i]
    
    return max
```
Another example is a function that merges two arrays. It takes two sorted arrays as input and returns one sorted array containing the numbers from both input arrays:

```kotlin
function merge(left, right):
merged[1, len(left) + len(right)] // new array

    i = 1      // 
    j = 1      // indices for loop
    k = 1      // 
    
    // iterate over two arrays while we cannot use all elements from any array
    while i <= len(left) and j <= len(right):
        if left[i] < right[j] then    // put element from left array to merged array
            merged[k] = left[i]
            i = i + 1   // move to next element in left array
        else:
            merged[k] = right[j] // put element from right array to merged array
            j = j + 1   // move to next element in right array
            k = k + 1   // move to next element in merged array
                
    while i <= len(left):    // move remains element in left array to merged array
        merged[k] = left[i]
        i = i + 1
        k = k + 1

    while j <= len(right):   // move remains element in right array to merged array
        merged[k] = right[j]
        j = j + 1
        k = k + 1

    return merged
```
Note that we don't care about passing arguments by value, by reference, and so on. If you change any variable inside the function, those changes are saved outside the function. Thus, if you need to keep an argument immutable, just make a copy of it to operate with. Consider this example of the swap function that swaps two numbers:

```kotlin
function swap(a, b):
a = b
b = a

c = d
d = c

swap(c, d)

print(c) // 5
print(d) // 3
```