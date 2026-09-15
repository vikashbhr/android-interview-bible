# 💜 Kotlin Null Safety — Complete Interview Guide (2026 Edition)

> Master Kotlin Null Safety from language fundamentals to compiler internals, Java interoperability, Compose state management, and production Android best practices.

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • PhonePe • Uber • Amazon • Microsoft • Razorpay • Flipkart

---

# 📚 Table of Contents

1. What is Null Safety?
2. Nullable vs Non-Nullable Types
3. Safe Call Operator (`?.`)
4. Elvis Operator (`?:`)
5. Not-Null Assertion (`!!`)
6. Safe Cast (`as?`)
7. Smart Casts
8. `let`, `run`, `takeIf` with Nullables
9. Platform Types (`String!`)
10. Java Interoperability
11. Compose Null Safety
12. JVM & Compiler Internals
13. Performance Discussion
14. Common Mistakes
15. Interview Questions
16. Cheat Sheet

---

# 1. What is Null Safety?

**Null Safety** is Kotlin's language feature that eliminates the majority of `NullPointerException (NPE)` errors at compile time.

In Java:

```java
String name = null;
System.out.println(name.length()); // Runtime NPE
```

In Kotlin:

```kotlin
val name: String = null
```

Compilation Error.

The compiler prevents assigning `null` to non-nullable variables.

---

# Why Kotlin Introduced Null Safety?

Tony Hoare called the null reference:

> "My billion-dollar mistake."

Kotlin makes **nullability part of the type system**.

---

# 2. Nullable vs Non-Nullable Types

## Non-Nullable Type

```kotlin
val name: String = "Android"
```

Cannot hold null.

```kotlin
name = null
```

Compilation Error.

---

## Nullable Type

```kotlin
val name: String? = "Android"

val city: String? = null
```

`String?` means the value **may be null**.

---

## Comparison

| Type | Can Hold Null? |
|------|----------------|
| `String` | ❌ No |
| `String?` | ✅ Yes |
| `Int` | ❌ No |
| `Int?` | ✅ Yes |
| `User` | ❌ No |
| `User?` | ✅ Yes |

---

# 3. Nullable Memory Representation

```kotlin
val user: User? = null
```

ASCII Memory Diagram

```
Stack

user ─────► null
```

---

```kotlin
val user: User? = User("Vikash")
```

```
Stack

user ───────────────┐
                    ▼

Heap

User

name = "Vikash"
```

A nullable variable is simply a reference that can point to `null`.

---

# 4. Safe Call Operator (`?.`)

Safely accesses a property only when the object is not null.

```kotlin
val name: String? = "Android"

println(name?.length)
```

Output

```
7
```

---

## If Null

```kotlin
val name: String? = null

println(name?.length)
```

Output

```
null
```

No crash.

---

## Nested Safe Calls

```kotlin
user?.address?.city?.uppercase()
```

Stops evaluation immediately if any reference is null.

---

### Java Equivalent

```java
if(user != null &&
   user.getAddress() != null &&
   user.getAddress().getCity() != null){
}
```

Much cleaner.

---

# 5. Elvis Operator (`?:`)

Provides a default value when nullable expression is null.

```kotlin
val name: String? = null

val result = name ?: "Guest"
```

Output

```
Guest
```

---

## Android Example

```kotlin
val title = intent.getStringExtra("title") ?: "Untitled"
```

Very common interview question.

---

## Throw with Elvis

```kotlin
val token = response.token
    ?: throw IllegalStateException("Missing token")
```

Great production pattern.

---

# 6. Not-Null Assertion (`!!`)

Forces Kotlin to treat nullable value as non-null.

```kotlin
val name: String? = "Android"

println(name!!.length)
```

Works.

---

## Dangerous Example

```kotlin
val name: String? = null

println(name!!.length)
```

Crash.

```
KotlinNullPointerException
```

---

## When Should You Use `!!`?

Almost never.

Use only when:

- Framework guarantees value.
- Test code.
- Temporary migration from Java.

---

# 7. Safe Cast (`as?`)

Returns null instead of throwing.

```kotlin
val value: Any = "Android"

val text = value as? String
```

Result

```
"Android"
```

---

## Invalid Cast

```kotlin
val value: Any = 100

val text = value as? String
```

Result

```
null
```

---

## Unsafe Cast (`as`)

```kotlin
val value: Any = 100

val text = value as String
```

Runtime crash.

---

# 8. Smart Cast

Compiler automatically converts nullable type after checking.

```kotlin
fun printName(name: String?) {

    if(name != null){
        println(name.length)
    }
}
```

Inside the block:

```
name : String
```

Compiler knows it's non-null.

---

## Smart Cast Doesn't Work

```kotlin
var name: String? = "Android"

if(name != null){

    name = null

    println(name.length)
}
```

Compiler Error.

Because `var` can change.

---

## Smart Cast Works with `val`

```kotlin
val name: String? = "Android"

if(name != null){
    println(name.length)
}
```

Preferred.

---

# 9. Using `let` with Nullables

Most common Android pattern.

```kotlin
name?.let {

    println(it.length)
}
```

Runs only when non-null.

---

## Multiple Nullable Objects

```kotlin
user?.address?.let {

    println(it.city)
}
```

---

## Avoid Nested `let`

Bad

```kotlin
user?.let {

    address?.let {

    }
}
```

Prefer early return.

---

# 10. `run`, `takeIf`, `takeUnless`

## run

```kotlin
val result = name?.run {
    uppercase()
}
```

---

## takeIf

```kotlin
val adult = user.takeIf {
    it.age >= 18
}
```

Returns object or null.

---

## takeUnless

```kotlin
val child = user.takeUnless {
    it.age >= 18
}
```

Opposite of takeIf.

---

# 11. Nullable Collections

## Nullable List

```kotlin
val users: List<User>? = null
```

List itself nullable.

---

## List with Nullable Items

```kotlin
val users: List<User?> = listOf(
    null,
    User("Android")
)
```

List exists.

Items nullable.

---

## Both Nullable

```kotlin
val users: List<User?>? = null
```

Interview favorite.

---

# 12. Null Safety with Collections

Filter null values.

```kotlin
val list = listOf("A", null, "B")

val filtered = list.filterNotNull()
```

Output

```
["A","B"]
```

Compiler infers

```
List<String>
```

---

# 13. Platform Types (`String!`)

Most important Java interoperability topic.

Java method

```java
String getName();
```

Kotlin sees

```
String!
```

Meaning:

Unknown nullability.

---

## Dangerous Example

```kotlin
val name = javaApi.getName()

println(name.length)
```

Possible NPE.

---

## Safe Way

```kotlin
val name = javaApi.getName()

println(name?.length)
```

---

# 14. Java Nullability Annotations

Java

```java
@Nullable
String getName();
```

Kotlin

```
String?
```

---

Java

```java
@NotNull
String getName();
```

Kotlin

```
String
```

Annotations improve interoperability.

---

# 15. Compose Null Safety

## Nullable State

```kotlin
var user by remember {
    mutableStateOf<User?>(null)
}
```

---

## Safe UI Rendering

```kotlin
user?.let {

    Text(it.name)
}
```

---

## Default UI State

```kotlin
val uiState = remember {

    mutableStateOf(HomeUiState())
}
```

Prefer non-null UI state objects.

---

## Loading State Pattern

```kotlin
data class HomeUiState(

    val user: User? = null,

    val loading: Boolean = false,

    val error: String? = null
)
```

Production Compose pattern.

---

# 16. ViewModel Best Practice

```kotlin
private val _state = MutableStateFlow(HomeUiState())

val state = _state.asStateFlow()
```

Avoid nullable StateFlow itself.

Expose immutable UI state.

---

# 17. JVM Boxing and Nullability

```kotlin
val count: Int = 10
```

JVM

```
int
```

---

```kotlin
val count: Int? = 10
```

JVM

```
Integer
```

Nullable primitives become boxed.

---

## Memory Difference

```
Int

Stack

count = 10
```

---

```
Int?

Stack

count ──────────────┐
                    ▼

Heap

Integer(10)
```

Extra allocation.

---

# 18. Compiler Internals

Kotlin compiler inserts null checks.

```kotlin
fun printName(name: String){
    println(name.length)
}
```

Generated bytecode includes:

```
Intrinsics.checkNotNullParameter(...)
```

Automatically validates parameters.

---

## Runtime Null Check

Passing null from Java results in runtime exception.

```
Parameter specified as non-null is null
```

---

# 19. Performance Discussion

## Safe Call Cost

```kotlin
user?.name
```

Compiler generates null check.

Very small overhead.

---

## Elvis Optimization

```kotlin
name ?: "Guest"
```

Compiles to simple conditional branch.

---

## Avoid Nullable Primitives

```kotlin
List<Int?>
```

Creates wrapper objects.

Can increase allocations.

---

# 20. Android Best Practices

## UI State

Always prefer immutable non-null state object.

```kotlin
data class LoginUiState(
    val loading: Boolean = false,
    val email: String = "",
    val error: String? = null
)
```

---

## Repository Layer

Return nullable only when absence is valid.

```kotlin
suspend fun getUser(id: String): User?
```

---

## Domain Layer

Use Result or sealed classes instead of nullable for failures.

---

# 21. Common Mistakes

### Mistake 1

Using `!!` everywhere.

Bad.

```kotlin
user!!.name
```

---

### Mistake 2

Nested nullable chains.

Prefer early returns.

---

### Mistake 3

Nullable StateFlow.

```kotlin
MutableStateFlow<User?>(null)
```

Prefer UI state object.

---

### Mistake 4

Ignoring platform types.

Java APIs may return null.

---

# 22. Real Android Interview Questions

## Basic

1. Difference between `String` and `String?`.
2. What is safe call operator?
3. What is Elvis operator?
4. What is `!!`?
5. Difference between `as` and `as?`.

---

## Intermediate

6. What is Smart Cast?
7. Why doesn't Smart Cast work with mutable variables?
8. Explain nullable collections.
9. Difference between `List<User?>` and `List<User>?`.
10. Explain `filterNotNull()`.

---

## Advanced

11. Explain platform types.
12. Explain Kotlin nullability annotations.
13. How does compiler generate null checks?
14. Why are nullable primitives boxed?
15. Explain null safety in Compose state management.

---

# 23. 2-Minute Interview Answer

> Kotlin's null safety is built into its type system. A non-nullable type like `String` cannot contain null, while `String?` explicitly allows null values. Safe calls (`?.`) and the Elvis operator (`?:`) provide safe ways to work with nullable objects without throwing exceptions. Smart casts allow the compiler to automatically treat nullable values as non-null after appropriate checks. Kotlin also inserts runtime null checks for Java interoperability through generated bytecode.

---

# 24. Cheat Sheet

| Operator | Purpose |
|----------|---------|
| `?` | Nullable Type |
| `?.` | Safe Call |
| `?:` | Elvis Operator |
| `!!` | Not Null Assertion |
| `as?` | Safe Cast |
| `is` | Type Check |
| `filterNotNull()` | Remove Null Values |

---

# 📝 Revision Summary

- Kotlin makes nullability part of the type system.
- Prefer `String` over `String?` unless null is meaningful.
- Use `?.` and `?:` instead of `!!`.
- Smart casts work best with immutable (`val`) references.
- Platform types (`String!`) are the biggest source of NPEs when interoperating with Java.
- Compose UI should prefer a non-null immutable `UiState` with nullable fields where appropriate.