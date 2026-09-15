# 💜 Kotlin Variables — Complete Interview Guide (2026 Edition)

> Learn Kotlin variables from fundamentals to JVM internals with Android interview examples.

**Difficulty:** Beginner → Advanced

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Amazon • PhonePe • Uber • Flipkart • Microsoft

---

# 📚 Table of Contents

1. What are Variables?
2. val vs var
3. Type Inference
4. Variable Declaration
5. Compile-time vs Runtime
6. Memory Representation
7. JVM Bytecode
8. Android Best Practices
9. Common Mistakes
10. Interview Questions
11. Cheat Sheet

---

# 1. What are Variables?

A **variable** is a named reference that stores a value in memory.

In Kotlin, variables are **strongly typed** and **null-safe** by default.

```kotlin
val name = "Vikash"
var age = 29
```

### Why Kotlin Variables Are Different

Unlike Java, Kotlin encourages **immutability first**.

| Java | Kotlin |
|------|--------|
| `final String name` | `val name` |
| `String name` | `var name` |

**Golden Rule:** Prefer `val` unless mutation is required.

---

# 2. val vs var

## val (Immutable Reference)

```kotlin
val company = "Google"
```

Cannot be reassigned.

```kotlin
company = "Amazon" // ❌ Compilation Error
```

## var (Mutable Reference)

```kotlin
var company = "Google"

company = "Amazon" // ✅ Allowed
```

---

## Memory Difference

```text
val name = "Vikash"

Stack
┌──────────────┐
│ name ───────────────┐
└──────────────┘       │
                       ▼
Heap
┌──────────────────────┐
│ "Vikash"             │
└──────────────────────┘
```

`val` prevents changing the **reference**, not necessarily the object.

---

### Mutable Object Example

```kotlin
val list = mutableListOf("A")

list.add("B")      // ✅ Allowed

list = mutableListOf() // ❌ Not Allowed
```

Interview trap.

`val` ≠ Immutable Object.

It means immutable reference.

---

# 3. Type Inference

Kotlin automatically detects types.

```kotlin
val age = 25

// Compiler infers Int.
```

Equivalent Java.

```java
final int age = 25;
```

---

## Explicit Types

```kotlin
val age: Int = 25

val salary: Double = 150000.50

val isAndroidDeveloper: Boolean = true
```

Use explicit types for:

- Public APIs.
- Library code.
- Readability.

---

## Compiler Inference Examples

```kotlin
val number = 10          // Int

val pi = 3.14            // Double

val letter = 'A'         // Char

val message = "Hello"     // String

val list = listOf(1,2,3) // List<Int>
```

---

# 4. Variable Declaration Rules

## Valid Names

```kotlin
val firstName = "Vikash"

val totalPrice = 100
```

## Invalid Names

```kotlin
val 1name = "Vikash"

val class = "Android"
```

Keywords cannot be variable names.

---

## Backticks

Rare interview topic.

```kotlin
val `class` = "Android"

println(`class`)
```

Useful for JSON mapping and testing.

---

# 5. Primitive Types in Kotlin

| Kotlin | JVM Primitive |
|---------|---------------|
| Int | int |
| Long | long |
| Float | float |
| Double | double |
| Boolean | boolean |
| Char | char |
| Byte | byte |
| Short | short |

---

## Numeric Literals

```kotlin
val decimal = 100

val hex = 0xFF

val binary = 0b1010

val million = 1_000_000
```

Underscores improve readability.

---

# 6. Compile-Time Constants

## const val

```kotlin
const val API_VERSION = "v1"
```

Requirements.

- Top-level.
- Object.
- Companion Object.

Cannot use runtime values.

```kotlin
const val currentTime = System.currentTimeMillis()

// ❌ Compilation Error
```

---

### val vs const val

| val | const val |
|-----|-----------|
| Runtime | Compile-time |
| Can call functions | Cannot |
| Object initialization | Constant pool |

---

# 7. lateinit Variables

Used for dependency injection.

```kotlin
lateinit var repository: UserRepository
```

Cannot be primitive.

```kotlin
lateinit var count: Int

// ❌ Error
```

---

## isInitialized

```kotlin
if (::repository.isInitialized) {
    repository.getUsers()
}
```

Android interview favorite.

---

# 8. lazy Variables

```kotlin
val database by lazy {
    createDatabase()
}
```

Initialized on first access.

Benefits:

- Avoid expensive initialization.
- Thread-safe by default.

---

### Lazy Modes

```kotlin
LazyThreadSafetyMode.SYNCHRONIZED

LazyThreadSafetyMode.PUBLICATION

LazyThreadSafetyMode.NONE
```

Interview question for senior Android engineers.

---

# 9. Variable Memory Model

## Stack vs Heap

```text
fun main() {

    val name = "Android"

    var age = 25
}
```

```text
Stack Memory

main()

name ───────────┐

age = 25        │
                ▼

Heap

"Android"
```

Primitive values stored differently from object references on JVM.

---

# 10. JVM Bytecode

Kotlin

```kotlin
val age = 25
```

Decompiled Java.

```java
final int age = 25;
```

---

Kotlin

```kotlin
var age = 25
```

Java

```java
int age = 25;

age = 30;
```

Compiler generates setters/getters for properties.

---

# 11. val in Data Classes

```kotlin
data class User(

    val name: String,

    val age: Int
)
```

Immutable model.

Preferred for UI State.

Compose recommendation.

---

# 12. var in Data Classes

```kotlin
data class User(

    var name: String
)
```

Mutable model.

Can trigger unexpected UI updates.

Use carefully.

---

# 13. Android Best Practices

## UI State

```kotlin
data class HomeUiState(

    val loading: Boolean,

    val users: List<User>
)
```

Entire UI state should usually use `val`.

---

## MutableStateFlow Pattern

```kotlin
private val _state = MutableStateFlow(HomeUiState())

val state = _state.asStateFlow()
```

Expose immutable state.

Interview favorite.

---

## ViewModel Example

```kotlin
class HomeViewModel : ViewModel() {

    private val _count = MutableStateFlow(0)

    val count = _count.asStateFlow()

    fun increment() {
        _count.value++
    }
}
```

---

# 14. Performance Discussion

## Why Prefer val?

Benefits.

- Easier compiler optimizations.
- Safer multithreading.
- Easier reasoning.
- Better Compose stability.

---

### Compose Stability

`val` properties help Compose determine whether recomposition can be skipped.

```kotlin
data class UiState(

    val name: String,

    val age: Int
)
```

Preferred.

---

# 15. Common Mistakes

### Mistake 1

Using `var` everywhere.

❌

```kotlin
var name = "Android"
```

✅

```kotlin
val name = "Android"
```

---

### Mistake 2

Confusing immutable reference with immutable object.

```kotlin
val list = mutableListOf(1)

list.add(2)
```

Still mutable.

---

### Mistake 3

Using `lateinit` for nullable values.

Use nullable instead.

```kotlin
var user: User? = null
```

---

# 16. Real Android Interview Questions

## Basic

1. Difference between `val` and `var`?
2. Is `val` completely immutable?
3. Can `lateinit` be nullable?
4. Can `lateinit` be primitive?
5. What is type inference?

---

## Intermediate

6. Difference between `const val` and `val`?
7. How does `lazy` work?
8. What are lazy thread safety modes?
9. Why expose immutable state from ViewModel?
10. Difference between immutable and mutable collections?

---

## Advanced

11. How does `val` help Compose performance?
12. How does Kotlin compile `val` to JVM bytecode?
13. Are primitives always stored on the stack?
14. Explain Kotlin property backing fields.
15. Explain compile-time constants.

---

# 17. Interview Answer (2 Minutes)

> `val` creates an immutable reference, while `var` creates a mutable reference. `val` doesn't guarantee the underlying object is immutable; it only prevents reassignment of the reference. Kotlin promotes immutability because it improves thread safety, readability, and Compose performance. `const val` is a compile-time constant stored in the JVM constant pool, while `val` is initialized at runtime.

---

# 18. Cheat Sheet

| Keyword | Meaning |
|---------|---------|
| `val` | Immutable reference |
| `var` | Mutable reference |
| `const val` | Compile-time constant |
| `lateinit` | Deferred initialization |
| `lazy` | First-access initialization |
| Type Inference | Compiler detects type automatically |

---

# 📝 Revision Summary

- Prefer **`val` by default**.
- Use **`var` only for mutable state**.
- `val` does **not** make mutable objects immutable.
- `const val` is compile-time only.
- `lateinit` is mainly for Android dependency injection and lifecycle-based initialization.
- `lazy` is ideal for expensive objects like Room databases or repositories.