# 💜 Kotlin Abstract Classes — Complete Interview Guide (2026 Edition)

> Master Kotlin Abstract Classes from fundamentals to template methods, Android architecture, Compose patterns, JVM internals, and Staff Engineer interview questions.

**Module:** Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • Razorpay • CRED

---

# 📚 Table of Contents

1. What is an Abstract Class?
2. Why Abstract Classes Exist
3. Declaring Abstract Classes
4. Abstract Properties
5. Abstract Functions
6. Concrete Functions Inside Abstract Classes
7. Constructors in Abstract Classes
8. Template Method Pattern
9. Abstract Class vs Interface
10. Abstract Classes in Android Architecture
11. Compose Examples
12. JVM Internals
13. Performance Discussion
14. Best Practices
15. Common Mistakes
16. Interview Questions
17. Cheat Sheet

---

# 1. What is an Abstract Class?

## 🎭 Remember This Story — Building a Smartphone Company

Imagine you're launching a smartphone company.

Every phone has:

- Power button.
- Volume buttons.
- Battery.
- Screen.

But different models behave differently:

- Gaming Phone
- Foldable Phone
- Budget Phone

The company defines a **base phone design**.

Every phone must implement certain features, while sharing common hardware behavior.

That's exactly an **Abstract Class**.

```
        Smartphone (Abstract)

               ▲
      ┌────────┼────────┐
      │        │        │
 Gaming     Foldable   Budget
```

---

## Definition

An **abstract class** is a class that **cannot be instantiated**.

It provides:

- Shared state.
- Shared implementation.
- Abstract members that subclasses must implement.

---

## Kotlin Example

```kotlin
abstract class Animal {

    abstract fun sound()
}
```

Cannot create:

```kotlin
Animal() ❌
```

Must create a subclass.

---

# 2. Why Abstract Classes Exist?

Abstract classes solve two problems:

### Shared Behavior

Every child inherits reusable code.

### Mandatory Behavior

Every child must implement required functionality.

---

## Example

```kotlin
abstract class PaymentProcessor {

    fun startPayment() {
        println("Starting Payment")
    }

    abstract fun processPayment()
}
```

Every payment processor starts payment the same way.

Processing differs.

---

# Real Android Examples

| Abstract Class | Child Classes |
|---------------|--------------|
| `ViewModel` | `HomeViewModel` |
| `RecyclerView.Adapter` | `UserAdapter` |
| `PagingSource` | `UserPagingSource` |
| `Worker` | `SyncWorker` |
| `BroadcastReceiver` | `NetworkReceiver` |

---

# 3. Declaring Abstract Classes

Basic syntax.

```kotlin
abstract class Vehicle
```

Cannot instantiate.

---

## Child Class

```kotlin
class Car : Vehicle()
```

Works.

---

## Abstract + Open?

No need.

`abstract` automatically makes the class open.

---

# 4. Abstract Properties

Abstract classes can declare properties without implementation.

```kotlin
abstract class Employee {

    abstract val salary: Double
}
```

Child implements.

```kotlin
class AndroidDeveloper : Employee() {

    override val salary = 250000.0
}
```

---

## Abstract Mutable Property

```kotlin
abstract class User {

    abstract var name: String
}
```

Child must implement getter/setter.

---

## Computed Property

```kotlin
abstract class Circle {

    abstract val radius: Double

    val area
        get() = Math.PI * radius * radius
}
```

Shared implementation.

---

# 5. Abstract Functions

Abstract functions have no implementation.

```kotlin
abstract class Animal {

    abstract fun sound()
}
```

Child must override.

---

## Example

```kotlin
class Dog : Animal() {

    override fun sound() {
        println("Bark")
    }
}
```

---

## Multiple Abstract Functions

```kotlin
abstract class PaymentGateway {

    abstract fun validate()

    abstract fun pay()

    abstract fun refund()
}
```

Useful in architecture.

---

# 6. Concrete Functions

Abstract classes may contain regular functions.

```kotlin
abstract class Animal {

    fun sleep() {
        println("Sleeping")
    }

    abstract fun sound()
}
```

Children inherit `sleep()` automatically.

---

## Override Optional Function

```kotlin
open fun walk() {
    println("Walking")
}
```

Children may override.

---

## Final Function

```kotlin
final fun breathe() {
    println("Breathing")
}
```

Cannot override.

---

# 7. Constructors in Abstract Classes

Unlike interfaces...

Abstract classes **can have constructors**.

```kotlin
abstract class User(
    val name: String
)
```

Child passes constructor.

```kotlin
class Admin(name: String) : User(name)
```

---

## Init Block

```kotlin
abstract class Animal {

    init {
        println("Animal Created")
    }
}
```

Runs before child initialization.

---

## Constructor Validation

```kotlin
abstract class Person(
    val age: Int
){

    init {
        require(age > 0)
    }
}
```

Useful shared validation.

---

# 8. Template Method Pattern ⭐⭐⭐⭐⭐

## 🎭 Remember This Story — Making Coffee

Every coffee shop follows the same process:

1. Boil water.
2. Brew coffee.
3. Pour into cup.
4. Add toppings.

Only toppings differ.

- Cappuccino
- Latte
- Mocha

Shared algorithm + customizable step.

---

## Kotlin Implementation

```kotlin
abstract class Coffee {

    fun prepare() {
        boilWater()
        brew()
        pour()
        toppings()
    }

    private fun boilWater() {}

    private fun pour() {}

    abstract fun brew()

    abstract fun toppings()
}
```

Child customizes only required steps.

---

## Latte

```kotlin
class Latte : Coffee() {

    override fun brew() {}

    override fun toppings() {}
}
```

---

## Android Example

`BaseActivity`

```kotlin
abstract class BaseActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setupViews()
        observeState()
    }

    abstract fun setupViews()

    abstract fun observeState()
}
```

Every activity follows the same lifecycle template.

---

# 9. Abstract Class vs Interface

This is one of Google's favorite interview questions.

<table>
  <table-row>
    <table-cell width="220">**Abstract Class**</table-cell>
    <table-cell>**Interface**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Can have constructor.</table-cell>
    <table-cell>No constructor.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Can hold state.</table-cell>
    <table-cell>No backing fields.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Single inheritance.</table-cell>
    <table-cell>Multiple inheritance.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Best for shared implementation.</table-cell>
    <table-cell>Best for contracts.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Supports visibility modifiers on state.</table-cell>
    <table-cell>Only abstract/computed properties.</table-cell>
  </table-row>
</table>

---

## Which One Should You Use?

### Use Interface

- Repository contract.
- Analytics.
- Logger.
- Navigator.
- Platform abstraction.

### Use Abstract Class

- BaseActivity.
- BaseViewModel.
- BaseWorker.
- Template lifecycle.
- Shared state.

---

# 10. Shared State Example

```kotlin
abstract class BaseRepository {

    protected val logger = Logger()

    protected var requestCount = 0

    fun logRequest() {
        requestCount++
    }
}
```

Children inherit logger and state.

Impossible with interfaces.

---

# 11. Android Architecture Examples

## Base ViewModel

```kotlin
abstract class BaseViewModel : ViewModel() {

    protected val errorHandler = ErrorHandler()

    protected fun launch(block: suspend () -> Unit) {}
}
```

Children inherit coroutine launcher.

---

## Base Repository

```kotlin
abstract class NetworkRepository(
    protected val api: ApiService
) {

    suspend fun execute() {}
}
```

Shared networking logic.

---

## Base Worker

```kotlin
abstract class SyncWorker(
    context: Context,
    params: WorkerParameters
) : CoroutineWorker(context, params)
```

Common sync logic.

---

# 12. Compose Examples

## Base Screen State

```kotlin
abstract class ScreenState {

    abstract val loading: Boolean

    abstract val error: String?
}
```

---

## Child State

```kotlin
data class HomeState(
    override val loading: Boolean,
    override val error: String?,
    val users: List<User>
) : ScreenState()
```

---

## Why Compose Usually Prefers Sealed Classes?

UI states are finite.

We'll cover this in Sealed Classes.

---

# 13. Abstract Companion Pattern

Factory inside abstract class.

```kotlin
abstract class Shape {

    abstract fun draw()

    companion object {

        fun createCircle(): Shape = Circle()
    }
}
```

Factory methods.

---

# 14. JVM Internals

Kotlin

```kotlin
abstract class Animal {

    abstract fun sound()

    fun sleep() {}
}
```

Decompiler

```java
public abstract class Animal {

    public abstract void sound();

    public void sleep(){}
}
```

Abstract methods remain abstract.

Regular methods compile normally.

---

## Constructor Bytecode

Abstract class constructors execute exactly like normal classes.

Children invoke them via `super()`.

---

# 15. Memory Diagram

```kotlin
val dog = Dog()
```

```
Stack

dog
 │
 ▼

Heap

Dog Object
-------------
Animal State
Dog State
```

Object contains inherited fields.

---

# 16. Performance Discussion

### Abstract Method

Virtual dispatch.

### Final Method

Can be optimized.

### Shared State

Reduces duplicate objects.

---

## Cost

Very small runtime overhead.

Architecture benefits outweigh it.

---

# 17. Android Best Practices

### Base Classes Should Stay Small

Keep only reusable lifecycle logic.

### Don't Create "God BaseActivity"

Avoid putting unrelated utilities.

### Prefer Composition for Independent Features

Instead of giant inheritance trees.

### Use Abstract Classes for Lifecycle Templates

Example:

- Worker.
- ViewModel.
- Activity.

---

# 18. Common Mistakes

### Mistake 1

Using abstract class for simple contract.

### Mistake 2

Putting business logic into BaseActivity.

### Mistake 3

Deep inheritance hierarchy.

### Mistake 4

Using abstract class when multiple inheritance is needed.

### Mistake 5

Calling abstract/open members inside constructor.

---

# 19. Real Android Interview Questions

## Basic

1. What is an abstract class?
2. Can abstract classes have constructors?
3. Can abstract classes have state?
4. Difference between abstract and open?

## Intermediate

5. Abstract class vs interface.
6. Template Method Pattern.
7. Abstract properties.
8. Final methods inside abstract classes.

## Advanced

9. JVM bytecode for abstract classes.
10. Why `ViewModel` is an abstract/open hierarchy?
11. BaseActivity architecture discussion.
12. Shared state vs dependency injection.
13. Initialization pitfalls with abstract members.

---

# 20. 2-Minute Interview Answer

> Abstract classes provide partial implementation plus mandatory behavior. Unlike interfaces, they can hold state, constructors, and reusable implementation. Android commonly uses abstract classes for lifecycle templates like BaseActivity, BaseViewModel, and Worker. Interfaces define contracts, while abstract classes define reusable behavior and state.

---

# 21. Cheat Sheet

| Concept | Syntax |
|---------|--------|
| Abstract Class | `abstract class Animal` |
| Abstract Function | `abstract fun sound()` |
| Abstract Property | `abstract val age: Int` |
| Constructor | `abstract class User(val name: String)` |
| Concrete Function | `fun sleep(){}` |
| Template Method | `prepare()` calling abstract methods |
| Final Method | `final fun breathe(){}` |

---

# 📝 Revision Summary

- Abstract classes **cannot be instantiated**.
- They support **constructors, state, and implementation**.
- Abstract functions/properties must be implemented by children.
- Template Method Pattern is a classic abstract class use case.
- Android uses abstract classes for reusable lifecycle components.
- Choose **interface for contracts**, **abstract class for shared behavior/state**.