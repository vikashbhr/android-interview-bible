# 💜 Kotlin Ranges & Progressions — Complete Interview Guide (2026 Edition)

> Master Kotlin ranges from fundamentals to `IntProgression`, custom ranges, Compose sliders, date ranges, JVM internals, and Android interview questions.

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐☆

**Companies:** Google • PhonePe • Uber • Amazon • Microsoft • Flipkart

---

# 📚 Table of Contents

1. What are Ranges?
2. Types of Ranges
3. Inclusive (`..`) Range
4. Exclusive (`until`) Range
5. Reverse (`downTo`) Range
6. Step Progression
7. `IntProgression`
8. Char Ranges
9. Long & Float Ranges
10. `in` and `!in`
11. Custom Ranges (`ClosedRange<T>`)
12. Date & Version Ranges
13. Compose Range APIs
14. JVM Internals
15. Performance
16. Best Practices
17. Common Mistakes
18. Interview Questions
19. Cheat Sheet

---

# 1. What are Ranges?

A **range** represents a sequence of values between a start and an end.

Kotlin provides a concise syntax for ranges using operators.

```kotlin
val range = 1..5
```

Output:

```text
1 2 3 4 5
```

Ranges are heavily used for:

- Loops
- Validation
- Pagination
- Dates
- Compose Sliders
- DSA

---

# 2. Types of Ranges

| Syntax | Meaning |
|--------|---------|
| `1..5` | Inclusive Range |
| `1 until 5` | End Exclusive |
| `5 downTo 1` | Reverse Range |
| `1..10 step 2` | Progression with Step |
| `'A'..'Z'` | Character Range |
| `0L..100L` | Long Range |

---

# 3. Inclusive Range (`..`)

Creates a `ClosedRange`.

```kotlin
val numbers = 1..5

println(numbers)
```

Output

```text
1..5
```

### Iterate

```kotlin
for (i in 1..5) {
    println(i)
}
```

Output

```text
1
2
3
4
5
```

---

## Contains Check

```kotlin
println(3 in 1..5)
```

Output

```text
true
```

---

## Android Example

Age validation.

```kotlin
if (age in 18..60) {
    // Eligible
}
```

---

# 4. Exclusive Range (`until`)

Upper bound excluded.

```kotlin
for (i in 0 until 5) {
    println(i)
}
```

Output

```text
0
1
2
3
4
```

Equivalent to:

```kotlin
0..4
```

---

## Why `until` Exists?

Useful for arrays and lists.

```kotlin
for (i in 0 until users.size) {
    println(users[i])
}
```

Avoids `IndexOutOfBoundsException`.

---

# 5. Reverse Range (`downTo`)

Iterates backwards.

```kotlin
for (i in 5 downTo 1) {
    println(i)
}
```

Output

```text
5
4
3
2
1
```

---

## Countdown Example

```kotlin
for (seconds in 10 downTo 1) {
    println(seconds)
}
```

Useful for timers.

---

# 6. Step Progression (`step`)

Changes increment size.

```kotlin
for (i in 0..10 step 2) {
    println(i)
}
```

Output

```text
0
2
4
6
8
10
```

---

## Reverse Step

```kotlin
for (i in 10 downTo 0 step 2) {
    println(i)
}
```

Output

```text
10
8
6
4
2
0
```

---

# 7. `IntProgression`

Every stepped range becomes an `IntProgression`.

```kotlin
val progression = 1..10 step 2
```

Internally:

```text
First = 1

Last = 9

Step = 2
```

---

## Properties

```kotlin
println(progression.first)

println(progression.last)

println(progression.step)
```

Output

```text
1
9
2
```

---

## Progression Memory Model

```
IntProgression

first = 1

last = 9

step = 2
```

No list allocation.

---

# 8. Range Objects

### IntRange

```kotlin
val range: IntRange = 1..5
```

### LongRange

```kotlin
val range = 1L..100L
```

### CharRange

```kotlin
val alphabet = 'A'..'Z'
```

### UIntRange

```kotlin
val colors = 0u..255u
```

Unsigned support.

---

# 9. Character Ranges

```kotlin
for (letter in 'A'..'F') {
    println(letter)
}
```

Output

```text
A
B
C
D
E
F
```

---

## Reverse Alphabet

```kotlin
for (letter in 'Z' downTo 'A') {
    println(letter)
}
```

Useful in parsing and validation.

---

# 10. Membership (`in` / `!in`)

Check if value belongs to range.

```kotlin
println(5 in 1..10)
```

Output

```text
true
```

---

## String Membership

```kotlin
val letter = 'C'

println(letter in 'A'..'Z')
```

---

## Negation

```kotlin
println(100 !in 1..50)
```

Output

```text
true
```

---

# 11. `contains()` Function

`in` translates to `contains()`.

```kotlin
if (value in range)
```

Compiler generates.

```kotlin
range.contains(value)
```

Important interview point.

---

# 12. Empty Ranges

```kotlin
val range = 5..1
```

Iterates zero times.

---

## Check Empty

```kotlin
println(range.isEmpty())
```

Output

```text
true
```

---

# 13. `reversed()`

Returns reverse progression.

```kotlin
val range = (1..5).reversed()
```

Output

```text
5 4 3 2 1
```

---

# 14. `first`, `last`, `step`

Useful properties.

```kotlin
val progression = 2..20 step 3

println(progression.first)

println(progression.last)

println(progression.step)
```

Output

```text
2
20
3
```

---

# 15. Custom Ranges (`ClosedRange<T>`)

Create ranges for custom types.

```kotlin
data class Version(val major:Int)

operator fun Version.rangeTo(other:Version)=
    object: ClosedRange<Version>{
        override val start= this@rangeTo
        override val endInclusive= other
    }
```

Rare Staff interview topic.

---

# 16. Date Ranges

```kotlin
val today = LocalDate.now()

val nextWeek = today..today.plusDays(7)
```

Useful for booking apps.

---

## Date Membership

```kotlin
if(date in nextWeek){
}
```

---

# 17. Version Ranges

Android version checks.

```kotlin
if(Build.VERSION.SDK_INT in
    Build.VERSION_CODES.M..
    Build.VERSION_CODES.UPSIDE_DOWN_CAKE){
}
```

Cleaner than chained comparisons.

---

# 18. Compose Range APIs

## Slider

```kotlin
Slider(
    value = progress,
    onValueChange = { progress = it },
    valueRange = 0f..100f
)
```

`ClosedFloatingPointRange<Float>`.

---

## Progress Indicator

```kotlin
LinearProgressIndicator(
    progress = progress / 100f
)
```

---

## Animation Range

```kotlin
animateFloatAsState(
    targetValue = if(expanded) 1f else 0f
)
```

---

# 19. Range Validation Examples

## Password Length

```kotlin
if(password.length in 8..16)
```

---

## OTP Length

```kotlin
require(code.length in 4..6)
```

---

## Pagination

```kotlin
if(page in 1..totalPages)
```

---

# 20. JVM Internals

`1..5`

Compiler generates.

```java
new IntRange(1,5)
```

---

## `until`

Compiler generates.

```java
RangesKt.until(1,5)
```

---

## `downTo`

Compiler generates.

```java
RangesKt.downTo(5,1)
```

---

## Primitive Optimization

Loops over ranges become indexed loops.

```kotlin
for(i in 1..5)
```

Compiles close to Java `for(int i=1;i<=5;i++)`.

---

# 21. Performance Discussion

## Range vs List

```kotlin
1..1_000_000
```

Does **not** allocate one million integers.

Stores only:

- start
- end
- step

Memory efficient.

---

## Sequence vs Range

Range is lazy iteration.

List allocates elements.

---

## CharRange Performance

Iterates Unicode values.

Very lightweight.

---

# 22. Android Best Practices

### List Iteration

Prefer

```kotlin
users.indices
```

instead of hardcoded indexes.

---

### Slider

Always use `valueRange`.

---

### Validation

Use `in` with ranges.

Cleaner and safer.

---

# 23. Common Mistakes

### Mistake 1

Using `..` for indexes.

Bad.

```kotlin
0..list.size
```

Last index crashes.

Use `until`.

---

### Mistake 2

Assuming `step` changes original range.

Returns new progression.

---

### Mistake 3

Creating lists from ranges unnecessarily.

```kotlin
(1..1000000).toList()
```

Allocates memory.

---

### Mistake 4

Using Float ranges in loops.

`FloatRange` isn't iterable.

---

# 24. Real Interview Questions

## Basic

1. Difference between `..` and `until`?
2. Difference between `downTo` and `reversed()`?
3. What does `step` return?
4. What is `IntRange`?
5. What is `IntProgression`?

---

## Intermediate

6. How does `in` work?
7. What is `contains()`?
8. Difference between `indices` and `until size`?
9. Explain empty ranges.
10. Explain `CharRange`.

---

## Advanced

11. JVM implementation of ranges.
12. Why are ranges memory efficient?
13. Explain `ClosedRange<T>`.
14. Explain Compose `valueRange`.
15. Performance difference between range and list iteration.

---

# 25. 2-Minute Interview Answer

> Kotlin ranges are lightweight objects representing a start, end, and step. They don't allocate collections unless explicitly converted with `toList()`. `IntRange` represents an inclusive range, while `IntProgression` represents ranges with custom steps. `until` creates an exclusive upper-bound range, making it ideal for array iteration. The `in` operator delegates to `contains()`, and ranges are optimized by the compiler into efficient JVM loops.

---

# 26. Cheat Sheet

| Syntax | Meaning |
|--------|---------|
| `1..5` | Inclusive Range |
| `1 until 5` | Exclusive Range |
| `5 downTo 1` | Reverse Range |
| `step 2` | Step Progression |
| `in range` | Membership |
| `!in range` | Not in Range |
| `reversed()` | Reverse Progression |
| `indices` | Valid Index Range |

---

# 📝 Revision Summary

- `IntRange` stores **start** and **end**.
- `IntProgression` stores **start**, **end**, and **step**.
- `until` is preferred for array/list indexes.
- `in` uses `contains()` internally.
- Ranges are memory efficient and don't allocate elements.
- Compose uses `ClosedFloatingPointRange<Float>` for sliders and animations.