# 💜 Kotlin Loops — Complete Interview Guide (2026 Edition)

> Master Kotlin loops from fundamentals to iterator internals, labels, sequences, Compose list rendering, JVM bytecode, and performance optimization.

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐☆

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • Flipkart

---

# 📚 Table of Contents

1. What are Loops?
2. `for` Loop
3. `while` Loop
4. `do...while` Loop
5. `repeat()` Function
6. Iterating Collections
7. `indices` and `withIndex()`
8. `forEach` vs `for`
9. Labels (`break@`, `continue@`)
10. Nested Loops
11. Sequence vs Collection Iteration
12. Compose & LazyColumn
13. JVM Iterator Internals
14. Performance Discussion
15. Common Mistakes
16. Interview Questions
17. Cheat Sheet

---

# 1. What are Loops?

Loops execute a block of code repeatedly until a condition is met.

Kotlin provides four primary iteration mechanisms:

| Loop | Use Case |
|------|----------|
| `for` | Iterate ranges, collections, arrays. |
| `while` | Unknown number of iterations. |
| `do...while` | Execute at least once. |
| `repeat()` | Execute fixed number of times. |

Unlike Java, Kotlin's `for` loop is based on **iterators**, not index variables.

---

# 2. `for` Loop

## Iterate Over Range

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

## Exclusive Range

```kotlin
for (i in 1 until 5) {
    println(i)
}
```

Output

```text
1
2
3
4
```

---

## Reverse Iteration

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

## Step

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

# 3. `while` Loop

Used when the number of iterations isn't known beforehand.

```kotlin
var count = 1

while (count <= 5) {
    println(count)
    count++
}
```

---

## Android Example

Polling until a condition becomes true.

```kotlin
while (!networkAvailable()) {
    delay(1000)
}
```

Usually replaced with Flow or callbacks in production.

---

# 4. `do...while` Loop

Executes **at least once**.

```kotlin
var number = 5

do {
    println(number)
    number--
} while (number > 0)
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

## Difference from `while`

| while | do...while |
|--------|------------|
| Condition checked first. | Condition checked after execution. |
| May execute zero times. | Executes at least once. |

---

# 5. `repeat()` Function

A Kotlin standard library helper.

```kotlin
repeat(3) {
    println("Android")
}
```

Output

```text
Android
Android
Android
```

---

## Index Parameter

```kotlin
repeat(5) { index ->
    println(index)
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

Great for UI placeholders and testing.

---

# 6. Iterating Collections

## List

```kotlin
val users = listOf("A", "B", "C")

for (user in users) {
    println(user)
}
```

---

## Set

```kotlin
for (item in setOf(1, 2, 3)) {
    println(item)
}
```

---

## Map

```kotlin
val map = mapOf(
    "A" to 1,
    "B" to 2
)

for ((key, value) in map) {
    println("$key -> $value")
}
```

Destructuring is supported automatically.

---

# 7. Arrays

```kotlin
val numbers = arrayOf(10, 20, 30)

for (number in numbers) {
    println(number)
}
```

---

## Primitive Arrays

```kotlin
val numbers = intArrayOf(1, 2, 3)
```

Avoid boxing.

Better performance.

---

# 8. `indices`

Access collection indexes safely.

```kotlin
val users = listOf("A", "B", "C")

for (i in users.indices) {
    println("${users[i]}")
}
```

Output

```text
A
B
C
```

---

# 9. `withIndex()`

Cleaner indexed iteration.

```kotlin
for ((index, user) in users.withIndex()) {
    println("$index -> $user")
}
```

Output

```text
0 -> A
1 -> B
2 -> C
```

Preferred over manual indexing.

---

# 10. `forEach`

Higher-order iteration.

```kotlin
users.forEach {
    println(it)
}
```

---

## Indexed Version

```kotlin
users.forEachIndexed { index, value ->
    println(index)
}
```

---

## Difference from `for`

| `for` | `forEach` |
|-------|-----------|
| Language keyword | Higher-order function |
| Supports `break` | Doesn't support `break` |
| Supports `continue` | Doesn't support `continue` |
| Faster in many cases | Lambda allocation possible |

Interview favorite.

---

# 11. `map` is NOT a Loop

Wrong usage.

```kotlin
users.map {
    println(it)
}
```

`map` creates a new list.

Correct.

```kotlin
users.forEach {
    println(it)
}
```

---

## `map`

Transforms.

```kotlin
val lengths = users.map {
    it.length
}
```

Output

```text
[1,1,1]
```

---

# 12. Labels (`break@`, `continue@`)

Very important Kotlin topic.

## Break Label

```kotlin
outer@ for (i in 1..3) {

    for (j in 1..3) {

        if (j == 2) break@outer

        println("$i $j")
    }
}
```

Breaks outer loop.

---

## Continue Label

```kotlin
outer@ for (i in 1..3) {

    for (j in 1..3) {

        if (j == 2) continue@outer

        println("$i $j")
    }
}
```

Continues outer loop.

---

## Lambda Labels

```kotlin
users.forEach label@{

    if (it == "B") return@label

    println(it)
}
```

Skips only current iteration.

---

## Non-local Return

```kotlin
fun printUsers(users: List<String>) {

    users.forEach {

        if (it == "B") return

        println(it)
    }
}
```

Returns from the entire function.

Senior interview favorite.

---

# 13. Nested Loops

```kotlin
for (row in 1..3) {
    for (col in 1..3) {
        print("$row,$col ")
    }
}
```

Applications:

- Matrix traversal.
- Chess board.
- Sudoku.
- Grid layouts.

---

# 14. Iterating Strings

```kotlin
for (char in "Compose") {
    println(char)
}
```

Characters are iterable.

---

# 15. Sequence Iteration

Collection.

```kotlin
numbers
    .filter { it > 5 }
    .map { it * 2 }
```

Creates intermediate collections.

---

Sequence.

```kotlin
numbers
    .asSequence()
    .filter { it > 5 }
    .map { it * 2 }
    .toList()
```

Lazy execution.

Better for large datasets.

---

## Execution Difference

Collection

```
List
 ↓ Filter
 ↓ New List
 ↓ Map
 ↓ New List
```

Sequence

```
Element
 ↓ Filter
 ↓ Map
 ↓ Next Element
```

---

# 16. Compose Iteration

## LazyColumn

```kotlin
LazyColumn {

    items(users) { user ->
        UserCard(user)
    }
}
```

Preferred.

---

## Indexed LazyColumn

```kotlin
itemsIndexed(users) { index, user ->
    UserCard(user)
}
```

---

## Don't Use `for` in Compose

Bad.

```kotlin
Column {
    for(user in users){
        UserCard(user)
    }
}
```

Works for small lists but composes everything.

Use `LazyColumn`.

---

# 17. JVM Iterator Internals

Kotlin.

```kotlin
for(user in users){
    println(user)
}
```

Compiler generates.

```java
Iterator iterator = users.iterator();

while(iterator.hasNext()){

    Object item = iterator.next();

}
```

Every `for` over collections uses an iterator.

---

## Range Optimization

```kotlin
for(i in 1..10)
```

No iterator object for primitive ranges.

Compiler optimizes into integer loop.

---

# 18. Performance Discussion

## `for` vs `forEach`

For collections:

- `for` often avoids lambda overhead.
- `forEach` is readable.

---

## Primitive Arrays

```kotlin
IntArray
```

Avoids boxing.

Better memory.

---

## Sequence

Better for multiple chained operations.

---

## Compose

Use `LazyColumn` for large datasets.

Avoid rendering thousands of composables with `Column`.

---

# 19. Android Best Practices

## RecyclerView Replacement

```kotlin
LazyColumn {
    items(messages) {
        MessageItem(it)
    }
}
```

---

## StateFlow Iteration

```kotlin
state.users.forEach {
    println(it.name)
}
```

---

## Repeat for Loading Skeleton

```kotlin
repeat(5) {
    LoadingCard()
}
```

Common Compose pattern.

---

# 20. Common Mistakes

### Mistake 1

Using `map` for side effects.

---

### Mistake 2

Using `forEach` when `break` is required.

---

### Mistake 3

Manual indexing.

Prefer `withIndex()`.

---

### Mistake 4

Using `Column` instead of `LazyColumn` for long lists.

---

### Mistake 5

Forgetting lazy sequences.

---

# 21. Real Interview Questions

## Basic

1. Difference between `for` and `while`.
2. Difference between `while` and `do...while`.
3. What does `repeat()` do?
4. What is `until`?
5. What is `downTo`?

---

## Intermediate

6. Difference between `forEach` and `for`.
7. Difference between `map` and `forEach`.
8. What is `withIndex()`?
9. What are labels?
10. Explain `return@forEach`.

---

## Advanced

11. How does Kotlin compile `for` loops?
12. Why are primitive array loops faster?
13. Difference between collection iteration and sequence iteration.
14. Why use `LazyColumn` instead of looping inside `Column`?
15. Explain non-local returns in inline functions.

---

# 22. 2-Minute Interview Answer

> Kotlin's `for` loop is built on the iterator pattern for collections, while primitive ranges are optimized into simple indexed loops. `forEach` is a higher-order inline function that's great for readability but doesn't support `break` or `continue` in the traditional sense. Labels enable controlled exits from nested loops and lambdas. In Android Compose, `LazyColumn` should be used instead of manually looping inside a `Column` for large datasets because it composes items lazily.

---

# 23. Cheat Sheet

| Syntax | Purpose |
|--------|---------|
| `1..5` | Inclusive range |
| `1 until 5` | Exclusive upper bound |
| `5 downTo 1` | Reverse range |
| `step 2` | Increment by step |
| `repeat(5)` | Execute block 5 times |
| `indices` | Iterate indexes |
| `withIndex()` | Index + value |
| `forEachIndexed` | Functional indexed iteration |
| `break@outer` | Break outer loop |
| `return@forEach` | Local return inside lambda |

---

# 📝 Revision Summary

- `for` is preferred for most iteration in Kotlin.
- `repeat()` is ideal for fixed iterations and Compose placeholders.
- `forEach` is for functional iteration; `map` is for transformation.
- Labels (`break@`, `continue@`, `return@forEach`) are important interview topics.
- Sequences provide lazy iteration and reduce intermediate allocations.
- In Compose, prefer `LazyColumn`/`LazyRow` over looping inside `Column`/`Row` for large collections.