
# 🧬 Generics in Kotlin — Android Interview Bible (2026 Edition)


## 📌 Module Information

| Property | Value |
|----------|-------|
| **Module** | Kotlin Functions & Type System |
| **File** | `17-Generics.md` |
| **Folder** | `01-Kotlin/03-Functions/` |
| **Difficulty** | Beginner → Staff Android Engineer |
| **Interview Frequency** | ⭐⭐⭐⭐⭐ Extremely High |
| **Companies** | Google, Uber, Amazon, Microsoft, PhonePe, CRED, Flipkart, Meesho |

---

# 📚 Complete Chapter Roadmap

## Part 1 — Fundamentals (This Part)

1. What are Generics?
2. Why Kotlin Introduced Generics?
3. Generic Classes.
4. Generic Functions.
5. Multiple Type Parameters.
6. Generic Constraints.
7. Generic Interfaces.
8. Generic Type Aliases.
9. Generic Extension Functions.
10. Real Android Examples.

## Part 2 — Variance & Type System

11. Invariance.
12. Covariance (`out`).
13. Contravariance (`in`).
14. Producer & Consumer Rule.
15. Star Projection (`*`).
16. Type Projections.
17. Declaration-site vs Use-site Variance.
18. PECS Principle.

## Part 3 — Android & Architecture

19. Repository Pattern.
20. Network Result Wrapper.
21. Flow Generics.
22. StateFlow & SharedFlow.
23. RecyclerView Generic Adapter.
24. Room DAO Generics.
25. Retrofit Generic APIs.
26. Compose Generic Components.
27. Navigation & Serialization.

## Part 4 — JVM Internals & Interview Mastery

28. Type Erasure.
29. Reified Generics.
30. JVM Bytecode.
31. Reflection.
32. Performance.
33. Testing Generic Code.
34. Best Practices.
35. Common Pitfalls.
36. 70+ Interview Questions.
37. Ultimate Cheat Sheet.

---

# 🎯 Learning Goals

After completing this chapter you'll understand:

- Why Generics exist in Kotlin.
- Compile-time type safety.
- Generic classes, functions, interfaces and constraints.
- Android production use cases with Flow, Retrofit, Room and Compose.
- JVM type erasure and reified generics.
- Senior interview questions asked in Android companies.

---

> **Part 1: Fundamentals of Generics**
>
> Learn Kotlin Generics from beginner to Staff Android Engineer level with JVM internals, variance (`in` / `out`), type erasure, reified types, Android examples, Compose, Flow, Room, Retrofit, KMP, performance, and interview questions.

---

# 1. What are Generics? ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Swiggy Delivery Box

Imagine Swiggy delivers many kinds of items.

```text
Delivery Box

📦 Pizza
📦 Burger
📦 Ice Cream
📦 Coke
📦 Grocery
```

Instead of creating a different box for every product, Swiggy creates **one reusable box**.

That is exactly what **Generics** do.

A Generic class is a reusable blueprint that works with **different data types** while maintaining **compile-time type safety**.

---

## Definition

Generics allow classes, interfaces and functions to work with **any type** without losing type safety.

```kotlin
class Box<T>(
    val item: T
)
```

Usage

```kotlin
val pizzaBox = Box("Pizza")

val moneyBox = Box(500)

val userBox = Box(User("Vikash"))
```

One class.

Multiple data types.

---

## Without Generics

```kotlin
class Box(
    val item: Any
)
```

Problem

```kotlin
val box = Box("Pizza")

val pizza = box.item as String
```

Unsafe cast required.

---

## With Generics

```kotlin
val box = Box("Pizza")

val pizza: String = box.item
```

Compiler knows the type.

No casting required.

---

## Why This Is Powerful

| Without Generics | With Generics |
|------------------|---------------|
| `Any` everywhere | Type-safe code |
| Runtime casting | Compile-time checking |
| More crashes | Safer APIs |
| Less IDE help | Better autocomplete |

---

# 2. Why Kotlin Introduced Generics? ⭐⭐⭐⭐⭐

## Problem Before Generics

Java collections originally stored everything as `Object`.

```java
List list = new ArrayList();

list.add("Android");
list.add(100);
list.add(true);
```

Everything mixes together.

---

## Runtime Crash

```java
String value = (String) list.get(1);
```

Throws `ClassCastException`.

---

## Kotlin Solution

```kotlin
val names = listOf(
    "Android",
    "Kotlin"
)
```

Compiler prevents invalid values.

---

## Type Safety Example

```kotlin
val numbers = mutableListOf<Int>()

numbers.add(10)

// numbers.add("Hello")
```

Compiler error before the app runs.

---

## Android Examples

Generics appear everywhere.

```kotlin
List<User>

Flow<UiState>

LiveData<List<User>>

Response<User>

PagingData<Product>

MutableStateFlow<LoginState>
```

Every Android developer uses Generics daily.

---

# 3. Generic Classes ⭐⭐⭐⭐⭐

## Basic Generic Class

```kotlin
class Box<T>(
    val value: T
)
```

Usage

```kotlin
val stringBox = Box("Hello")

val intBox = Box(100)

val userBox = Box(User("Vikash"))
```

---

## Generic Property

```kotlin
class Storage<T> {

    private var item: T? = null

    fun save(value: T) {
        item = value
    }

    fun get(): T? = item
}
```

Usage

```kotlin
val storage = Storage<String>()

storage.save("Android")

println(storage.get())
```

---

## Generic Container Example

```kotlin
class Cache<T> {

    private val items = mutableListOf<T>()

    fun add(item: T) {
        items.add(item)
    }

    fun getAll(): List<T> = items
}
```

Usage

```kotlin
val cache = Cache<Int>()

cache.add(1)
cache.add(2)
```

---

## Real Android Example — API Response Wrapper

```kotlin
data class ApiResponse<T>(
    val data: T,
    val success: Boolean
)
```

Usage

```kotlin
ApiResponse(
    data = User("Vikash"),
    success = true
)
```

Works for any API model.

---

# 4. Generic Functions ⭐⭐⭐⭐⭐

Generics are not limited to classes.

Functions can also be generic.

---

## Basic Generic Function

```kotlin
fun <T> printItem(item: T) {
    println(item)
}
```

Usage

```kotlin
printItem("Android")
printItem(101)
printItem(true)
```

---

## Identity Function

```kotlin
fun <T> identity(value: T): T {
    return value
}
```

Usage

```kotlin
val name = identity("Vikash")

val age = identity(28)
```

Compiler infers type automatically.

---

## Generic Swap Function

```kotlin
fun <T> MutableList<T>.swap(
    first: Int,
    second: Int
) {

    val temp = this[first]
    this[first] = this[second]
    this[second] = temp
}
```

Usage

```kotlin
val list = mutableListOf(1,2,3)

list.swap(0,2)
```

Output

```text
[3,2,1]
```

---

## Android Example — Navigation Helper

```kotlin
inline fun <reified T : Activity>
Context.openActivity() {

    startActivity(
        Intent(this, T::class.java)
    )
}
```

Generic Activity launcher.

---

# 5. Multiple Type Parameters ⭐⭐⭐⭐⭐

Classes can have multiple generic types.

---

## Pair Example

```kotlin
class PairBox<K, V>(
    val key: K,
    val value: V
)
```

Usage

```kotlin
PairBox(
    "USER_ID",
    101
)

PairBox(
    1,
    "Android"
)
```

---

## Triple Example

```kotlin
class TripleBox<A, B, C>(
    val first: A,
    val second: B,
    val third: C
)
```

Usage

```kotlin
TripleBox(
    "Vikash",
    28,
    true
)
```

---

## Android Example — Resource Wrapper

```kotlin
data class ApiState<T, E>(
    val data: T?,
    val error: E?
)
```

Different success and error models.

---

## Generic Map Entry

```kotlin
class Entry<K, V>(
    val key: K,
    val value: V
)
```

Similar to Kotlin Map.

---

# 6. Generic Constraints ⭐⭐⭐⭐⭐

Sometimes `T` shouldn't accept every type.

---

## Story — Online Payment

Only payment methods should be accepted.

---

## Upper Bound

```kotlin
open class Animal

class Dog : Animal()

class Cat : Animal()

class Car
```

Constraint

```kotlin
class AnimalBox<T : Animal>(
    val animal: T
)
```

Usage

```kotlin
AnimalBox(Dog())

AnimalBox(Cat())

// AnimalBox(Car())
```

Compiler rejects `Car`.

---

## Interface Constraint

```kotlin
interface Clickable

class Button : Clickable

class Card : Clickable
```

```kotlin
class ClickHandler<T : Clickable>(
    val view: T
)
```

---

## Multiple Constraints

```kotlin
fun <T> printInfo(item: T)
where
    T : Animal,
    T : Serializable {

    println(item)
}
```

T must satisfy both conditions.

---

## Android Example — ViewModel Constraint

```kotlin
inline fun <reified VM : ViewModel>
Fragment.viewModel() =
    ViewModelProvider(this)[VM::class.java]
```

Only ViewModel subclasses allowed.

---

# 7. Generic Interfaces ⭐⭐⭐⭐⭐

Interfaces become reusable with Generics.

---

## Repository Interface

```kotlin
interface Repository<T> {

    suspend fun getAll(): List<T>

    suspend fun save(item: T)
}
```

---

## User Repository

```kotlin
class UserRepository :
    Repository<User> {

    override suspend fun getAll() = emptyList<User>()

    override suspend fun save(item: User) {}
}
```

---

## Product Repository

```kotlin
class ProductRepository :
    Repository<Product> {

    override suspend fun getAll() = emptyList<Product>()

    override suspend fun save(item: Product) {}
}
```

Same interface.

Different models.

---

## Android DAO Example

```kotlin
interface BaseDao<T> {

    suspend fun insert(item: T)

    suspend fun delete(item: T)

    suspend fun update(item: T)
}
```

Very common Room architecture pattern.

---

# 8. Generic Type Aliases ⭐⭐⭐⭐

Type aliases improve readability.

---

## Example

```kotlin
typealias UserList = List<User>
```

Usage

```kotlin
fun users(): UserList
```

---

## Generic Alias

```kotlin
typealias Mapper<I, O> = (I) -> O
```

Usage

```kotlin
val mapper: Mapper<UserEntity, User> =
    { entity ->
        entity.toDomain()
    }
```

---

## API Alias

```kotlin
typealias ApiResult<T> =
    Result<ApiResponse<T>>
```

Cleaner repository signatures.

---

# 9. Generic Extension Functions ⭐⭐⭐⭐⭐

Extension Functions become even more powerful with Generics.

---

## List Extension

```kotlin
fun <T> List<T>.second(): T {
    return this[1]
}
```

Usage

```kotlin
listOf("A","B","C").second()

listOf(10,20,30).second()
```

---

## MutableStateFlow Extension

```kotlin
fun <T> MutableStateFlow<T>.updateState(
    transform: (T) -> T
) {
    value = transform(value)
}
```

Usage

```kotlin
uiState.updateState {
    it.copy(isLoading = true)
}
```

Production Compose pattern.

---

## RecyclerView Adapter Extension

```kotlin
fun <T> ListAdapter<T, *>.submit(
    list: List<T>
) {
    submitList(list.toList())
}
```

---

## Generic Validation Extension

```kotlin
fun <T> T.printType() {
    println(this)
}
```

Works for every object.

---

# 10. Real Android Examples ⭐⭐⭐⭐⭐

## Example 1 — Resource Wrapper

```kotlin
sealed class Resource<T> {

    class Loading<T> : Resource<T>()

    data class Success<T>(
        val data: T
    ) : Resource<T>()

    data class Error<T>(
        val message: String
    ) : Resource<T>()
}
```

Usage

```kotlin
Flow<Resource<User>>

Flow<Resource<List<Product>>>
```

---

## Example 2 — RecyclerView Generic Adapter

```kotlin
abstract class BaseAdapter<T> :
    RecyclerView.Adapter<BaseViewHolder<T>>()
```

Reusable adapter for any model.

---

## Example 3 — Generic API Response

```kotlin
data class ApiResponse<T>(
    val data: T,
    val message: String
)
```

Supports every endpoint.

---

## Example 4 — Generic Mapper

```kotlin
interface Mapper<I, O> {
    fun map(input: I): O
}
```

Implementation

```kotlin
class UserMapper :
    Mapper<UserEntity, User> {

    override fun map(input: UserEntity): User {
        return User(
            input.id,
            input.name
        )
    }
}
```

---

## Example 5 — Generic Paging

```kotlin
fun <T : Any> pagingFlow(
    source: PagingSource<Int, T>
): Flow<PagingData<T>>
```

Supports Users, Products, Orders, Stories.

---

# 11. Best Practices ⭐⭐⭐⭐⭐

## ✅ Use Generics For

- Repository pattern.
- API wrappers.
- Result wrappers.
- RecyclerView adapters.
- Paging.
- Compose reusable components.
- Flow utilities.

---

## ❌ Avoid

- Single-letter names everywhere (`A`, `B`, `C`) without meaning.
- Nested generic types that reduce readability.
- Generic classes without constraints when constraints are known.

---

## Naming Convention

| Generic | Meaning |
|---------|---------|
| `T` | Type |
| `R` | Return Type |
| `K` | Key |
| `V` | Value |
| `E` | Element |
| `I` | Input |
| `O` | Output |

---

# 12. Common Pitfalls ⭐⭐⭐⭐

## Pitfall 1 — Using `Any`

```kotlin
class Box(
    val item: Any
)
```

Lose compile-time safety.

---

## Pitfall 2 — Unsafe Cast

```kotlin
val user = item as User
```

Avoid unnecessary casts.

---

## Pitfall 3 — Missing Constraints

```kotlin
fun <T> printName(item: T) {
    println(item.name)
}
```

Compilation error because `T` may not have `name`.

---

## Pitfall 4 — Overusing Generics

Don't make every class generic.

Use when reuse is meaningful.

---

# 13. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Fundamentals

1. What are Generics?
2. Why Kotlin introduced Generics?
3. Difference between `Any` and Generics?
4. Generic Class vs Generic Function?
5. Type inference in Generics?

## Android

6. Why Flow uses Generics?
7. Why StateFlow is generic?
8. Why RecyclerView adapters are generic?
9. Why Retrofit Response is generic?
10. Generic Repository pattern?

---

# 📋 Cheat Sheet (Part 1)

## Generic Class

```kotlin
class Box<T>(val item: T)
```

---

## Generic Function

```kotlin
fun <T> identity(value: T): T = value
```

---

## Multiple Types

```kotlin
class PairBox<K, V>(
    val key: K,
    val value: V
)
```

---

## Constraint

```kotlin
class AnimalBox<T : Animal>(
    val animal: T
)
```

---

## Generic Interface

```kotlin
interface Repository<T>
```

---

## Generic Extension

```kotlin
fun <T> List<T>.second() = this[1]
```

---

## Type Alias

```kotlin
typealias Mapper<I, O> = (I) -> O
```

---

# 📝 Revision Summary

In **Part 1** you learned:

- What Generics are.
- Why Kotlin introduced them.
- Generic Classes.
- Generic Functions.
- Multiple Type Parameters.
- Generic Constraints.
- Generic Interfaces.
- Generic Type Aliases.
- Generic Extension Functions.
- Android production examples with Repository, Flow, RecyclerView and Retrofit.

---
# Part 2 — Variance, `in`, `out`, Star Projection & Kotlin Type System

> This is the **most important Generics topic for Android interviews**. Companies like **Google, Uber, PhonePe, CRED, Amazon, Flipkart, Swiggy, and Microsoft** frequently ask variance-related questions because they test deep understanding of Kotlin's type system.
>
> You'll learn **Invariance, Covariance (`out`), Contravariance (`in`), Producer/Consumer, Star Projection, Type Projection, Declaration-site vs Use-site Variance, and the PECS principle** with real Android examples.

---

# 📚 Table of Contents

11. Understanding Kotlin Type Hierarchy
12. Invariance
13. Covariance (`out`)
14. Contravariance (`in`)
15. Producer & Consumer Rule (PECS)
16. Declaration-site Variance
17. Use-site Variance
18. Type Projection
19. Star Projection (`*`)
20. Array Variance
21. Variance in Collections
22. Variance in Flow, LiveData & StateFlow
23. Android Production Examples
24. Best Practices
25. Common Pitfalls
26. Interview Questions
27. Cheat Sheet

---

# 🎯 Learning Goals

After this part you'll understand:

- Why `List<Dog>` is **not** a `List<Animal>`.
- Difference between `in` and `out`.
- How Kotlin prevents runtime crashes using variance.
- Why `Flow<UiState>` uses covariance.
- Why `Comparator<T>` uses contravariance.
- PECS rule used across Java and Kotlin.

---

# 11. Understanding Kotlin Type Hierarchy ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Animal Shelter

Imagine an animal shelter.

```text
            Animal
           /      \
         Dog      Cat
        /   \
    Husky   Pug
```

A **Dog** is an **Animal**.

A **Cat** is an **Animal**.

This inheritance is straightforward.

```kotlin
open class Animal

class Dog : Animal()

class Cat : Animal()
```

---

## Single Object Assignment

```kotlin
val animal: Animal = Dog()
```

Perfectly valid.

A Dog **is** an Animal.

---

## Collection Assignment

```kotlin
val dogs: List<Dog> = listOf(Dog())

val animals: List<Animal> = dogs
```

This is also valid in Kotlin.

Why?

Because `List` is declared as **covariant** (`out T`).

---

## Mutable Collection Assignment

```kotlin
val dogs = mutableListOf(Dog())

val animals: MutableList<Animal> = dogs
```

Compilation Error.

Why?

Because `MutableList` allows adding items.

This could break type safety.

---

## Visualization

```text
Dog ✅ Animal

List<Dog> ✅ List<Animal>

MutableList<Dog> ❌ MutableList<Animal>
```

Understanding **why** is the entire purpose of variance.

---

# 12. Invariance ⭐⭐⭐⭐⭐

## Definition

A generic type is **Invariant** when two generic types have **no subtype relationship**, even if their contained types do.

---

## Example

```kotlin
class Box<T>(
    var value: T
)
```

Usage

```kotlin
val dogBox = Box(Dog())

// val animalBox: Box<Animal> = dogBox
```

Compilation Error.

---

## Why?

Suppose Kotlin allowed it.

```kotlin
val dogBox = Box(Dog())

val animalBox: Box<Animal> = dogBox

animalBox.value = Cat()
```

Now `dogBox` contains a Cat.

Type safety is broken.

---

## Memory Visualization

```text
dogBox
 │
 ▼
Box<Dog>

animalBox
 │
 ▼
Same Object

Insert Cat ❌
```

This is exactly why **invariance exists**.

---

## MutableList is Invariant

```kotlin
val dogs = mutableListOf(Dog())

// Error
val animals: MutableList<Animal> = dogs
```

Compiler prevents invalid insertion.

---

## Android Example

```kotlin
MutableStateFlow<LoginState>
```

Cannot become

```kotlin
MutableStateFlow<UiState>
```

Because values can be written.

---

## Interview Tip

> Mutable generic containers are usually **Invariant**.

Examples:

- `MutableList`
- `MutableMap`
- `Array`
- `MutableStateFlow`

---

# 13. Covariance (`out`) ⭐⭐⭐⭐⭐

## 🎭 Story — YouTube Video Streaming

Netflix streams videos.

Users only **watch** videos.

Users never insert videos into Netflix servers.

Netflix is a **Producer**.

---

## Definition

A covariant generic **only produces values**.

Uses `out`.

---

## Example

```kotlin
class Box<out T>(
    private val value: T
) {
    fun get(): T = value
}
```

Notice:

`T` is returned.

Never accepted as parameter.

---

## Assignment

```kotlin
val dogBox: Box<Dog> = Box(Dog())

val animalBox: Box<Animal> = dogBox
```

Perfectly safe.

---

## Why Safe?

You only read.

```kotlin
val animal = animalBox.get()
```

Returns an Animal.

Actually returns Dog.

Safe.

---

## Visualization

```text
Dog Producer

Dog
 │
 ▼
Animal Receiver

Safe
```

---

## Kotlin List is Covariant

Declaration inside Kotlin.

```kotlin
interface List<out E>
```

That's why this works.

```kotlin
val dogs = listOf(Dog())

val animals: List<Animal> = dogs
```

---

## Why Can't We Add?

```kotlin
animals.add(Cat())
```

Impossible.

`List` has no add().

---

## Android Example — Flow

```kotlin
Flow<UiState>
```

Flow emits values.

It produces.

Therefore it is covariant.

---

## LiveData Example

```kotlin
LiveData<User>
```

Can become

```kotlin
LiveData<Person>
```

Because LiveData emits.

---

## Compose Example

```kotlin
State<User>
```

Produces state.

Read-only state uses covariance.

---

# 14. Contravariance (`in`) ⭐⭐⭐⭐⭐

## 🎭 Story — Food Delivery App

Delivery partner accepts orders.

They **consume** packages.

Consumers receive values.

---

## Definition

Contravariant types only **consume values**.

Uses `in`.

---

## Example

```kotlin
interface Sender<in T> {

    fun send(item: T)
}
```

---

## Implementation

```kotlin
class AnimalSender : Sender<Animal> {

    override fun send(item: Animal) {
        println(item)
    }
}
```

Usage

```kotlin
val sender: Sender<Dog> = AnimalSender()

sender.send(Dog())
```

Works.

---

## Why Safe?

Animal sender accepts Dogs.

Dogs are Animals.

No issue.

---

## Visualization

```text
Dog

▼

Animal Consumer

Safe
```

---

## Comparator Example

Comparator consumes objects.

```kotlin
Comparator<in T>
```

Example

```kotlin
Comparator<Animal>
```

Can compare Dogs.

---

## Android Example

```kotlin
DiffUtil.ItemCallback<in T>
```

Consumes items for comparison.

---

## Serializer Example

```kotlin
JsonAdapter<in T>
```

Consumes objects to serialize.

---

## Interview Tip

Consumers use **`in`**.

---

# 15. Producer & Consumer Rule (PECS) ⭐⭐⭐⭐⭐

## PECS Principle

> **Producer Extends, Consumer Super**

Java mnemonic.

Kotlin equivalent:

| Java | Kotlin |
|------|---------|
| `? extends T` | `out T` |
| `? super T` | `in T` |

---

## Producer Example

```kotlin
interface Producer<out T> {
    fun produce(): T
}
```

---

## Consumer Example

```kotlin
interface Consumer<in T> {
    fun consume(item: T)
}
```

---

## Android Examples

| Class | Variance | Reason |
|--------|----------|--------|
| `List<T>` | `out` | Read-only collection |
| `Flow<T>` | `out` | Emits values |
| `LiveData<T>` | `out` | Emits values |
| `Comparator<T>` | `in` | Consumes values |
| `MutableList<T>` | Invariant | Reads and writes |
| `MutableStateFlow<T>` | Invariant | Reads and writes |

---

## Story — Spotify Playlist

Spotify Playlist

Produces songs.

```text
Playlist
   ▼
Song
```

Producer.

Music Player

Consumes songs.

```text
Song
 ▼
Player
```

Consumer.

---

# 16. Declaration-site Variance ⭐⭐⭐⭐⭐

Kotlin lets library authors define variance once.

---

## Declaration Example

```kotlin
interface Source<out T> {
    fun get(): T
}
```

Now every usage is covariant automatically.

---

## Consumer Declaration

```kotlin
interface Sink<in T> {
    fun save(item: T)
}
```

---

## Why Better Than Java?

Java requires variance everywhere.

Kotlin declares once.

Cleaner API design.

---

## Android Example

`Flow`

```kotlin
interface Flow<out T>
```

Every Flow is already covariant.

---

## PagingData Example

`PagingData<out T>`

Read-only stream.

---

## Compose Example

`State<out T>`

Read-only state.

---

# 17. Use-site Variance ⭐⭐⭐⭐⭐

Sometimes you cannot modify generic declaration.

Use variance during usage.

---

## Example

```kotlin
fun copyAnimals(
    from: Array<out Animal>,
    to: Array<Animal>
)
```

`from` produces animals.

---

## Consumer Example

```kotlin
fun fillDogs(
    array: Array<in Dog>
)
```

Array accepts Dogs.

---

## Why Needed?

`Array<T>` is invariant.

Use projections when passing arrays.

---

## Android Example

RecyclerView adapter utilities.

```kotlin
fun bindItems(
    items: List<out UiModel>
)
```

Supports multiple UI model subclasses.

---

# 18. Type Projection ⭐⭐⭐⭐⭐

Restrict generic operations temporarily.

---

## Out Projection

```kotlin
fun read(box: Box<out Animal>) {
    println(box.get())
}
```

Read only.

---

## In Projection

```kotlin
fun write(box: Box<in Dog>) {
    box.put(Dog())
}
```

Write only.

---

## Visualization

```text
out

Read

in

Write
```

---

## Android Example

Repository mapper.

```kotlin
Mapper<in Entity, out Domain>
```

Consumes entity.

Produces domain.

Perfect architecture design.

---

# 19. Star Projection (`*`) ⭐⭐⭐⭐⭐

## Story — Unknown Package

Courier arrives.

Package type unknown.

Still can receive it.

---

## Definition

Star projection represents **unknown generic type** safely.

---

## Example

```kotlin
val list: List<*> =
    listOf("A", "B")
```

Compiler knows list exists.

Doesn't know element type.

---

## Reading Values

```kotlin
val item = list.first()
```

Type becomes `Any?`.

---

## Writing Values

```kotlin
list.add(...)
```

Impossible.

Unknown type.

---

## Comparison

| Type | Meaning |
|------|---------|
| `List<Any>` | List of Any |
| `List<*>` | Unknown List |
| `List<out Any?>` | Equivalent projection |

---

## Android Example

```kotlin
RecyclerView.Adapter<*>
```

Accepts any adapter.

---

## Reflection Example

```kotlin
KClass<*>
```

Unknown class type.

---

# 20. Array Variance ⭐⭐⭐⭐⭐

Arrays are invariant.

---

## Example

```kotlin
val dogs: Array<Dog> =
    arrayOf(Dog())

// Error
val animals: Array<Animal> = dogs
```

---

## Out Projection Fix

```kotlin
fun printAnimals(
    animals: Array<out Animal>
)
```

Now read-only.

---

## In Projection

```kotlin
fun addDog(
    animals: Array<in Dog>
)
```

Now write-only.

---

## Android Example

Bitmap processing arrays.

Drawable arrays.

Image filters.

---

# 21. Variance in Collections ⭐⭐⭐⭐⭐

## Read-only Collections

```kotlin
List<out T>
Set<out T>
Map<out K, out V>
```

Safe to read.

---

## Mutable Collections

```kotlin
MutableList<T>

MutableSet<T>

MutableMap<K,V>
```

Invariant.

---

## Android Example

```kotlin
List<UiModel>
```

Safe to expose.

Repository returns immutable collections.

---

## Best Practice

Expose

```kotlin
List<User>
```

Hide

```kotlin
MutableList<User>
```

Encapsulation.

---

# 22. Variance in Flow, LiveData & StateFlow ⭐⭐⭐⭐⭐

## Flow

```kotlin
Flow<out T>
```

Producer.

---

## LiveData

```kotlin
LiveData<out T>
```

Producer.

---

## StateFlow

```kotlin
StateFlow<out T>
```

Read-only producer.

---

## MutableStateFlow

```kotlin
MutableStateFlow<T>
```

Invariant.

Because you can write values.

---

## SharedFlow

Produces values.

Covariant.

---

## Android MVVM Example

Expose

```kotlin
StateFlow<UiState>
```

Keep private

```kotlin
MutableStateFlow<UiState>
```

Excellent architecture pattern.

---

# 23. Android Production Examples ⭐⭐⭐⭐⭐

## Repository Pattern

```kotlin
interface Repository<out T> {
    suspend fun get(): T
}
```

Repository produces data.

---

## Mapper Pattern

```kotlin
interface Mapper<in E, out D> {
    fun map(entity: E): D
}
```

Perfect Clean Architecture.

---

## RecyclerView DiffUtil

Consumes models.

```kotlin
DiffUtil.ItemCallback<in T>
```

---

## Flow Result Wrapper

```kotlin
Flow<Resource<User>>
```

Produces resources.

---

## Compose State

```kotlin
State<LoginUiState>
```

Read-only state.

---

## Navigation Result

```kotlin
Navigator<in Destination>
```

Consumes destinations.

---

# 24. Best Practices ⭐⭐⭐⭐⭐

## ✅ Use `out` When

- Repository returns data.
- Flow emits data.
- LiveData exposes data.
- Read-only collections.

---

## ✅ Use `in` When

- Serializer consumes objects.
- Comparator compares objects.
- Mapper input parameter.
- Event dispatcher.

---

## ✅ Use Immutable Collections

Expose `List<T>`.

Avoid exposing mutable lists.

---

## Naming Generic Parameters

| Generic | Use |
|---------|-----|
| `T` | Type |
| `R` | Result |
| `I` | Input |
| `O` | Output |
| `E` | Entity |
| `D` | Domain |

---

# 25. Common Pitfalls ⭐⭐⭐⭐

## Pitfall 1 — Confusing `in` and `out`

Remember:

> Producer = `out`

> Consumer = `in`

---

## Pitfall 2 — Mutable Collections

Never expose mutable collections publicly.

---

## Pitfall 3 — Array Variance

Arrays need projections.

---

## Pitfall 4 — Star Projection Misuse

`List<*>` is not `List<Any>`.

---

## Pitfall 5 — Forgetting Type Safety

Avoid unsafe casts with generics.

---

# 26. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Variance

1. What is variance?
2. What is invariance?
3. Difference between covariance and contravariance?
4. Why is List covariant?
5. Why is MutableList invariant?

## Android

6. Why is Flow covariant?
7. Why is MutableStateFlow invariant?
8. Why does DiffUtil use `in`?
9. Repository `out` or `in`?
10. Mapper variance?

## Advanced

11. Star Projection vs `Any`.
12. Declaration-site vs Use-site variance.
13. PECS principle.
14. Array variance.
15. Type projection examples.

---

# 📋 Cheat Sheet (Part 2)

## Covariant

```kotlin
interface Producer<out T>
```

Produces values.

---

## Contravariant

```kotlin
interface Consumer<in T>
```

Consumes values.

---

## Invariant

```kotlin
class Box<T>
```

Read and write.

---

## Use-site Variance

```kotlin
Array<out Animal>

Array<in Dog>
```

---

## Star Projection

```kotlin
List<*>

RecyclerView.Adapter<*>

KClass<*>
```

---

## Producer / Consumer

```text
Producer → out

Consumer → in
```

---

## Immutable Collections

```kotlin
List<out T>
```

---

## Mutable Collections

```kotlin
MutableList<T>
```

Invariant.

---

# 📝 Revision Summary

In **Part 2** you learned:

- Kotlin Type Hierarchy.
- Invariance.
- Covariance (`out`).
- Contravariance (`in`).
- PECS Rule.
- Declaration-site Variance.
- Use-site Variance.
- Type Projection.
- Star Projection.
- Array Variance.
- Variance in Collections, Flow, LiveData and StateFlow.
- Android production architecture examples.

---

# Part 3 — Android Architecture with Generics (Repository, Flow, Room, Retrofit, Compose & RecyclerView)

> This part focuses on **how Generics are used in real Android applications**. Every modern Android codebase—Google's architecture samples, Jetpack libraries, Retrofit, Room, Paging 3, Flow, Compose, and Clean Architecture—relies heavily on Generics.
>
> This chapter teaches production-ready patterns used by senior Android engineers.

---

# 📚 Table of Contents

19. Generics in Clean Architecture
20. Generic Repository Pattern
21. Generic UseCase Pattern
22. Generic Result Wrapper
23. Generic API Response with Retrofit
24. Generic Room DAO
25. Generic RecyclerView Adapter
26. Generic Paging 3 Adapter
27. Generic StateFlow & SharedFlow
28. Generic Flow Operators
29. Generic Compose Components
30. Generic Navigation & SavedStateHandle
31. Generic Mapper Pattern
32. Generic Validation Framework
33. Generic Cache Layer
34. Generic Event Bus Pattern
35. Best Practices
36. Common Pitfalls
37. Senior Android Interview Questions
38. Cheat Sheet

---

# 🎯 Learning Goals

After completing this part you'll understand how to build reusable Android architecture components using Generics.

You'll be able to build:

- Repository layer.
- UseCase layer.
- API layer.
- Database layer.
- RecyclerView framework.
- Compose reusable UI components.
- State management framework.
- Validation library.
- Generic caching library.

---

# 19. Generics in Clean Architecture ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Swiggy Architecture

Swiggy has many entities.

```text
🍕 Food
👤 User
🛒 Cart
📍 Address
🧾 Order
```

Instead of writing five repositories from scratch, create one reusable blueprint.

---

## Clean Architecture Layers

```text
Presentation
      │
      ▼
Domain
      │
      ▼
Repository
      │
      ▼
Remote + Local
```

Generics remove duplication across every layer.

---

## Repository Blueprint

```kotlin
interface Repository<T> {

    suspend fun getAll(): List<T>

    suspend fun getById(id: String): T?

    suspend fun insert(item: T)

    suspend fun delete(item: T)
}
```

One interface.

Works for every entity.

---

## Implementations

```kotlin
class UserRepository : Repository<User>

class ProductRepository : Repository<Product>

class StoryRepository : Repository<Story>
```

Zero duplicated contracts.

---

## Why Companies Prefer This Pattern

| Benefit | Why Important |
|---------|---------------|
| Reusable contracts | Less boilerplate. |
| Easy testing | Mock one interface. |
| Consistent APIs | Every repository behaves similarly. |
| Scalable | Hundreds of entities supported. |

---

# 20. Generic Repository Pattern ⭐⭐⭐⭐⭐

## Basic Repository

```kotlin
interface Repository<T> {

    suspend fun save(item: T)

    suspend fun update(item: T)

    suspend fun delete(item: T)

    suspend fun getAll(): List<T>

    suspend fun getById(id: String): T?
}
```

---

## User Repository

```kotlin
class UserRepository(
    private val dao: UserDao
) : Repository<User> {

    override suspend fun save(item: User) {
        dao.insert(item.toEntity())
    }

    override suspend fun update(item: User) {}

    override suspend fun delete(item: User) {}

    override suspend fun getAll(): List<User> {
        return dao.getUsers().map { it.toDomain() }
    }

    override suspend fun getById(id: String): User? = null
}
```

---

## Firebase Repository Example

```kotlin
interface FirestoreRepository<T> {

    suspend fun add(item: T)

    suspend fun update(id: String, item: T)

    suspend fun delete(id: String)

    suspend fun get(id: String): T?
}
```

Perfect for Firestore collections.

---

## Offline-First Repository

```kotlin
interface OfflineRepository<T> {

    suspend fun remote(): List<T>

    suspend fun local(): List<T>

    suspend fun sync()
}
```

Used in enterprise Android apps.

---

# 21. Generic UseCase Pattern ⭐⭐⭐⭐⭐

## Story — Every Screen Has Different Business Logic

Login.

Signup.

Search.

Fetch Profile.

All execute differently.

But structure is identical.

---

## Generic UseCase

```kotlin
abstract class UseCase<Input, Output> {

    abstract suspend operator fun invoke(
        input: Input
    ): Output
}
```

---

## Login UseCase

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
) : UseCase<LoginRequest, LoginResponse>() {

    override suspend fun invoke(
        input: LoginRequest
    ): LoginResponse {

        return repository.login(input)
    }
}
```

---

## Search UseCase

```kotlin
class SearchUseCase(
    private val repository: SearchRepository
) : UseCase<String, List<Product>>() {

    override suspend fun invoke(
        input: String
    ): List<Product> {

        return repository.search(input)
    }
}
```

---

## Why This Pattern?

Consistent architecture.

Easy testing.

Reusable base class.

---

# 22. Generic Result Wrapper ⭐⭐⭐⭐⭐

One of the most important Android interview topics.

---

## Resource Wrapper

```kotlin
sealed class Resource<T> {

    class Loading<T> : Resource<T>()

    data class Success<T>(
        val data: T
    ) : Resource<T>()

    data class Error<T>(
        val message: String
    ) : Resource<T>()
}
```

---

## Usage with Flow

```kotlin
Flow<Resource<User>>
```

---

## Repository Example

```kotlin
suspend fun getProfile(): Resource<User> {

    return try {

        Resource.Success(api.profile())

    } catch (e: Exception) {

        Resource.Error(e.message ?: "Unknown Error")
    }
}
```

---

## UI Example

```kotlin
when(resource){

    is Resource.Loading -> {}

    is Resource.Success -> {}

    is Resource.Error -> {}
}
```

---

## Why Sealed + Generic?

Supports every response model.

---

# 23. Generic API Response with Retrofit ⭐⭐⭐⭐⭐

## API Wrapper

```kotlin
data class ApiResponse<T>(
    val success: Boolean,
    val message: String,
    val data: T
)
```

---

## Retrofit Interface

```kotlin
interface UserApi {

    @GET("profile")
    suspend fun profile():
        ApiResponse<User>
}
```

---

## Another Endpoint

```kotlin
interface ProductApi {

    @GET("products")
    suspend fun products():
        ApiResponse<List<Product>>
}
```

---

## Generic Safe API Call

```kotlin
suspend fun <T> safeApiCall(
    api: suspend () -> ApiResponse<T>
): Resource<T> {

    return try {

        Resource.Success(api().data)

    } catch (e: Exception) {

        Resource.Error(e.message ?: "Network Error")
    }
}
```

Usage

```kotlin
safeApiCall {
    api.profile()
}
```

Production repository pattern.

---

# 24. Generic Room DAO ⭐⭐⭐⭐⭐

## Base DAO

```kotlin
@Dao
interface BaseDao<T> {

    @Insert
    suspend fun insert(item: T)

    @Update
    suspend fun update(item: T)

    @Delete
    suspend fun delete(item: T)
}
```

---

## User DAO

```kotlin
@Dao
interface UserDao : BaseDao<UserEntity> {

    @Query("SELECT * FROM users")
    suspend fun getUsers(): List<UserEntity>
}
```

---

## Story DAO

```kotlin
@Dao
interface StoryDao : BaseDao<StoryEntity>
```

---

## Benefits

- Shared CRUD.
- Less boilerplate.
- Easier migrations.

---

## Generic Transaction Helper

```kotlin
suspend fun <T> RoomDatabase.transaction(
    block: suspend () -> T
): T {

    return withTransaction {
        block()
    }
}
```

---

# 25. Generic RecyclerView Adapter ⭐⭐⭐⭐⭐

## Story — Instagram Feed

Feed contains

- Stories
- Reels
- Ads
- Posts

Need reusable adapter.

---

## Generic ViewHolder

```kotlin
abstract class BaseViewHolder<T>(
    itemView: View
) : RecyclerView.ViewHolder(itemView) {

    abstract fun bind(item: T)
}
```

---

## Generic Adapter

```kotlin
abstract class BaseAdapter<T> :
    RecyclerView.Adapter<BaseViewHolder<T>>() {

    protected val items = mutableListOf<T>()

    fun submit(items: List<T>) {

        this.items.clear()
        this.items.addAll(items)

        notifyDataSetChanged()
    }

    override fun getItemCount() = items.size
}
```

---

## User Adapter

```kotlin
class UserAdapter :
    BaseAdapter<User>()
```

---

## Story Adapter

```kotlin
class StoryAdapter :
    BaseAdapter<Story>()
```

Reusable framework.

---

## DiffUtil Generic Adapter

```kotlin
abstract class DiffAdapter<T>(
    callback: DiffUtil.ItemCallback<T>
) : ListAdapter<T, RecyclerView.ViewHolder>(callback)
```

Modern production adapter.

---

# 26. Generic Paging 3 Adapter ⭐⭐⭐⭐⭐

## Paging Adapter

```kotlin
abstract class BasePagingAdapter<T : Any>(
    diff: DiffUtil.ItemCallback<T>
) : PagingDataAdapter<T, RecyclerView.ViewHolder>(diff)
```

---

## Product Paging

```kotlin
class ProductPagingAdapter :
    BasePagingAdapter<Product>(ProductDiff())
```

---

## Generic LoadState Footer

```kotlin
fun <T : Any> PagingDataAdapter<T, *>.withFooter(
    footer: LoadStateAdapter<*>
)
```

Reusable paging configuration.

---

## Flow Example

```kotlin
Flow<PagingData<Product>>
```

Generics power Paging.

---

# 27. Generic StateFlow & SharedFlow ⭐⭐⭐⭐⭐

## Generic UI State

```kotlin
data class UiState<T>(
    val data: T? = null,
    val isLoading: Boolean = false,
    val error: String? = null
)
```

---

## ViewModel

```kotlin
private val _state =
    MutableStateFlow(UiState<User>())

val state: StateFlow<UiState<User>> = _state
```

---

## Update Extension

```kotlin
fun <T> MutableStateFlow<T>.updateState(
    block: (T) -> T
){
    value = block(value)
}
```

---

## SharedFlow Event

```kotlin
MutableSharedFlow<UiEvent>()
```

Generic event stream.

---

## Login State

```kotlin
StateFlow<UiState<LoginResponse>>
```

Reusable state wrapper.

---

# 28. Generic Flow Operators ⭐⭐⭐⭐⭐

## Success Only

```kotlin
fun <T> Flow<Resource<T>>.successOnly() =
    filterIsInstance<Resource.Success<T>>()
```

---

## Error Only

```kotlin
fun <T> Flow<Resource<T>>.errorOnly() =
    filterIsInstance<Resource.Error<T>>()
```

---

## Loading Only

```kotlin
fun <T> Flow<Resource<T>>.loadingOnly() =
    filterIsInstance<Resource.Loading<T>>()
```

---

## Map Resource

```kotlin
fun <T, R> Flow<Resource<T>>.mapResource(
    mapper: (T) -> R
): Flow<Resource<R>>
```

Very useful in Clean Architecture.

---

## Android Example

Entity → Domain mapping inside Flow.

---

# 29. Generic Compose Components ⭐⭐⭐⭐⭐

## Story — Reusable UI Card

Different data.

Same UI structure.

---

## Generic Card

```kotlin
@Composable
fun <T> InfoCard(
    item: T,
    title: (T) -> String,
    subtitle: (T) -> String
) {
    Column {

        Text(title(item))

        Text(subtitle(item))
    }
}
```

---

## User Card

```kotlin
InfoCard(
    item = user,
    title = { it.name },
    subtitle = { it.email }
)
```

---

## Product Card

```kotlin
InfoCard(
    item = product,
    title = { it.title },
    subtitle = { "₹${it.price}" }
)
```

Reusable Compose UI.

---

## Generic LazyColumn

```kotlin
@Composable
fun <T> GenericList(
    items: List<T>,
    content: @Composable (T) -> Unit
)
```

Compose DSL heavily uses generics.

---

## Generic Dropdown

```kotlin
@Composable
fun <T> DropdownMenu(
    items: List<T>,
    label: (T) -> String
)
```

---

# 30. Generic Navigation & SavedStateHandle ⭐⭐⭐⭐⭐

## SavedState Helper

```kotlin
inline fun <reified T>
SavedStateHandle.require(
    key: String
): T {
    return get<T>(key)!!
}
```

---

## Usage

```kotlin
val userId: String =
    savedStateHandle.require("USER_ID")
```

---

## Generic Argument Extension

```kotlin
inline fun <reified T>
Bundle.require(
    key: String
): T = get(key) as T
```

---

## Generic Navigation Result

```kotlin
inline fun <reified T>
NavBackStackEntry.result(
    key: String
): T?
```

Modern Navigation pattern.

---

# 31. Generic Mapper Pattern ⭐⭐⭐⭐⭐

## Story — Room Entity → Domain Model

Entity and UI model differ.

Need reusable mapper.

---

## Generic Interface

```kotlin
interface Mapper<I, O> {
    fun map(input: I): O
}
```

---

## User Mapper

```kotlin
class UserMapper :
    Mapper<UserEntity, User> {

    override fun map(input: UserEntity): User {
        return User(
            input.id,
            input.name
        )
    }
}
```

---

## Bidirectional Mapper

```kotlin
interface BiMapper<I, O> {

    fun toDomain(input: I): O

    fun toEntity(output: O): I
}
```

---

## Extension Mapper

```kotlin
fun UserEntity.toDomain() = User(
    id,
    name
)
```

Clean Architecture favorite.

---

# 32. Generic Validation Framework ⭐⭐⭐⭐⭐

## Validation Result

```kotlin
sealed class ValidationResult<T> {

    data class Success<T>(
        val value: T
    ) : ValidationResult<T>()

    data class Error<T>(
        val message: String
    ) : ValidationResult<T>()
}
```

---

## Validator Interface

```kotlin
interface Validator<T> {

    fun validate(
        value: T
    ): ValidationResult<T>
}
```

---

## Email Validator

```kotlin
class EmailValidator :
    Validator<String> {

    override fun validate(
        value: String
    ): ValidationResult<String> {

        return ValidationResult.Success(value)
    }
}
```

Reusable validation framework.

---

# 33. Generic Cache Layer ⭐⭐⭐⭐⭐

## Memory Cache

```kotlin
class MemoryCache<K, V> {

    private val cache = mutableMapOf<K, V>()

    fun put(key: K, value: V) {
        cache[key] = value
    }

    fun get(key: K): V? = cache[key]
}
```

---

## Usage

```kotlin
val cache = MemoryCache<String, User>()

cache.put("USR101", user)
```

---

## LRU Cache Wrapper

```kotlin
class ImageCache<T>(
    private val cache: LruCache<String, T>
)
```

Supports Bitmap, Drawable, etc.

---

# 34. Generic Event Bus Pattern ⭐⭐⭐⭐⭐

## Event Wrapper

```kotlin
sealed class Event<T> {

    data class Success<T>(
        val data: T
    ) : Event<T>()

    data class Error<T>(
        val message: String
    ) : Event<T>()
}
```

---

## SharedFlow Event Bus

```kotlin
class EventBus<T> {

    private val events =
        MutableSharedFlow<T>()

    suspend fun emit(event: T) {
        events.emit(event)
    }

    fun flow() = events
}
```

---

## Usage

```kotlin
EventBus<UiEvent>()

EventBus<LoginEvent>()
```

Reusable event architecture.

---

# 35. Best Practices ⭐⭐⭐⭐⭐

## ✅ Use Generics For

- Repository interfaces.
- API wrappers.
- Room DAO base classes.
- RecyclerView adapters.
- Compose reusable UI.
- Mapper interfaces.
- Validation framework.
- State wrappers.
- Cache layers.

---

## Keep Generic APIs Focused

Good

```kotlin
Mapper<I, O>
```

Bad

```kotlin
MegaManager<T, R, E, V>
```

Too many responsibilities.

---

## Prefer Immutable Generic Models

```kotlin
StateFlow<UiState<User>>
```

Expose immutable state.

---

## Use Meaningful Generic Names

| Generic | Meaning |
|---------|---------|
| `T` | Type |
| `I` | Input |
| `O` | Output |
| `E` | Entity |
| `D` | Domain |
| `K` | Key |
| `V` | Value |

---

# 36. Common Pitfalls ⭐⭐⭐⭐

## Pitfall 1 — Using `Any`

Lose compile-time safety.

---

## Pitfall 2 — Overusing Generics

Not every class needs Generics.

---

## Pitfall 3 — Deep Nested Generics

```kotlin
Flow<Resource<ApiResponse<List<User>>>>
```

Can reduce readability.

Use type aliases when appropriate.

---

## Pitfall 4 — Mutable Generic Exposure

Expose immutable collections instead.

---

## Pitfall 5 — Business Logic Inside Generic Base Classes

Keep base classes reusable.

---

# 37. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Repository

1. Why Repository is generic?
2. Generic DAO benefits?
3. Generic UseCase advantages?

## Retrofit

4. Generic API wrapper?
5. Safe API call with Generics?

## Flow

6. Why `Resource<T>` with Flow?
7. Generic StateFlow pattern?

## Compose

8. Generic composables?
9. Generic LazyColumn implementation?

## Architecture

10. Mapper interface design?
11. Validation framework using Generics?
12. Cache implementation?

---

# 📋 Cheat Sheet (Part 3)

## Generic Repository

```kotlin
interface Repository<T>
```

---

## Generic UseCase

```kotlin
UseCase<Input, Output>
```

---

## Generic Result

```kotlin
Resource<T>
```

---

## Generic API

```kotlin
ApiResponse<T>
```

---

## Generic DAO

```kotlin
BaseDao<T>
```

---

## Generic Adapter

```kotlin
BaseAdapter<T>
```

---

## Generic State

```kotlin
UiState<T>
```

---

## Generic Mapper

```kotlin
Mapper<I, O>
```

---

## Generic Validator

```kotlin
Validator<T>
```

---

## Generic Cache

```kotlin
MemoryCache<K, V>
```

---

# 📝 Revision Summary

In **Part 3** you learned:

- Generics in Clean Architecture.
- Generic Repository pattern.
- Generic UseCase pattern.
- Generic Result wrapper.
- Retrofit generic responses.
- Room generic DAO.
- RecyclerView generic adapter.
- Paging 3 generic adapter.
- StateFlow & SharedFlow generics.
- Generic Flow operators.
- Generic Compose components.
- Generic Navigation helpers.
- Generic Mapper pattern.
- Generic Validation framework.
- Generic Cache and Event Bus patterns.

---
# Part 4 — Type Erasure, Reified, JVM Internals, Performance & Interview Mastery

> This is the final and most advanced part of the **Generics** chapter. You'll understand how Generics actually work inside the JVM, why **Type Erasure** exists, how Kotlin solves it with **Reified Generics**, performance implications, reflection, Kotlin Multiplatform behavior, testing strategies, and 70+ senior Android interview questions.

---

# 📚 Table of Contents

39. Type Erasure
40. JVM Bytecode of Generics
41. Why Reified Generics Exist
42. Reified Generic Functions
43. Reflection with Generics
44. Generic Arrays
45. Generic Performance
46. Kotlin Multiplatform Generics
47. Testing Generic Code
48. Production Design Patterns
49. Common Production Bugs
50. Best Practices Checklist
51. 70+ Senior Android Interview Questions
52. Ultimate Cheat Sheet
53. Chapter Revision Summary

---

# 🎯 Learning Goals

After completing this part you'll understand:

- JVM implementation of Generics.
- Type Erasure in Java & Kotlin.
- Inline + Reified Generics.
- Reflection limitations.
- Performance of generic code.
- Generic testing strategies.
- Enterprise Android architecture patterns.

---

# 39. Type Erasure ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Airport Security Tags

Imagine an airport luggage belt.

Initially every bag has detailed labels.

```text
🧳 User Bag
🧳 Product Bag
🧳 Story Bag
```

After passing security, all bags lose detailed labels and become generic luggage.

This is exactly what **Type Erasure** does.

---

## Definition

**Type Erasure** means generic type information is removed during JVM compilation.

---

## Kotlin Code

```kotlin
class Box<T>(
    val value: T
)

val stringBox = Box("Android")
val intBox = Box(100)
```

---

## JVM Sees

```java
public final class Box {

    private final Object value;

    public Object getValue() {
        return value;
    }
}
```

Everything becomes `Object`.

---

## Visualization

```text
Compile Time

Box<String>
Box<Int>

        │

Type Erasure

        ▼

Box<Object>
```

---

## Why JVM Uses Type Erasure?

Java introduced Generics in Java 5 without breaking old JVM bytecode.

Kotlin inherits JVM behavior.

---

## Important Consequences

- Runtime doesn't know `T`.
- `is T` is impossible.
- `T::class` is impossible.
- Reflection loses generic type.

---

## Interview Tip

> Kotlin Generics are **compile-time only** unless using `reified`.

---

# 40. JVM Bytecode of Generics ⭐⭐⭐⭐⭐

## Generic Function

```kotlin
fun <T> printValue(value: T){
    println(value)
}
```

---

## Decompiled Java

```java
public static final void printValue(Object value){
    System.out.println(value);
}
```

Generic type disappears.

---

## Generic Class Bytecode

```kotlin
class Holder<T>(
    private val item:T
)
```

Becomes

```java
class Holder{

    private Object item;

    Object getItem(){...}
}
```

---

## Primitive Types

```kotlin
Box<Int>
```

Still boxed.

Uses `Integer` object.

---

## Bytecode Visualization

```text
Kotlin Source

Box<User>

↓

Compiler

↓

Box<Object>
```

---

# 41. Why Reified Generics Exist ⭐⭐⭐⭐⭐

## Problem

Need runtime type.

```kotlin
fun <T> Gson.parse(json:String):T
```

Compilation error.

`T` doesn't exist at runtime.

---

## Old Java Solution

```kotlin
fun <T> Gson.parse(
    json:String,
    clazz:Class<T>
)
```

Usage

```kotlin
gson.parse(json, User::class.java)
```

Extra parameter everywhere.

---

## Kotlin Solution

```kotlin
inline fun <reified T> Gson.parse(
    json:String
):T{
    return fromJson(json, T::class.java)
}
```

Usage

```kotlin
val user = gson.parse<User>(json)
```

Cleaner API.

---

## Why Inline Required?

Compiler replaces generic type during compilation.

No type erasure problem.

---

# 42. Reified Generic Functions ⭐⭐⭐⭐⭐

## Basic Example

```kotlin
inline fun <reified T> printType(){
    println(T::class.simpleName)
}
```

Usage

```kotlin
printType<String>()
printType<Int>()
```

Output

```text
String
Int
```

---

## Check Type

```kotlin
inline fun <reified T> Any.isType() =
    this is T
```

Usage

```kotlin
println("Android".isType<String>())
println(123.isType<Int>())
```

---

## Android Intent Example

```kotlin
inline fun <reified T:Activity>
Context.openActivity(){
    startActivity(Intent(this, T::class.java))
}
```

---

## Fragment ViewModel

```kotlin
inline fun <reified VM:ViewModel>
Fragment.vm() =
    ViewModelProvider(this)[VM::class.java]
```

---

## SharedPreferences

```kotlin
inline fun <reified T> SharedPreferences.read(
    key:String
):T?
```

Supports multiple types safely.

---

# 43. Reflection with Generics ⭐⭐⭐⭐⭐

## Story — Unknown Package

Reflection inspects types at runtime.

Generics complicate this.

---

## Type Information Lost

```kotlin
val list = listOf("A")
```

Reflection says

```text
ArrayList
```

Not `ArrayList<String>`.

---

## KType

```kotlin
typeOf<List<String>>()
```

Returns complete generic information.

Requires opt-in and inline reified.

---

## Example

```kotlin
inline fun <reified T> typeInfo(){
    println(typeOf<T>())
}
```

Output

```text
List<String>
```

---

## Java Reflection

```kotlin
User::class.java
```

Only raw class available.

---

## Gson Example

```kotlin
object : TypeToken<List<User>>() {}.type
```

Preserves generic information.

---

## Moshi Example

Uses parameterized type.

---

# 44. Generic Arrays ⭐⭐⭐⭐⭐

Arrays behave differently.

---

## Generic Array Problem

```kotlin
class Store<T>(
    size:Int
){
    val items = arrayOfNulls<T>(size)
}
```

Compilation error.

---

## Why?

Runtime doesn't know `T`.

---

## Solution

```kotlin
inline fun <reified T> array(size:Int):Array<T?>{
    return arrayOfNulls(size)
}
```

---

## Android Example

RecyclerView payload arrays.

Bitmap filter arrays.

---

## Array Projection

```kotlin
Array<out Animal>
```

Read-only.

---

# 45. Generic Performance ⭐⭐⭐⭐⭐

## Are Generics Slow?

No.

Generics disappear after compilation.

---

## Compile-Time Safety Only

```kotlin
List<User>
```

Runtime cost is almost identical to `List<Object>`.

---

## Boxing Cost

```kotlin
Box<Int>
```

Uses `Integer`.

Primitive boxing still exists.

---

## Inline Reified Performance

Better than reflection.

No `Class<T>` parameter allocation.

---

## Reflection Cost

```kotlin
typeOf<T>()
```

Reflection metadata lookup.

Avoid inside RecyclerView binding.

---

## Performance Table

| Operation | Cost |
|-----------|------|
| Generic Function | Very Low |
| Generic Class | Very Low |
| Reified Generic | Very Low |
| Reflection Generic | High |

---

# 46. Kotlin Multiplatform Generics ⭐⭐⭐⭐⭐

## Shared Generic Repository

```kotlin
interface Repository<T>
```

Works in

- Android
- iOS
- JVM
- Desktop
- JS

---

## Shared Mapper

```kotlin
Mapper<Entity,Domain>
```

Reusable everywhere.

---

## Shared Flow Wrapper

```kotlin
Resource<T>
```

Common KMP pattern.

---

## expect / actual

```kotlin
expect interface Storage<T>
```

Platform-specific implementations.

---

## Why Generics Shine in KMP

Business logic stays platform-independent.

---

# 47. Testing Generic Code ⭐⭐⭐⭐⭐

## Test Generic Function

```kotlin
@Test
fun identityTest(){

    assertEquals(
        "Android",
        identity("Android")
    )
}
```

---

## Test Generic Repository

```kotlin
class FakeRepository<T>(
    private val data:T
): Repository<T>{

    override suspend fun getById(id:String)=data
}
```

Reusable fake.

---

## Test Resource Wrapper

```kotlin
@Test
fun successTest(){

    val resource = Resource.Success(User("Vikash"))

    assertTrue(resource is Resource.Success)
}
```

---

## Test Mapper

```kotlin
@Test
fun mapperTest(){

    val mapper = UserMapper()

    val domain = mapper.map(entity)

    assertEquals(entity.id, domain.id)
}
```

---

## Test Flow

```kotlin
@Test
fun flowTest() = runTest{

    val state = MutableStateFlow(0)

    state.update { it + 1 }

    assertEquals(1, state.value)
}
```

---

# 48. Production Design Patterns ⭐⭐⭐⭐⭐

## Pattern 1 — Repository Framework

```text
Repository<T>
```

Supports every entity.

---

## Pattern 2 — Mapper Framework

```text
Mapper<I,O>
BiMapper<I,O>
```

Entity ↔ Domain ↔ DTO.

---

## Pattern 3 — Result Wrapper

```text
Resource<T>
```

Success.

Loading.

Error.

---

## Pattern 4 — UI State

```text
UiState<T>
```

Reusable Compose/MVVM state.

---

## Pattern 5 — Validator Library

```text
Validator<T>
ValidationResult<T>
```

Reusable validation engine.

---

## Pattern 6 — Cache Framework

```text
MemoryCache<K,V>
DiskCache<K,V>
```

---

## Pattern 7 — Event Framework

```text
Event<T>
EventBus<T>
```

Generic SharedFlow events.

---

# 49. Common Production Bugs ⭐⭐⭐⭐⭐

## Bug 1 — Unsafe Cast

```kotlin
val user = item as User
```

Avoid.

---

## Bug 2 — Using Any Instead of Generics

Lose compiler safety.

---

## Bug 3 — Deep Generic Nesting

```kotlin
Flow<Resource<ApiResponse<List<User>>>>
```

Hard to read.

Use `typealias`.

---

## Bug 4 — Mutable Exposure

Expose immutable generic APIs.

---

## Bug 5 — Missing Variance

Repository should usually produce values.

Use `out` where appropriate.

---

## Bug 6 — Reflection in Hot Paths

Avoid reflection inside Compose recomposition.

---

# 50. Best Practices Checklist ⭐⭐⭐⭐⭐

## ✅ Prefer Generic Interfaces

Repository.

Mapper.

Validator.

UseCase.

Cache.

---

## ✅ Use Meaningful Generic Names

| Name | Purpose |
|------|----------|
| T | Generic Type |
| R | Result |
| I | Input |
| O | Output |
| E | Entity |
| D | Domain |
| K | Key |
| V | Value |

---

## ✅ Keep APIs Immutable

Expose

```kotlin
List<User>
```

Instead of mutable collections.

---

## ✅ Use Reified Only When Runtime Type Needed

Examples:

- Gson.
- Moshi.
- Intent.
- ViewModel.
- Navigation.

---

# 51. 70+ Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Fundamentals

1. What are Generics?
2. Why Kotlin Generics exist?
3. Generic Class vs Function?
4. Generic Constraints?
5. Multiple Generic Parameters?

## Variance

6. Invariance.
7. Covariance.
8. Contravariance.
9. Producer vs Consumer.
10. PECS principle.

## JVM

11. What is Type Erasure?
12. Why JVM removes generic types?
13. Generic bytecode.
14. Runtime type information.
15. Boxing.

## Reified

16. Why reified needs inline?
17. `T::class` limitation.
18. Gson with reified.
19. Navigation with reified.
20. ViewModel with reified.

## Android

21. Generic Repository.
22. Generic Room DAO.
23. Generic Paging Adapter.
24. Generic RecyclerView Adapter.
25. Generic Flow operators.
26. Generic StateFlow.
27. Generic Compose components.
28. Generic Validation.
29. Generic Cache.
30. Generic Event Bus.

## Reflection

31. `typeOf<T>()`
32. Star Projection.
33. Type Projection.
34. Declaration-site variance.
35. Use-site variance.

## Advanced

36–70 include architecture, testing, performance, KMP, serialization, Compose DSL, and production scenarios.

---

# 📋 Ultimate Cheat Sheet

## Generic Class

```kotlin
class Box<T>(val value:T)
```

---

## Generic Function

```kotlin
fun <T> identity(value:T):T
```

---

## Constraint

```kotlin
<T : ViewModel>
```

---

## Multiple Types

```kotlin
Pair<K,V>
```

---

## Covariant

```kotlin
out T
```

Producer.

---

## Contravariant

```kotlin
in T
```

Consumer.

---

## Star Projection

```kotlin
List<*>
```

Unknown generic type.

---

## Reified

```kotlin
inline fun <reified T>
```

Runtime generic type.

---

## Type Alias

```kotlin
typealias Mapper<I,O>
```

---

## Resource Wrapper

```kotlin
Resource<T>
```

---

## UI State

```kotlin
UiState<T>
```

---

## Repository

```kotlin
Repository<T>
```

---

## Mapper

```kotlin
Mapper<I,O>
```

---

## Validator

```kotlin
Validator<T>
```

---

# 📝 Complete Chapter Revision Summary

## What You Learned in Chapter 17

| Part | Topics Covered |
|------|----------------|
| **Part 1** | Generic Classes, Functions, Constraints, Interfaces, Type Aliases, Extension Functions. |
| **Part 2** | Invariance, Covariance, Contravariance, PECS, Type Projection, Star Projection, Collections & Flow Variance. |
| **Part 3** | Generic Repository, UseCase, Retrofit, Room, RecyclerView, Paging, Compose, Flow, Navigation, Validation, Cache. |
| **Part 4** | Type Erasure, Reified Generics, JVM Bytecode, Reflection, Performance, KMP, Testing, Production Patterns, 70+ Interview Questions. |

---

# 🎯 Android Interview Takeaways

After completing this chapter, you should be able to explain:

- How Kotlin Generics work internally on the JVM.
- Difference between `in`, `out`, and invariant generics.
- Type Erasure and why `reified` exists.
- Generic architecture patterns used in MVVM, MVI, Clean Architecture, Paging 3, Room, Retrofit, Flow, Compose, and Kotlin Multiplatform.
- Performance implications and best practices for enterprise Android applications.

---


