### STRINGS extension functions:

// Extension Function to add spaces before an uppercase Character

```kotlin

fun CharSequence.addSpaces(): String {
        var output = this[0].toString()

        for (i in 1..this.lastIndex) {
            if (this[i].toString().matches("[A-Z]".toRegex()))
            {
                output += " ${this[i]}"

            } else output += this[i]
        }
        return output
    }
```

// Extension Function to unnacent spanish vocals

```kotlin
fun CharSequence.unaccent(): String {
    var outString = ""
    for(char in this)

        when(char) {
            'á'-> outString += 'a'
            'é'-> outString += 'e'
            'í'-> outString += 'i'
            'ó'-> outString += 'o'
            'ú'-> outString += 'u'
            else -> outString += char
        }
    return outString
}
```

### Activities and Fragments extension functions:

// Function to hide the keyboard
```kotlin
fun AppCompatActivity.hideKeyboard() {
    val view = this.currentFocus
    if (view != null) {
        val imm = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager
        imm.hideSoftInputFromWindow(view.windowToken, 0)
    }
    window.setSoftInputMode(WindowManager.LayoutParams.SOFT_INPUT_STATE_ALWAYS_HIDDEN)
}

fun Fragment.hideKeyboard() {
    val activity = this.activity
    if (activity is AppCompatActivity) {
        activity.hideKeyboard()
    }
}
```




