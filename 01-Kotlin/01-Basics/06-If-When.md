# 💜 Kotlin If & When — Complete Interview Guide (2026 Edition)

> Master Kotlin conditional expressions from basics to exhaustive `when`, smart casts, sealed classes, Compose UI state reducers, JVM bytecode, and performance.

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • PhonePe • Uber • Amazon • Microsoft • Razorpay

---

# 📚 Table of Contents

1. What are Conditional Expressions?
2. `if` as Statement vs Expression
3. Nested `if`
4. `when` Basics
5. `when` as Expression
6. `when` with Multiple Conditions
7. `when` with Ranges
8. `when` with Collections (`in`)
9. `when` with Smart Casts (`is`)
10. Exhaustive `when`
11. `when` with Enums
12. `when` with Sealed Classes
13. Compose UI State Pattern
14. JVM Bytecode
15. Performance Discussion
16. Best Practices
17. Common Mistakes
18. Interview Questions
19. Cheat Sheet

---

# 1. What are Conditional Expressions?

Conditional expressions allow the program to execute different code paths based on conditions.

Kotlin has two primary conditional constructs:

- `if`
- `when`

Unlike Java, **both can return values**.

---

# 2. `if` as Statement

Traditional control flow.

```kotlin
if (age >= 18) {
    println("Eligible")
}
```

Nothing is returned.

---

## `if` as Expression

A major Kotlin feature.

```kotlin
val status = if (age >= 18) {
    "Adult"
} else {
    "Minor"
}
```

Output:

```text
Adult
```

The entire `if` returns a value.

### Java Equivalent

```java
String status;

if(age >= 18){
    status = "Adult";
}else{
    status = "Minor";
}
```

Kotlin removes temporary variables.

---

# 3. Expression Body Example

```kotlin
fun max(a: Int, b: Int) =
    if (a > b) a else b
```

Compiler infers return type.

---

# 4. Nested `if`

```kotlin
if (user != null) {
    if (user.isPremium) {
        println("Premium User")
    }
}
```

---

## Better Kotlin Style

```kotlin
if (user?.isPremium == true) {
    println("Premium User")
}
```

Cleaner and null-safe.

---

# 5. `when` Basics

`when` is Kotlin's replacement for Java `switch`.

```kotlin
when(day) {
    1 -> println("Monday")
    2 -> println("Tuesday")
    else -> println("Unknown")
}
```

---

## Multiple Values

```kotlin
when(day) {
    1, 7 -> println("Weekend")
    2,3,4,5,6 -> println("Working Day")
}
```

---

# 6. `when` as Expression

```kotlin
val result = when(score) {
    in 90..100 -> "A"
    in 80..89 -> "B"
    else -> "C"
}
```

Returns `String`.

---

## No Temporary Variable Needed

```kotlin
return when(role){
    ADMIN -> "Admin"
    USER -> "User"
}
```

---

# 7. `when` Without Argument

Acts like chained `if`.

```kotlin
when {
    age < 13 -> "Child"
    age < 18 -> "Teen"
    else -> "Adult"
}
```

Very common in Android reducers.

---

# 8. `when` with Ranges

```kotlin
when(score){
    in 90..100 -> "A"
    in 80..89 -> "B"
    in 70..79 -> "C"
    else -> "Fail"
}
```

Compiler uses range checks.

---

# 9. `when` with `in`

Collection membership.

```kotlin
when(color){
    in listOf("Red","Pink") -> "Warm"
    in listOf("Blue","Green") -> "Cool"
    else -> "Unknown"
}
```

---

# 10. `when` with Type Checking (`is`)

Smart casting.

```kotlin
fun print(value: Any){

    when(value){

        is String -> println(value.length)

        is Int -> println(value + 10)

        else -> println(value)
    }
}
```

No explicit casting required.

---

## Multiple Type Checks

```kotlin
when(value){
    is Int, is Long -> println("Number")
}
```

---

# 11. Smart Cast Inside `when`

```kotlin
when(val response = apiResult){

    is Success -> showData(response.data)

    is Error -> showError(response.message)

    is Loading -> showLoading()
}
```

Very common MVI pattern.

---

# 12. Exhaustive `when`

Compiler ensures every case is handled.

## Enum Example

```kotlin
enum class Theme{
    LIGHT,DARK,SYSTEM
}

val color = when(theme){
    Theme.LIGHT -> White
    Theme.DARK -> Black
    Theme.SYSTEM -> Gray
}
```

No `else` needed.

---

## Why Exhaustive Matters?

Compiler warns if a new enum value is added.

Safer than Java.

---

# 13. `when` with Sealed Classes

Most important Android interview topic.

```kotlin
sealed class UiState{

    object Loading:UiState()

    data class Success(val users:List<User>):UiState()

    data class Error(val message:String):UiState()
}
```

---

### UI Rendering

```kotlin
when(state){

    is UiState.Loading -> LoadingView()

    is UiState.Success -> UserList(state.users)

    is UiState.Error -> ErrorView(state.message)
}
```

No `else`.

---

## Why Sealed + when?

- Compile-time safety.
- Better Compose rendering.
- Better MVI reducers.

---

# 14. Compose UI State Reducer Pattern

```kotlin
@Composable
fun HomeScreen(state: HomeUiState){

    when(state){

        HomeUiState.Loading -> LoadingScreen()

        is HomeUiState.Success ->
            UserList(state.users)

        is HomeUiState.Error ->
            ErrorScreen(state.message)
    }
}
```

This is the recommended Compose architecture pattern.

---

# 15. ViewModel Reducer Example

```kotlin
private fun reduce(action:HomeAction){

    _state.value = when(action){

        is HomeAction.Refresh ->
            HomeUiState.Loading

        is HomeAction.Success ->
            HomeUiState.Success(action.users)

        is HomeAction.Error ->
            HomeUiState.Error(action.message)
    }
}
```

Frequently asked in PhonePe/Uber interviews.

---

# 16. `if` vs `when`

| `if` | `when` |
|------|--------|
| Two-way branching | Multiple branches |
| Boolean expressions | Values, ranges, types |
| Good for simple logic | Better for state machines |
| Nested becomes messy | Cleaner syntax |

---

# 17. JVM Bytecode

## `if`

```kotlin
if(a>b) a else b
```

Compiles to JVM jump instructions.

```
IF_ICMPGT
GOTO
```

---

## `when` on Integers

Compiler generates optimized switch instructions.

```
TABLESWITCH
```

or

```
LOOKUPSWITCH
```

depending on value distribution.

---

## `when` on Strings

Compiler generates hash-based lookup.

Similar to modern Java switch implementation.

---

# 18. Performance Discussion

### Integer `when`

Fast lookup.

### Enum `when`

Compiler generates mapping array.

### String `when`

Uses hashcode + equality.

### Range `when`

Sequential range checks.

---

# 19. Android Best Practices

## UI State

Prefer `when` over nested `if`.

---

## Navigation

```kotlin
when(destination){
    HOME -> HomeScreen()
    PROFILE -> ProfileScreen()
}
```

---

## Permission Handling

```kotlin
when(permissionState.status){
    Granted -> ...
    Denied -> ...
}
```

---

# 20. Common Mistakes

### Mistake 1

Using nested `if` for state rendering.

Prefer sealed class + `when`.

---

### Mistake 2

Using `else` unnecessarily with sealed classes.

Compiler loses exhaustiveness checking.

---

### Mistake 3

Returning different types.

```kotlin
val result = when(score){
    1 -> "One"
    else -> 2
}
```

Type becomes `Any`.

---

# 21. Interview Questions

## Basic

1. Difference between `if` statement and expression?
2. Difference between `when` and Java switch?
3. Can `when` return a value?
4. What is exhaustive `when`?

## Intermediate

5. `when` with ranges?
6. `when` with collections?
7. `when` with type checks?
8. Smart cast inside `when`?

## Advanced

9. `when` with sealed classes.
10. JVM bytecode generated for `when`.
11. Difference between `TABLESWITCH` and `LOOKUPSWITCH`.
12. Why is exhaustive `when` useful in Compose?
13. Performance comparison of `if` vs `when`.

---

# 22. 2-Minute Interview Answer

> Kotlin's `if` and `when` are expressions, meaning they can return values directly. `when` is much more powerful than Java's `switch` because it supports ranges, collections, type checks, smart casts, and sealed classes. With sealed classes and enums, `when` becomes exhaustive, allowing the compiler to verify that every possible state is handled. This makes it ideal for Compose UI state rendering and MVI reducers.

---

# 23. Cheat Sheet

| Syntax | Purpose |
|--------|---------|
| `if (...)` | Conditional statement |
| `val x = if (...)` | Conditional expression |
| `when(value)` | Value matching |
| `when { ... }` | Boolean conditions |
| `in 1..10` | Range matching |
| `is String` | Type matching |
| `!is User` | Negative type check |
| Sealed + `when` | Exhaustive state handling |

---

# 📝 Revision Summary

- `if` is both a statement and an expression.
- `when` is Kotlin's powerful replacement for `switch`.
- Prefer `when` for UI state rendering.
- Use sealed classes + exhaustive `when` in Compose/MVI.
- `when` supports ranges, collections, smart casts, and enums.
- Integer `when` compiles to optimized JVM switch instructions.