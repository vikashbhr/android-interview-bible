# 💜 Kotlin Type Inference — Complete Interview Guide (2026 Edition)

> Master Kotlin's type inference system from compiler basics to generic inference, smart casts, builder inference, and JVM behavior.

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • PhonePe • Uber • Amazon • Microsoft • Razorpay

---

# 📚 Table of Contents

1. What is Type Inference?
2. How Kotlin Compiler Infers Types
3. Local Type Inference
4. Function Return Type Inference
5. Generic Type Inference
6. Builder Type Inference (Kotlin 2.x)
7. Smart Cast Inference
8. Type Inference in Lambdas
9. Compose Type Inference
10. JVM & Compiler Internals
11. Performance Discussion
12. Common Mistakes
13. Interview Questions
14. Cheat Sheet

---

# 1. What is Type Inference?

**Type inference** means the Kotlin compiler automatically determines the type of an expression without requiring you to explicitly declare it.

Instead of writing:

```kotlin
val age: Int = 28
```

You simply write:

```kotlin
val age = 28
```

Compiler infers:

```kotlin
val age: Int
```

## Why Kotlin Uses Type Inference?

- Less boilerplate.
- Safer than dynamically typed languages.
- Better readability.
- Compile-time safety.
- Better IDE autocomplete.

---

# 2. How Kotlin Compiler Infers Types

The compiler analyzes the expression on the right side.

```kotlin
val name = "Android"
```

### Compiler Steps

```
Source Code
     │
     ▼
Lexical Analysis
     │
     ▼
Parser
     │
     ▼
Type Resolver
     │
     ▼
Inferred Type = String
```

The inferred type becomes part of the program during compilation.

---

# 3. Local Variable Type Inference

Most common inference.

```kotlin
val count = 10
```

Compiler infers:

```kotlin
Int
```

### More Examples

```kotlin
val isPremium = true        // Boolean

val salary = 99.99          // Double

val city = "Delhi"          // String

val letters = listOf("A")   // List<String>
```

---

## Numeric Inference Rules

| Expression | Inferred Type |
|------------|---------------|
| `10` | Int |
| `10L` | Long |
| `10F` | Float |
| `10.0` | Double |
| `'A'` | Char |

---

# 4. Function Return Type Inference

Kotlin can infer return types for expression functions.

### Expression Function

```kotlin
fun square(number: Int) = number * number
```

Compiler infers:

```kotlin
Int
```

Equivalent:

```kotlin
fun square(number: Int): Int {
    return number * number
}
```

---

## Block Body Doesn't Infer

```kotlin
fun square(number: Int) {
    return number * number
}
```

Compilation error.

Need explicit return type.

```kotlin
fun square(number: Int): Int {
    return number * number
}
```

---

## Interview Rule

Expression body → Return type inferred.

Block body → Explicit return type recommended.

---

# 5. Collection Type Inference

```kotlin
val users = listOf("A", "B")
```

Compiler infers:

```kotlin
List<String>
```

### Mutable Collection

```kotlin
val users = mutableListOf("A")
```

Type:

```kotlin
MutableList<String>
```

---

## Mixed Types

```kotlin
val values = listOf(1, "Android")
```

Compiler infers:

```kotlin
List<Any>
```

Common interview question.

---

# 6. Generic Type Inference

Kotlin infers generic types automatically.

### Example

```kotlin
val names = mutableListOf("Vikash")
```

Compiler infers:

```kotlin
MutableList<String>
```

---

### Generic Function

```kotlin
fun <T> printItem(item: T) {
    println(item)
}

printItem("Android")
```

Compiler infers:

```kotlin
T = String
```

---

### Multiple Generic Parameters

```kotlin
fun <K, V> createPair(key: K, value: V): Pair<K, V> {
    return Pair(key, value)
}

val pair = createPair("Age", 29)
```

Compiler infers:

```
Pair<String, Int>
```

---

# 7. Builder Type Inference (Kotlin 2.x)

A modern Kotlin interview topic.

### Example

```kotlin
val numbers = buildList {
    add(1)
    add(2)
    add(3)
}
```

Compiler infers:

```kotlin
List<Int>
```

No explicit generic needed.

---

## Complex Builder Example

```kotlin
val map = buildMap {
    put("A", 1)
    put("B", 2)
}
```

Compiler infers:

```kotlin
Map<String, Int>
```

Builder inference became significantly smarter in Kotlin 2.x.

---

# 8. Lambda Type Inference

The compiler infers parameter and return types.

### Example

```kotlin
val multiply = { x: Int, y: Int ->
    x * y
}
```

Type inferred:

```kotlin
(Int, Int) -> Int
```

---

### Implicit `it`

```kotlin
val names = listOf("A", "B")

names.forEach {
    println(it)
}
```

Compiler infers:

```kotlin
it: String
```

---

### Map Operator

```kotlin
val lengths = names.map {
    it.length
}
```

Compiler infers:

```
List<Int>
```

---

# 9. Smart Cast Inference

One of Kotlin's biggest language features.

### Example

```kotlin
fun printLength(value: Any) {
    if (value is String) {
        println(value.length)
    }
}
```

Compiler automatically casts `value` to `String`.

---

## Nullable Smart Cast

```kotlin
var name: String? = "Android"

if (name != null) {
    println(name.length)
}
```

Inside the block:

```kotlin
name: String
```

---

## When Smart Cast Doesn't Work

```kotlin
var name: String? = "Android"

if (name != null) {
    name = null
    println(name.length)
}
```

Compiler error.

Reason:

Variable is mutable.

---

# 10. Type Inference with `when`

```kotlin
val result = when(score) {
    in 90..100 -> "A"
    in 80..89 -> "B"
    else -> "C"
}
```

Compiler infers:

```kotlin
String
```

---

## Different Branch Types

```kotlin
val result = when(score) {
    1 -> "One"
    else -> 2
}
```

Compiler infers:

```
Any
```

---

# 11. Type Inference in Compose

Compose heavily relies on type inference.

### State

```kotlin
val count = remember {
    mutableStateOf(0)
}
```

Compiler infers:

```kotlin
MutableState<Int>
```

---

### Derived State

```kotlin
val isEven by remember {
    derivedStateOf {
        count.value % 2 == 0
    }
}
```

Compiler infers:

```
Boolean
```

---

### rememberSaveable

```kotlin
val name by rememberSaveable {
    mutableStateOf("")
}
```

Compiler infers:

```
MutableState<String>
```

---

# 12. Type Inference vs Explicit Types

## Inference Preferred

```kotlin
val users = repository.getUsers()
```

Cleaner.

---

## Explicit Preferred

Public APIs.

```kotlin
val users: List<User> = repository.getUsers()
```

Improves readability.

---

## Library Code

Always prefer explicit return types.

```kotlin
fun users(): List<User>
```

---

# 13. JVM Bytecode View

Kotlin

```kotlin
val number = 10
```

Generated Java

```java
final int number = 10;
```

---

### Generic Example

Kotlin

```kotlin
val users = listOf("A")
```

Java

```java
List<String> users = Collections.singletonList("A");
```

---

# 14. Performance Discussion

## Why Type Inference Has No Runtime Cost

Inference happens entirely during compilation.

Runtime already knows the resolved types.

### Benefits

- Better optimization.
- Better bytecode.
- No reflection involved.
- No runtime penalty.

---

## Compose Optimization

Stable inferred types help Compose skip recomposition.

Example:

```kotlin
val uiState = remember {
    mutableStateOf(HomeUiState())
}
```

Compiler knows the exact generic type.

---

# 15. Common Mistakes

### Mistake 1

Mixed collection types.

```kotlin
val list = listOf(1, "Android")
```

Result:

```
List<Any>
```

Avoid unless intentional.

---

### Mistake 2

Using `Any` unnecessarily.

```kotlin
val value: Any = "Android"
```

Loses type safety.

---

### Mistake 3

Relying on inference in public APIs.

Prefer explicit return types.

---

### Mistake 4

Mutable variables prevent smart casts.

Use `val` whenever possible.

---

# 16. Interview Questions

## Basic

1. What is type inference?
2. Does Kotlin infer return types?
3. Difference between expression body and block body?
4. What type is inferred for `10.0`?
5. What type is inferred for `listOf(1,2,3)`?

---

## Intermediate

6. Explain generic type inference.
7. Explain smart casts.
8. Why doesn't smart cast work on mutable variables?
9. Explain lambda type inference.
10. Explain builder inference.

---

## Advanced

11. Explain Kotlin type inference algorithm.
12. Explain constraint solving during generic inference.
13. Explain Compose type inference.
14. Explain bytecode generated after inference.
15. Does type inference affect runtime performance?

---

# 17. 2-Minute Interview Answer

> Kotlin type inference is a compile-time feature where the compiler determines the type of variables, expressions, lambdas, generics, and function return values. It improves readability without sacrificing type safety because all inferred types become explicit during compilation. Smart casts, builder inference, and generic inference are extensions of the same type resolution system used by the Kotlin compiler.

---

# 18. Cheat Sheet

| Expression | Inferred Type |
|------------|---------------|
| `10` | Int |
| `10L` | Long |
| `10.0` | Double |
| `"Hello"` | String |
| `'A'` | Char |
| `true` | Boolean |
| `listOf("A")` | List<String> |
| `mutableListOf(1)` | MutableList<Int> |
| `buildList { add(1) }` | List<Int> |

---

# 📝 Revision Summary

- Type inference is **compile-time only**.
- Kotlin infers variables, generics, lambdas, and expression function return types.
- Smart casts work only when the compiler can guarantee immutability.
- Builder inference is an advanced Kotlin 2.x feature.
- Type inference has **zero runtime overhead**.
- Prefer explicit types for public APIs and library code.