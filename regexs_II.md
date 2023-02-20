### Sets of characters
Each set consists of multiple characters but corresponds to a single character in the string. Sets are enclosed in square brackets []. For example, the set "[abc]" means that a single character "a", "b", or "c" can match it. Take a look at the example below:

```kotlin
val regex = Regex("[bcr]at") // it matches strings "bat", "cat", "rat", but not "fat"
regex.matches("rat") // true
regex.matches("fat") // false
```

You can use as many sets as you want and combine them with regular characters. There are two sets in the following example:

```kotlin
val pattern = "[ab]x[12]" // can match a or b followed by x followed by either 1 or 2
```
This pattern can be successfully matched by the following strings:

`"ax1", "ax2", "bx1", "bx2"`

Meanwhile, the following strings do not match the pattern:

`"xa1", "aax1", "bx"`

As you can see, the order of sets in regular expressions is important. On the other hand, the order of characters within the set does not matter.

### Ranges of characters
Sometimes we want to make our character sets quite large. In this case, we don't have to write them all down: we can specify a range designated by the hyphen symbol - instead. The character that precedes the hyphen denotes the starting point of the range; the character after the hyphen is the last character that falls into the range. We can put characters into a set as a range if they immediately follow each other in the ASCII/Unicode encoding table. This includes both alphabetically ordered letters and numeric values. For example, we can write a set that matches every digit:

```kotlin
val anyDigitPattern = "[0-9]" // matches any digit from 0 to 9
```
The same works for letter ranges, such as "[a-z]" or "[A-Z]". These ranges match all Latin lowercase and uppercase letters respectively. These patterns are case-sensitive; for a case-insensitive match, we can write the following regex:

val anyLetterPattern = "[a-zA-Z]" // matches any letter "a", "b", ..., "A", "B", ...
Note that although the range [A-z] is technically valid, it includes additional symbols that are placed between uppercase and lowercase letters in the ASCII table.

As you can see, you can easily put several ranges in one set and mix them with separate characters in any order:

```kotlin
val anyLetterPattern = "[a-z!?.A-Z]" // matches any letter as well as "!", "?", and "."
```
### Excluding characters
In some cases, you may want to define which characters are not wanted. Then, you can write a set that will match everything except the characters mentioned in it. To do that, we write the hat character ^ as the first one in the set.
```kotlin
val regex = "[^abc]".toRegex() // matches everything except "a", "b", and "c"

regex.matches("a") // false
regex.matches("b") // false
regex.matches("c") // false
regex.matches("d") // true

```
The same works for ranges:

```kotlin
val regex = "[^1-6]".toRegex()

regex.matches("1") // false
regex.matches("2") // false
regex.matches("0") // true
regex.matches("9") // true
```
### Avoiding characters in sets
The general rule is that you do not need to avoid special characters within sets if they are used in their literal meaning. For example, the set [.?!] will match a single period, a question mark, an exclamation mark, and nothing else. However, the characters used to define a set or a range should be avoided or put in a neutral position – in case we look for their literal symbols:

to match the hyphen character, we should put it in the first or in the last position in the set: "[-a-z]" matches lowercase letters and the hyphen, and "[A-Z-]" matches uppercase letters and the hyphen;

hat ^ does not need to be avoided if placed anywhere but the beginning. This way, the set "[^a-z^]" matches everything except for lowercase letters and the hat character;

square brackets should always be escaped:

```kotlin
val regex = Regex("[\\[\\]]") // matches "[" and "]"
```
### Alternations
So far, we've been talking about single characters. However, there's also a way to match longer sequences. The vertical bar | is used to match character sequences either before or after the symbol:

```kotlin
val regex = "yes|no|maybe".toRegex() // matches "yes", "no", or "maybe", but not "y" or "e"

regex.matches("no") // true
```
This is useful in situations when we want to look for one of several particular words, for example, "bear", "bat", or "bird" to complete the sentence The giant ___ scared me and when it's easier to indicate whole words. The vertical bar can be used together with parentheses, which designate the boundaries of alternating substrings: everything within the parentheses is an optional substring that can match the alternation block:

```kotlin
val scaryAnimal = "(b|r|go)at".toRegex()  // matches "bat", "rat", or "goat"
val answer = "The answer is definitely (yes|no|maybe)".toRegex()
```
In general, alternations are quite similar to sets: they describe multiple alternatives that a particular part of the pattern can match. However, while sets can match only a single character in the string, alternations are used to define multi-character alternatives.