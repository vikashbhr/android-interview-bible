# 💜 Kotlin Data Types — Complete Interview Guide (2026 Edition)

> Understand Kotlin data types from language fundamentals to JVM memory representation, boxing/unboxing, unsigned types, and Android performance.

**Difficulty:** Beginner → Advanced

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Amazon • Uber • PhonePe • Microsoft • Razorpay

---

# 📚 Table of Contents

1. What are Data Types?
2. Primitive vs Reference Types
3. Kotlin Numeric Types
4. String
5. Char
6. Boolean
7. Any, Unit, Nothing
8. Unsigned Types
9. Type Conversion
10. Boxing & Unboxing
11. JVM Memory Representation
12. Android Best Practices
13. Common Mistakes
14. Interview Questions
15. Cheat Sheet

---

# 1. What are Data Types?

A **data type** defines what kind of value a variable can hold.

Kotlin is **statically typed**, meaning every variable has a type known at compile time.

```kotlin
val age: Int = 28
val name: String = "Vikash"
val isAndroidDeveloper: Boolean = true
```

The compiler uses the type to:

- Validate assignments.
- Generate JVM bytecode.
- Optimize memory.
- Prevent invalid operations.

---

# 2. Kotlin Data Type Hierarchy

```
                 Any
               /  |  \
            Number String Boolean
              |
      Byte Short Int Long
            Float Double
```

Everything in Kotlin is conceptually an object, but many numeric types compile to JVM primitives.

---

# 3. Primitive vs Reference Types

## Kotlin View

```kotlin
val age = 28
val name = "Android"
```

Everything behaves like an object.

## JVM Reality

| Kotlin | JVM |
|--------|-----|
| Int | int |
| Long | long |
| Boolean | boolean |
| Double | double |
| Char | char |
| String | java.lang.String |

The compiler maps Kotlin primitive types to JVM primitives whenever possible.

---

# 4. Numeric Types

## Integer Types

| Type | Size | Range |
|------|-----:|------:|
| Byte | 8-bit | -128 to 127 |
| Short | 16-bit | -32,768 to 32,767 |
| Int | 32-bit | ±2.1 Billion |
| Long | 64-bit | ±9 Quintillion |

### Examples

```kotlin
val byteValue: Byte = 127
val shortValue: Short = 32000
val intValue: Int = 100000
val longValue: Long = 10000000000L
```

Notice the `L` suffix.

---

## Floating Point Types

| Type | Size |
|------|------|
| Float | 32-bit |
| Double | 64-bit |

```kotlin
val piFloat = 3.14F
val piDouble = 3.1415926535
```

`Double` is Kotlin's default floating-point type.

---

## Numeric Literals

```kotlin
val decimal = 100

val hex = 0xFF

val binary = 0b1010

val million = 1_000_000

val longValue = 100L

val floatValue = 12.5F
```

Underscores improve readability.

---

# 5. String

Strings are immutable.

```kotlin
val language = "Kotlin"
```

### Multiline String

```kotlin
val message = """
Welcome
to
Android Interview Bible
""".trimIndent()
```

### String Template

```kotlin
val name = "Vikash"

println("Hello $name")

println("Length = ${name.length}")
```

---

## String Memory

```
Stack

name ─────────────┐
                  ▼
Heap

"Kotlin"
```

Strings are stored on the heap.

---

# 6. Char

A character stores exactly one Unicode character.

```kotlin
val grade: Char = 'A'
```

Difference:

```kotlin
'A'      // Char

"A"      // String
```

---

## Unicode Example

```kotlin
val heart = '\u2764'
```

Useful in Android localization.

---

# 7. Boolean

Represents true or false.

```kotlin
val isLoggedIn = true

val isPremium = false
```

### Boolean Expressions

```kotlin
val eligible = age >= 18
```

---

# 8. Any, Unit and Nothing

These are favorite interview topics.

## Any

Root type of all Kotlin classes.

```kotlin
fun printValue(value: Any) {
    println(value)
}
```

Equivalent to Java's `Object`.

---

## Unit

Represents "no meaningful return value."

```kotlin
fun showToast(): Unit {
    println("Hello")
}
```

Equivalent to Java `void`.

---

## Nothing

Represents a function that never returns.

```kotlin
fun fail(message: String): Nothing {
    throw IllegalArgumentException(message)
}
```

Used for exceptions and infinite loops.

---

# 9. Unsigned Types

Introduced to represent positive-only numbers.

| Signed | Unsigned |
|--------|----------|
| Byte | UByte |
| Short | UShort |
| Int | UInt |
| Long | ULong |

Example.

```kotlin
val color: UInt = 0xFF0000FFu
```

Common Android use cases:

- ARGB Colors.
- Bitmap values.
- Binary protocols.

---

# 10. Type Conversion

Kotlin does **not** perform implicit numeric conversion.

❌ Invalid.

```kotlin
val number: Int = 10

val longValue: Long = number
```

Compiler error.

---

## Explicit Conversion

```kotlin
val number = 10

val longValue = number.toLong()

val floatValue = number.toFloat()

val doubleValue = number.toDouble()
```

---

## Available Conversion Functions

```kotlin
toByte()

toShort()

toInt()

toLong()

toFloat()

toDouble()

toChar()
```

---

# 11. Boxing and Unboxing

One of the most important senior interview topics.

## Primitive

```kotlin
val number = 10
```

Compiles to JVM primitive.

```
int number = 10;
```

---

## Boxed Integer

```kotlin
val list = listOf(1,2,3)
```

Inside collections:

```
Integer
```

Objects are boxed.

---

## Boxing Example

```kotlin
val a: Int = 100

val b: Int? = a
```

`Int?` becomes `Integer`.

---

## Unboxing

```kotlin
val nullable: Int? = 10

val primitive = nullable!!
```

Compiler converts wrapper → primitive.

---

## Performance Impact

Avoid nullable primitives in performance-sensitive code.

---

# 12. JVM Memory Representation

## Primitive

```
Stack

age = 25
```

Stored directly.

---

## Object

```
Stack

name ───────────┐
                ▼

Heap

"Kotlin"
```

Reference points to heap object.

---

## Collection Memory

```
Stack

numbers ───────────────┐
                       ▼

Heap

ArrayList

 ├── Integer(1)
 ├── Integer(2)
 └── Integer(3)
```

Each element is boxed.

---

# 13. Type Inference

Compiler infers types automatically.

```kotlin
val age = 28

// Int

val price = 99.5

// Double

val names = listOf("A","B")

// List<String>
```

---

# 14. Smart Cast Example

```kotlin
fun printLength(value: Any) {

    if (value is String) {

        println(value.length)
    }
}
```

Compiler smart casts to `String`.

---

# 15. Android Best Practices

## Use Int for Resource IDs

```kotlin
@DrawableRes val icon: Int
```

---

## Use Long for Time

```kotlin
val timestamp = System.currentTimeMillis()
```

---

## Use Immutable String

```kotlin
val title = "Android Interview Bible"
```

---

## UI State

```kotlin
data class UserUiState(

    val name: String,

    val age: Int,

    val isLoading: Boolean
)
```

Compose prefers immutable state.

---

# 16. Common Mistakes

## Mistake 1

Using Float unnecessarily.

Prefer Double unless API requires Float.

---

## Mistake 2

Expecting implicit conversion.

```kotlin
val number = 10

val longValue: Long = number
```

Not allowed.

---

## Mistake 3

Using String instead of Char.

```kotlin
'A'

"A"
```

Different types.

---

## Mistake 4

Ignoring boxing.

```kotlin
List<Int>
```

Creates boxed Integer objects.

---

# 17. Real Interview Questions

## Basic

1. What are Kotlin primitive types?
2. Difference between Float and Double?
3. Difference between Char and String?
4. What is Any?
5. What is Unit?

---

## Intermediate

6. Why doesn't Kotlin allow implicit conversion?
7. Difference between Any and Object?
8. What is Nothing?
9. Explain unsigned types.
10. Explain type inference.

---

## Advanced

11. Explain boxing and unboxing.
12. Why is `Int?` different from `Int`?
13. How are primitives represented on JVM?
14. Explain smart cast internals.
15. Explain Kotlin numeric literal optimizations.

---

# 18. 2-Minute Interview Answer

> Kotlin has both primitive-like numeric types and object types. On the JVM, the compiler maps non-nullable numeric types like `Int` to primitives (`int`) for performance, while nullable types like `Int?` are boxed into wrapper classes (`Integer`). Kotlin requires explicit numeric conversion to avoid implicit precision loss. `Any` is the root type, `Unit` represents no return value, and `Nothing` represents code paths that never return.

---

# 19. Cheat Sheet

| Type | JVM | Nullable Version |
|------|-----|------------------|
| Int | int | Integer |
| Long | long | Long |
| Double | double | Double |
| Float | float | Float |
| Boolean | boolean | Boolean |
| Char | char | Character |
| String | String | String |

---

# 📝 Revision Summary

- Kotlin is **statically typed**.
- `Double` is the default decimal type.
- No implicit numeric conversion.
- `Any` is the root type.
- `Unit` replaces `void`.
- `Nothing` represents unreachable code.
- Nullable primitives are boxed.
- Prefer immutable (`val`) values in Android UI state.