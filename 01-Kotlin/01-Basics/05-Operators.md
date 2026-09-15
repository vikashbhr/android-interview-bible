# 💜 Kotlin Operators — Complete Interview Guide (2026 Edition)

> Master every Kotlin operator from language fundamentals to operator overloading, JVM bytecode, Compose delegates, bitwise operations, and Android interview questions.

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • Razorpay

---

# 📚 Table of Contents

1. What are Operators?
2. Arithmetic Operators
3. Assignment Operators
4. Comparison Operators
5. Logical Operators
6. Increment & Decrement
7. Range Operator
8. `in` / `!in`
9. `is` / `!is`
10. Elvis Operator
11. Safe Call Operator
12. Bitwise Operators
13. Operator Overloading
14. Infix Functions
15. Delegation (`by`)
16. Compose Operator Usage
17. JVM Internals
18. Performance
19. Best Practices
20. Interview Questions
21. Cheat Sheet

---

# 1. What are Operators?

Operators perform operations on operands.

```kotlin
val result = 10 + 20
```

Here:

- `10` and `20` → Operands
- `+` → Operator

---

## Operator Categories

| Category | Examples |
|----------|----------|
| Arithmetic | `+ - * / %` |
| Assignment | `= += -= *=` |
| Comparison | `== != > < >= <=` |
| Logical | `&& || !` |
| Range | `.. until downTo step` |
| Type Check | `is !is` |
| Membership | `in !in` |
| Null Safety | `?. ?: !!` |
| Bitwise | `shl shr and or xor` |
| Delegation | `by` |

---

# 2. Arithmetic Operators

## Addition

```kotlin
val total = 20 + 30
```

## Subtraction

```kotlin
val remaining = 50 - 15
```

## Multiplication

```kotlin
val price = 100 * 5
```

## Division

```kotlin
val average = 100 / 4
```

## Modulus

```kotlin
val remainder = 11 % 2
```

Output:

```text
1
```

---

## Integer Division Trap

```kotlin
println(5 / 2)
```

Output

```text
2
```

To get decimal:

```kotlin
println(5 / 2.0)
```

Output

```text
2.5
```

Interview favorite.

---

# 3. Assignment Operators

## Basic Assignment

```kotlin
var count = 10
```

## Compound Assignment

```kotlin
count += 5
count -= 2
count *= 3
count /= 2
count %= 2
```

Equivalent

```kotlin
count = count + 5
```

---

## String Concatenation

```kotlin
var text = "Android"

text += " Compose"
```

---

# 4. Comparison Operators

Used to compare values.

```kotlin
10 == 10
10 != 5
10 > 5
10 < 20
10 >= 10
10 <= 5
```

Output type is always `Boolean`.

---

## Structural Equality (`==`)

Checks contents.

```kotlin
val a = "Android"
val b = "Android"

println(a == b)
```

Output

```text
true
```

---

## Referential Equality (`===`)

Checks references.

```kotlin
val a = String(charArrayOf('A'))
val b = String(charArrayOf('A'))

println(a === b)
```

Output

```text
false
```

---

## `==` vs `===`

| Operator | Meaning |
|----------|---------|
| `==` | Structural Equality |
| `===` | Reference Equality |
| `!=` | Structural Inequality |
| `!==` | Reference Inequality |

Very common interview question.

---

# 5. Logical Operators

## AND

```kotlin
val result = age > 18 && isVerified
```

Short-circuit evaluation.

---

## OR

```kotlin
val login = isGoogle || isEmail
```

---

## NOT

```kotlin
val disabled = !isEnabled
```

---

## Short Circuit Example

```kotlin
user != null && user.isPremium
```

Second condition executes only if first is true.

---

# 6. Increment & Decrement

## Prefix

```kotlin
var count = 10

println(++count)
```

Output

```text
11
```

---

## Postfix

```kotlin
var count = 10

println(count++)
println(count)
```

Output

```text
10
11
```

---

## Prefix vs Postfix Memory

```text
count++

Return old value.

Then increment.
```

```text
++count

Increment.

Return new value.
```

Interview favorite.

---

# 7. Range Operators

## Closed Range

```kotlin
1..5
```

Output

```text
1 2 3 4 5
```

---

## until

```kotlin
1 until 5
```

Output

```text
1 2 3 4
```

Upper bound excluded.

---

## downTo

```kotlin
5 downTo 1
```

Output

```text
5 4 3 2 1
```

---

## step

```kotlin
1..10 step 2
```

Output

```text
1 3 5 7 9
```

---

## reversed()

```kotlin
(1..5).reversed()
```

---

# 8. `in` and `!in`

Membership operator.

```kotlin
if (3 in 1..5)
```

True.

---

## Collection Membership

```kotlin
if ("Android" in listOf("Android", "Compose"))
```

---

## Negation

```kotlin
if (10 !in 1..5)
```

---

# Android Example

```kotlin
if (Build.VERSION.SDK_INT in Build.VERSION_CODES.LOLLIPOP..Build.VERSION_CODES.TIRAMISU)
```

---

# 9. `is` and `!is`

Type checking.

```kotlin
if (value is String)
```

Compiler smart casts.

---

## Negation

```kotlin
if (value !is String)
```

---

## Smart Cast Example

```kotlin
if (value is User) {
    println(value.name)
}
```

No explicit cast required.

---

# 10. Elvis Operator (`?:`)

Default value for nullable expressions.

```kotlin
val title = intent.getStringExtra("title") ?: "Unknown"
```

---

## Throw Expression

```kotlin
val token = response.token ?: error("Missing Token")
```

---

# 11. Safe Call Operator (`?.`)

Safely access nullable properties.

```kotlin
user?.address?.city
```

Stops evaluation on first null.

---

# 12. Not Null Assertion (`!!`)

```kotlin
println(user!!.name)
```

Throws `KotlinNullPointerException` if null.

Avoid in production.

---

# 13. Bitwise Operators

Kotlin uses functions instead of symbols.

## AND

```kotlin
val result = 5 and 3
```

---

## OR

```kotlin
5 or 3
```

---

## XOR

```kotlin
5 xor 3
```

---

## Left Shift

```kotlin
1 shl 3
```

Output

```text
8
```

---

## Right Shift

```kotlin
8 shr 2
```

Output

```text
2
```

---

## Unsigned Shift

```kotlin
-1 ushr 1
```

Advanced JVM topic.

---

## Binary Example

```
5 = 0101

3 = 0011

AND = 0001

OR = 0111

XOR = 0110
```

---

# 14. Operator Precedence

| Priority | Operators |
|----------|-----------|
| Highest | `()` |
| Unary | `! ++ --` |
| Multiplication | `* / %` |
| Addition | `+ -` |
| Range | `..` |
| Comparison | `< > <= >=` |
| Equality | `== != === !==` |
| Logical AND | `&&` |
| Logical OR | `||` |
| Elvis | `?:` |
| Assignment | `=` |

Interview question.

---

# 15. Operator Overloading

Kotlin allows operators to map to functions.

Example.

```kotlin
data class Point(val x:Int,val y:Int){

    operator fun plus(other:Point)=
        Point(x+other.x,y+other.y)
}
```

Usage.

```kotlin
val result = p1 + p2
```

Compiler calls.

```kotlin
p1.plus(p2)
```

---

## Supported Operators

| Operator | Function |
|----------|----------|
| `+` | `plus()` |
| `-` | `minus()` |
| `*` | `times()` |
| `/` | `div()` |
| `%` | `rem()` |
| `[]` | `get()` / `set()` |
| `()` | `invoke()` |
| `+=` | `plusAssign()` |

---

## `invoke()` Example

```kotlin
class Logger{

    operator fun invoke(message:String){
        println(message)
    }
}

val logger = Logger()

logger("Compose Loaded")
```

---

# 16. Infix Functions

Creates readable DSL-like syntax.

```kotlin
infix fun Int.squareMultiply(value:Int)=
    this * value
```

Usage.

```kotlin
5 squareMultiply 10
```

Output.

```text
50
```

---

## Requirements

- Member or extension function.
- One parameter.
- Marked with `infix`.

---

# 17. Delegation Operator (`by`)

Very important for Android & Compose.

## Lazy Delegation

```kotlin
val database by lazy {
    createDatabase()
}
```

---

## Observable

```kotlin
var name by Delegates.observable(""){
    _, old, new ->
}
```

---

## Compose State Delegation

```kotlin
var count by remember {
    mutableStateOf(0)
}
```

Without `by`.

```kotlin
val count = remember {
    mutableStateOf(0)
}

count.value++
```

`by` delegates to `.value`.

Compose interview favorite.

---

# 18. `get()` and `set()` Operator

```kotlin
class Storage{

    operator fun get(index:Int)=...

    operator fun set(index:Int,value:Int){}
}
```

Usage.

```kotlin
storage[0] = 10

println(storage[0])
```

---

# 19. Destructuring Operator

```kotlin
val (name, age) = User("Vikash",29)
```

Compiler calls.

```kotlin
component1()

component2()
```

Generated for data classes.

---

# 20. Compose Operator Usage

## `by`

```kotlin
var text by remember {
    mutableStateOf("")
}
```

Cleaner syntax.

---

## `rememberSaveable`

```kotlin
var email by rememberSaveable {
    mutableStateOf("")
}
```

---

## Delegated ViewModel State

```kotlin
val uiState by viewModel.state.collectAsState()
```

Very common interview question.

---

# 21. JVM Internals

Kotlin

```kotlin
a + b
```

Compiler generates.

```java
a.plus(b)
```

For objects.

Primitives become JVM instructions.

---

## `==` Bytecode

For nullable objects compiler generates:

```java
Intrinsics.areEqual(a,b)
```

Handles null safely.

---

## Primitive Comparison

```kotlin
10 == 10
```

Becomes.

```java
if_icmpeq
```

No boxing.

---

# 22. Performance Discussion

## Prefer `==`

Safer than `equals()`.

Compiler handles null.

---

## Avoid `===`

Unless checking identity.

Useful for singleton/object comparison.

---

## Bitwise vs Logical

```kotlin
&&
```

Short-circuit.

```kotlin
and()
```

Evaluates both sides.

Can impact performance.

---

# 23. Android Best Practices

## Resource IDs

```kotlin
if (id == R.id.loginButton)
```

---

## Compose State

Use `by` delegation.

---

## Range Validation

```kotlin
if(age in 18..60)
```

Readable.

---

## Safe Call + Elvis

```kotlin
val title = state.title ?: ""
```

Preferred in UI.

---

# 24. Common Mistakes

### Mistake 1

Using `===` for String comparison.

Bad.

```kotlin
name === "Android"
```

Use `==`.

---

### Mistake 2

Using `!!`.

---

### Mistake 3

Using postfix when prefix required.

---

### Mistake 4

Using `until` expecting inclusive range.

`until` excludes last value.

---

### Mistake 5

Confusing logical `&&` with bitwise `and()`.

---

# 25. Real Interview Questions

## Basic

1. Difference between `==` and `===`?
2. Difference between `&&` and `||`?
3. Difference between `..` and `until`?
4. What does `in` do?
5. What does `is` do?

---

## Intermediate

6. Explain operator overloading.
7. Explain `invoke()` operator.
8. Explain `componentN()` operators.
9. Difference between prefix and postfix increment.
10. Explain delegation operator `by`.

---

## Advanced

11. How does Kotlin compile `+` operator?
12. Explain `Intrinsics.areEqual()`.
13. Explain bitwise operators on JVM.
14. Explain Compose property delegation.
15. Explain performance implications of boxing during operator calls.

---

# 26. 2-Minute Interview Answer

> Kotlin operators are syntactic sugar over function calls and JVM instructions. For primitive types, arithmetic operators compile directly to JVM bytecode instructions, while object operators compile to functions like `plus()`. Kotlin distinguishes structural equality (`==`) from referential equality (`===`). Operators like `by`, `in`, `is`, `?:`, and `?.` are language-level features heavily used in Android development, especially with Compose state management and null safety.

---

# 27. Cheat Sheet

| Operator | Meaning |
|----------|---------|
| `==` | Structural Equality |
| `===` | Referential Equality |
| `?.` | Safe Call |
| `?:` | Elvis |
| `!!` | Not Null Assertion |
| `is` | Type Check |
| `in` | Membership |
| `..` | Inclusive Range |
| `until` | Exclusive Upper Range |
| `by` | Delegation |
| `as?` | Safe Cast |
| `shl` | Left Shift |
| `shr` | Right Shift |
| `ushr` | Unsigned Right Shift |

---

# 📝 Revision Summary

- `==` compares values, `===` compares references.
- `?.` and `?:` are the preferred null-safe operators.
- `by` powers Compose state delegation (`mutableStateOf`, `collectAsState`).
- `in` works with ranges and collections.
- Kotlin bitwise operations use named functions (`and`, `or`, `xor`, `shl`, `shr`) instead of Java symbols.
- Operator overloading maps operators to functions like `plus()`, `minus()`, `invoke()`, `get()`, and `set()`.