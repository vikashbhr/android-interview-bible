# 💜 Kotlin Type Casting — Complete Interview Guide (2026 Edition)

> Master Kotlin type casting from basic conversions to smart casts, unsafe vs safe casts, reified generics, type erasure, JVM internals, and Android interview patterns.

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • PhonePe • Uber • Amazon • Microsoft • Razorpay • Flipkart

---

# 📚 Table of Contents

1. What is Type Casting?
2. Why Kotlin Doesn't Support Implicit Casting
3. Numeric Type Conversion
4. Unsafe Cast (`as`)
5. Safe Cast (`as?`)
6. Smart Cast
7. Type Checking (`is`)
8. Casting Collections
9. Generic Type Casting & Type Erasure
10. Reified Type Parameters
11. Java Interoperability
12. Compose & Serialization Examples
13. JVM Internals
14. Performance Discussion
15. Common Mistakes
16. Interview Questions
17. Cheat Sheet

---

# 1. What is Type Casting?

**Type casting** converts one type into another.

Kotlin supports two categories:

- **Type Conversion** → Numeric conversions (`Int` → `Long`)
- **Reference Casting** → Object casting (`Any` → `String`)

Unlike Java, Kotlin separates these concepts clearly.

---

# 2. Why Kotlin Doesn't Support Implicit Casting

Java allows automatic widening.

### Java

```java
int number = 10;
long value = number;
```

Works automatically.

### Kotlin

```kotlin
val number: Int = 10

val value: Long = number
```

❌ Compilation Error

Kotlin requires explicit conversion to avoid accidental precision loss.

---

## Why?

Example:

```kotlin
val price = 10.9

val total = price.toInt()
```

Output

```
10
```

The compiler forces you to acknowledge the conversion.

---

# 3. Numeric Type Conversion

### Int → Long

```kotlin
val number = 10

val value = number.toLong()
```

### Long → Int

```kotlin
val value = 100L

val number = value.toInt()
```

### Int → Double

```kotlin
val number = 10

val decimal = number.toDouble()
```

### Double → Float

```kotlin
val value = 99.5

val floatValue = value.toFloat()
```

---

## All Conversion Functions

| Function | Converts To |
|----------|-------------|
| `toByte()` | Byte |
| `toShort()` | Short |
| `toInt()` | Int |
| `toLong()` | Long |
| `toFloat()` | Float |
| `toDouble()` | Double |
| `toChar()` | Char |
| `toUInt()` | UInt |
| `toULong()` | ULong |

---

# 4. Unsafe Cast (`as`)

`as` forcefully casts an object.

```kotlin
val value: Any = "Android"

val text = value as String

println(text.length)
```

Works because the object is actually a `String`.

---

## Runtime Crash

```kotlin
val value: Any = 100

val text = value as String
```

Runtime Exception

```
ClassCastException
```

---

## Nullable Unsafe Cast

```kotlin
val value: Any? = null

val text = value as String
```

Runtime Exception

```
NullPointerException
```

---

## Nullable Version

```kotlin
val value: Any? = "Android"

val text = value as String?
```

Allowed.

---

# 5. Safe Cast (`as?`)

Returns `null` instead of crashing.

```kotlin
val value: Any = 100

val text = value as? String

println(text)
```

Output

```
null
```

---

## Android Example

```kotlin
val activity = context as? MainActivity

activity?.showSnackBar()
```

Safe pattern.

---

## Safe Cast + Elvis

```kotlin
val activity = context as? MainActivity
    ?: return
```

Common Android production code.

---

# 6. Smart Cast

Compiler automatically casts after type checking.

```kotlin
fun printLength(value: Any){

    if(value is String){
        println(value.length)
    }
}
```

Compiler treats `value` as `String`.

---

## Smart Cast with `when`

```kotlin
when(value){

    is String -> println(value.length)

    is Int -> println(value + 1)
}
```

---

## Why Smart Cast Doesn't Work

```kotlin
var value: String? = "Android"

if(value != null){

    value = null

    println(value.length)
}
```

Compilation Error.

Reason:

Mutable (`var`) value can change.

---

## Works with `val`

```kotlin
val value: String? = "Android"

if(value != null){
    println(value.length)
}
```

---

# 7. Type Checking (`is`)

Checks runtime type.

```kotlin
val value: Any = "Android"

println(value is String)
```

Output

```
true
```

---

## Negative Check

```kotlin
if(value !is String){
    return
}
```

After return, compiler smart casts.

---

# 8. Casting Collections

## List<Any>

```kotlin
val list: List<Any> = listOf("A","B")
```

Unsafe

```kotlin
val names = list as List<String>
```

Compiles with warning.

---

## Safe Filtering

```kotlin
val names = list.filterIsInstance<String>()
```

Output

```
["A","B"]
```

Recommended.

---

## Mixed List

```kotlin
val list = listOf("A",1,true)
```

```kotlin
val strings = list.filterIsInstance<String>()
```

Output

```
["A"]
```

Very useful.

---

# 9. Generic Type Casting

### Problem

```kotlin
fun cast(list: List<Any>): List<String>{
    return list as List<String>
}
```

Compiler warning.

Why?

Type Erasure.

---

## Runtime Reality

```
List<String>

↓

List
```

Generic information is erased on JVM.

---

## Dangerous Example

```kotlin
val list = listOf(1,2,3) as List<String>

println(list.first())
```

Crash.

---

# 10. Type Erasure

One of the most common senior interview topics.

### JVM Erases Generic Types

```kotlin
List<Int>

List<String>

List<User>
```

Become

```
List
```

at runtime.

---

## Why?

Java compatibility.

---

## Consequence

Cannot check.

```kotlin
if(list is List<String>)
```

Compilation Error.

---

## Allowed

```kotlin
if(list is List<*>)
```

Star Projection.

---

# 11. Star Projection (`*`)

Represents unknown generic type.

```kotlin
fun printItems(list: List<*>){
    list.forEach {
        println(it)
    }
}
```

Safer than `List<Any>`.

---

## Difference

| `List<Any>` | `List<*>` |
|-------------|-----------|
| Elements are Any | Unknown type |
| Can add Any | Read-only unknown |

Interview favorite.

---

# 12. Reified Type Parameters

Huge Kotlin interview topic.

Normally impossible.

```kotlin
fun <T> check(value: Any){
    value is T
}
```

Compilation Error.

---

## Solution

```kotlin
inline fun <reified T> check(value: Any): Boolean{
    return value is T
}
```

Usage.

```kotlin
check<String>("Android")
```

Returns true.

---

## Why `inline`?

Compiler replaces function call with actual type.

No type erasure.

---

## Android Example

```kotlin
inline fun <reified T: Activity>
Context.findActivity(): T?{
    return this as? T
}
```

Very common utility.

---

# 13. Safe Generic Casting Utility

```kotlin
inline fun <reified T> Any.castOrNull(): T?{
    return this as? T
}
```

Usage.

```kotlin
val name = value.castOrNull<String>()
```

---

# 14. Java Interoperability

Java platform types.

```java
Object getData();
```

Kotlin

```kotlin
val value: Any! = javaApi.getData()
```

Need safe cast.

```kotlin
val user = value as? User
```

---

## Serializable Example

```kotlin
val user = intent.getSerializableExtra("user") as? User
```

Safe approach.

---

## Parcelable Example

```kotlin
intent.getParcelableExtra<User>("user")
```

Preferred.

---

# 15. JSON Serialization Example

```kotlin
val json = Json.decodeFromString<User>(response)
```

Reified API internally.

---

## Moshi/Gson Example

```kotlin
val user = gson.fromJson(json, User::class.java)
```

Uses Java class reference.

---

# 16. Compose Navigation Example

```kotlin
val parent = LocalContext.current as? Activity
```

Safe cast avoids crash during Preview.

---

## ViewModel Factory Example

```kotlin
val owner = LocalViewModelStoreOwner.current
    as? NavBackStackEntry
```

Common Compose navigation pattern.

---

# 17. JVM Internals

### `as`

Compiler inserts `CHECKCAST`.

```
CHECKCAST java/lang/String
```

Throws `ClassCastException` if invalid.

---

### `is`

Compiler inserts

```
INSTANCEOF
```

Returns Boolean.

---

### Smart Cast

Compiler performs control-flow analysis.

No runtime cast if already guaranteed.

---

# 18. Boxing and Casting

```kotlin
val value: Int = 10

val number: Number = value
```

Primitive boxed into `Integer`.

---

## Number to Int

```kotlin
val number: Number = 10L

val intValue = number.toInt()
```

Uses conversion function.

---

# 19. Performance Discussion

## `is`

Very fast JVM `INSTANCEOF`.

---

## `as`

Runtime cast instruction.

Slight overhead.

---

## Smart Cast

Compile-time optimization.

No additional runtime cost.

---

## Reified

Avoids reflection.

Faster generic utilities.

---

# 20. Android Best Practices

### Prefer Smart Cast

```kotlin
if(user is AdminUser){
    user.permissions()
}
```

---

### Prefer Safe Cast

```kotlin
val activity = context as? Activity
```

---

### Avoid `as`

Unless guaranteed.

---

### Prefer `filterIsInstance()`

Instead of unsafe collection casts.

---

# 21. Common Mistakes

### Mistake 1

Using `as` instead of `as?`.

---

### Mistake 2

Expecting implicit numeric conversion.

---

### Mistake 3

Checking generic types directly.

```kotlin
list is List<String>
```

Not allowed.

---

### Mistake 4

Ignoring type erasure.

---

### Mistake 5

Using reflection when reified works.

---

# 22. Real Android Interview Questions

## Basic

1. Difference between `as` and `as?`.
2. Difference between `is` and `as`.
3. Why doesn't Kotlin allow implicit numeric conversion?
4. What is smart cast?
5. When does smart cast fail?

---

## Intermediate

6. Explain type erasure.
7. Explain star projection.
8. Explain `filterIsInstance()`.
9. Why does `List<String>` lose type at runtime?
10. Explain `CHECKCAST` and `INSTANCEOF`.

---

## Advanced

11. Why must reified functions be inline?
12. How does Kotlin implement smart casts?
13. Difference between reflection and reified generics.
14. Explain casting nullable platform types.
15. Explain Compose safe casting patterns.

---

# 23. 2-Minute Interview Answer

> Kotlin distinguishes numeric conversions from object casting. Numeric conversions are always explicit using functions like `toLong()` or `toInt()`. Object casting uses `as` for unsafe casts and `as?` for safe casts that return null instead of throwing `ClassCastException`. Smart casts are a compiler feature that automatically casts values after successful type checks. Due to JVM type erasure, generic type information is unavailable at runtime unless inline reified type parameters are used.

---

# 24. Cheat Sheet

| Syntax | Purpose |
|--------|---------|
| `is` | Type check |
| `!is` | Negative type check |
| `as` | Unsafe cast |
| `as?` | Safe cast |
| `toInt()` | Numeric conversion |
| `toLong()` | Numeric conversion |
| `filterIsInstance<T>()` | Safe collection filtering |
| `List<*>` | Star projection |
| `inline reified` | Runtime generic type access |

---

# 📝 Revision Summary

- Kotlin **never performs implicit numeric conversion**.
- Use `toInt()`, `toLong()`, etc. for numeric conversion.
- `as` throws `ClassCastException`; `as?` returns `null`.
- Smart casts are compile-time optimizations after `is` checks.
- JVM erases generic types (**type erasure**).
- `inline + reified` is Kotlin's solution for runtime generic type information.
- Prefer `filterIsInstance<T>()` over unsafe collection casts in Android code.