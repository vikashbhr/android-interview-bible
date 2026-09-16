# 🏗️ Design Patterns in Kotlin & Android — Android Interview Bible (2026 Edition)


## 📌 Module Information

| Property | Value |
|----------|-------|
| **Module** | Kotlin OOP & Software Design |
| **File** | `19-Design-Patterns.md` |
| **Folder** | `01-Kotlin/02-OOP/` |
| **Difficulty** | Intermediate → Staff Android Engineer |
| **Interview Frequency** | ⭐⭐⭐⭐⭐ Extremely High |
| **Companies** | Google, Uber, Amazon, Microsoft, PhonePe, Flipkart, CRED |

---

# 📚 Complete Chapter Roadmap

## Part 1 — Creational Patterns (This Part)

1. What are Design Patterns?
2. SOLID + Design Patterns Relationship.
3. Classification of Design Patterns.
4. Singleton Pattern.
5. Factory Pattern.
6. Abstract Factory Pattern.
7. Builder Pattern.
8. Prototype Pattern.
9. Object Pool Pattern.
10. Dependency Injection as Creational Pattern.
11. Android Examples.
12. Best Practices.
13. Interview Questions.
14. Cheat Sheet.

## Part 2 — Structural Patterns

15. Adapter Pattern.
16. Decorator Pattern.
17. Facade Pattern.
18. Bridge Pattern.
19. Composite Pattern.
20. Proxy Pattern.
21. Flyweight Pattern.
22. Kotlin Delegation Pattern.
23. Android Examples.

## Part 3 — Behavioral Patterns

24. Strategy Pattern.
25. Observer Pattern.
26. Command Pattern.
27. State Pattern.
28. Mediator Pattern.
29. Chain of Responsibility.
30. Iterator Pattern.
31. Visitor Pattern.
32. Template Method Pattern.
33. Memento Pattern.
34. Android Examples.

## Part 4 — Android Production Patterns

35. MVVM.
36. Repository Pattern.
37. UseCase Pattern.
38. Clean Architecture Pattern.
39. Paging Pattern.
40. UDF/MVI Pattern.
41. Compose Patterns.
42. WorkManager Pattern.
43. Coroutine Patterns.
44. Performance.
45. Testing.
46. Refactoring Legacy Code.
47. 100+ Interview Questions.
48. Ultimate Cheat Sheet.

---

# 🎯 Learning Goals

After completing this chapter you'll understand:

- All 23 GoF design patterns.
- Kotlin implementation of every important pattern.
- Android production usage.
- Hilt, Retrofit, Room, Compose, Coroutines architecture patterns.
- Senior Android interview scenarios.

---

> **Part 1: Creational Design Patterns**
>
> Design Patterns are reusable solutions to common software design problems. Every mature Android application—from Google Maps to PhonePe to Swiggy—uses dozens of design patterns.

---

# 1. What are Design Patterns? ⭐⭐⭐⭐⭐

## 🎭 Real World Story — LEGO Building Instructions

Imagine LEGO.

You can build:

- House
- Car
- Airport
- Castle

LEGO doesn't invent new building techniques every time.

It uses **reusable construction patterns**.

Software Design Patterns are exactly that.

Reusable solutions to recurring design problems.

---

## Definition

A Design Pattern is a proven reusable solution for a common software design problem.

It is **not code**.

It is **a design blueprint**.

---

## Why Developers Use Design Patterns

| Problem | Pattern Helps |
|----------|---------------|
| Object creation | Factory |
| One shared instance | Singleton |
| Dynamic behavior | Strategy |
| Event communication | Observer |
| Wrapping functionality | Decorator |
| Complex object creation | Builder |

---

## Android Examples

| Android Component | Pattern |
|-------------------|---------|
| Hilt | Dependency Injection |
| Retrofit Builder | Builder |
| Room Database | Singleton |
| LiveData | Observer |
| Compose Modifier | Decorator |
| RecyclerView Adapter | Adapter |
| Navigation | Command + State |

---

# 2. SOLID + Design Patterns Relationship ⭐⭐⭐⭐⭐

Design patterns help implement SOLID principles.

| SOLID Principle | Common Pattern |
|-----------------|---------------|
| SRP | Strategy, Facade |
| OCP | Strategy, Decorator |
| LSP | Composition |
| ISP | Adapter |
| DIP | Dependency Injection |

---

## Example

Instead of

```kotlin
class PaymentManager {
    fun pay(type: String){}
}
```

Use Strategy Pattern.

Open for extension.

Closed for modification.

---

# 3. Classification of Design Patterns ⭐⭐⭐⭐⭐

GoF (Gang of Four) divides patterns into three groups.

## 1️⃣ Creational Patterns

Responsible for object creation.

| Pattern | Purpose |
|----------|----------|
| Singleton | Single instance |
| Factory | Create objects |
| Abstract Factory | Create related families |
| Builder | Build complex object |
| Prototype | Clone object |
| Object Pool | Reuse expensive objects |

---

## 2️⃣ Structural Patterns

Responsible for object relationships.

- Adapter
- Decorator
- Composite
- Bridge
- Proxy
- Flyweight
- Facade

---

## 3️⃣ Behavioral Patterns

Responsible for communication.

- Strategy
- Observer
- Command
- State
- Mediator
- Chain of Responsibility
- Visitor
- Iterator
- Template Method
- Memento

---

# 4. Singleton Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Firebase Database Connection

Entire app should use **one database connection**.

Never create 20 Firebase instances.

---

## Definition

Ensures only one instance exists throughout application lifecycle.

---

## Kotlin Singleton

```kotlin
object AnalyticsManager {

    fun track(event: String) {
        println(event)
    }
}
```

Usage

```kotlin
AnalyticsManager.track("Login")
```

---

## Why `object` Is Singleton?

Compiler creates one global instance.

---

## JVM View

```text
AnalyticsManager.INSTANCE
```

Static singleton instance.

---

## Thread Safe Singleton

```kotlin
object NetworkManager
```

Initialized lazily.

Thread-safe.

---

## Android Example — Room Database

```kotlin
Room.databaseBuilder(...)
```

Only one database instance.

---

## Companion Singleton

```kotlin
class Logger private constructor(){

    companion object {

        val instance = Logger()
    }
}
```

---

## Problems

- Global mutable state.
- Difficult testing.
- Hidden dependencies.

Use carefully.

---

# 5. Factory Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Food Ordering

Customer orders Pizza.

Kitchen decides which pizza to create.

Customer doesn't know implementation.

---

## Definition

Factory creates objects without exposing creation logic.

---

## Without Factory

```kotlin
val payment = UpiPayment()
```

---

## Factory Version

```kotlin
interface Payment

class UpiPayment : Payment

class CardPayment : Payment

object PaymentFactory {

    fun create(type: String): Payment {

        return when(type){

            "UPI" -> UpiPayment()

            "CARD" -> CardPayment()

            else -> error("Unknown")
        }
    }
}
```

---

## Usage

```kotlin
val payment =
    PaymentFactory.create("UPI")
```

---

## Android Example

Notification channel factory.

ViewModel factory.

Worker factory.

Fragment factory.

---

## ViewModel Factory

```kotlin
class LoginViewModelFactory(...)
```

Common Android interview topic.

---

# 6. Abstract Factory Pattern ⭐⭐⭐⭐⭐

## Story — Cross Platform UI

Need Android widgets.

Need iOS widgets.

Factory creates platform family.

---

## Abstract Factory

```kotlin
interface UiFactory {

    fun createButton(): Button

    fun createTextField(): TextField
}
```

---

## Android Factory

```kotlin
class AndroidFactory : UiFactory
```

---

## IOS Factory

```kotlin
class IOSFactory : UiFactory
```

---

## Client

```kotlin
val factory: UiFactory =
    AndroidFactory()

factory.createButton()
```

---

## Android Example

Theme factories.

Dark theme.

Light theme.

Tablet UI.

Phone UI.

---

# 7. Builder Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Pizza Builder

Pizza has many optional ingredients.

---

## Problem

Huge constructor.

```kotlin
Pizza(...)
```

20 parameters.

---

## Builder

```kotlin
class PizzaBuilder {

    private var cheese = false

    private var corn = false

    fun cheese() = apply {
        cheese = true
    }

    fun corn() = apply {
        corn = true
    }

    fun build() = Pizza(...)
}
```

---

## Usage

```kotlin
PizzaBuilder()

    .cheese()

    .corn()

    .build()
```

Readable API.

---

## Android Example — Retrofit Builder

```kotlin
Retrofit.Builder()

.addConverterFactory(...)

.client(...)

.baseUrl(...)

.build()
```

---

## Notification Builder

```kotlin
NotificationCompat.Builder(...)
```

Builder Pattern.

---

## Compose Modifier Builder

```kotlin
Modifier.padding().background()
```

Fluent Builder style.

---

# 8. Prototype Pattern ⭐⭐⭐⭐⭐

## Story — WhatsApp Forward Message

Copy existing message.

Modify copied message.

---

## Definition

Clone existing object instead of creating new one.

---

## Kotlin Data Class

```kotlin
data class User(
    val name:String,
    val age:Int
)
```

Clone

```kotlin
val user2 = user.copy(age = 30)
```

---

## Deep Copy Example

Nested object cloning.

---

## Android Example

UiState copy.

```kotlin
state.copy(isLoading = true)
```

Compose uses this extensively.

---

# 9. Object Pool Pattern ⭐⭐⭐⭐⭐

## Story — Database Connections

Creating database connection is expensive.

Reuse existing connections.

---

## Object Pool

```kotlin
class ConnectionPool {

    private val pool = mutableListOf<Connection>()
}
```

Borrow.

Return.

Reuse.

---

## Android Example

Bitmap Pool.

Glide Bitmap Pool.

RecyclerView ViewHolder recycling.

Thread Pool.

Coroutine Dispatcher Pool.

---

## RecyclerView Recycling

ViewHolder pool avoids repeated object creation.

Huge performance optimization.

---

# 10. Dependency Injection as Creational Pattern ⭐⭐⭐⭐⭐

## Story — Restaurant Ingredients

Chef receives ingredients.

Doesn't create them.

---

## Constructor Injection

```kotlin
class LoginViewModel(
    private val repository: LoginRepository
)
```

---

## Hilt Provides Objects

```kotlin
@Provides
fun provideRepository(...)
```

---

## Object Graph

```text
Hilt

↓

Repository

↓

ViewModel
```

Creation responsibility moved to DI container.

---

## Why It Fits Creational Category

DI controls object creation lifecycle.

---

# 11. Android Production Examples ⭐⭐⭐⭐⭐

| Android Library | Pattern |
|-----------------|---------|
| RoomDatabase | Singleton |
| Retrofit.Builder | Builder |
| OkHttpClient.Builder | Builder |
| Hilt | Dependency Injection |
| WorkManager Factory | Factory |
| FragmentFactory | Factory |
| ViewModelProvider.Factory | Factory |
| Glide BitmapPool | Object Pool |
| Data Class copy() | Prototype |

---

# 12. Best Practices ⭐⭐⭐⭐⭐

## Singleton

- Stateless preferred.
- Avoid mutable globals.

---

## Factory

- Hide creation logic.
- Return interfaces.

---

## Builder

- Use for optional parameters.
- Immutable final object.

---

## Prototype

- Prefer immutable data classes.
- Use copy().

---

## DI

- Constructor injection first.
- Avoid Service Locator.

---

# 13. Common Pitfalls ⭐⭐⭐⭐

## Singleton Abuse

Global state everywhere.

---

## Factory Returning Concrete Classes

Prefer interfaces.

---

## Giant Builder

Split nested builders.

---

## Copy Not Deep Copy

Understand shallow copy limitations.

---

## Manual Dependency Creation

Use Hilt/Koin.

---

# 14. Senior Android Interview Questions ⭐⭐⭐⭐⭐

1. Singleton implementation in Kotlin?
2. Why object declaration is thread-safe?
3. Factory vs Builder?
4. Factory vs Abstract Factory?
5. Prototype vs copy()?
6. Builder in Retrofit?
7. Object Pool in RecyclerView?
8. Hilt as creational pattern?
9. Room singleton implementation?
10. ViewModelProvider.Factory usage?

---

# 📋 Cheat Sheet (Part 1)

## Singleton

```kotlin
object AnalyticsManager
```

---

## Factory

```kotlin
PaymentFactory.create("UPI")
```

---

## Abstract Factory

```kotlin
UiFactory
```

Creates related objects.

---

## Builder

```kotlin
Retrofit.Builder()

NotificationCompat.Builder()
```

---

## Prototype

```kotlin
user.copy(age = 30)
```

---

## Object Pool

```text
RecyclerView Pool

Bitmap Pool

Connection Pool
```

---

## Dependency Injection

```text
Hilt

↓

Repository

↓

ViewModel
```

---

# 📝 Revision Summary

In **Part 1** you learned:

- What Design Patterns are.
- Pattern classification.
- Singleton Pattern.
- Factory Pattern.
- Abstract Factory Pattern.
- Builder Pattern.
- Prototype Pattern.
- Object Pool Pattern.
- Dependency Injection as a creational pattern.
- Android production examples and interview questions.

---

# Part 2 — Structural Design Patterns (Android Architecture Edition)

> Structural Design Patterns explain **how classes and objects are combined to build larger systems**. Android frameworks like **RecyclerView, Retrofit, OkHttp, Glide, Coil, Jetpack Compose, Navigation, ViewBinding, Room, and WorkManager** use these patterns extensively.

---

# 📚 Table of Contents

15. Introduction to Structural Patterns
16. Adapter Pattern
17. Decorator Pattern
18. Facade Pattern
19. Bridge Pattern
20. Composite Pattern
21. Proxy Pattern
22. Flyweight Pattern
23. Kotlin Delegation Pattern
24. Android Production Examples
25. Best Practices
26. Common Pitfalls
27. Senior Android Interview Questions
28. Cheat Sheet

---

# 🎯 Learning Goals

After completing this part you'll understand:

- Every Structural GoF pattern.
- Kotlin implementations.
- Android production use cases.
- Jetpack Compose internals.
- RecyclerView architecture.
- Glide, Coil and OkHttp design.

---

# 15. Introduction to Structural Design Patterns ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Building an Apartment

Imagine building an apartment.

You already have rooms, doors, windows and furniture.

Now the challenge is **connecting** these objects together.

Structural patterns answer:

> **How do objects collaborate?**

---

## Structural Pattern Overview

| Pattern | Purpose | Android Example |
|----------|----------|----------------|
| Adapter | Convert one interface into another. | RecyclerView Adapter |
| Decorator | Add behavior dynamically. | Compose Modifier |
| Facade | Simplify a complex subsystem. | Repository |
| Bridge | Separate abstraction from implementation. | ImageLoader + Decoder |
| Composite | Tree structure. | Compose UI Tree |
| Proxy | Placeholder / access control. | Retrofit Proxy |
| Flyweight | Share objects. | RecyclerView ViewHolder Pool |
| Delegation | Forward work to another object. | Kotlin `by` |

---

## Android Libraries Using Structural Patterns

```text
Jetpack Compose
│
├── Decorator
├── Composite
└── Bridge

Retrofit
│
├── Proxy
├── Builder
└── Adapter

RecyclerView
│
├── Adapter
├── Flyweight
└── Composite
```

---

# 16. Adapter Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Universal Mobile Charger

Your charger has USB-C.

Laptop has USB-A.

Need an adapter.

Adapter converts one interface into another.

---

## Definition

Adapter converts one incompatible interface into another compatible interface.

---

## Example

### Existing Interface

```kotlin
interface UsbC {
    fun charge()
}
```

### Old Charger

```kotlin
class UsbACharger {
    fun power() {
        println("Charging using USB-A")
    }
}
```

### Adapter

```kotlin
class ChargerAdapter(
    private val charger: UsbACharger
) : UsbC {

    override fun charge() {
        charger.power()
    }
}
```

---

## Usage

```kotlin
val charger: UsbC =
    ChargerAdapter(UsbACharger())

charger.charge()
```

Output

```text
Charging using USB-A
```

---

## Android Example — RecyclerView Adapter ⭐⭐⭐⭐⭐

RecyclerView understands ViewHolders.

Your app has Products.

Adapter converts Product → ViewHolder.

```text
Product List

↓

RecyclerView Adapter

↓

ViewHolder

↓

UI
```

---

## RecyclerView Adapter

```kotlin
class ProductAdapter :
    RecyclerView.Adapter<ProductViewHolder>()
```

Classic Adapter Pattern.

---

## Another Android Example — Paging Adapter

```kotlin
PagingDataAdapter<Product, ProductViewHolder>
```

Adapts paging data to RecyclerView.

---

## Compose Example

Compose Adapter

```kotlin
LazyColumn {
    items(products){
        ProductCard(it)
    }
}
```

`items()` adapts List → UI tree.

---

## Real Companies

- RecyclerView
- ViewPager2
- Paging 3
- Spinner Adapter
- ListAdapter

---

# 17. Decorator Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Coffee Shop Extras

Start with Coffee.

Add Milk.

Add Chocolate.

Add Caramel.

Each addition wraps previous coffee.

---

## Definition

Decorator adds new behavior without changing original object.

---

## Base Interface

```kotlin
interface Coffee {
    fun cost(): Int
}
```

---

## Simple Coffee

```kotlin
class BasicCoffee : Coffee {

    override fun cost() = 100
}
```

---

## Decorator

```kotlin
class MilkDecorator(
    private val coffee: Coffee
) : Coffee {

    override fun cost() =
        coffee.cost() + 20
}
```

---

## Multiple Decorations

```kotlin
val coffee =
    ChocolateDecorator(
        MilkDecorator(
            BasicCoffee()
        )
    )
```

Output

```text
150
```

---

## Android Example — Modifier ⭐⭐⭐⭐⭐

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Blue)
    .border(1.dp, Color.Red)
    .clickable { }
```

Each modifier decorates previous modifier.

---

## Visualization

```text
Modifier

↓

Padding

↓

Background

↓

Border

↓

Clickable
```

---

## Android Example — OkHttp Interceptors ⭐⭐⭐⭐⭐

```text
Request

↓

Logging

↓

Auth

↓

Cache

↓

Retry

↓

Server
```

Every interceptor decorates request.

---

## Coil / Glide

Decorators add:

- Blur.
- Rounded Corners.
- Circle Crop.
- Placeholder.
- Crossfade.

---

# 18. Facade Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — ATM Machine

ATM hides:

- Bank Server.
- Card Validation.
- Balance Check.
- Cash Dispenser.
- Printer.

User presses one button.

---

## Definition

Facade provides a simplified interface to a complex subsystem.

---

## Complex Subsystem

```kotlin
class BankApi
class CashDispenser
class ReceiptPrinter
class CardValidator
```

---

## Facade

```kotlin
class ATMFacade(
    private val bankApi: BankApi,
    private val validator: CardValidator,
    private val printer: ReceiptPrinter
) {

    fun withdraw(amount: Int) {
        println("Withdraw ₹$amount")
    }
}
```

---

## Android Example — Repository ⭐⭐⭐⭐⭐

Repository hides

- API.
- Room.
- Cache.
- Preferences.

```text
UI

↓

Repository

↓

API + DB + Cache
```

UI interacts with one facade.

---

## Android Example — WorkManager

WorkManager hides:

- AlarmManager.
- JobScheduler.
- Foreground Service.

Simple API.

---

## Android Example — Media3 Player

Media controller hides ExoPlayer internals.

---

# 19. Bridge Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — TV Remote

Remote should work with Samsung, Sony, LG.

Don't create every combination.

---

## Abstraction

```kotlin
interface Tv {
    fun on()
}
```

---

## Implementations

```kotlin
class SamsungTv : Tv

class SonyTv : Tv
```

---

## Bridge

```kotlin
class Remote(
    private val tv: Tv
) {

    fun power() {
        tv.on()
    }
}
```

---

## Usage

```kotlin
Remote(SamsungTv())

Remote(SonyTv())
```

---

## Android Example — Image Loader

```text
ImageLoader

↓

Glide

Coil

Picasso
```

Bridge between abstraction and implementation.

---

## Compose Example

Material Theme bridges typography, color and shapes.

---

## Android Graphics

Canvas bridges drawing APIs with implementations.

---

# 20. Composite Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Company Organization

Company contains departments.

Department contains employees.

Tree structure.

---

## Definition

Treat individual objects and groups uniformly.

---

## Tree Visualization

```text
Company

├── HR

│   ├── Alice

│   └── Bob

├── Engineering

│   ├── Android

│   ├── Backend

│   └── QA
```

---

## Kotlin Example

```kotlin
interface Component {
    fun show()
}
```

---

## Leaf

```kotlin
class Employee(
    private val name: String
) : Component {

    override fun show() {
        println(name)
    }
}
```

---

## Composite

```kotlin
class Department : Component {

    private val children = mutableListOf<Component>()

    fun add(component: Component) {
        children.add(component)
    }

    override fun show() {
        children.forEach { it.show() }
    }
}
```

---

## Android Example — Compose UI Tree ⭐⭐⭐⭐⭐

```kotlin
Column {

    Row {

        Icon()

        Text()
    }

    Button()
}
```

Everything forms a tree.

---

## Visualization

```text
Column

├── Row

│   ├── Icon

│   └── Text

└── Button
```

---

## Android View Tree

XML hierarchy is Composite Pattern.

---

## Navigation Graph

Nested graphs compose destinations.

---

# 21. Proxy Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Security Guard

Visitor cannot enter building directly.

Security guard controls access.

---

## Definition

Proxy controls access to another object.

---

## Subject

```kotlin
interface Api {
    fun getUser()
}
```

---

## Real Object

```kotlin
class UserApi : Api
```

---

## Proxy

```kotlin
class AuthProxy(
    private val api: UserApi
) : Api {

    override fun getUser() {
        println("Checking Token")
        api.getUser()
    }
}
```

---

## Android Example — Retrofit ⭐⭐⭐⭐⭐

Retrofit creates proxy implementations.

```kotlin
interface UserApi {

    @GET("user")
    suspend fun profile(): User
}
```

No implementation exists.

Retrofit generates proxy at runtime.

---

## Android Example — Room DAO

Room generates DAO implementation.

Proxy object executes SQL.

---

## Lazy Proxy Example

Load image only when needed.

Used by Glide.

---

# 22. Flyweight Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Chess Game

32 chess pieces.

Only few unique piece types.

Share common objects.

---

## Definition

Reuse shared immutable objects to reduce memory.

---

## Flyweight Example

```kotlin
class TreeType(
    val color: String
)
```

Thousands of trees share same TreeType.

---

## Android Example — RecyclerView ViewHolder ⭐⭐⭐⭐⭐

RecyclerView doesn't create ViewHolder repeatedly.

Uses pool.

---

## ViewHolder Pool

```text
RecyclerView

↓

RecycledViewPool

↓

ViewHolder Reuse
```

Huge memory optimization.

---

## Bitmap Pool

Glide reuses Bitmaps.

---

## Compose Remember

`remember {}` shares object across recompositions.

Flyweight-like optimization.

---

# 23. Kotlin Delegation Pattern ⭐⭐⭐⭐⭐

## Delegation Review

```kotlin
interface Logger

class FirebaseLogger : Logger
```

---

## Delegation

```kotlin
class Analytics(
    logger: Logger
) : Logger by logger
```

Automatic forwarding.

---

## Android Example

Analytics providers.

Notification providers.

Storage providers.

---

## Compose State Delegation

```kotlin
var text by remember {
    mutableStateOf("")
}
```

Property delegation built into Compose.

---

## SharedPreferences Delegation

Libraries expose delegated properties.

---

# 24. Android Production Examples ⭐⭐⭐⭐⭐

| Android Feature | Pattern |
|-----------------|---------|
| RecyclerView | Adapter |
| Compose Modifier | Decorator |
| Repository | Facade |
| Retrofit Interface | Proxy |
| Room DAO | Proxy |
| RecyclerView Pool | Flyweight |
| Compose Tree | Composite |
| Glide / Coil | Bridge + Decorator |
| Navigation Graph | Composite |
| Kotlin `by` | Delegation |

---

## Architecture Visualization

```text
UI Tree

Composite

↓

Modifier

Decorator

↓

Repository

Facade

↓

Retrofit

Proxy

↓

RecyclerView

Adapter
```

---

# 25. Best Practices ⭐⭐⭐⭐⭐

## Adapter

- Convert interfaces.
- Avoid business logic.

---

## Decorator

- Add behavior dynamically.
- Keep decorators independent.

---

## Facade

- Hide subsystem complexity.
- Keep facade lightweight.

---

## Proxy

- Authentication.
- Lazy loading.
- Logging.
- Caching.

---

## Composite

- Tree-like UI.
- Nested navigation.
- Menu structures.

---

## Flyweight

- Share immutable objects.
- Avoid unnecessary allocations.

---

# 26. Common Pitfalls ⭐⭐⭐⭐

## Adapter Doing Business Logic

Keep adapter focused on conversion.

---

## God Facade

Repository shouldn't become a God object.

---

## Too Many Decorators

Long Modifier chains may affect readability.

---

## Proxy With Heavy Logic

Proxy should forward or control access.

---

## Composite Recursion Bugs

Avoid circular parent-child references.

---

# 27. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Adapter

1. RecyclerView Adapter pattern?
2. ListAdapter vs Adapter?
3. PagingDataAdapter?

## Decorator

4. Compose Modifier pattern?
5. OkHttp Interceptor?
6. Glide Transformations?

## Facade

7. Repository as Facade?
8. WorkManager Facade?

## Bridge

9. ImageLoader architecture?
10. MaterialTheme bridge?

## Composite

11. Compose UI tree?
12. Navigation graph?

## Proxy

13. Retrofit dynamic proxy?
14. Room DAO proxy?

## Flyweight

15. RecyclerView Pool?
16. Bitmap Pool?
17. remember() optimization?

---

# 📋 Cheat Sheet (Part 2)

## Adapter

```text
Model → Adapter → UI
```

---

## Decorator

```kotlin
Modifier.padding().background().clickable()
```

---

## Facade

```text
UI → Repository → API + DB
```

---

## Bridge

```text
Remote → TV Implementation
```

---

## Composite

```text
Column

├── Row

└── Button
```

---

## Proxy

```text
Client → Proxy → Real Object
```

---

## Flyweight

```text
RecyclerView Pool

Bitmap Pool

remember {}
```

---

## Delegation

```kotlin
class Manager(
    logger: Logger
) : Logger by logger
```

---

# 📝 Revision Summary

In **Part 2** you learned:

- Adapter Pattern.
- Decorator Pattern.
- Facade Pattern.
- Bridge Pattern.
- Composite Pattern.
- Proxy Pattern.
- Flyweight Pattern.
- Kotlin Delegation Pattern.
- Android examples from RecyclerView, Compose, Retrofit, Room, Glide, Coil and Navigation.

---
# Part 3 — Behavioral Design Patterns (Android Architecture Edition)

> Learn behavioral patterns with **real Android production examples** from Jetpack Compose, LiveData, StateFlow, Coroutines, Navigation, WorkManager, Room, Paging 3, and Clean Architecture.

---

# 📚 Table of Contents

24. Introduction to Behavioral Patterns
25. Strategy Pattern
26. Observer Pattern
27. Command Pattern
28. State Pattern
29. Mediator Pattern
30. Chain of Responsibility
31. Iterator Pattern
32. Visitor Pattern
33. Template Method Pattern
34. Memento Pattern
35. Android Production Examples
36. Best Practices
37. Common Pitfalls
38. Senior Android Interview Questions
39. Cheat Sheet

---

# 🎯 Learning Goals

After completing this part you'll understand:

- Communication between objects.
- Runtime behavior switching.
- State management.
- Event handling.
- Navigation architecture.
- Coroutines communication patterns.
- LiveData, Flow and Compose internals.

---

# 24. Introduction to Behavioral Patterns ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Swiggy Food Delivery Journey

When you place an order, many independent systems communicate.

```text
Customer

↓

Order Service

↓

Restaurant

↓

Delivery Partner

↓

Notification Service

↓

Payment Service
```

Nobody knows everything.

Everyone communicates through defined behaviors.

Behavioral patterns solve this problem.

---

## Behavioral Pattern Overview

| Pattern | Purpose | Android Example |
|---------|----------|----------------|
| Strategy | Switch behavior at runtime | Payment methods |
| Observer | Event subscription | LiveData / StateFlow |
| Command | Encapsulate request | Navigation events |
| State | Object changes behavior by state | Login UI State |
| Mediator | Central communication hub | ViewModel |
| Chain of Responsibility | Request pipeline | OkHttp Interceptors |
| Iterator | Sequential traversal | LazyColumn items |
| Visitor | Separate operations from object | AST / Compose Compiler |
| Template Method | Fixed algorithm skeleton | Worker execution |
| Memento | Save & restore state | SavedStateHandle |

---

# 25. Strategy Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Google Maps Navigation

Google Maps supports:

- Car
- Bike
- Walking
- Metro

Same destination.

Different strategy.

---

## Definition

Encapsulates interchangeable algorithms.

---

## Strategy Interface

```kotlin
interface NavigationStrategy {
    fun navigate(destination: String)
}
```

---

## Strategies

```kotlin
class CarNavigation : NavigationStrategy {
    override fun navigate(destination: String) {
        println("Driving to $destination")
    }
}

class WalkingNavigation : NavigationStrategy {
    override fun navigate(destination: String) {
        println("Walking to $destination")
    }
}
```

---

## Context

```kotlin
class Navigator(
    private var strategy: NavigationStrategy
) {

    fun navigate(destination: String) {
        strategy.navigate(destination)
    }

    fun change(strategy: NavigationStrategy) {
        this.strategy = strategy
    }
}
```

---

## Runtime Behavior

```kotlin
navigator.change(WalkingNavigation())
```

Behavior changes without modifying Navigator.

---

## Android Example — Payment Gateway ⭐⭐⭐⭐⭐

```text
PaymentManager

↓

UPI Strategy

Card Strategy

Wallet Strategy

NetBanking Strategy
```

Runtime payment selection.

---

## Android Example — Authentication

```text
LoginManager

↓

Google Login

Phone Login

Email Login

Facebook Login
```

---

## Compose Example

Animation strategies.

Different easing functions.

---

## Interview Tip

**OCP (Open Closed Principle)** is implemented using Strategy Pattern.

---

# 26. Observer Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — YouTube Subscribers

You upload video.

Subscribers receive notification automatically.

Publisher doesn't know subscribers personally.

---

## Definition

One-to-many dependency.

Observers receive updates automatically.

---

## Kotlin Example

### Observer

```kotlin
interface Observer {
    fun update(message: String)
}
```

### Subject

```kotlin
class Channel {

    private val observers = mutableListOf<Observer>()

    fun subscribe(observer: Observer) {
        observers.add(observer)
    }

    fun upload(video: String) {
        observers.forEach {
            it.update(video)
        }
    }
}
```

---

## Usage

```kotlin
channel.subscribe(user)

channel.upload("Kotlin Tutorial")
```

Output

```text
New Video: Kotlin Tutorial
```

---

## Android Example — LiveData ⭐⭐⭐⭐⭐

```kotlin
viewModel.user.observe(this) {
    // Observer
}
```

LiveData is Subject.

Activity is Observer.

---

## StateFlow Example

```kotlin
viewModel.state.collect {
    // Observer
}
```

---

## Compose Example

```kotlin
val state by viewModel.state.collectAsState()
```

Compose automatically observes state.

---

## Flow Visualization

```text
MutableStateFlow

↓

StateFlow

↓

Compose Screen

↓

UI Updates
```

---

## Room Example

Room emits Flow updates.

Observer receives database changes.

---

# 27. Command Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — TV Remote

Remote button doesn't know TV implementation.

Button sends command.

---

## Definition

Encapsulates a request into an object.

---

## Command Interface

```kotlin
interface Command {
    fun execute()
}
```

---

## Concrete Command

```kotlin
class LightOnCommand : Command {

    override fun execute() {
        println("Light On")
    }
}
```

---

## Invoker

```kotlin
class Remote {

    fun press(command: Command) {
        command.execute()
    }
}
```

---

## Android Example — Navigation ⭐⭐⭐⭐⭐

Navigation events.

```kotlin
sealed interface NavigationCommand {

    data object Home : NavigationCommand

    data class Profile(
        val userId: String
    ) : NavigationCommand
}
```

---

## ViewModel Emits Command

```kotlin
_event.emit(
    NavigationCommand.Profile(id)
)
```

UI executes navigation.

---

## Snackbar Commands

```text
Show Snackbar

Hide Snackbar

Dismiss Snackbar
```

Commands encapsulate UI actions.

---

## WorkManager Example

Enqueue work request.

Command object.

---

# 28. State Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Zomato Order Tracking

Order changes behavior depending on state.

```text
Pending

↓

Accepted

↓

Preparing

↓

Out For Delivery

↓

Delivered
```

---

## Definition

Object behavior changes according to internal state.

---

## State Interface

```kotlin
interface OrderState {
    fun next()
}
```

---

## Concrete States

```kotlin
class PendingState : OrderState

class PreparingState : OrderState

class DeliveredState : OrderState
```

---

## Context

```kotlin
class Order(
    private var state: OrderState
)
```

---

## Android Example — UI State ⭐⭐⭐⭐⭐

```kotlin
sealed interface LoginUiState {

    data object Idle : LoginUiState

    data object Loading : LoginUiState

    data class Success(val user: User) : LoginUiState

    data class Error(val message: String) : LoginUiState
}
```

Compose reacts to state.

---

## Compose Usage

```kotlin
when(state){

    Loading -> LoadingScreen()

    Success -> HomeScreen()

    Error -> ErrorScreen()
}
```

---

## Download Manager

States:

- Idle
- Downloading
- Paused
- Completed
- Failed

---

# 29. Mediator Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Airport Control Tower

Planes don't communicate directly.

Tower coordinates everything.

---

## Definition

Central object coordinates communication between multiple objects.

---

## Example

```kotlin
class ChatMediator {

    fun send(message: String){
        println(message)
    }
}
```

Users communicate through mediator.

---

## Android Example — ViewModel ⭐⭐⭐⭐⭐

ViewModel mediates between UI and Repository.

```text
UI

↓

ViewModel

↓

Repository

↓

Database / API
```

---

## Another Example

Payment screen.

Mediator coordinates:

- Coupon
- Wallet
- Payment
- Analytics

---

## Compose Example

Scaffold coordinates:

- SnackbarHost
- FAB
- TopBar
- Drawer

---

# 30. Chain of Responsibility ⭐⭐⭐⭐⭐

## 🎭 Story — Passport Verification

Request passes through multiple checkpoints.

```text
Identity

↓

Photo

↓

Address

↓

Police Verification

↓

Passport Approved
```

---

## Definition

Request flows through chain until handled.

---

## Handler

```kotlin
abstract class Handler {

    abstract fun handle(request: String)
}
```

---

## Android Example — OkHttp Interceptors ⭐⭐⭐⭐⭐

```text
Request

↓

LoggingInterceptor

↓

AuthInterceptor

↓

CacheInterceptor

↓

RetryInterceptor

↓

Server
```

Classic Chain of Responsibility.

---

## Validation Chain

```text
Email Validator

↓

Password Validator

↓

OTP Validator
```

---

## Compose Input Validation

TextField validators chained together.

---

## WorkManager Constraints

Battery.

Network.

Storage.

Charging.

Chain before execution.

---

# 31. Iterator Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Netflix Playlist

Move through episodes sequentially.

---

## Definition

Traverse collection without exposing implementation.

---

## Kotlin Example

```kotlin
val iterator = list.iterator()

while(iterator.hasNext()){
    println(iterator.next())
}
```

---

## Android Example — LazyColumn ⭐⭐⭐⭐⭐

```kotlin
LazyColumn {

    items(users){
        UserCard(it)
    }
}
```

Compose internally iterates items lazily.

---

## RecyclerView

Adapter iterates data source.

---

## Paging 3

Iterator over paginated items.

---

# 32. Visitor Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Income Tax Inspection

Visitor performs operations without modifying employees.

---

## Definition

Separate algorithm from object structure.

---

## Example

```kotlin
interface Visitor {

    fun visit(circle: Circle)

    fun visit(square: Square)
}
```

---

## Android Example

Compose Compiler visits composable tree.

---

## Kotlin Compiler

AST visitor.

Annotation processor.

KSP.

---

## XML Parser

Visitor traverses XML nodes.

---

# 33. Template Method Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Coffee Recipe

Recipe steps fixed.

Different coffee customizes brewing.

---

## Definition

Algorithm skeleton in base class.

Subclasses customize steps.

---

## Base Class

```kotlin
abstract class Beverage {

    fun prepare(){

        boilWater()

        brew()

        pour()

        addExtras()
    }

    abstract fun brew()

    abstract fun addExtras()
}
```

---

## Coffee

```kotlin
class Coffee : Beverage()
```

---

## Android Example — Worker ⭐⭐⭐⭐⭐

```kotlin
abstract class CoroutineWorker {

    suspend fun startWork(){

        doWork()
    }

    abstract suspend fun doWork(): Result
}
```

Framework controls algorithm.

Developer implements step.

---

## Fragment Lifecycle

Lifecycle methods follow Template Method.

---

## Activity Lifecycle

`onCreate()`, `onStart()`, `onResume()` customize predefined lifecycle.

---

# 34. Memento Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Google Docs Undo

Every edit saved.

Undo restores previous state.

---

## Definition

Capture and restore object state.

---

## Memento

```kotlin
data class EditorState(
    val text: String
)
```

---

## Editor

```kotlin
class Editor {

    private val history = mutableListOf<EditorState>()
}
```

---

## Android Example — SavedStateHandle ⭐⭐⭐⭐⭐

```kotlin
savedStateHandle["USER_ID"] = id
```

State restored after process death.

---

## Compose rememberSaveable

```kotlin
rememberSaveable {
    mutableStateOf("")
}
```

Automatically restores state.

---

## Navigation State

BackStackEntry stores state snapshots.

---

# 35. Android Production Examples ⭐⭐⭐⭐⭐

| Android Component | Pattern |
|-------------------|---------|
| LiveData | Observer |
| StateFlow | Observer |
| SharedFlow | Observer |
| Navigation Events | Command |
| UI State | State |
| ViewModel | Mediator |
| OkHttp Interceptor | Chain of Responsibility |
| RecyclerView Iterator | Iterator |
| CoroutineWorker | Template Method |
| SavedStateHandle | Memento |
| rememberSaveable | Memento |
| Payment Gateway | Strategy |

---

## Architecture Visualization

```text
UI

↓

ViewModel (Mediator)

↓

Repository

↓

API Chain

↓

Server

↓

StateFlow (Observer)

↓

Compose UI
```

---

# 36. Best Practices ⭐⭐⭐⭐⭐

## Strategy

- Runtime behavior.
- Payment.
- Login.
- Analytics.

---

## Observer

- UI state.
- Database changes.
- Event streams.

---

## Command

- Navigation.
- Snackbar.
- Dialog actions.

---

## State

- Loading/Error/Success UI.
- Authentication.
- Downloads.

---

## Chain

- Validation pipeline.
- Network interceptors.
- Middleware.

---

## Memento

- Saved UI state.
- Draft messages.
- Form restoration.

---

# 37. Common Pitfalls ⭐⭐⭐⭐

## Observer Memory Leak

Always observe with lifecycle awareness.

---

## Event Replay Issue

Don't use StateFlow for one-time events.

Use SharedFlow or Channel.

---

## Giant State Class

Split into smaller state objects.

---

## Long Interceptor Chain

Keep interceptors focused.

---

## Strategy Explosion

Avoid creating dozens of tiny strategies unnecessarily.

---

# 38. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Strategy

1. Strategy vs State?
2. Payment Strategy?
3. Login Strategy?

## Observer

4. LiveData vs StateFlow?
5. SharedFlow vs StateFlow?
6. Compose observation mechanism?

## Command

7. Navigation events?
8. Snackbar events?
9. WorkManager request?

## State

10. Sealed UI State?
11. Reducer pattern?
12. Download state machine?

## Chain

13. OkHttp interceptor order?
14. Validation chain?
15. Middleware pattern?

## Memento

16. rememberSaveable?
17. SavedStateHandle?
18. Process death restoration?

---

# 📋 Cheat Sheet (Part 3)

## Strategy

```text
Behavior changes at runtime.
```

---

## Observer

```text
Publisher → Subscribers
```

LiveData, Flow, StateFlow.

---

## Command

```text
ViewModel → NavigationCommand → UI
```

---

## State

```text
Idle

↓

Loading

↓

Success

↓

Error
```

---

## Mediator

```text
UI ↔ ViewModel ↔ Repository
```

---

## Chain of Responsibility

```text
Request

↓

Interceptor

↓

Interceptor

↓

Server
```

---

## Iterator

```kotlin
items(list){ }
```

---

## Template Method

```kotlin
CoroutineWorker.doWork()
```

---

## Memento

```kotlin
rememberSaveable

SavedStateHandle
```

---

# 📝 Revision Summary

In **Part 3** you learned:

- Strategy Pattern.
- Observer Pattern.
- Command Pattern.
- State Pattern.
- Mediator Pattern.
- Chain of Responsibility.
- Iterator Pattern.
- Visitor Pattern.
- Template Method Pattern.
- Memento Pattern.
- Android examples from LiveData, StateFlow, SharedFlow, Navigation, Compose, WorkManager and SavedStateHandle.

---

# Part 4 — Android Production Patterns, MVVM, MVI, Compose, Coroutines & Interview Mastery

> Learn how design patterns combine together to build production Android apps.

---

# 📚 Table of Contents

35. MVVM Pattern
36. Repository Pattern
37. UseCase (Interactor) Pattern
38. Clean Architecture Pattern
39. UDF / MVI Pattern
40. Paging 3 Pattern
41. Jetpack Compose Architectural Patterns
42. Coroutine Design Patterns
43. WorkManager Pattern
44. Offline First Pattern
45. Event Pattern (StateFlow, SharedFlow, Channel)
46. Dependency Injection Pattern (Hilt)
47. Testing Patterns
48. Refactoring Legacy Android Apps
49. Performance Considerations
50. Best Practices Checklist
51. 100+ Senior Android Interview Questions
52. Ultimate Cheat Sheet
53. Chapter Revision Summary

---

# 🎯 Learning Goals

After completing this part you'll understand:

- Production Android architecture.
- Design patterns used in every Jetpack library.
- Event driven UI.
- Offline-first apps.
- Coroutines communication patterns.
- Testing architecture.
- Large-scale Android application design.

---

# 35. MVVM Pattern ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Zomato Restaurant Screen

A restaurant screen contains:

- Menu
- Reviews
- Offers
- Photos
- Availability

UI should never directly call APIs.

---

## MVVM Architecture

<svg viewBox="0 0 640 360" xmlns="http://www.w3.org/2000/svg">
  <rect x="220" y="20" width="200" height="55" rx="12" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="320" y="52" text-anchor="middle" font-size="16" fill="#16a34a">Compose / Activity</text>

  <rect x="220" y="120" width="200" height="55" rx="12" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="320" y="152" text-anchor="middle" font-size="16" fill="#2563eb">ViewModel</text>

  <rect x="220" y="220" width="200" height="55" rx="12" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="320" y="252" text-anchor="middle" font-size="16" fill="#ea580c">Repository</text>

  <rect x="40" y="305" width="180" height="40" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="130" y="330" text-anchor="middle" font-size="14" fill="#7c3aed">Remote API</text>

  <rect x="420" y="305" width="180" height="40" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="510" y="330" text-anchor="middle" font-size="14" fill="#7c3aed">Room Database</text>

  <path d="M320 75 V120 M320 175 V220 M270 275 V305 M370 275 V305"
        stroke="#64748b" stroke-width="2" fill="none"/>
</svg>

---

## Responsibilities

<table columnSizing="equal">
  <table-row>
    <table-cell width="160">**Layer**</table-cell>
    <table-cell>**Responsibility**</table-cell>
  </table-row>
  <table-row>
    <table-cell>UI</table-cell>
    <table-cell>Display state.</table-cell>
  </table-row>
  <table-row>
    <table-cell>ViewModel</table-cell>
    <table-cell>Business state management.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Repository</table-cell>
    <table-cell>Coordinates data sources.</table-cell>
  </table-row>
  <table-row>
    <table-cell>API / Room</table-cell>
    <table-cell>Actual data providers.</table-cell>
  </table-row>
</table>

---

## ViewModel Example

```kotlin
@HiltViewModel
class RestaurantViewModel @Inject constructor(
    private val repository: RestaurantRepository
) : ViewModel() {

    val state = repository
        .restaurantState()
        .stateIn(...)
}
```

---

## Patterns Used Inside MVVM

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Pattern**</table-cell>
    <table-cell>**Where Used**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Observer</table-cell>
    <table-cell>LiveData / StateFlow</table-cell>
  </table-row>
  <table-row>
    <table-cell>Mediator</table-cell>
    <table-cell>ViewModel</table-cell>
  </table-row>
  <table-row>
    <table-cell>Facade</table-cell>
    <table-cell>Repository</table-cell>
  </table-row>
  <table-row>
    <table-cell>Strategy</table-cell>
    <table-cell>Validation / Payment</table-cell>
  </table-row>
  <table-row>
    <table-cell>Singleton</table-cell>
    <table-cell>Room Database</table-cell>
  </table-row>
</table>

---

# 36. Repository Pattern ⭐⭐⭐⭐⭐

## Story — Flipkart Product Screen

Repository hides multiple data sources.

<svg viewBox="0 0 640 280" xmlns="http://www.w3.org/2000/svg">
  <rect x="210" y="15" width="220" height="45" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="320" y="42" text-anchor="middle" font-size="15" fill="#ea580c">ProductRepository</text>

  <rect x="20" y="120" width="180" height="50" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="110" y="150" text-anchor="middle" font-size="14" fill="#2563eb">Remote API</text>

  <rect x="230" y="120" width="180" height="50" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="320" y="150" text-anchor="middle" font-size="14" fill="#16a34a">Room Database</text>

  <rect x="440" y="120" width="180" height="50" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="530" y="150" text-anchor="middle" font-size="14" fill="#7c3aed">Memory Cache</text>

  <path d="M110 120 V90 H320 V60 M320 60 V90 H320 M530 120 V90 H320"
        stroke="#64748b" stroke-width="2" fill="none"/>
</svg>

---

## Repository Example

```kotlin
class ProductRepository(
    private val api: ProductApi,
    private val dao: ProductDao,
    private val cache: MemoryCache<String, Product>
)
```

---

## Offline First Flow

```text
Room Database

↓

UI

↑

Repository

↓

Remote API
```

Database becomes source of truth.

---

## Benefits

- Offline support.
- Caching.
- Easy testing.
- Single source of truth.

---

# 37. UseCase (Interactor) Pattern ⭐⭐⭐⭐⭐

## Story — PhonePe UPI Payment

Payment process contains multiple steps.

<svg viewBox="0 0 640 120" xmlns="http://www.w3.org/2000/svg">
  <rect x="10" y="35" width="120" height="45" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="70" y="62" text-anchor="middle" font-size="13" fill="#2563eb">Validate User</text>

  <rect x="170" y="35" width="120" height="45" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="230" y="62" text-anchor="middle" font-size="13" fill="#16a34a">Check Balance</text>

  <rect x="330" y="35" width="120" height="45" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="390" y="62" text-anchor="middle" font-size="13" fill="#ea580c">Transfer Money</text>

  <rect x="490" y="35" width="120" height="45" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="550" y="62" text-anchor="middle" font-size="13" fill="#7c3aed">Notify User</text>

  <path d="M130 58 H170 M290 58 H330 M450 58 H490"
        stroke="#64748b" stroke-width="2"/>
</svg>

---

## Generic UseCase

```kotlin
abstract class UseCase<I, O> {

    abstract suspend operator fun invoke(input: I): O
}
```

---

## Login UseCase

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

---

## Why UseCases?

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Benefit**</table-cell>
    <table-cell>**Reason**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Business logic isolation</table-cell>
    <table-cell>ViewModel stays small.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Reuse</table-cell>
    <table-cell>Multiple screens share UseCase.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Testing</table-cell>
    <table-cell>Easy unit testing.</table-cell>
  </table-row>
</table>

---

# 38. Clean Architecture Pattern ⭐⭐⭐⭐⭐

## Uncle Bob Architecture

<svg viewBox="0 0 640 360" xmlns="http://www.w3.org/2000/svg">
  <circle cx="320" cy="180" r="150" fill="#2563eb" opacity="0.05" stroke="#2563eb"/>
  <circle cx="320" cy="180" r="110" fill="#16a34a" opacity="0.05" stroke="#16a34a"/>
  <circle cx="320" cy="180" r="70" fill="#ea580c" opacity="0.05" stroke="#ea580c"/>

  <text x="320" y="55" text-anchor="middle" font-size="15" fill="#2563eb">Frameworks (Compose / Activity / Room / Retrofit)</text>

  <text x="320" y="120" text-anchor="middle" font-size="15" fill="#16a34a">Interface Adapters (Repository / Mapper / DAO)</text>

  <text x="320" y="190" text-anchor="middle" font-size="15" fill="#ea580c">UseCases (Business Rules)</text>

  <text x="320" y="250" text-anchor="middle" font-size="15" fill="#111827">Entities</text>
</svg>

---

## Dependency Rule

Dependencies always point inward.

```text
UI

↓

Repository

↓

UseCase

↓

Entity
```

---

## Android Folder Structure

```text
feature-home/

data/

domain/

presentation/

core/
```

---

## Patterns Used

- Repository
- Factory
- Mapper
- Observer
- Strategy
- DI
- State

---

# 39. UDF / MVI Pattern ⭐⭐⭐⭐⭐

## Story — Banking Transfer Screen

Everything flows in one direction.

<svg viewBox="0 0 640 220" xmlns="http://www.w3.org/2000/svg">
  <rect x="20" y="80" width="120" height="50" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="80" y="110" text-anchor="middle" font-size="14" fill="#2563eb">Intent</text>

  <rect x="190" y="80" width="120" height="50" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="250" y="110" text-anchor="middle" font-size="14" fill="#16a34a">Reducer</text>

  <rect x="360" y="80" width="120" height="50" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="420" y="110" text-anchor="middle" font-size="14" fill="#ea580c">State</text>

  <rect x="510" y="80" width="110" height="50" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="565" y="110" text-anchor="middle" font-size="14" fill="#7c3aed">UI</text>

  <path d="M140 105 H190 M310 105 H360 M480 105 H510"
        stroke="#64748b" stroke-width="2"/>
</svg>

---

## Intent

User actions.

```kotlin
sealed interface LoginIntent {

    data class Login(
        val email: String,
        val password: String
    ) : LoginIntent

    data object Retry : LoginIntent
}
```

---

## State

```kotlin
data class LoginState(...)
```

---

## Reducer

Pure function.

```kotlin
fun reduce(...)
```

---

## Compose Integration

```kotlin
val state by viewModel.state.collectAsState()
```

---

## MVI vs MVVM

<table columnSizing="equal">
  <table-row>
    <table-cell width="180">**MVVM**</table-cell>
    <table-cell>**MVI / UDF**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Two-way interactions possible.</table-cell>
    <table-cell>Strict one-way data flow.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Multiple mutable states.</table-cell>
    <table-cell>Single immutable state object.</table-cell>
  </table-row>
  <table-row>
    <table-cell>ViewModel exposes state.</table-cell>
    <table-cell>Reducer produces new state.</table-cell>
  </table-row>
</table>

---

# 40. Paging 3 Pattern ⭐⭐⭐⭐⭐

## Story — Instagram Infinite Feed

Don't load 10,000 posts.

Load pages.

---

## Paging Architecture

<svg viewBox="0 0 640 240" xmlns="http://www.w3.org/2000/svg">
  <rect x="220" y="15" width="200" height="45" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="320" y="42" text-anchor="middle" font-size="14" fill="#2563eb">Pager</text>

  <rect x="20" y="90" width="170" height="50" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="105" y="120" text-anchor="middle" font-size="14" fill="#16a34a">PagingSource</text>

  <rect x="240" y="90" width="160" height="50" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="320" y="120" text-anchor="middle" font-size="14" fill="#ea580c">Repository</text>

  <rect x="450" y="90" width="170" height="50" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="535" y="120" text-anchor="middle" font-size="14" fill="#7c3aed">RemoteMediator</text>

  <rect x="210" y="180" width="220" height="45" rx="10" fill="#0891b2" opacity="0.12" stroke="#0891b2"/>
  <text x="320" y="207" text-anchor="middle" font-size="14" fill="#0891b2">RecyclerView / LazyColumn</text>

  <path d="M320 60 V90 M190 115 H240 M400 115 H450 M320 140 V180"
        stroke="#64748b" stroke-width="2" fill="none"/>
</svg>

---

## Patterns Used

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Pattern**</table-cell>
    <table-cell>**Where Used**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Factory</table-cell>
    <table-cell>Pager creates PagingSource.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Iterator</table-cell>
    <table-cell>PagingData iteration.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Observer</table-cell>
    <table-cell>Flow of PagingData.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Adapter</table-cell>
    <table-cell>PagingDataAdapter.</table-cell>
  </table-row>
</table>

---

## Android Example

```kotlin
Pager(
    config = PagingConfig(...)
)
```

Factory creates paging pipeline.

---

# 41. Jetpack Compose Architectural Patterns ⭐⭐⭐⭐⭐

## Story — LEGO UI

Small composables build large UI.

---

## Compose Tree

<svg viewBox="0 0 320 300" xmlns="http://www.w3.org/2000/svg">
  <rect x="95" y="10" width="130" height="36" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="160" y="33" text-anchor="middle" font-size="14" fill="#2563eb">Scaffold</text>

  <rect x="15" y="80" width="90" height="36" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="60" y="103" text-anchor="middle" font-size="12" fill="#16a34a">TopBar</text>

  <rect x="115" y="80" width="90" height="36" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="160" y="103" text-anchor="middle" font-size="12" fill="#ea580c">Content</text>

  <rect x="215" y="80" width="90" height="36" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="260" y="103" text-anchor="middle" font-size="12" fill="#7c3aed">FAB</text>

  <rect x="105" y="150" width="110" height="36" rx="10" fill="#0891b2" opacity="0.12" stroke="#0891b2"/>
  <text x="160" y="173" text-anchor="middle" font-size="12" fill="#0891b2">LazyColumn</text>

  <rect x="20" y="230" width="70" height="30" rx="8" fill="#64748b" opacity="0.12" stroke="#64748b"/>
  <text x="55" y="249" text-anchor="middle" font-size="11" fill="#475569">Card</text>

  <rect x="125" y="230" width="70" height="30" rx="8" fill="#64748b" opacity="0.12" stroke="#64748b"/>
  <text x="160" y="249" text-anchor="middle" font-size="11" fill="#475569">Image</text>

  <rect x="230" y="230" width="70" height="30" rx="8" fill="#64748b" opacity="0.12" stroke="#64748b"/>
  <text x="265" y="249" text-anchor="middle" font-size="11" fill="#475569">Text</text>

  <path d="M160 46 V80 M160 116 V150 M160 186 V200 M160 200 H55 V230 M160 200 V230 M160 200 H265 V230"
        stroke="#64748b" stroke-width="2" fill="none"/>
</svg>

---

## Patterns Used

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Pattern**</table-cell>
    <table-cell>**Compose Usage**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Composite</table-cell>
    <table-cell>UI tree.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Decorator</table-cell>
    <table-cell>Modifier chain.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Observer</table-cell>
    <table-cell>collectAsState().</table-cell>
  </table-row>
  <table-row>
    <table-cell>State</table-cell>
    <table-cell>MutableState.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Memento</table-cell>
    <table-cell>rememberSaveable().</table-cell>
  </table-row>
</table>

---

## State Hoisting

```text
Parent owns state

↓

Child receives state

↓

Child emits events
```

UDF pattern.

---

# 42. Coroutine Design Patterns ⭐⭐⭐⭐⭐

## Structured Concurrency

<svg viewBox="0 0 640 240" xmlns="http://www.w3.org/2000/svg">
  <rect x="240" y="15" width="160" height="42" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="320" y="40" text-anchor="middle" font-size="14" fill="#2563eb">viewModelScope</text>

  <rect x="40" y="110" width="140" height="42" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="110" y="135" text-anchor="middle" font-size="13" fill="#16a34a">Load User</text>

  <rect x="250" y="110" width="140" height="42" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="320" y="135" text-anchor="middle" font-size="13" fill="#ea580c">Load Cart</text>

  <rect x="460" y="110" width="140" height="42" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="530" y="135" text-anchor="middle" font-size="13" fill="#7c3aed">Load Offers</text>

  <path d="M320 57 V80 M320 80 H110 V110 M320 80 V110 M320 80 H530 V110"
        stroke="#64748b" stroke-width="2" fill="none"/>
</svg>

---

## Producer Consumer Pattern

```text
Producer

↓

Channel

↓

Consumer
```

Used in Coroutines Channels.

---

## Fan Out Pattern

One producer.

Multiple consumers.

Example:

Notifications.

Analytics.

Logging.

---

## Fan In Pattern

Multiple producers.

One consumer.

Example:

Combine Flows.

---

## Supervisor Pattern

```text
SupervisorJob

├── Child 1
├── Child 2
└── Child 3
```

Child failure doesn't cancel siblings.

---

## Android Example

Loading

- Profile
- Notifications
- Wallet

independently.

---

# 43. WorkManager Pattern ⭐⭐⭐⭐⭐

## Story — Background Upload Queue

Tasks survive app restart.

---

## WorkManager Pipeline

<svg viewBox="0 0 640 240" xmlns="http://www.w3.org/2000/svg">
  <rect x="25" y="90" width="140" height="44" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="95" y="118" text-anchor="middle" font-size="13" fill="#2563eb">Constraints</text>

  <rect x="250" y="90" width="140" height="44" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="320" y="118" text-anchor="middle" font-size="13" fill="#16a34a">Worker</text>

  <rect x="475" y="90" width="140" height="44" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="545" y="118" text-anchor="middle" font-size="13" fill="#ea580c">Scheduler</text>

  <path d="M165 112 H250 M390 112 H475"
        stroke="#64748b" stroke-width="2"/>
</svg>

---

## Patterns Used

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Pattern**</table-cell>
    <table-cell>**WorkManager Usage**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Builder</table-cell>
    <table-cell>WorkRequest.Builder.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Factory</table-cell>
    <table-cell>WorkerFactory.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Command</table-cell>
    <table-cell>Enqueue work request.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Template Method</table-cell>
    <table-cell>doWork().</table-cell>
  </table-row>
</table>

---

## Worker Example

```kotlin
class SyncWorker(...) : CoroutineWorker(...)
```

---

# 44. Offline First Pattern ⭐⭐⭐⭐⭐

## Story — WhatsApp Messages

Messages appear instantly.

Sync happens later.

---

## Architecture

<svg viewBox="0 0 640 260" xmlns="http://www.w3.org/2000/svg">
  <rect x="220" y="15" width="200" height="42" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="320" y="40" text-anchor="middle" font-size="14" fill="#2563eb">Repository</text>

  <rect x="20" y="90" width="180" height="48" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="110" y="120" text-anchor="middle" font-size="13" fill="#16a34a">Room Database</text>

  <rect x="440" y="90" width="180" height="48" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="530" y="120" text-anchor="middle" font-size="13" fill="#ea580c">Remote API</text>

  <rect x="220" y="190" width="200" height="42" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="320" y="216" text-anchor="middle" font-size="14" fill="#7c3aed">UI</text>

  <path d="M320 57 V75 M200 114 H220 M420 114 H440 M320 138 V190"
        stroke="#64748b" stroke-width="2" fill="none"/>
</svg>

---

## Flow

```text
API

↓

Room

↓

Flow

↓

Compose
```

---

## Patterns Used

- Repository
- Observer
- Singleton
- Factory
- Strategy

---

## Android Example

Google's **Now in Android** app follows this architecture.

---

# 45. Event Pattern ⭐⭐⭐⭐⭐

## State vs Event

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**State**</table-cell>
    <table-cell>**Event**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Persistent.</table-cell>
    <table-cell>One-time.</table-cell>
  </table-row>
  <table-row>
    <table-cell>UI survives rotation.</table-cell>
    <table-cell>Snackbar, Navigation.</table-cell>
  </table-row>
</table>

---

## SharedFlow

One-time events.

```kotlin
MutableSharedFlow<UiEvent>()
```

---

## Channel

Point-to-point communication.

```kotlin
Channel<NavigationCommand>()
```

---

## Event Wrapper

```kotlin
sealed interface UiEvent {

    data object NavigateHome : UiEvent

    data class ShowToast(
        val message: String
    ) : UiEvent
}
```

---

## Compose Example

```kotlin
LaunchedEffect(Unit) {

    events.collect { event ->

    }
}
```

---

# 46. Dependency Injection Pattern (Hilt) ⭐⭐⭐⭐⭐

## Story — Hospital

Doctors receive equipment.

They don't manufacture it.

---

## Hilt Graph

<svg viewBox="0 0 640 250" xmlns="http://www.w3.org/2000/svg">
  <rect x="230" y="10" width="180" height="42" rx="10" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="320" y="36" text-anchor="middle" font-size="14" fill="#2563eb">Hilt Container</text>

  <rect x="20" y="90" width="160" height="42" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="100" y="116" text-anchor="middle" font-size="13" fill="#16a34a">Retrofit</text>

  <rect x="240" y="90" width="160" height="42" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="320" y="116" text-anchor="middle" font-size="13" fill="#ea580c">Room</text>

  <rect x="460" y="90" width="160" height="42" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="540" y="116" text-anchor="middle" font-size="13" fill="#7c3aed">Repository</text>

  <rect x="220" y="180" width="200" height="42" rx="10" fill="#0891b2" opacity="0.12" stroke="#0891b2"/>
  <text x="320" y="206" text-anchor="middle" font-size="14" fill="#0891b2">ViewModel</text>

  <path d="M320 52 V70 M100 132 V150 H320 V180 M320 132 V180 M540 132 V150 H320"
        stroke="#64748b" stroke-width="2" fill="none"/>
</svg>

---

## Patterns Used

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Pattern**</table-cell>
    <table-cell>**Hilt Usage**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Singleton</table-cell>
    <table-cell>Application scope.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Factory</table-cell>
    <table-cell>Provides dependencies.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Builder</table-cell>
    <table-cell>Component graph.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Composition</table-cell>
    <table-cell>Constructor Injection.</table-cell>
  </table-row>
</table>

---

# 47. Testing Patterns ⭐⭐⭐⭐⭐

## Fake Repository Pattern

```kotlin
class FakeUserRepository
```

---

## Test Double Types

<table columnSizing="equal">
  <table-row>
    <table-cell width="180">**Type**</table-cell>
    <table-cell>**Purpose**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Fake</table-cell>
    <table-cell>Working simplified implementation.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Mock</table-cell>
    <table-cell>Verifies interactions.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Stub</table-cell>
    <table-cell>Returns predefined values.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Spy</table-cell>
    <table-cell>Wraps real object.</table-cell>
  </table-row>
</table>

---

## ViewModel Testing Pattern

```text
ViewModel

↓

Fake Repository

↓

Fake API
```

---

## StateFlow Testing

Use Turbine.

Observe emissions.

---

# 48. Refactoring Legacy Android Apps ⭐⭐⭐⭐⭐

## Before

```text
BaseActivity

↓

BaseFragment

↓

BaseViewModel
```

Huge inheritance hierarchy.

---

## After

```text
Activity

HAS

Navigator

SnackbarManager

PermissionManager

AnalyticsTracker
```

Composition replaces inheritance.

---

## Migration Strategy

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Step**</table-cell>
    <table-cell>**Action**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Extract analytics.</table-cell>
    <table-cell>AnalyticsTracker.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Extract permissions.</table-cell>
    <table-cell>PermissionManager.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Extract loading.</table-cell>
    <table-cell>LoadingController.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Inject dependencies.</table-cell>
    <table-cell>Hilt.</table-cell>
  </table-row>
</table>

---

# 49. Performance Considerations ⭐⭐⭐⭐⭐

## Compose

`remember {}` caches objects.

---

## RecyclerView

ViewHolder Pool.

DiffUtil.

Payload updates.

---

## Coroutines

SupervisorJob.

Structured concurrency.

Dispatchers.

---

## Memory Optimization

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Pattern**</table-cell>
    <table-cell>**Optimization**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Flyweight</table-cell>
    <table-cell>Bitmap reuse.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Object Pool</table-cell>
    <table-cell>ViewHolder reuse.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Singleton</table-cell>
    <table-cell>Database reuse.</table-cell>
  </table-row>
</table>

---

# 50. Best Practices Checklist ⭐⭐⭐⭐⭐

## Choose the Right Pattern

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Problem**</table-cell>
    <table-cell>**Pattern**</table-cell>
  </table-row>
  <table-row>
    <table-cell>One object only.</table-cell>
    <table-cell>Singleton.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Create object.</table-cell>
    <table-cell>Factory.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Complex object creation.</table-cell>
    <table-cell>Builder.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Runtime behavior.</table-cell>
    <table-cell>Strategy.</table-cell>
  </table-row>
  <table-row>
    <table-cell>State management.</table-cell>
    <table-cell>State.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Event updates.</table-cell>
    <table-cell>Observer.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Background pipeline.</table-cell>
    <table-cell>Chain of Responsibility.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Tree UI.</table-cell>
    <table-cell>Composite.</table-cell>
  </table-row>
  <table-row>
    <table-cell>UI modifiers.</table-cell>
    <table-cell>Decorator.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Hide complexity.</table-cell>
    <table-cell>Facade.</table-cell>
  </table-row>
</table>

---

# 51. 100+ Senior Android Interview Questions ⭐⭐⭐⭐⭐

## MVVM

1. Why MVVM?
2. Repository responsibility?
3. ViewModel lifecycle?
4. StateFlow vs LiveData?
5. SharedFlow vs Channel?

## MVI

6. Reducer?
7. Intent?
8. Side effects?
9. State immutability?
10. Event handling?

## Compose

11. Why Compose uses Composite Pattern?
12. Modifier Decorator Pattern?
13. remember vs rememberSaveable?
14. State Hoisting?
15. Snapshot State?

## Coroutines

16. SupervisorJob?
17. Structured concurrency?
18. Channel pattern?
19. Flow combine?
20. Producer Consumer?

## Architecture

21. Offline First?
22. Single Source of Truth?
23. Clean Architecture dependency rule?
24. UseCase benefits?
25. Repository caching strategy?

*(Continue practicing scenario-based design questions instead of memorizing definitions.)*

---

# 📋 Ultimate Design Pattern Cheat Sheet

<table columnSizing="equal">
  <table-row>
    <table-cell width="220">**Pattern**</table-cell>
    <table-cell>**Android Example**</table-cell>
  </table-row>
  <table-row>
    <table-cell>Singleton</table-cell>
    <table-cell>Room Database.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Factory</table-cell>
    <table-cell>ViewModelProvider.Factory.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Builder</table-cell>
    <table-cell>Retrofit.Builder().</table-cell>
  </table-row>
  <table-row>
    <table-cell>Prototype</table-cell>
    <table-cell>UiState.copy().</table-cell>
  </table-row>
  <table-row>
    <table-cell>Adapter</table-cell>
    <table-cell>RecyclerView.Adapter.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Decorator</table-cell>
    <table-cell>Compose Modifier.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Facade</table-cell>
    <table-cell>Repository.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Bridge</table-cell>
    <table-cell>ImageLoader abstraction.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Composite</table-cell>
    <table-cell>Compose UI Tree.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Proxy</table-cell>
    <table-cell>Retrofit dynamic proxy.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Flyweight</table-cell>
    <table-cell>RecyclerView Pool.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Strategy</table-cell>
    <table-cell>Payment/Auth Strategy.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Observer</table-cell>
    <table-cell>StateFlow / LiveData.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Command</table-cell>
    <table-cell>Navigation Events.</table-cell>
  </table-row>
  <table-row>
    <table-cell>State</table-cell>
    <table-cell>UI State.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Mediator</table-cell>
    <table-cell>ViewModel.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Chain</table-cell>
    <table-cell>OkHttp Interceptors.</table-cell>
  </table-row>
  <table-row>
    <table-cell>Iterator</table-cell>
    <table-cell>LazyColumn items().</table-cell>
  </table-row>
  <table-row>
    <table-cell>Template Method</table-cell>
    <table-cell>CoroutineWorker.doWork().</table-cell>
  </table-row>
  <table-row>
    <table-cell>Memento</table-cell>
    <table-cell>rememberSaveable().</table-cell>
  </table-row>
</table>

---

# 📝 Complete Chapter Revision Summary

## What You Learned in Chapter 19

<table columnSizing="equal">
  <table-row>
    <table-cell width="140">**Part**</table-cell>
    <table-cell>**Topics Covered**</table-cell>
  </table-row>
  <table-row>
    <table-cell>**Part 1**</table-cell>
    <table-cell>Singleton, Factory, Abstract Factory, Builder, Prototype, Object Pool, Dependency Injection.</table-cell>
  </table-row>
  <table-row>
    <table-cell>**Part 2**</table-cell>
    <table-cell>Adapter, Decorator, Facade, Bridge, Composite, Proxy, Flyweight, Kotlin Delegation.</table-cell>
  </table-row>
  <table-row>
    <table-cell>**Part 3**</table-cell>
    <table-cell>Strategy, Observer, Command, State, Mediator, Chain of Responsibility, Iterator, Visitor, Template Method, Memento.</table-cell>
  </table-row>
  <table-row>
    <table-cell>**Part 4**</table-cell>
    <table-cell>MVVM, Repository, Clean Architecture, MVI/UDF, Paging 3, Compose Patterns, Coroutine Patterns, WorkManager, Offline First, Event Patterns, Testing, Performance, Refactoring.</table-cell>
  </table-row>
</table>

---

# 🏆 Android Design Pattern Mind Map (Interview Revision)

<svg viewBox="0 0 760 540" xmlns="http://www.w3.org/2000/svg">
  <rect x="270" y="20" width="220" height="52" rx="14" fill="#2563eb" opacity="0.12" stroke="#2563eb"/>
  <text x="380" y="52" text-anchor="middle" font-size="18" fill="#2563eb">Android Design Patterns</text>

  <rect x="40" y="120" width="170" height="46" rx="10" fill="#16a34a" opacity="0.12" stroke="#16a34a"/>
  <text x="125" y="148" text-anchor="middle" font-size="15" fill="#16a34a">Creational</text>

  <rect x="295" y="120" width="170" height="46" rx="10" fill="#ea580c" opacity="0.12" stroke="#ea580c"/>
  <text x="380" y="148" text-anchor="middle" font-size="15" fill="#ea580c">Structural</text>

  <rect x="550" y="120" width="170" height="46" rx="10" fill="#7c3aed" opacity="0.12" stroke="#7c3aed"/>
  <text x="635" y="148" text-anchor="middle" font-size="15" fill="#7c3aed">Behavioral</text>

  <path d="M380 72 V96 M380 96 H125 V120 M380 96 V120 M380 96 H635 V120"
        stroke="#64748b" stroke-width="2" fill="none"/>

  <text x="20" y="205" font-size="13">Singleton</text>
  <text x="20" y="225" font-size="13">Factory</text>
  <text x="20" y="245" font-size="13">Builder</text>
  <text x="20" y="265" font-size="13">Prototype</text>

  <text x="260" y="205" font-size="13">Adapter</text>
  <text x="260" y="225" font-size="13">Decorator</text>
  <text x="260" y="245" font-size="13">Composite</text>
  <text x="260" y="265" font-size="13">Facade</text>
  <text x="260" y="285" font-size="13">Proxy</text>

  <text x="520" y="205" font-size="13">Strategy</text>
  <text x="520" y="225" font-size="13">Observer</text>
  <text x="520" y="245" font-size="13">State</text>
  <text x="520" y="265" font-size="13">Command</text>
  <text x="520" y="285" font-size="13">Mediator</text>

  <rect x="180" y="360" width="400" height="130" rx="14" fill="#0891b2" opacity="0.08" stroke="#0891b2"/>
  <text x="380" y="390" text-anchor="middle" font-size="16" fill="#0891b2">Android Architecture Uses All Patterns Together</text>

  <text x="210" y="420" font-size="13">MVVM • Repository • Hilt • Compose • Paging • Coroutines</text>
  <text x="210" y="445" font-size="13">Room • Retrofit • WorkManager • Navigation • StateFlow</text>
</svg>

---

# 🎯 Android Interview Takeaways

After completing **Chapter 19**, you should be able to explain:

- **23 Gang of Four Design Patterns** with Kotlin implementations.
- Which design patterns power **Jetpack Compose**, **Hilt**, **Retrofit**, **Room**, **Paging 3**, **WorkManager**, **Navigation**, **Coroutines**, and **StateFlow**.
- How multiple patterns combine inside **MVVM**, **MVI**, and **Clean Architecture**.
- Performance and testing implications of each pattern.
- Real production examples from enterprise Android applications.

---