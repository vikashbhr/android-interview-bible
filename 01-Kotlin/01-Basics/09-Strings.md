# 💜 Kotlin Strings — Complete Interview Guide (2026 Edition)

> Master Kotlin Strings from Unicode fundamentals to JVM String Pool, memory optimization, Regex, Android localization, Compose text rendering, and performance engineering.

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • PhonePe • Uber • Amazon • Microsoft • Flipkart • Razorpay

---

# 📚 Table of Contents

## Part 1 — Foundation (This Chapter)

1. What is String?
2. String Immutability
3. String Pool (JVM)
4. UTF-16 & Unicode
5. String Memory Model
6. String Templates
7. Raw Strings
8. Common String Operations
9. Equality (`==` vs `===`)
10. JVM Bytecode

## Part 2 — Advanced

11. StringBuilder vs StringBuffer
12. Regex
13. String Formatting
14. Android Localization
15. Compose Text APIs
16. Performance Optimization
17. Interview Questions
18. Cheat Sheet

---

# 1. What is String?

A **String** is an immutable sequence of Unicode characters.

```kotlin
val language = "Kotlin"
```

Internally on the JVM, Kotlin's `String` is **`java.lang.String`**.

Unlike Java's primitive types, `String` is always an object.

---

## Why Strings Matter in Android?

Strings are everywhere:

- UI Text
- API Responses
- JSON Parsing
- Navigation Routes
- Deep Links
- Resource IDs
- Logging
- Compose `Text()`

A typical Android app may create **thousands of String objects** every second.

Understanding String performance matters.

---

# 2. String Immutability

Kotlin Strings cannot be modified after creation.

```kotlin
val name = "Android"

name += " Compose"
```

Looks mutable, but actually creates a **new String**.

---

## Memory Diagram

```text
Before

Stack

name ──────────────┐
                   ▼

Heap

"Android"
```

After concatenation

```text
Stack

name ──────────────┐
                   ▼

Heap

"Android Compose"

Old object:

"Android"
```

A brand-new String object is created.

---

## Why Immutable?

Benefits:

- Thread-safe.
- Cached in String Pool.
- Safe hashCode caching.
- Safer sharing across threads.

Interview favorite.

---

# 3. JVM String Pool

One of the most asked senior interview topics.

## What is String Pool?

The JVM stores identical string literals only once.

```kotlin
val a = "Android"

val b = "Android"
```

Memory

```text
Stack

a ───────┐
b ───────┘
          ▼

String Pool

"Android"
```

Both variables point to the same object.

---

## New String Object

```kotlin
val a = "Android"

val b = String(charArrayOf('A','n','d','r','o','i','d'))
```

Memory

```text
String Pool

"Android"

Heap

new String("Android")
```

Now references differ.

---

## Equality Example

```kotlin
println(a == b)
```

Output

```text
true
```

Contents equal.

---

```kotlin
println(a === b)
```

Output

```text
false
```

Different objects.

---

# 4. Structural vs Referential Equality

| Operator | Meaning |
|----------|---------|
| `==` | Compare contents |
| `===` | Compare references |

---

## Interview Trap

```kotlin
val a = "ABC"

val b = "ABC"

println(a === b)
```

Output

```text
true
```

Because String Pool reused object.

---

## Runtime String

```kotlin
val b = "A" + "BC"
```

Compile-time constant.

Still pooled.

---

## Dynamic Concatenation

```kotlin
val value = "BC"

val b = "A" + value
```

Not pooled.

Different object.

---

# 5. UTF-16 & Unicode

Kotlin Strings use UTF-16 encoding on the JVM.

Each character occupies **16 bits** (one UTF-16 code unit).

```kotlin
val letter = "A"
```

Unicode

```
U+0041
```

---

## Emoji Example

```kotlin
val emoji = "😀"
```

Length

```kotlin
println(emoji.length)
```

Output

```text
2
```

Why?

Emoji uses **surrogate pairs**.

---

## UTF-16 Diagram

```text
😀

Unicode Code Point

U+1F600

UTF-16

High Surrogate

Low Surrogate
```

Interview favorite.

---

# 6. String Length Isn't Character Count

```kotlin
val emoji = "👨‍👩‍👧‍👦"

println(emoji.length)
```

Length is much larger than visible characters.

Reason:

Multiple Unicode code points.

---

## Safe Character Iteration

Use code points for Unicode-sensitive applications.

Android emoji handling relies on this.

---

# 7. String Memory Representation

```kotlin
val city = "Delhi"
```

Memory

```text
Stack

city ─────────────┐
                  ▼

String Pool

"Delhi"
```

---

## Heap Representation

```text
java.lang.String

hash

coder

value[]
```

Modern JVM stores characters internally using optimized byte arrays depending on encoding.

---

# 8. String Templates

Kotlin's biggest usability improvement over Java.

## Variable

```kotlin
val name = "Vikash"

println("Hello $name")
```

---

## Expression

```kotlin
println("${name.uppercase()}")
```

Anything inside `{}` is evaluated.

---

## Nested Expressions

```kotlin
println("Length = ${name.length}")
```

---

## Android Example

```kotlin
Text("Welcome ${user.name}")
```

Compose uses templates frequently.

---

# 9. Raw Strings

Triple quotes preserve formatting.

```kotlin
val json = """
{
    "name":"Android",
    "version":16
}
""".trimIndent()
```

No escaping needed.

---

## Multiline SQL

```kotlin
val query = """
SELECT *
FROM users
WHERE age > 18
""".trimIndent()
```

---

## Difference

| Regular String | Raw String |
|---------------|------------|
| Escape required | No escaping |
| Supports `\n` | Preserves newline |
| Uses `"` | Uses `"""` |

---

# 10. Escaping Characters

```kotlin
val text = "Android\nCompose"
```

Escape sequences.

| Escape | Meaning |
|--------|---------|
| `\n` | New Line |
| `\t` | Tab |
| `\"` | Double Quote |
| `\\` | Backslash |
| `\r` | Carriage Return |

---

# 11. Common String Operations

## uppercase()

```kotlin
name.uppercase()
```

---

## lowercase()

```kotlin
name.lowercase()
```

---

## replace()

```kotlin
name.replace("Android","Compose")
```

---

## substring()

```kotlin
name.substring(0,3)
```

---

## trim()

```kotlin
" Android ".trim()
```

---

## startsWith()

```kotlin
name.startsWith("And")
```

---

## endsWith()

```kotlin
name.endsWith("oid")
```

---

## contains()

```kotlin
name.contains("roid")
```

---

## split()

```kotlin
val words = "A,B,C".split(",")
```

Output

```text
[A,B,C]
```

---

## joinToString()

```kotlin
listOf("A","B").joinToString("-")
```

Output

```text
A-B
```

---

# 12. Destructuring Split Result

```kotlin
val(name,age)= "Vikash,29".split(",")
```

Useful in parsing.

---

# 13. String Comparison

```kotlin
"abc"=="abc"
```

True.

---

## Ignore Case

```kotlin
"ANDROID".equals("android",true)
```

True.

---

## compareTo()

```kotlin
"Apple".compareTo("Banana")
```

Lexicographical comparison.

---

# 14. Lexicographical Ordering

```text
Apple

Banana

Cat

Dog
```

Used in sorting.

---

# 15. String Interpolation Performance

```kotlin
"$name is $age"
```

Compiler converts into `StringBuilder` calls.

Equivalent Java

```java
new StringBuilder()
.append(name)
.append(age)
.toString();
```

No runtime reflection.

---

# 16. JVM Bytecode

Kotlin

```kotlin
val message = "Hello $name"
```

Java

```java
StringBuilder builder =
new StringBuilder();

builder.append("Hello ");

builder.append(name);

builder.toString();
```

Interview favorite.

---

# 17. Android Best Practices

## Resource Strings

Never hardcode UI text.

```kotlin
Text(stringResource(R.string.welcome))
```

---

## Formatting Resources

```xml
<string name="welcome">
    Welcome %1$s
</string>
```

```kotlin
stringResource(
    R.string.welcome,
    user.name
)
```

Supports localization.

---

## Navigation Routes

```kotlin
"profile/${user.id}"
```

Use string templates carefully.

---

# 18. Common Mistakes

### Mistake 1

Using `===` for String comparison.

Bad.

```kotlin
name === "Android"
```

---

### Mistake 2

Repeated concatenation inside loops.

Creates many temporary Strings.

---

### Mistake 3

Hardcoded UI strings.

Breaks localization.

---

### Mistake 4

Assuming `length` equals visible character count.

Emoji disproves this.

---

# 19. Real Interview Questions

## Basic

1. Are Kotlin Strings mutable?
2. Difference between `==` and `===` for Strings?
3. What is String Pool?
4. What are raw strings?
5. What is string interpolation?

## Intermediate

6. Why are Strings immutable?
7. What encoding does Kotlin use on JVM?
8. Why does `"😀".length == 2`?
9. Explain surrogate pairs.
10. Explain compile-time string concatenation.

## Advanced

11. JVM implementation of String Pool.
12. StringBuilder generated by compiler.
13. Memory representation of Strings.
14. Performance implications of concatenation.
15. Localization best practices in Android.

---

# 20. Cheat Sheet

| Operation | Example |
|-----------|---------|
| Template | `"Hello $name"` |
| Expression | `"${user.age}"` |
| Raw String | `"""JSON"""` |
| Replace | `replace()` |
| Trim | `trim()` |
| Split | `split(",")` |
| Join | `joinToString()` |
| Uppercase | `uppercase()` |
| Lowercase | `lowercase()` |
| Contains | `contains()` |

---

# 📝 Revision Summary

- Kotlin Strings are immutable.
- JVM stores literals in the **String Pool**.
- `==` compares contents, `===` compares references.
- Kotlin Strings use UTF-16 on the JVM.
- Emoji may occupy multiple UTF-16 code units.
- String templates compile into `StringBuilder`.
- Use `stringResource()` for Android localization.

---

# 🚀 Part 2 — Advanced Strings (Senior Android Interview Edition)

> This section covers StringBuilder, Regex, Android Localization, Jetpack Compose Text APIs, JVM internals, and performance optimization.

---

# 21. StringBuilder vs StringBuffer vs buildString()

One of the most frequently asked Android performance questions.

## Why StringBuilder?

Since Strings are immutable, every concatenation creates a new String object.

### ❌ Bad Practice

```kotlin
var message = ""

for (i in 1..5) {
    message += i
}
```

Every iteration creates a brand new String object.

### Memory Allocation

```
Iteration 1 -> "1"
Iteration 2 -> "12"
Iteration 3 -> "123"
Iteration 4 -> "1234"
Iteration 5 -> "12345"
```

Five intermediate strings are allocated.

---

## StringBuilder

```kotlin
val builder = StringBuilder()

for (i in 1..5) {
    builder.append(i)
}

val result = builder.toString()
```

### Internal Memory Representation

```
Heap

StringBuilder
----------------------------
value[]   -> ['1','2','3','4','5']
count     -> 5
capacity  -> 16
```

The builder mutates its internal array instead of creating new String objects.

---

## Capacity Growth

```kotlin
val builder = StringBuilder(16)
```

Default capacity is **16 characters**.

When capacity exceeds the limit, JVM expands it approximately like:

```
16
34
70
142
286
```

This minimizes memory reallocations.

---

## StringBuffer

```kotlin
val buffer = StringBuffer()

buffer.append("Android")
```

### Difference Between StringBuilder & StringBuffer

| StringBuilder | StringBuffer |
|---------------|--------------|
| Mutable | Mutable |
| Not synchronized | Synchronized |
| Faster | Slower |
| Preferred in Android | Rarely needed |

Use **StringBuilder** unless multiple threads modify the same buffer.

---

## Kotlin `buildString {}`

Kotlin provides a DSL around StringBuilder.

```kotlin
val result = buildString {
    append("Hello ")
    append("Android ")
    append("Interview Bible")
}
```

Internally it uses `StringBuilder`.

This is the idiomatic Kotlin solution.

---

# 22. StringBuilder vs `+` vs Templates

| Scenario | Recommended |
|----------|-------------|
| Two or three strings | String Template (`"$name"`) |
| UI text | String Template |
| Inside loops | StringBuilder |
| DSL creation | buildString |
| Background parsing | StringBuilder |

---

## Compiler Optimization

```kotlin
val text = "Hello " + name
```

Compiler converts it into:

```java
new StringBuilder()
    .append("Hello ")
    .append(name)
    .toString()
```

Small concatenations are optimized automatically.

---

# 23. String Formatting

## String Templates

```kotlin
val name = "Vikash"
val age = 29

println("$name is $age years old.")
```

---

## Expressions

```kotlin
println("${name.uppercase()} has ${name.length} letters.")
```

Everything inside `{}` is evaluated.

---

## Decimal Formatting

```kotlin
val price = "%.2f".format(99.5678)
```

Output

```
99.57
```

---

## Locale Formatting

```kotlin
val amount = "%,.2f".format(1234567.89)
```

Output (US Locale)

```
1,234,567.89
```

Useful for finance apps.

---

# 24. Regular Expressions (Regex)

Regex is a common Android interview topic.

## Creating Regex

```kotlin
val regex = Regex("\\d+")
```

Matches one or more digits.

---

## matches()

```kotlin
regex.matches("12345")
```

Returns

```text
true
```

---

## containsMatchIn()

```kotlin
regex.containsMatchIn("Order123")
```

Returns true because digits exist.

---

## replace()

```kotlin
val phone = "987-654-3210"

val cleaned = phone.replace(Regex("-"), "")
```

Output

```
9876543210
```

---

## split()

```kotlin
val result = "A,B;C".split(Regex("[,;]"))
```

Output

```
[A, B, C]
```

---

# 25. Regex Groups

```kotlin
val regex = Regex("(\\d+)-(\\d+)")

val match = regex.find("123-456")
```

Access captured groups.

```kotlin
println(match?.groupValues)
```

Output

```
[123-456, 123, 456]
```

---

## Named Groups

```kotlin
val regex = Regex("(?<user>\\w+)@(?<domain>\\w+\\.\\w+)")
```

Useful in parsing emails.

---

# 26. Useful Regex Patterns

## Email Validation

```kotlin
val emailRegex =
    Regex("[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}")
```

---

## Indian Mobile Number

```kotlin
Regex("^[6-9]\\d{9}$")
```

---

## OTP Validation

```kotlin
Regex("^\\d{6}$")
```

---

## URL Validation

```kotlin
Regex("https?://.*")
```

---

## Password Validation

```kotlin
Regex("^(?=.*[A-Z])(?=.*\\d).{8,}$")
```

Requires uppercase + digit + minimum length.

---

# 27. Advanced String APIs

## removePrefix()

```kotlin
"https://google.com".removePrefix("https://")
```

Output

```
google.com
```

---

## removeSuffix()

```kotlin
"Android.kt".removeSuffix(".kt")
```

Output

```
Android
```

---

## substringBefore()

```kotlin
"user@gmail.com".substringBefore("@")
```

Output

```
user
```

---

## substringAfter()

```kotlin
"user@gmail.com".substringAfter("@")
```

Output

```
gmail.com
```

---

## substringBeforeLast()

```kotlin
"photo.profile.png".substringBeforeLast(".")
```

Output

```
photo.profile
```

---

## substringAfterLast()

```kotlin
"photo.profile.png".substringAfterLast(".")
```

Output

```
png
```

---

## removeRange()

```kotlin
"AndroidCompose".removeRange(7, 14)
```

Output

```
Android
```

---

# 28. chunked()

Split string into fixed-size chunks.

```kotlin
"123456789".chunked(3)
```

Output

```
[123,456,789]
```

Useful for OTP formatting.

---

## windowed()

Sliding windows.

```kotlin
"ABCDE".windowed(3)
```

Output

```
[ABC, BCD, CDE]
```

Used in DSA and analytics.

---

## zip()

```kotlin
"ABC".zip("123")
```

Output

```
[(A,1), (B,2), (C,3)]
```

---

## padStart()

```kotlin
"25".padStart(5, '0')
```

Output

```
00025
```

---

## padEnd()

```kotlin
"Android".padEnd(10, '*')
```

Output

```
Android***
```

---

## repeat()

```kotlin
"*".repeat(5)
```

Output

```
*****
```

---

# 29. Android Localization

Never hardcode UI strings.

## ❌ Bad

```kotlin
Text("Login")
```

---

## ✅ Good

```xml
<string name="login">Login</string>
```

```kotlin
Text(stringResource(R.string.login))
```

Supports translations automatically.

---

## String Arguments

```xml
<string name="welcome">Welcome %1$s</string>
```

Compose

```kotlin
Text(
    stringResource(
        R.string.welcome,
        user.name
    )
)
```

---

## Multiple Parameters

```xml
<string name="balance">%1$s has ₹%2$d</string>
```

```kotlin
stringResource(
    R.string.balance,
    user.name,
    balance
)
```

---

# 30. Plural Resources

Android handles pluralization.

```xml
<plurals name="messages">
    <item quantity="one">%d message</item>
    <item quantity="other">%d messages</item>
</plurals>
```

Compose

```kotlin
pluralStringResource(
    R.plurals.messages,
    count,
    count
)
```

Output changes based on quantity.

---

# 31. Right-To-Left (RTL)

Enable RTL support.

```xml
<application
    android:supportsRtl="true"/>
```

Avoid manual string concatenation.

Prefer formatted resources.

---

# 32. AnnotatedString (Jetpack Compose)

Compose supports multiple styles in one Text.

```kotlin
Text(
    buildAnnotatedString {

        append("Android ")

        withStyle(
            SpanStyle(fontWeight = FontWeight.Bold)
        ) {
            append("Interview Bible")
        }
    }
)
```

---

## Multiple Colors

```kotlin
buildAnnotatedString {

    withStyle(SpanStyle(color = Color.Blue)) {
        append("Android ")
    }

    withStyle(SpanStyle(color = Color.Red)) {
        append("Compose")
    }
}
```

---

## Clickable Text

```kotlin
buildAnnotatedString {

    pushStringAnnotation(
        tag = "URL",
        annotation = "https://example.com"
    )

    append("Open Website")

    pop()
}
```

Useful for Terms & Conditions.

---

# 33. TextField String Handling

```kotlin
var name by rememberSaveable {
    mutableStateOf("")
}
```

Compose state should be immutable String values.

---

## Password Field

```kotlin
OutlinedTextField(
    value = password,
    onValueChange = { password = it },
    visualTransformation = PasswordVisualTransformation()
)
```

---

# 34. JVM String Pool Deep Dive

### Compile-Time Constant

```kotlin
val a = "Android"
val b = "Android"
```

Same pooled object.

```
a === b
true
```

---

### Runtime Concatenation

```kotlin
val suffix = "roid"

val c = "And" + suffix
```

Different object.

```
a === c
false
```

---

### Compile-Time Concatenation

```kotlin
val c = "And" + "roid"
```

Compiler folds constant.

```
a === c
true
```

---

# 35. String Interning

Move runtime strings into String Pool.

```kotlin
val runtime = String(charArrayOf('A','B','C'))

val pooled = runtime.intern()
```

Useful when many duplicate strings exist.

Rare Android usage.

---

# 36. Historical Substring Memory Leak

Older JVM versions shared the original character array.

```
Large String (100 MB)
        |
substring()
        |
Small String shares same array
```

Entire large array stayed in memory.

Modern JVM copies substring into a new array.

---

# 37. Performance Best Practices

## Avoid Regex Recreation

❌ Bad

```kotlin
Regex("\\d+").matches(text)
```

Creates Regex every call.

---

## Good

```kotlin
private val numberRegex = Regex("\\d+")
```

Compile once.

---

## Avoid String Concatenation in Loops

Prefer StringBuilder.

---

## Prefer Templates for UI

```kotlin
Text("Welcome $name")
```

Cleaner than concatenation.

---

## Avoid `toString()` on Nullable Values

Instead of:

```kotlin
user?.name.toString()
```

Use:

```kotlin
user?.name ?: ""
```

---

# 38. Common Android Interview Traps

### `String` vs `Char`

```kotlin
'A'
```

Character.

```kotlin
"A"
```

String.

---

### `length` with Emoji

```kotlin
"😀".length
```

Returns `2`, not `1`.

---

### `==` vs `===`

Always use `==` unless checking object identity.

---

### `StringBuilder` Thread Safety

Not thread-safe.

Use `StringBuffer` only when synchronization is required.

---

# 39. Senior Android Interview Questions

## Basic

1. Why are Strings immutable?
2. Difference between `String` and `Char`?
3. Difference between `==` and `===`?

## Intermediate

4. What is String Pool?
5. StringBuilder vs StringBuffer.
6. Why is `buildString` recommended?
7. Explain UTF-16.

## Advanced

8. JVM String Pool implementation.
9. What is `intern()`?
10. Compiler optimization of string templates.
11. Why is regex expensive?
12. Compose `AnnotatedString`.
13. Android localization best practices.
14. Why does emoji length differ from visible characters?

---

# 40. Cheat Sheet (Advanced)

| API | Purpose |
|------|---------|
| `buildString {}` | Efficient string creation |
| `StringBuilder.append()` | Mutable concatenation |
| `Regex.matches()` | Full match |
| `Regex.find()` | First match |
| `containsMatchIn()` | Partial match |
| `chunked()` | Fixed chunks |
| `windowed()` | Sliding window |
| `padStart()` | Left padding |
| `padEnd()` | Right padding |
| `removePrefix()` | Remove prefix |
| `substringAfter()` | After delimiter |
| `AnnotatedString` | Rich Compose text |
| `stringResource()` | Localized Android string |
| `pluralStringResource()` | Quantity-based string |

---

# 📝 Final Revision Summary

- Strings are immutable JVM objects backed by the String Pool.
- Use `StringBuilder` or `buildString {}` for repeated concatenation.
- Kotlin templates compile to `StringBuilder`.
- Regex objects should be reused instead of recreated.
- Android apps should always use localized string resources and plurals.
- Compose rich text is built with `AnnotatedString` and `SpanStyle`.
- UTF-16 encoding means visible characters and `length` are not always the same.