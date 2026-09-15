# 💜 Kotlin Interfaces — Complete Interview Guide (2026 Edition)

> Master Kotlin Interfaces from fundamentals to multiple inheritance, default implementations, JVM bytecode, Android architecture, Compose, KMP, and Staff Engineer interview questions.

**Module:** Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • Razorpay • CRED

---

# 📚 Table of Contents

1. What is an Interface?
2. Why Interfaces Exist
3. Declaring Interfaces
4. Implementing Interfaces
5. Interface Properties
6. Default Method Implementations
7. Multiple Interface Inheritance
8. Conflict Resolution (`super<Interface>`)
9. Functional Interfaces (`fun interface`)
10. Interfaces vs Abstract Classes
11. Interfaces in Android Architecture
12. Interfaces in Jetpack Compose
13. KMP & Interfaces
14. JVM Internals
15. Performance Discussion
16. Best Practices
17. Common Mistakes
18. Interview Questions
19. Cheat Sheet

---

# 1. What is an Interface?

## 🎭 Remember This Story — Food Delivery App

Imagine you're building **Swiggy**.

Every delivery partner must know how to:

- Accept an order.
- Pick up food.
- Deliver food.

It doesn't matter whether the partner is riding a bike, bicycle, or scooter.

The app only needs a **contract**.

That contract is an **Interface**.

```
DeliveryPartner (Contract)

        ▲
        │
 ┌──────┼────────┐
 │      │        │
Bike   Cycle   Scooter
```

Each partner implements the same contract differently.

---

## Definition

An **interface defines a contract**.

It tells a class:

> "If you implement me, you must provide these behaviors."

Unlike classes, interfaces primarily define behavior, not object state.

---

# 2. Why Interfaces Exist?

Without interfaces:

```kotlin
class PaymentProcessor {
    fun pay() {}
}
```

Every payment method depends on this concrete class.

Difficult to replace.

With interfaces:

```kotlin
interface PaymentGateway {
    fun pay(amount: Double)
}
```

Now implementations can vary.

- Razorpay
- Stripe
- PayPal
- Google Pay

This is **dependency inversion**.

---

# Real Android Examples

| Interface | Implementation |
|-----------|---------------|
| Repository | UserRepositoryImpl |
| Retrofit API | Generated Implementation |
| Room DAO | Generated Implementation |
| CoroutineDispatcher | Dispatchers.IO |
| Navigator | AppNavigator |
| Logger | FirebaseLogger / ConsoleLogger |

---

# 3. Declaring Interfaces

Basic syntax.

```kotlin
interface Animal {

    fun sound()
}
```

No implementation required.

---

## Implement Interface

```kotlin
class Dog : Animal {

    override fun sound() {
        println("Bark")
    }
}
```

Output

```
Bark
```

---

## Multiple Methods

```kotlin
interface Player {

    fun play()

    fun stop()

    fun pause()
}
```

Every method must be implemented unless default implementation exists.

---

# 4. Interface Properties

Interfaces can declare properties.

```kotlin
interface Animal {

    val species: String
}
```

Implementation:

```kotlin
class Dog : Animal {

    override val species = "Mammal"
}
```

---

## Computed Property

```kotlin
interface Circle {

    val radius: Double

    val area: Double
        get() = Math.PI * radius * radius
}
```

No backing field.

---

## Why No Backing Fields?

Interfaces **cannot store state**.

Properties are abstract or computed.

---

# 5. Default Method Implementation

Unlike Java before Java 8, Kotlin interfaces can provide implementation.

```kotlin
interface Animal {

    fun sleep() {
        println("Sleeping...")
    }
}
```

Implementation class inherits it automatically.

```kotlin
class Dog : Animal
```

`Dog.sleep()` works without overriding.

---

## Override Default Implementation

```kotlin
class Dog : Animal {

    override fun sleep() {
        println("Dog Sleeping")
    }
}
```

---

# 6. Interface vs Class Methods

```kotlin
interface Animal {

    fun sound()

    fun sleep() {
        println("Sleep")
    }
}
```

- `sound()` → Abstract.
- `sleep()` → Default implementation.

---

# 7. Multiple Interface Inheritance

Kotlin allows implementing multiple interfaces.

```kotlin
interface Flyable {
    fun fly()
}

interface Swimmable {
    fun swim()
}

class Duck : Flyable, Swimmable {

    override fun fly() {
        println("Flying")
    }

    override fun swim() {
        println("Swimming")
    }
}
```

This solves the diamond problem safely.

---

## Android Example

```kotlin
class HomeViewModel :
    ViewModel(),
    Navigator,
    AnalyticsLogger
```

One class implements multiple contracts.

---

# 8. Conflict Resolution

## Diamond Problem

```
        Animal
       /      \
 PetAnimal   WildAnimal
       \      /
         Dog
```

If both interfaces implement the same function...

```kotlin
interface A {

    fun greet() {
        println("A")
    }
}

interface B {

    fun greet() {
        println("B")
    }
}
```

Child must resolve ambiguity.

```kotlin
class Demo : A, B {

    override fun greet() {

        super<A>.greet()

        super<B>.greet()

        println("Demo")
    }
}
```

Output

```
A
B
Demo
```

---

## Why Explicit `super<Interface>`?

Removes ambiguity at compile time.

Interview favorite.

---

# 9. Functional Interfaces (`fun interface`)

Kotlin supports SAM (Single Abstract Method).

```kotlin
fun interface ClickListener {

    fun onClick()
}
```

Usage:

```kotlin
val listener = ClickListener {
    println("Clicked")
}
```

Cleaner lambda syntax.

---

## Android Example

```kotlin
button.setOnClickListener {
    println("Clicked")
}
```

SAM conversion.

---

## Why `fun interface`?

Ideal for callbacks.

Examples:

- Click Listener
- Retry Listener
- Permission Callback
- Analytics Callback

---

# 10. Interface Inheritance

Interfaces can inherit interfaces.

```kotlin
interface Animal {

    fun eat()
}

interface Pet : Animal {

    fun owner()
}
```

Implementation:

```kotlin
class Dog : Pet {

    override fun eat() {}

    override fun owner() {}
}
```

---

# 11. Interfaces vs Abstract Classes

| Interface | Abstract Class |
|-----------|----------------|
| Multiple inheritance | Single inheritance |
| No constructor | Has constructor |
| No state | Can store state |
| Contract first | Shared implementation |
| Best for architecture | Best for base behavior |

---

## Which Should You Use?

**Use Interface when:**

- Multiple implementations exist.
- Dependency Injection.
- Repository contracts.
- Callbacks.
- Platform abstraction.

**Use Abstract Class when:**

- Shared state.
- Shared implementation.
- Lifecycle template.
- Base Activity.

---

# 12. Android Repository Pattern

## 🎭 Story — Food Ordering System

App only knows "OrderRepository".

It doesn't know whether data comes from:

- Network
- Cache
- Database

```
OrderRepository

     ▲
     │
 ┌───┴────────────┐
 │                │
RemoteRepo    LocalRepo
```

---

## Interface

```kotlin
interface UserRepository {

    suspend fun users(): List<User>
}
```

Implementation

```kotlin
class UserRepositoryImpl(
    private val api: ApiService
) : UserRepository {

    override suspend fun users() =
        api.users()
}
```

ViewModel depends only on interface.

---

## Benefits

- Easy testing.
- Easy mocking.
- Replace implementation.

---

# 13. Retrofit Uses Interfaces

```kotlin
interface ApiService {

    @GET("users")
    suspend fun users(): List<User>
}
```

You never implement it.

Retrofit generates implementation at runtime.

---

## Why Interface?

Dynamic proxy generation.

Huge interview topic.

---

# 14. Room DAO Uses Interfaces / Abstract Classes

```kotlin
@Dao
interface UserDao {

    @Query("SELECT * FROM user")
    suspend fun users(): List<User>
}
```

Room generates implementation.

---

# 15. Jetpack Compose Example

## Navigator Interface

```kotlin
interface Navigator {

    fun navigate(route: String)

    fun back()
}
```

Implementation.

```kotlin
class ComposeNavigator(
    private val navController: NavController
) : Navigator {

    override fun navigate(route: String) {
        navController.navigate(route)
    }

    override fun back() {
        navController.popBackStack()
    }
}
```

UI depends on interface.

---

## Analytics Interface

```kotlin
interface Analytics {

    fun log(event: String)
}
```

Implementation.

- Firebase.
- Mixpanel.
- Console.

Swap anytime.

---

# 16. KMP (Kotlin Multiplatform)

Interfaces abstract platform-specific code.

```kotlin
interface PlatformLogger {

    fun log(message: String)
}
```

Android:

```kotlin
class AndroidLogger : PlatformLogger
```

iOS:

```kotlin
class IOSLogger : PlatformLogger
```

Shared code depends only on interface.

---

# 17. JVM Internals

Kotlin

```kotlin
interface Animal {

    fun sound()

    fun sleep() {
        println("Sleep")
    }
}
```

Decompiler (simplified)

```java
public interface Animal {

    void sound();

    default void sleep() {
        System.out.println("Sleep");
    }
}
```

Default methods become JVM default methods (or compatibility helpers depending on target).

---

## Interface Dispatch

```
Animal Reference

↓

Dog Object

↓

Dog.sound()
```

Uses dynamic dispatch.

---

# 18. Interface Properties Internals

```kotlin
interface User {

    val age: Int
}
```

Decompiler

```java
int getAge();
```

Only getter.

No field.

---

## Computed Property

```kotlin
val area
    get() = radius * radius
```

Compiler generates only getter.

---

# 19. Performance Discussion

### Interface Calls

Virtual dispatch.

Very small overhead.

Negligible in Android apps.

---

### Functional Interface

No anonymous object allocation in many lambda cases.

Compiler performs SAM optimization.

---

### Default Methods

Reduce duplicated implementation.

---

# 20. Android Best Practices

### Repository Depends on Interface

```kotlin
class HomeViewModel(
    private val repository: UserRepository
)
```

---

### Keep Interfaces Small

Bad:

```kotlin
interface MegaRepository {
    // 40 methods
}
```

Good:

- AuthRepository
- UserRepository
- PaymentRepository

Interface Segregation Principle.

---

### Use `fun interface` for Callbacks

Cleaner APIs.

---

### Use Interface for Platform Abstraction

Especially in KMP.

---

# 21. Common Mistakes

### Mistake 1

Using abstract class when interface is enough.

### Mistake 2

Large "God Interfaces".

### Mistake 3

Trying to store state inside interface.

### Mistake 4

Forgetting conflict resolution.

### Mistake 5

Using inheritance instead of interface composition.

---

# 22. Real Android Interview Questions

## Basic

1. What is an interface?
2. Interface vs class?
3. Can interfaces have implementation?
4. Can interfaces have properties?
5. Can interfaces have constructors?

---

## Intermediate

6. Interface vs abstract class.
7. Multiple inheritance in Kotlin.
8. What is `super<A>` syntax?
9. Why interfaces have no backing fields?
10. `fun interface` vs interface.

---

## Advanced

11. How Retrofit implements interfaces?
12. JVM bytecode for interface default methods.
13. Interface dispatch vs class dispatch.
14. Why repository pattern uses interfaces?
15. Interface segregation principle in Android.

---

# 23. 2-Minute Interview Answer

> Kotlin interfaces define contracts that multiple classes can implement. Unlike abstract classes, interfaces support multiple inheritance and can contain default method implementations but cannot hold state through backing fields. Modern Android architecture uses interfaces extensively for repositories, navigators, analytics, and platform abstraction because they improve testability, dependency injection, and maintainability.

---

# 24. Cheat Sheet

| Concept | Syntax |
|---------|--------|
| Interface | `interface Animal` |
| Implement | `class Dog : Animal` |
| Default Method | `fun sleep() {}` |
| Property | `val species: String` |
| Functional Interface | `fun interface ClickListener` |
| Multiple Interfaces | `class Duck : Flyable, Swimmable` |
| Conflict Resolution | `super<A>.greet()` |

---

# 📝 Revision Summary

- Interfaces define **contracts**, not object state.
- Kotlin supports **multiple interface inheritance**.
- Interfaces can contain **default implementations**.
- Interface properties do not have backing fields.
- `fun interface` enables SAM conversion for lambdas.
- Android architecture relies heavily on interfaces for repositories, Retrofit APIs, Room DAOs, navigation, analytics, and KMP platform abstraction.