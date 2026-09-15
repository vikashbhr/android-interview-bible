# 💜 Kotlin Cheat Sheet — Android Interview Bible (8+ Years)

> One-page revision guide covering all Kotlin Basics for Android interviews.

**Last Updated:** 2026 Edition

**Topics Covered:** Variables, Data Types, Null Safety, Operators, If/When, Loops, Ranges, Strings, Type Casting.

---

# 📚 Quick Navigation

- Variables (`val`, `var`, `const`, `lateinit`, `lazy`)
- Data Types
- Null Safety
- Operators
- If & When
- Loops
- Ranges
- Strings
- Type Casting
- Android Interview Best Practices

---

# 1. Variables

| Keyword | Meaning | Interview Note |
|---------|---------|----------------|
| `val` | Immutable reference | Preferred in Compose & ViewModel |
| `var` | Mutable reference | Use only when mutation is required |
| `const val` | Compile-time constant | Top-level / object / companion object only |
| `lateinit var` | Deferred initialization | Cannot be primitive or nullable |
| `lazy {}` | First access initialization | Thread-safe by default |

### Example

```kotlin
const val API_VERSION = "v1"

lateinit var repository: UserRepository

val database by lazy {
    createDatabase()
}
```

---

# 2. Data Types

| Kotlin | JVM Type |
|--------|----------|
| Int | int |
| Long | long |
| Float | float |
| Double | double |
| Boolean | boolean |
| Char | char |
| String | java.lang.String |

### Special Types

| Type | Meaning |
|------|---------|
| `Any` | Root type |
| `Unit` | No meaningful return value |
| `Nothing` | Function never returns |

### Numeric Conversion

```kotlin
val number = 10

number.toLong()
number.toFloat()
number.toDouble()
```

**No implicit conversion in Kotlin.**

---

# 3. Null Safety

### Nullable vs Non-Nullable

```kotlin
val name: String = "Android"

val city: String? = null
```

### Operators

| Operator | Purpose |
|----------|---------|
| `?.` | Safe Call |
| `?:` | Elvis Operator |
| `!!` | Not Null Assertion |
| `as?` | Safe Cast |

### Smart Cast

```kotlin
if (name != null) {
    println(name.length)
}
```

### Android Pattern

```kotlin
val title = intent.getStringExtra("title") ?: "Untitled"
```

---

# 4. Operators

## Equality

```kotlin
a == b     // Structural Equality

a === b    // Referential Equality
```

## Range

```kotlin
1..10

1 until 10

10 downTo 1

1..20 step 2
```

## Type Check

```kotlin
value is String

value !is User
```

## Delegation

```kotlin
var count by remember {
    mutableStateOf(0)
}
```

---

# 5. If & When

### If Expression

```kotlin
val status =
    if (age >= 18) "Adult" else "Minor"
```

### When Expression

```kotlin
when(state){

    is Loading -> ...

    is Success -> ...

    is Error -> ...
}
```

### Exhaustive When

Use with **sealed classes** and **enums**.

---

# 6. Loops

## For

```kotlin
for(i in 1..5)
```

## While

```kotlin
while(condition)
```

## Repeat

```kotlin
repeat(5){
    println(it)
}
```

## Indexed Iteration

```kotlin
users.withIndex()

users.forEachIndexed { index, user -> }
```

### Labels

```kotlin
break@outer

continue@outer

return@forEach
```

---

# 7. Ranges

| Syntax | Meaning |
|--------|---------|
| `1..5` | Inclusive |
| `1 until 5` | Exclusive |
| `5 downTo 1` | Reverse |
| `step 2` | Increment |
| `in range` | Membership |

### Useful APIs

```kotlin
range.first
range.last
range.step
range.reversed()
```

---

# 8. Strings

### Templates

```kotlin
"Hello $name"

"${user.age}"
```

### Raw String

```kotlin
"""
JSON
"""
```

### Common APIs

```kotlin
uppercase()

lowercase()

trim()

replace()

split(",")

joinToString()

substringBefore("@")

substringAfter("@")
```

### StringBuilder

```kotlin
buildString {
    append("Hello")
}
```

Preferred for repeated concatenation.

---

# 9. Regex

### Create

```kotlin
val regex = Regex("\\d+")
```

### APIs

```kotlin
matches()

find()

containsMatchIn()

replace()

split()
```

### Common Patterns

| Purpose | Regex |
|--------|-------|
| Email | `[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}` |
| Phone | `^[6-9]\\d{9}$` |
| OTP | `^\\d{6}$` |

---

# 10. Type Casting

### Safe vs Unsafe

```kotlin
value as String

value as? String
```

### Smart Cast

```kotlin
if(value is User){
    value.name
}
```

### Reified Generic

```kotlin
inline fun <reified T> Any.castOrNull(): T? =
    this as? T
```

### Collection Casting

```kotlin
list.filterIsInstance<User>()
```

Preferred over `as List<User>`.

---

# 11. JVM Interview Nuggets

| Topic | JVM Detail |
|-------|------------|
| `Int` | Primitive `int` |
| `Int?` | Boxed `Integer` |
| String Template | Compiles to `StringBuilder` |
| `==` | `Intrinsics.areEqual()` |
| `as` | `CHECKCAST` |
| `is` | `INSTANCEOF` |
| Smart Cast | Compile-time control flow analysis |

---

# 12. Compose Quick Revision

### State

```kotlin
var text by rememberSaveable {
    mutableStateOf("")
}
```

### UI State

```kotlin
data class HomeUiState(
    val loading: Boolean = false,
    val users: List<User> = emptyList(),
    val error: String? = null
)
```

### LazyColumn

```kotlin
LazyColumn {
    items(users){
        UserCard(it)
    }
}
```

---

# 13. Android Best Practices

## Prefer `val`

```kotlin
val uiState = HomeUiState()
```

## Avoid `!!`

Use safe calls or Elvis.

## Use `stringResource()`

```kotlin
Text(stringResource(R.string.title))
```

## Use `pluralStringResource()`

For quantity-based strings.

## Use `buildString()`

For complex text creation.

---

# 14. Top Interview Traps

| Trap | Correct Answer |
|------|----------------|
| `val` means immutable object | ❌ Immutable reference only |
| `==` compares references | ❌ `==` compares values |
| Kotlin allows implicit numeric conversion | ❌ Explicit conversion required |
| `String.length` equals visible characters | ❌ UTF-16 code units |
| `List<String>` exists at runtime | ❌ Type erasure |
| `as` is safe | ❌ `as?` is safe |

---

# 15. Senior Interview One-Liners

### `val` vs `var`

> `val` prevents reassignment, not object mutation.

### Smart Cast

> Smart cast works only when the compiler can guarantee immutability.

### StringBuilder

> Repeated `+` inside loops creates O(n²) allocations; `StringBuilder` is O(n).

### Reified Generics

> `inline + reified` preserves generic type information at call site.

### Sealed + When

> Exhaustive `when` gives compile-time safety for UI state handling.

---

# 🎯 8+ Years Android Interview Checklist

- [x] val / var / const / lazy / lateinit
- [x] Primitive vs Boxed Types
- [x] Null Safety Operators
- [x] Smart Cast Internals
- [x] Equality (`==` vs `===`)
- [x] Operator Overloading
- [x] If & When Expressions
- [x] Sealed Classes + Exhaustive When
- [x] Loops, Labels & Sequences
- [x] Ranges & Progressions
- [x] String Pool & UTF-16
- [x] Regex APIs
- [x] StringBuilder vs buildString
- [x] Safe/Unsafe Casting
- [x] Type Erasure & Reified Generics

---

# 🚀 Revision Time

| Time Available | Read |
|---------------|------|
| **5 Minutes** | JVM Nuggets + Interview Traps |
| **15 Minutes** | Entire Cheat Sheet |
| **30 Minutes** | Cheat Sheet + Topic READMEs |
| **1 Hour** | Basics README + All 10 Chapters |