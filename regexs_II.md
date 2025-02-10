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

### The list of quantifiers
Here is a list of quantifiers to be remembered:
+ "+" matches one or more instances of the preceding character;
+ "*" matches zero or more instances of the preceding character;
+ "{n}" matches exactly n instances of the preceding character;
+ "{n,m}" matches at least n but not more than m instances of the preceding character;
+ "{n,}" matches at least n instances of the preceding character;
+ "{0,m}" matches no more than m instances of the preceding character.

Note that there is also another quantifier, ?, which makes the preceding character optional. It is short for {0,1}. We will not consider this quantifier here because you should already know it.

### The plus quantifier
Below you can see how we use the plus character, which matches one or more occurrences of the preceding character:

```kotlin
val regex = "ca+b".toRegex()

regex.matches("cab") // true
regex.matches("caaaaab") // true
regex.matches("cb") // false because it does not have at least one instance of 'a'
```
As you can see, it matches only those strings that have one or more instances of the 'a' character.

### The asterisk quantifier
The example below demonstrates the use of the asterisk character, which matches zero or more occurrences of the preceding character:

```kotlin
val regex = "A[0-3]*".toRegex()

regex.matches("A")  // true because the pattern matches zero or more occurrences
regex.matches("A0") // true
regex.matches("A000111222333") // true
```
As you can see, the asterisk quantifier, unlike the plus quantifier, allows the pattern to also match the strings that do not contain the "quantified" character at all.

In the following example, there is a pattern describing the string "John" located between an undefined number of undefined characters in the text:

```kotlin
val johnRegex = ".*John.*".toRegex() // it matches all strings containing the substring "John"

val textWithJohn = "My friend John is a computer programmer"

johnRegex.matches(textWithJohn) // true

val john = "John"

johnRegex.matches(john) // true

val textWithoutJohn = "My friend is a computer programmer"

johnRegex.matches(textWithoutJohn) // false
```
So, the asterisk quantifier can be used to check whether a substring of a string matches a pattern. Using it, we can skip spaces or any other characters we don't want to predict in our pattern.

### Specifying the number of repetitions
Both previous quantifiers have a wide range of applications, but they do not allow you to specify how many times a character may occur. Fortunately, there is a group of quantifiers that allow specifying the number of instances in curly braces: {n}, {n,m}, and {n,}.

An important clarification: no spaces are supposed to be used inside curly braces. There can be only one or two numbers and, optionally, a comma. Putting spaces inside curly braces leads to the "deactivation" of the quantifier and, as a result, a totally different regular expression.
Take a look at the example where we demonstrate how to match exactly n instances of the preceding character using the {n} quantifier:

```kotlin
val regex = "[0-9]{4}".toRegex() // four digits

regex.matches("6342")  // true
regex.matches("9034")  // true

regex.matches("182")   // false
regex.matches("54312") // false
```
Matching from n to m instances is possible thanks to the {n,m} quantifier. Note that the range specified in curly braces is inclusive at both ends: m encountered instances also count as a match. This is standard for the regex language regardless of the implementation.

```kotlin
val regex = "1{2,3}".toRegex()

regex.matches("1")    // false
regex.matches("11")   // true
regex.matches("111")  // true
regex.matches("1111") // false
```
The last example demonstrates how to match at least n instances using the {n,} quantifier:


```kotlin
val regex = "ab{4,}".toRegex()

regex.matches("abb") // false, not enough 'b'
regex.matches("abbbb") // true
regex.matches("abbbbbbb") // true
```
The quantifier that matches not more than m instances works similarly. Try it yourself.

### The list of shorthands
There are several pre-defined shorthands for the commonly used character sets:

**\d** is any digit, short for [0-9];

**\s** is a whitespace character (including tab and newline), short for [ \t\n\x0B\f\r];

**\w** is an alphanumeric character (letter or numeral), short for [a-zA-Z_0-9];

**\b** is a word boundary. This one is a bit trickier: it doesn't match any specific character but rather matches the boundary between an alphanumeric character or underscore and a non-alphanumeric character (for example, a whitespace character) or a boundary of a string (its end or start). This way, "\ba" matches all words (sequences of alphanumeric characters) starting with "a", "a\b" matches all words ending with "a", and "\ba\b" matches all separate "a" characters preceded and followed by non-alphanumeric characters.

There are also negative counterparts of these shorthands that are equivalent to the restrictive sets and match everything except for the characters mentioned above:


**\D** is a non-digit, short for [^0-9];

**\S** is a non-whitespace character, short for [^ \t\n\x0B\f\r];

**\W** is a non-alphanumeric character, short for [^a-zA-Z_0-9].

**\B** is a non-word boundary. It matches the case opposite to that of the \b shorthand: it finds its match every time whenever there is no "gap" between alphanumeric characters. For example, "a\B" matches all words that start with "a".

These shorthands make writing common patterns much easier.

Each shorthand has the same first letter as its representation (digit, space, word, boundary). The uppercase characters are used to designate the shorthands for negative character classes.

### Example
Let's consider an example with the listed shorthands. Remember that in Kotlin we use an additional backslash \ character for escaping.

```kotlin
val regex = "\\s\\w\\d\\s".toRegex()

regex.matches(" A5 ")   // true
regex.matches(" 33 ")   // true
regex.matches("\tA4\t") // true because tabs are whitespace as well

regex.matches("q18q") // false, 'q' is not a space
regex.matches(" AB ") // false, 'B' is not a digit
regex.matches(" -1 ") // false, '-' is not an alphanumeric character, but '1' is OK.
```
Another way to write shorthand is to use raw strings. You don't need to escape \ in this case:

```kotlin
val regex = """\W\S\D\S\W""".toRegex()
regex.matches(" 9o9 ")  // true
regex.matches("\nA 1 ")   // true
regex.matches("\tAl4\t") // true

regex.matches(" \taa ") // false, '\t' is a space
regex.matches("_BBB ") // false, '_' is an alphanumeric character
```
Here's how boundary shorthands work in Kotlin code:

```kotlin
val startRegex = "\\bcat".toRegex() // matches the part of the word that starts with "cat"
val endRegex = "cat\\b".toRegex() // matches the part of the word that ends with "cat"
val wholeRegex = "\\bcat\\b".toRegex() // matches the whole word "cat"
```
For now, we are not applying them in practice because we only deal with the matches method, which requires a full string to match the regexp.

If you do not want to use shorthands, you can write the same regex as below:

```kotlin
val regex = "[ \\t\\n\\x0B\\f\\r][a-zA-Z_0-9][0-9][ \\t\\n\\x0B\\f\\r]".toRegex()
```

This regex, however, is long and not nearly as readable as the previous ones. It also has a lot of character repetitions. You can use the predefined shorthands instead of commonly used sets and ranges to simplify your regexes and make them more readable.

### Regexps and split()
You are already familiar with split() from the topics in which we discussed string processing. Let's quickly recap what it is.

This method is used to split a string into parts according to a certain principle: the argument of split() defines the sequence of characters around which the string is split. The result of split() is a list of strings containing parts of the original string. So let's see what the overloaded split() method with the use of regular expressions looks like. You will see that it is quite similar!

The definition looks as follows:

```kotlin
fun CharSequence.split(
regex: Regex,
limit: Int = 0
): List
```
Let's consider the arguments. The regex argument is responsible for the regular expression which defines the delimiter, and limit sets the maximum number of substrings to return. Zero by default means no limit is set.

So, split() splits the input char sequence around the matches of a given regular expression.

### Regexps and split(): example
If you remember, in previous topics we looked at an example of using split() in phone number splitting. We split an American phone number into its country code, area code, central office code, and other remaining digits:

```kotlin
val number = "+1-213-345-6789"
val parts = number.split("-") // {"+1", "213", "345", "6789"}
```
Now let's make the previously considered problem more complex. There may be parentheses in the number. Or there may be none. We need to cover both of these cases.

This is where regular expressions help us! Let's compose a regular expression that will match either the separator "-", or "-(", or ")-". We will use toRegex() to create a regular expression from a string.

`"(-\\(|\\)-|-)".toRegex()`
Let's check the method for both variants of number spelling:

```kotlin
val number = "+1-213-345-6789"
val brackets = "+1-(213)-345-6789"
// splitting all substrings in number with brackets
val splitBrackets = brackets.split("(-\\(|\\)-|-)".toRegex()) // {"+1", "213", "345", "6789"}
// splitting only two substring
val splitFirstBrackets = number.split("(-\\(|\\)-|-)".toRegex(), 2) // {"+1", "213-345-6789"}
// splitting all substrings in number without brackets
val splitNumber = number.split("(-\\(|\\)-|-)".toRegex()) // {"+1", "213", "345", "6789"}
```
Everything works!

### Regexps and replace()
Let's look at the use of regexps with another method: replace(). As you may remember, this method is used to replace some parts of the original string with new ones:

```kotlin
fun String.replace(
oldValue: String,
newValue: String,
ignoreCase: Boolean = false
): String
```
Accordingly, we can either define the desired parts of the string directly as another string or cover more cases using a regular expression.

Let's look at the overload of replace() for regular expressions:

```kotlin
fun CharSequence.replace(
regex: Regex,
replacement: String
): String
```
As arguments, it accepts regex, which searches for replaceable parts in the text, and the string with which they will be replaced.

### Regexps and replace(): example
So, let's look at an example of using replace() with regexp.

Suppose we have a text in which we need to replace all the digits with the string "[digits]". It will be difficult to define exactly what we want with one string. But with regular expressions, everything is solved very easily:

```kotlin
val withDigits = "The first test flight of Falcon 9 was on June 4, 2010, " +
"from Cape Canaveral, Florida, and the first resupply mission " +
"to the ISS was made on October 7, 2012."
val processedString = withDigits.replace("\\d+".toRegex(), "[digits]")
```
It's easy to see that regex \\d+ matches all occurrences of one or more numbers in the text.

As a result of executing the code above, the following text will be stored in processedString:

The first test flight of Falcon [digits] was on June [digits], [digits], from Cape Canaveral, Florida, and the first resupply mission to the ISS was made on October [digits], [digits].

As you can see, all the numbers in the text have been replaced with the specified string.

### find() and findAll()
Finally, let's take a look at two more functions that will definitely come in handy.

The find function is used to find the first match of a regular expression in the input. It searches from the start index of the input string.

Let's make the phone number regex even more versatile and see how it all works:

```kotlin
val regex = """[+]?[(]?[0-9]{1,4}[)]?[-0-9]*""".toRegex() // phone number template
val matchResult = regex.find("Her phone number is +1-234-567-89-01. You can also call the second one: +1-111-568-01-01")!!
print(matchResult.value) // +1-234-567-89-01
```
Note that we care about null safety. The text may not contain matches at all and the result of find() is String? so you should use the !! operator or the Elvis operator.
The findAll function is required if you want to find all matches. It returns all suitable substrings at once:

```kotlin
val regex = """\d{4}-\d{2}-\d{2}""".toRegex() // date template in format YYYY-MM-DD
val matchResult =
regex.findAll("Harry was born 1980-07-31 in the Godric's Hollow."
+ "In 1997-12-24, on Christmas Eve, he returned there"
+ "for the legendary Gryffindor sword")
for (matches in matchResult) println(matches.value)
//1980-07-31
//1997-12-24
```