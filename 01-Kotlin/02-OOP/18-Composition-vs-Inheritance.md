# 🧩 Composition vs Inheritance — Android Interview Bible (2026 Edition)


## 📌 Module Information

| Property | Value |
|----------|-------|
| **Module** | Kotlin OOP & Design Principles |
| **File** | `18-Composition-vs-Inheritance.md` |
| **Folder** | `01-Kotlin/02-OOP/` |
| **Difficulty** | Intermediate → Staff Android Engineer |
| **Interview Frequency** | ⭐⭐⭐⭐⭐ Extremely High |
| **Companies** | Google, Uber, Amazon, Microsoft, PhonePe, CRED, Flipkart, Meesho |

---

# 📚 Complete Chapter Roadmap

## Part 1 — Fundamentals (This Part)

1. What is Inheritance?
2. What is Composition?
3. Composition vs Inheritance Overview.
4. Real World Story — Swiggy Delivery App.
5. "IS-A" vs "HAS-A" Relationship.
6. Why Composition is Preferred in Modern Android.
7. Tight Coupling vs Loose Coupling.
8. Kotlin Example — Car & Engine.
9. Android Example — Activity & ViewModel.
10. Best Practices.
11. Common Pitfalls.
12. Interview Questions.
13. Cheat Sheet.

## Part 2 — Advanced Design Patterns

14. Strategy Pattern.
15. Decorator Pattern.
16. Delegation (`by` keyword).
17. Interface Delegation.
18. Property Delegation.
19. Composition with Dependency Injection.
20. SOLID Principles Connection.
21. Liskov Substitution Principle.
22. Open/Closed Principle.

## Part 3 — Android Architecture

23. MVVM Architecture.
24. Repository Composition.
25. UseCase Composition.
26. Compose UI Composition.
27. RecyclerView Composition.
28. Navigation Composition.
29. Room Composition.
30. Retrofit Composition.
31. Coroutines Composition.

## Part 4 — JVM Internals & Interview Mastery

32. Memory Comparison.
33. Performance Comparison.
34. Testing Comparison.
35. Refactoring Inheritance to Composition.
36. Production Patterns.
37. Anti Patterns.
38. 60+ Interview Questions.
39. Ultimate Cheat Sheet.

---

# 🎯 Learning Goals

After completing this chapter you'll understand:

- Difference between **Inheritance** and **Composition**.
- When to use each.
- Why Google recommends **Composition over Inheritance**.
- How Jetpack Compose is built around composition.
- How Clean Architecture favors composition.
- Interview-ready design decisions with real Android examples.

---

> **Part 1: Fundamentals & Design Philosophy**
>
> Learn one of the **most important Object-Oriented Design (OOD)** concepts for Android interviews. This chapter covers **Composition over Inheritance**, SOLID principles, Clean Architecture, Jetpack Compose, MVVM, Delegation, Strategy Pattern, Dependency Injection, and production Android examples.

---

# 1. What is Inheritance? ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Family Tree

Imagine a family.

```text
👨 Grandfather
      │
      ▼
👨 Father
      │
      ▼
👦 Son
```

The son inherits characteristics from the father.

This is **Inheritance**.

---

## Definition

Inheritance allows one class to acquire the properties and behavior of another class.

```kotlin
open class Animal {

    fun eat() {
        println("Animal is eating")
    }
}

class Dog : Animal()
```

Usage

```kotlin
val dog = Dog()

dog.eat()
```

Output

```text
Animal is eating
```

Dog inherited `eat()`.

---

## "IS-A" Relationship

A Dog **IS AN** Animal.

A Cat **IS AN** Animal.

A Husky **IS A** Dog.

```text
Animal
├── Dog
├── Cat
└── Bird
```

Inheritance models hierarchy.

---

## Why Inheritance Was Popular?

- Code reuse.
- Polymorphism.
- Hierarchical modeling.
- OOP foundation.

But it comes with trade-offs.

---

# 2. What is Composition? ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Car Assembly

A car is made of many independent parts.

```text
🚗 Car
│
├── Engine
├── Wheels
├── Battery
├── Steering
└── Music System
```

A Car **HAS AN** Engine.

A Car **HAS** Wheels.

The engine exists independently.

This is **Composition**.

---

## Definition

Composition means building a class using **other classes as components** instead of extending them.

```kotlin
class Engine {

    fun start() {
        println("Engine Started")
    }
}

class Car(
    private val engine: Engine
) {

    fun drive() {
        engine.start()
        println("Car Driving")
    }
}
```

Usage

```kotlin
val car = Car(Engine())

car.drive()
```

Output

```text
Engine Started
Car Driving
```

---

## "HAS-A" Relationship

A Car **HAS AN** Engine.

A User **HAS AN** Address.

An Activity **HAS A** ViewModel.

---

## Visualization

```text
Car
 │
 ├── Engine
 ├── Battery
 └── Wheels
```

Objects collaborate instead of inherit.

---

# 3. Composition vs Inheritance Overview ⭐⭐⭐⭐⭐

## Side-by-Side Comparison

| Feature | Inheritance | Composition |
|--------|-------------|-------------|
| Relationship | IS-A | HAS-A |
| Coupling | Tight | Loose |
| Flexibility | Less | High |
| Reuse | Hierarchy | Object Collaboration |
| Runtime Replacement | Difficult | Easy |
| Testing | Harder | Easier |
| Android Recommendation | Limited | Preferred |

---

## Visualization

### Inheritance

```text
Animal
   │
   ▼
 Dog
```

### Composition

```text
Dog
 │
 ├── Tail
 ├── Collar
 └── FoodStrategy
```

Composition builds objects from smaller behaviors.

---

# 4. Real World Story — Swiggy Delivery App ⭐⭐⭐⭐⭐

Suppose Swiggy supports multiple delivery methods.

### Inheritance Approach

```text
DeliveryPartner

├── BikeDeliveryPartner
├── CycleDeliveryPartner
├── WalkingDeliveryPartner
└── DroneDeliveryPartner
```

Now Swiggy introduces Electric Bike.

Need another subclass.

Explosion of subclasses begins.

---

## Composition Approach

```text
DeliveryPartner
│
├── Vehicle
├── NavigationService
├── PaymentCollector
└── NotificationSender
```

Now replace vehicle dynamically.

```kotlin
DeliveryPartner(Bike())

DeliveryPartner(Cycle())

DeliveryPartner(Drone())
```

No hierarchy explosion.

---

## Why This Is Better?

Behavior changes without creating new subclasses.

This is how scalable systems evolve.

---

# 5. IS-A vs HAS-A Relationship ⭐⭐⭐⭐⭐

## IS-A Examples

| Child | Parent |
|-------|--------|
| Dog | Animal |
| CarActivity | Activity |
| Circle | Shape |
| Button | View |

---

## HAS-A Examples

| Object | Component |
|--------|-----------|
| Car | Engine |
| User | Address |
| Activity | ViewModel |
| Repository | ApiService |
| ViewModel | UseCase |

---

## Decision Rule

Ask yourself:

> **Can I say "IS-A"?**

If yes → Inheritance may fit.

> **Can I say "HAS-A"?**

If yes → Composition is usually better.

---

## Example

```text
LoginViewModel IS A ViewModel ✅

LoginViewModel HAS A Repository ✅

Repository HAS AN ApiService ✅
```

---

# 6. Why Composition is Preferred in Modern Android ⭐⭐⭐⭐⭐

## 🎭 Story — Google Maps

Google Maps screen contains:

```text
Map Screen

├── LocationProvider
├── CameraController
├── PermissionManager
├── RouteCalculator
├── MarkerRenderer
└── AnalyticsTracker
```

Would inheritance make sense?

No.

Every responsibility is a separate object.

---

## Android Architecture Example

```text
Activity

HAS

ViewModel

HAS

Repository

HAS

ApiService
```

Everything uses composition.

---

## Google's Recommendation

Jetpack libraries encourage object collaboration instead of deep inheritance trees.

Examples:

- ViewModel
- Repository
- UseCase
- Hilt
- Navigation
- Compose

---

## Benefits

- Easier testing.
- Smaller classes.
- Better reuse.
- Better SOLID compliance.
- Runtime flexibility.

---

# 7. Tight Coupling vs Loose Coupling ⭐⭐⭐⭐⭐

## Tight Coupling (Inheritance)

```kotlin
open class Animal {

    fun eat() {}
}

class Dog : Animal()
```

Dog is permanently tied to Animal.

Changing Animal affects Dog.

---

## Loose Coupling (Composition)

```kotlin
class Engine

class Car(
    private val engine: Engine
)
```

Replace Engine anytime.

---

## Replace at Runtime

```kotlin
val petrolCar = Car(PetrolEngine())

val electricCar = Car(ElectricEngine())
```

No Car modification.

---

## Android Example

```kotlin
class LoginViewModel(
    private val repository: AuthRepository
)
```

Swap FakeRepository during testing.

---

## Visualization

### Tight Coupling

```text
Dog
 │
 ▼
Animal
```

### Loose Coupling

```text
Car
 │
 ▼
Engine Interface
```

---

# 8. Kotlin Example — Car & Engine ⭐⭐⭐⭐⭐

## Inheritance Version

```kotlin
open class Engine {

    fun start() {
        println("Engine Started")
    }
}

class Car : Engine() {

    fun drive() {
        start()
    }
}
```

Problem:

A Car is **not** an Engine.

Wrong modeling.

---

## Composition Version

```kotlin
class Engine {

    fun start() {
        println("Engine Started")
    }
}

class Car(
    private val engine: Engine
) {

    fun drive() {
        engine.start()
        println("Driving")
    }
}
```

Correct relationship.

---

## Upgrade Engine

```kotlin
interface Engine {
    fun start()
}

class PetrolEngine : Engine {

    override fun start() {
        println("Petrol Engine")
    }
}

class ElectricEngine : Engine {

    override fun start() {
        println("Electric Engine")
    }
}

class Car(
    private val engine: Engine
)
```

Swap implementation freely.

---

## Output

```text
Petrol Engine

Electric Engine
```

Same Car.

Different behavior.

---

# 9. Android Example — Activity & ViewModel ⭐⭐⭐⭐⭐

## Wrong Inheritance

```kotlin
class LoginViewModel :
    LoginRepository()
```

ViewModel is **not** a Repository.

Bad design.

---

## Correct Composition

```kotlin
class LoginViewModel(
    private val repository: LoginRepository
) : ViewModel()
```

ViewModel **HAS A** Repository.

---

## Repository Composition

```kotlin
class LoginRepository(
    private val api: LoginApi,
    private val dao: UserDao
)
```

Repository has dependencies.

---

## Complete MVVM Flow

```text
Activity
   │
   ▼
ViewModel
   │
   ▼
Repository
   │
 ┌─┴─────────┐
 ▼           ▼
API        Database
```

Pure composition.

---

## Testing Example

```kotlin
class FakeRepository : LoginRepository
```

Inject fake implementation into ViewModel.

Very easy.

---

# 10. Best Practices ⭐⭐⭐⭐⭐

## ✅ Prefer Composition When

- Sharing behavior.
- Injecting dependencies.
- Runtime replacement.
- Testing.
- Clean Architecture.
- Multiple responsibilities.

---

## ✅ Use Inheritance When

- True "IS-A" relationship.
- Framework lifecycle classes.
- Polymorphic models.
- Base UI models (carefully).

---

## Android Examples

| Use Composition | Use Inheritance |
|-----------------|-----------------|
| ViewModel → Repository | ViewModel → AndroidX ViewModel |
| Repository → ApiService | Activity → ComponentActivity |
| Adapter → DiffUtil | Fragment → BottomSheetDialogFragment |
| Car → Engine | Exception hierarchy |

---

# 11. Common Pitfalls ⭐⭐⭐⭐

## Pitfall 1 — Inheritance for Code Reuse

Don't inherit just to reuse methods.

---

## Pitfall 2 — Deep Hierarchies

```text
Animal
  │
Dog
  │
PetDog
  │
GermanShepherd
  │
PoliceDog
```

Hard to maintain.

---

## Pitfall 3 — God Base Class

```kotlin
BaseActivity
```

1000+ lines.

Every screen inherits unnecessary functionality.

---

## Pitfall 4 — Composition Explosion

Don't inject 20 dependencies into one class.

Split responsibilities.

---

# 12. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Fundamentals

1. What is Inheritance?
2. What is Composition?
3. Difference between IS-A and HAS-A?
4. Why Composition is preferred over Inheritance?
5. Tight coupling vs loose coupling?

## Android

6. Why ViewModel has Repository?
7. Why Repository has ApiService?
8. Why Jetpack Compose uses composition?
9. Activity inheritance vs dependency composition?
10. Composition in MVVM?

---

# 📋 Cheat Sheet (Part 1)

## Inheritance

```kotlin
class Dog : Animal()
```

Relationship:

```text
Dog IS-A Animal
```

---

## Composition

```kotlin
class Car(
    val engine: Engine
)
```

Relationship:

```text
Car HAS-A Engine
```

---

## Android Composition

```text
Activity
   │
ViewModel
   │
Repository
   │
ApiService
```

---

## Decision Rule

| Ask Yourself | Choose |
|--------------|--------|
| IS-A? | Inheritance |
| HAS-A? | Composition |

---

# 📝 Revision Summary

In **Part 1** you learned:

- What Inheritance is.
- What Composition is.
- IS-A vs HAS-A relationships.
- Tight coupling vs loose coupling.
- Why modern Android favors Composition.
- Kotlin Car & Engine example.
- MVVM architecture uses Composition.
- Best practices and interview questions.

---

# Part 2 — Delegation, Strategy Pattern, Decorator Pattern & SOLID Principles

> This is one of the most important **Senior Android System Design + OOP** interview topics. Modern Android architecture uses **Composition + Delegation** everywhere: Jetpack Compose, Hilt, Retrofit, OkHttp, Navigation, Paging, Coroutines, WorkManager, and Kotlin itself.

---

# 📚 Table of Contents

14. Why Composition Wins in Large Android Apps
15. Strategy Pattern
16. Decorator Pattern
17. Delegation in Kotlin (`by`)
18. Interface Delegation
19. Property Delegation
20. Dependency Injection + Composition
21. Composition and SOLID Principles
22. Liskov Substitution Principle
23. Open Closed Principle
24. Real Android Examples
25. Best Practices
26. Common Anti Patterns
27. Senior Interview Questions
28. Cheat Sheet

---

# 🎯 Learning Goals

After this part you'll understand:

- Strategy Pattern using Composition.
- Decorator Pattern used in OkHttp and Compose.
- Kotlin Delegation (`by`) in depth.
- Property Delegation (`lazy`, `observable`, `vetoable`).
- How Hilt uses Composition.
- Why SOLID principles naturally encourage Composition.

---

# 14. Why Composition Wins in Large Android Apps ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Amazon App

Amazon has one product screen.

Features include:

```text
📦 Product Screen

├── Wishlist
├── Reviews
├── Ratings
├── Coupons
├── Payment
├── Recommendations
├── Inventory
├── Analytics
├── Share
└── Cart
```

Imagine implementing this using inheritance.

```text
BaseProductScreen
      │
PremiumProductScreen
      │
CouponProductScreen
      │
InventoryProductScreen
      │
AnalyticsProductScreen
```

Eventually you'll have dozens of subclasses.

This is called **Inheritance Explosion**.

---

## Composition Solution

```text
ProductScreen

HAS

WishlistManager
ReviewManager
CouponManager
AnalyticsTracker
CartManager
RecommendationEngine
```

Each feature becomes independent.

---

## Benefits

| Problem | Composition Solution |
|---------|----------------------|
| Huge hierarchy | Small reusable objects |
| Difficult testing | Mock dependencies |
| Code duplication | Shared components |
| Runtime flexibility | Swap implementations |

---

## Google Recommendation

Jetpack libraries avoid deep inheritance trees.

Instead they expose composable building blocks.

---

# 15. Strategy Pattern ⭐⭐⭐⭐⭐

## Definition

Strategy Pattern allows changing an algorithm or behavior **at runtime** without modifying the main class.

It is a perfect example of **Composition over Inheritance**.

---

## 🎭 Story — Google Maps Navigation

Google Maps offers multiple routes.

```text
Destination

🚗 Car
🚶 Walk
🚴 Cycle
🚇 Metro
```

Each navigation algorithm differs.

Don't inherit `GoogleMaps`.

Inject strategy.

---

## Step 1 — Strategy Interface

```kotlin
interface NavigationStrategy {

    fun navigate(destination: String)
}
```

---

## Step 2 — Implement Strategies

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

class BikeNavigation : NavigationStrategy {

    override fun navigate(destination: String) {
        println("Cycling to $destination")
    }
}
```

---

## Step 3 — Compose Strategy

```kotlin
class Navigator(
    private var strategy: NavigationStrategy
) {

    fun navigate(destination: String) {
        strategy.navigate(destination)
    }

    fun changeStrategy(
        newStrategy: NavigationStrategy
    ) {
        strategy = newStrategy
    }
}
```

---

## Runtime Change

```kotlin
val navigator = Navigator(CarNavigation())

navigator.navigate("Airport")

navigator.changeStrategy(WalkingNavigation())

navigator.navigate("Office")
```

Output

```text
Driving to Airport

Walking to Office
```

---

## Android Example — Login Providers

```text
LoginManager

HAS

GoogleLoginStrategy
FacebookLoginStrategy
EmailLoginStrategy
PhoneLoginStrategy
```

Runtime login selection.

---

## Payment Example

```text
PaymentManager

HAS

UPI
Credit Card
Wallet
NetBanking
Cash
```

Same manager.

Different strategies.

---

# 16. Decorator Pattern ⭐⭐⭐⭐⭐

## Definition

Decorator adds new behavior **without modifying the original class**.

Uses composition.

---

## 🎭 Story — Coffee Shop

Coffee

Add Milk.

Add Chocolate.

Add Caramel.

Each topping decorates coffee.

---

## Base Component

```kotlin
interface Coffee {

    fun price(): Int
}
```

---

## Basic Coffee

```kotlin
class BasicCoffee : Coffee {

    override fun price() = 100
}
```

---

## Decorator

```kotlin
class MilkDecorator(
    private val coffee: Coffee
) : Coffee {

    override fun price() =
        coffee.price() + 20
}
```

---

## Chocolate Decorator

```kotlin
class ChocolateDecorator(
    private val coffee: Coffee
) : Coffee {

    override fun price() =
        coffee.price() + 30
}
```

---

## Usage

```kotlin
val coffee =
    ChocolateDecorator(
        MilkDecorator(
            BasicCoffee()
        )
    )

println(coffee.price())
```

Output

```text
150
```

---

## Android Example — OkHttp Interceptors

```text
Request

↓

LoggingInterceptor

↓

AuthInterceptor

↓

CacheInterceptor

↓

NetworkInterceptor

↓

Server
```

Every interceptor decorates request.

---

## Compose Example

```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Blue)
    .clickable { }
```

Every Modifier decorates previous modifier.

One of the best real-world decorator examples.

---

# 17. Delegation in Kotlin (`by`) ⭐⭐⭐⭐⭐

## What is Delegation?

Delegation means one object forwards work to another object.

Kotlin provides first-class support.

---

## Without Delegation

```kotlin
class Printer {

    fun print(message: String) {
        println(message)
    }
}

class UserPrinter(
    private val printer: Printer
) {

    fun print(message: String) {
        printer.print(message)
    }
}
```

Boilerplate forwarding.

---

## Kotlin Delegation

```kotlin
interface Printer {

    fun print(message: String)
}

class ConsolePrinter : Printer {

    override fun print(message: String) {
        println(message)
    }
}

class UserPrinter(
    printer: Printer
) : Printer by printer
```

One line replaces boilerplate.

---

## Usage

```kotlin
val printer =
    UserPrinter(ConsolePrinter())

printer.print("Hello Kotlin")
```

Output

```text
Hello Kotlin
```

---

## Why It's Powerful

Kotlin generates forwarding methods automatically.

---

## JVM Visualization

```text
UserPrinter

↓

ConsolePrinter

↓

println()
```

---

# 18. Interface Delegation ⭐⭐⭐⭐⭐

## Story — Swiggy Notification Service

Swiggy sends notifications through multiple providers.

---

## Interface

```kotlin
interface NotificationSender {

    fun send(message: String)
}
```

---

## Providers

```kotlin
class FirebaseSender :
    NotificationSender {

    override fun send(message: String) {
        println("Firebase: $message")
    }
}

class SmsSender :
    NotificationSender {

    override fun send(message: String) {
        println("SMS: $message")
    }
}
```

---

## Delegation

```kotlin
class NotificationManager(
    sender: NotificationSender
) : NotificationSender by sender
```

---

## Usage

```kotlin
NotificationManager(FirebaseSender())
    .send("Order Delivered")

NotificationManager(SmsSender())
    .send("OTP Sent")
```

Behavior changes via composition.

---

## Android Example

```text
AnalyticsManager

HAS

FirebaseAnalytics
Mixpanel
MoEngage
CleverTap
```

Delegation chooses provider.

---

# 19. Property Delegation ⭐⭐⭐⭐⭐

Property delegation is another Kotlin feature built on composition.

---

## Lazy Delegation

```kotlin
val repository by lazy {
    UserRepository()
}
```

Repository created only when first used.

---

## Lifecycle Story

Login screen opens.

Repository isn't created until login button pressed.

Saves memory.

---

## Observable Delegation

```kotlin
var username by Delegates.observable("") { _, old, new ->
    println("$old -> $new")
}
```

Usage

```kotlin
username = "Vikash"
username = "Android"
```

Output

```text
 -> Vikash

Vikash -> Android
```

---

## Vetoable Delegation

```kotlin
var age by Delegates.vetoable(18) { _, _, new ->
    new >= 18
}
```

Usage

```kotlin
age = 25
age = 10
```

Second assignment rejected.

---

## Map Delegation

```kotlin
class User(
    map: Map<String, Any>
) {

    val name: String by map

    val age: Int by map
}
```

Useful with JSON.

---

## Android Example

Compose state.

```kotlin
var name by remember {
    mutableStateOf("")
}
```

`by` delegates property access to MutableState.

---

# 20. Dependency Injection + Composition ⭐⭐⭐⭐⭐

## Story — Food Delivery Kitchen

Restaurant doesn't cook everything itself.

It asks specialists.

---

## Without DI

```kotlin
class LoginViewModel {

    private val repository =
        LoginRepository()
}
```

Tightly coupled.

---

## With Composition

```kotlin
class LoginViewModel(
    private val repository: LoginRepository
)
```

Dependency injected.

---

## Hilt Example

```kotlin
@HiltViewModel
class LoginViewModel @Inject constructor(
    private val repository: LoginRepository
) : ViewModel()
```

Composition.

---

## Repository

```kotlin
class LoginRepository @Inject constructor(
    private val api: LoginApi,
    private val dao: UserDao
)
```

Repository composed of smaller services.

---

## Complete Architecture

```text
Activity

↓

ViewModel

↓

Repository

↓

API + Database + Preferences
```

No inheritance between layers.

---

## Testing

Inject fake implementation.

```kotlin
LoginViewModel(FakeRepository())
```

Very easy.

---

# 21. Composition and SOLID Principles ⭐⭐⭐⭐⭐

Composition naturally supports SOLID.

---

## Single Responsibility Principle

Instead of giant class:

```text
UserManager

Login
Register
Analytics
Storage
Notification
```

Split responsibilities.

```text
LoginService

NotificationService

AnalyticsTracker

UserRepository
```

Composed together.

---

## Dependency Inversion Principle

Depend on abstractions.

```kotlin
interface Analytics

class FirebaseAnalytics : Analytics

class MixpanelAnalytics : Analytics
```

ViewModel depends on interface.

---

## Interface Segregation Principle

Small interfaces.

```kotlin
interface Reader

interface Writer
```

Compose behaviors.

---

## Open Closed Principle

Add new implementations.

Don't modify existing class.

Strategy Pattern example.

---

# 22. Liskov Substitution Principle ⭐⭐⭐⭐⭐

## Definition

A child class should replace parent class **without breaking behavior**.

---

## Good Example

```kotlin
open class Bird {

    open fun fly() {}
}

class Sparrow : Bird()
```

Works.

---

## Bad Example

```kotlin
class Penguin : Bird() {

    override fun fly() {
        throw Exception()
    }
}
```

Breaks LSP.

---

## Better Design with Composition

```kotlin
interface FlyBehavior

class CanFly : FlyBehavior

class CannotFly : FlyBehavior

class Bird(
    private val flyBehavior: FlyBehavior
)
```

Now Penguin gets `CannotFly`.

No broken inheritance.

---

## Android Example

Instead of

```text
VideoPlayer

AudioPlayer

ImagePlayer
```

Compose playback behaviors.

---

# 23. Open Closed Principle ⭐⭐⭐⭐⭐

## Story — Payment Gateway

Need new payment methods.

---

## Bad Design

```kotlin
class PaymentManager {

    fun pay(type:String){
        when(type){
            "UPI" -> {}
            "CARD" -> {}
            "PAYPAL" -> {}
        }
    }
}
```

Modify manager every time.

---

## Composition Design

```kotlin
interface PaymentStrategy {
    fun pay()
}
```

Implement strategies.

No manager modification.

---

## Add Apple Pay

```kotlin
class ApplePayStrategy :
    PaymentStrategy
```

No existing code changes.

---

## Android Example

Notification providers.

Analytics providers.

Image loaders.

Storage providers.

---

# 24. Real Android Examples ⭐⭐⭐⭐⭐

## Jetpack Compose

Everything is composition.

```kotlin
Column {
    Header()
    ProfileCard()
    ActionButtons()
}
```

Small composables build UI.

---

## Retrofit

```text
Retrofit

HAS

ConverterFactory
CallAdapterFactory
OkHttpClient
```

---

## OkHttp

```text
OkHttpClient

HAS

Interceptors
Authenticator
Cache
CookieJar
```

Composition everywhere.

---

## RecyclerView

Adapter composes:

- ViewHolder
- DiffUtil
- ClickListener
- ImageLoader

---

## ViewModel

ViewModel composes:

- Repository
- UseCases
- Analytics
- Preferences

---

## Navigation

Navigator composes destinations instead of inheriting screens.

---

# 25. Best Practices ⭐⭐⭐⭐⭐

## ✅ Prefer Composition For

- Business logic.
- Services.
- Analytics.
- Payments.
- Authentication.
- Repositories.
- ViewModels.
- UseCases.
- Compose UI.

---

## ✅ Use Delegation When

- Forwarding interface behavior.
- Wrapping implementations.
- Reducing boilerplate.

---

## ✅ Use Decorator When

- Adding behavior dynamically.
- Logging.
- Authentication.
- Caching.
- UI modifiers.

---

# 26. Common Anti Patterns ⭐⭐⭐⭐

## God BaseActivity

1000+ lines.

Every screen inherits unnecessary code.

---

## Deep ViewModel Hierarchies

```text
BaseViewModel

↓

AuthViewModel

↓

LoginViewModel

↓

GoogleLoginViewModel
```

Hard to maintain.

---

## Fake Composition

Injecting 20 dependencies into one class.

Split into smaller services.

---

## Inheritance for Utilities

Don't inherit helper classes.

Inject them.

---

# 27. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Fundamentals

1. Composition vs Inheritance?
2. IS-A vs HAS-A?
3. Tight coupling vs loose coupling?
4. Why Google prefers composition?

## Kotlin

5. What is delegation?
6. Difference between interface delegation and property delegation?
7. What does `by` generate in bytecode?

## Design Patterns

8. Strategy pattern?
9. Decorator pattern?
10. Dependency Injection uses composition how?

## Android

11. How Compose follows composition?
12. OkHttp interceptor design?
13. ViewModel architecture?
14. Repository architecture?
15. Hilt and composition?

---

# 📋 Cheat Sheet (Part 2)

## Strategy Pattern

```kotlin
Navigator(
    CarNavigation()
)
```

Runtime behavior replacement.

---

## Decorator Pattern

```kotlin
Modifier
    .padding()
    .background()
    .clickable()
```

Layer behavior dynamically.

---

## Interface Delegation

```kotlin
class UserPrinter(
    printer: Printer
) : Printer by printer
```

---

## Property Delegation

```kotlin
val repo by lazy { ... }

var state by remember { mutableStateOf(...) }

var name by Delegates.observable(...)
```

---

## DI + Composition

```text
ViewModel

HAS Repository

Repository HAS ApiService

Repository HAS Dao
```

---

## SOLID Mapping

| SOLID | Composition Benefit |
|-------|----------------------|
| SRP | Split responsibilities |
| OCP | Add new strategies |
| LSP | Replace inheritance with behaviors |
| ISP | Small interfaces |
| DIP | Depend on abstractions |

---

# 📝 Revision Summary

In **Part 2** you learned:

- Why Composition scales better than Inheritance.
- Strategy Pattern.
- Decorator Pattern.
- Kotlin Delegation (`by`).
- Interface Delegation.
- Property Delegation (`lazy`, `observable`, `vetoable`).
- Dependency Injection through Composition.
- SOLID principles using Composition.
- LSP and OCP with real Android examples.
- Jetpack Compose, Retrofit, OkHttp and Hilt architecture patterns.

---
# Part 3 — Android Architecture: Composition in MVVM, Jetpack Compose, Repository, Hilt, RecyclerView & Coroutines

> This part connects **Composition over Inheritance** directly to **real Android development**. You'll see how Google-designed Jetpack libraries are built using composition instead of inheritance. This is the architecture used in **Google I/O samples, Now in Android, CRED, PhonePe, Uber, Flipkart, Swiggy, Zomato, and Meesho**.

---

# 📚 Table of Contents

23. Composition in MVVM Architecture
24. Repository Composition
25. UseCase Composition
26. Hilt & Dependency Injection
27. Jetpack Compose — Why It's Called Compose
28. Composition in UI Components
29. RecyclerView Composition
30. Navigation Composition
31. Room Database Composition
32. Retrofit & OkHttp Composition
33. Coroutine Composition
34. State Management with Composition
35. Modular Architecture with Composition
36. Testing Architecture with Composition
37. Best Practices
38. Common Pitfalls
39. Senior Interview Questions
40. Cheat Sheet

---

# 🎯 Learning Goals

After completing this part you'll understand how composition is used across every layer of a production Android application.

You'll be able to explain:

- Why MVVM uses composition.
- Why Hilt injects dependencies.
- Why Jetpack Compose abandoned XML inheritance.
- Why OkHttp Interceptors are decorators.
- Why Repository is composed of multiple data sources.
- Why Coroutines compose asynchronous workflows.

---

# 23. Composition in MVVM Architecture ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Swiggy Order Screen

When you open Swiggy's order screen, many independent systems work together.

```text
Order Screen

├── UI
├── Order ViewModel
├── Cart Repository
├── Payment Repository
├── Analytics Tracker
├── Notification Manager
├── Location Service
└── Network Monitor
```

Instead of one giant class inheriting everything, every responsibility is composed together.

---

## MVVM Architecture

```text
UI (Activity / Fragment / Compose)
             │
             ▼
        ViewModel
             │
      ┌──────┼─────────┐
      ▼      ▼         ▼
 Repository UseCases Analytics
      │
 ┌────┴─────┐
 ▼          ▼
Remote    Local
```

Everything communicates through composition.

---

## ViewModel Example

```kotlin
@HiltViewModel
class LoginViewModel @Inject constructor(
    private val loginUseCase: LoginUseCase,
    private val analyticsTracker: AnalyticsTracker,
    private val networkMonitor: NetworkMonitor
) : ViewModel() {

    fun login(email: String, password: String) {
        analyticsTracker.track("login_clicked")
    }
}
```

### Why This Is Composition

`LoginViewModel` **HAS**

- LoginUseCase
- AnalyticsTracker
- NetworkMonitor

It **IS NOT** any of them.

---

## Benefits

| Benefit | Example |
|---------|---------|
| Easy testing | Inject fake repository. |
| Independent features | Replace analytics provider. |
| Loose coupling | Swap network implementation. |
| Better scalability | Add logging without touching ViewModel. |

---

# 24. Repository Composition ⭐⭐⭐⭐⭐

## Story — Amazon Product Repository

Product data comes from multiple sources.

```text
ProductRepository

HAS

Remote API
Local Room
Memory Cache
Preference Manager
```

---

## Repository Implementation

```kotlin
class ProductRepository(
    private val api: ProductApi,
    private val dao: ProductDao,
    private val cache: MemoryCache<String, Product>,
    private val preferences: PreferenceManager
)
```

---

## Repository Responsibilities

```text
Repository

Remote Fetch

↓

Cache

↓

Database

↓

UI
```

---

## Offline First Pattern

```kotlin
class StoryRepository(
    private val remote: StoryApi,
    private val local: StoryDao
) {

    suspend fun getStories(): List<Story> {

        val cached = local.getStories()

        if (cached.isNotEmpty()) return cached

        val remoteStories = remote.getStories()

        local.insert(remoteStories)

        return remoteStories
    }
}
```

Composition makes this workflow easy.

---

## Why Not Inheritance?

```text
RemoteRepository

↓

CachedRepository

↓

OfflineRepository
```

Hard to maintain.

Composition keeps responsibilities independent.

---

# 25. UseCase Composition ⭐⭐⭐⭐⭐

## Story — PhonePe Money Transfer

Transfer money requires multiple operations.

```text
TransferMoneyUseCase

HAS

ValidateUserUseCase
CheckBalanceUseCase
TransferApi
TransactionLogger
NotificationSender
```

---

## Example

```kotlin
class TransferMoneyUseCase(
    private val validator: ValidateUserUseCase,
    private val balanceChecker: CheckBalanceUseCase,
    private val repository: PaymentRepository,
    private val logger: TransactionLogger
)
```

---

## Execution Flow

```text
Validate

↓

Check Balance

↓

Transfer

↓

Log Transaction

↓

Notify User
```

Each responsibility is independent.

---

## Why This Is Powerful

Reuse validation in:

- Send Money
- Pay Bills
- Recharge
- UPI Payment

No duplicated code.

---

# 26. Hilt & Dependency Injection ⭐⭐⭐⭐⭐

## Story — Restaurant Kitchen

Chef doesn't make ingredients.

Kitchen supplies ingredients.

---

## Hilt Provides Dependencies

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule {

    @Provides
    fun provideApi(): UserApi {
        TODO()
    }
}
```

---

## ViewModel Receives Dependencies

```kotlin
@HiltViewModel
class ProfileViewModel @Inject constructor(
    private val repository: UserRepository
) : ViewModel()
```

---

## Dependency Graph

```text
Hilt

↓

Api

↓

Repository

↓

ViewModel

↓

Compose Screen
```

Composition managed automatically.

---

## Multiple Implementations

```kotlin
interface AnalyticsTracker
```

Implementations

```kotlin
FirebaseAnalyticsTracker

MixpanelAnalyticsTracker

NoOpAnalyticsTracker
```

Swap implementation without modifying ViewModel.

---

## Testing

```kotlin
ProfileViewModel(
    FakeRepository()
)
```

Dependency Injection + Composition = Testability.

---

# 27. Jetpack Compose — Why It's Called Compose ⭐⭐⭐⭐⭐

## 🎭 Story — LEGO Blocks

Jetpack Compose UI is built from small reusable blocks.

```text
Profile Screen

Header()

UserCard()

StatsSection()

RecentOrders()

BottomActions()
```

Every UI element is composed together.

---

## Compose Screen

```kotlin
@Composable
fun ProfileScreen() {

    Column {

        Header()

        UserInfoCard()

        OrderHistory()

        LogoutButton()
    }
}
```

---

## Visualization

```text
Column

├── Header

├── UserInfo

├── Orders

└── Button
```

UI is assembled instead of inherited.

---

## XML World

```text
BaseActivity

↓

BaseFragment

↓

BaseLayout

↓

ChildLayout
```

Compose eliminates much of this hierarchy.

---

## Compose Philosophy

Everything is a function.

Functions compose UI.

---

# 28. Composition in UI Components ⭐⭐⭐⭐⭐

## Story — Instagram Feed Card

Feed card has many reusable widgets.

---

## Small Composables

```kotlin
@Composable
fun FeedCard(post: Post) {

    Column {

        UserHeader(post.user)

        PostImage(post.image)

        ActionBar(post)

        Caption(post.caption)
    }
}
```

---

## Reusable Building Blocks

`UserHeader()` reused in

- Feed.
- Story.
- Profile.
- Reels.

---

## Product Card Example

```kotlin
@Composable
fun ProductCard(product: Product) {

    Card {

        ProductImage(product)

        ProductPrice(product)

        BuyButton(product)
    }
}
```

---

## Modifier Composition

```kotlin
Modifier
    .padding(16.dp)
    .fillMaxWidth()
    .background(Color.White)
    .clickable { }
```

Each modifier decorates previous one.

Decorator Pattern + Composition.

---

# 29. RecyclerView Composition ⭐⭐⭐⭐⭐

## Story — Flipkart Home Screen

Each card contains different UI.

---

## Adapter Composition

```text
RecyclerView

HAS

Adapter

HAS

ViewHolder

HAS

ClickListener

HAS

ImageLoader
```

---

## Generic Adapter

```kotlin
class ProductAdapter(
    private val imageLoader: ImageLoader,
    private val clickListener: (Product) -> Unit
)
```

---

## ViewHolder

```kotlin
class ProductViewHolder(
    private val binding: ItemProductBinding,
    private val imageLoader: ImageLoader
)
```

Image loading delegated.

---

## Swipe Support

```text
RecyclerView

HAS

ItemTouchHelper

HAS

SwipeCallback
```

Behavior added through composition.

---

## DiffUtil

```text
Adapter

HAS

DiffUtil.ItemCallback
```

Independent comparison logic.

---

# 30. Navigation Composition ⭐⭐⭐⭐⭐

## Story — Banking App Navigation

Screens don't inherit each other.

Navigation coordinates screens.

---

## Navigator

```kotlin
class Navigator(
    private val navController: NavController
)
```

---

## Screen

```kotlin
navigator.navigate(ProfileDestination)
```

---

## Sealed Destinations

```kotlin
sealed interface Destination {

    data object Home : Destination

    data object Profile : Destination
}
```

Composable destinations.

---

## Deep Link Manager

```text
Navigator

HAS

DeepLinkParser

HAS

RouteBuilder
```

Independent navigation utilities.

---

# 31. Room Database Composition ⭐⭐⭐⭐⭐

## Story — WhatsApp Database

Database contains multiple DAOs.

---

## Database

```kotlin
@Database(...)
abstract class AppDatabase : RoomDatabase() {

    abstract fun userDao(): UserDao

    abstract fun chatDao(): ChatDao

    abstract fun storyDao(): StoryDao
}
```

Database composes DAOs.

---

## Repository Uses DAO

```kotlin
class ChatRepository(
    private val dao: ChatDao
)
```

---

## DAO Uses Entity

```text
Repository

↓

DAO

↓

Entity

↓

Database
```

Each layer isolated.

---

# 32. Retrofit & OkHttp Composition ⭐⭐⭐⭐⭐

## Story — API Request Journey

A request passes through multiple components.

---

## OkHttp Pipeline

```text
Request

↓

LoggingInterceptor

↓

AuthInterceptor

↓

RetryInterceptor

↓

CacheInterceptor

↓

Server
```

Decorator Pattern.

---

## Retrofit Composition

```kotlin
Retrofit.Builder()

.addConverterFactory(...)

.addCallAdapterFactory(...)

.client(okHttpClient)
```

Retrofit has converters and adapters.

---

## Auth Interceptor

```kotlin
class AuthInterceptor(
    private val tokenProvider: TokenProvider
)
```

Composition.

---

## Image Loader Example

Coil/Glide compose:

- Memory cache.
- Disk cache.
- Decoder.
- Fetcher.
- Transformation.

---

# 33. Coroutine Composition ⭐⭐⭐⭐⭐

## Story — Swiggy Checkout

Checkout performs many async operations.

---

## Compose Async Tasks

```kotlin
viewModelScope.launch {

    val user = async { repository.user() }

    val cart = async { repository.cart() }

    val coupons = async { repository.coupons() }

    awaitAll(user, cart, coupons)
}
```

---

## Flow Composition

```kotlin
combine(
    userFlow,
    cartFlow,
    couponFlow
){ user, cart, coupon ->

    CheckoutState(user, cart, coupon)
}
```

---

## Multiple Flows

```text
User Flow

Cart Flow

Coupon Flow

↓

Checkout Flow
```

Composition of asynchronous streams.

---

## Extension Composition

```kotlin
repository.userFlow()

    .map { }

    .filter { }

    .combine(...)
```

Each operator composes previous flow.

---

# 34. State Management with Composition ⭐⭐⭐⭐⭐

## UI State

```kotlin
data class ProfileUiState(
    val user: User? = null,
    val posts: List<Post> = emptyList(),
    val isLoading: Boolean = false,
    val error: String? = null
)
```

---

## ViewModel

```kotlin
private val _state =
    MutableStateFlow(ProfileUiState())
```

---

## Compose UI

```kotlin
val state by viewModel.state.collectAsState()
```

---

## State Breakdown

```text
Profile Screen

HAS

User State

Post State

Loading State

Error State
```

Smaller reusable state objects.

---

## MVI Example

```text
Intent

↓

Reducer

↓

State

↓

UI
```

Reducer composes new state.

---

# 35. Modular Architecture with Composition ⭐⭐⭐⭐⭐

## Story — CRED Android Project

Large Android apps are split into modules.

---

## Module Structure

```text
app

core-ui

core-network

core-database

feature-home

feature-profile

feature-payment

feature-settings
```

---

## Feature Module

```text
Feature Profile

HAS

Repository

UseCases

ViewModel

UI
```

---

## Core Module

```text
Core Network

HAS

Retrofit

OkHttp

Interceptors
```

---

## Benefits

- Independent compilation.
- Better ownership.
- Smaller APK modules.
- Easier testing.

---

# 36. Testing Architecture with Composition ⭐⭐⭐⭐⭐

## Story — Testing Login Screen

Replace dependencies.

---

## Fake Repository

```kotlin
class FakeLoginRepository :
    LoginRepository
```

---

## Inject Fake

```kotlin
val vm = LoginViewModel(
    FakeLoginRepository()
)
```

---

## Fake Analytics

```kotlin
class FakeAnalyticsTracker :
    AnalyticsTracker
```

---

## Unit Test

```kotlin
@Test
fun loginSuccess(){

    val vm = LoginViewModel(
        FakeRepository()
    )

    vm.login()

    assert(...)
}
```

No Android framework needed.

---

## Why Composition Helps Testing

| Dependency | Replace With |
|------------|--------------|
| Repository | FakeRepository |
| Analytics | FakeAnalytics |
| Network | FakeApi |
| DAO | FakeDao |

---

# 37. Best Practices ⭐⭐⭐⭐⭐

## ✅ Compose Small Components

- Header.
- Footer.
- Cards.
- Buttons.
- Dialogs.

---

## ✅ Inject Dependencies

Don't create them manually.

---

## ✅ Keep Repository Thin

Repository coordinates.

Business logic belongs in UseCases.

---

## ✅ Use Interfaces

Depend on abstractions.

---

## ✅ Compose State

Split UI state into reusable models.

---

# 38. Common Pitfalls ⭐⭐⭐⭐

## Pitfall 1 — Massive ViewModel

ViewModel with API, DB, Analytics and Validation.

Split responsibilities.

---

## Pitfall 2 — Repository Doing Everything

Repository shouldn't contain UI logic.

---

## Pitfall 3 — Giant Compose Screen

Break into reusable composables.

---

## Pitfall 4 — Too Many Dependencies

Constructor with 15 parameters.

Create service groups.

---

## Pitfall 5 — Deep Module Dependency

Keep dependency graph acyclic.

---

# 39. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## MVVM

1. Why ViewModel uses Repository?
2. Why Repository uses API + DAO?
3. Why UseCase layer exists?

## Compose

4. Why Jetpack Compose is called Compose?
5. Difference between XML inheritance and Compose composition?

## Hilt

6. How Hilt supports composition?
7. Why constructor injection is preferred?

## RecyclerView

8. Why Adapter composes ViewHolder?
9. Why DiffUtil is injected?

## Coroutines

10. What is Flow composition?
11. Combine vs Zip?
12. Why StateFlow works well with Compose?

---

# 📋 Cheat Sheet (Part 3)

## MVVM Composition

```text
Activity

↓

ViewModel

↓

UseCase

↓

Repository

↓

API + Room
```

---

## Compose UI

```kotlin
Column {

    Header()

    Body()

    Footer()
}
```

---

## Hilt

```kotlin
@HiltViewModel

@Inject constructor(...)
```

---

## Repository

```kotlin
Repository(
    api,
    dao,
    cache
)
```

---

## Retrofit

```text
Retrofit

HAS

OkHttp

HAS

Interceptors
```

---

## Flow

```kotlin
combine(flow1, flow2)
```

---

## State

```kotlin
MutableStateFlow

↓

StateFlow

↓

collectAsState()
```

---

# 📝 Revision Summary

In **Part 3** you learned:

- Composition in MVVM architecture.
- Repository composition.
- UseCase composition.
- Hilt dependency injection.
- Jetpack Compose composition philosophy.
- RecyclerView composition.
- Navigation composition.
- Room composition.
- Retrofit & OkHttp composition.
- Coroutine and Flow composition.
- State management.
- Modular architecture.
- Testing architecture using composition.

---
# Part 4 — JVM, Performance, Testing, Refactoring & Interview Mastery

> This is the **final and most advanced** part of the Composition vs Inheritance chapter. You'll learn how inheritance and composition behave on the JVM, memory layout, performance trade-offs, testing strategies, refactoring techniques used in large Android apps, and **60+ senior interview questions**.

---

# 📚 Table of Contents

41. JVM Memory Model
42. How Inheritance Works in JVM
43. How Composition Works in JVM
44. Performance Comparison
45. Memory Comparison
46. Testing Composition vs Inheritance
47. Refactoring Inheritance to Composition
48. Production Android Patterns
49. Anti Patterns in Android
50. Best Practices Checklist
51. 60+ Senior Android Interview Questions
52. Ultimate Cheat Sheet
53. Chapter Revision Summary

---

# 🎯 Learning Goals

After completing this part you'll understand:

- JVM implementation differences.
- Memory layout of composed objects.
- Performance implications.
- Why composition is easier to test.
- How to refactor legacy Android projects.
- Enterprise architecture recommendations.

---

# 41. JVM Memory Model ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Apartment Building

Imagine two apartment designs.

### Design A — Inheritance

```text
Penthouse

extends

Luxury Apartment

extends

Apartment
```

Every apartment carries everything from its parents.

---

### Design B — Composition

```text
Apartment

HAS

Kitchen

Bedroom

Bathroom

Balcony
```

Each room is a separate component.

---

## JVM Visualization

### Inheritance Object

```text
Dog Object

+-----------------------+
| Animal Fields         |
|-----------------------|
| name                  |
| age                   |
| weight                |
|-----------------------|
| Dog Fields            |
| breed                 |
| barkVolume            |
+-----------------------+
```

Parent fields become part of child object.

---

### Composition Object

```text
Car Object

+------------------------+
| engine reference ------|------+
| battery reference -----|---+  |
| wheel reference -------|--+|  |
+------------------------+  ||  |
                             ||  |
Engine Object                ||  |
Battery Object               ||  |
Wheel Object                 ++--+
```

Objects reference other objects.

---

## Key Difference

| Inheritance | Composition |
|-------------|------------|
| Fields copied into child object. | References stored. |
| Single object grows. | Multiple collaborating objects. |

---

# 42. How Inheritance Works in JVM ⭐⭐⭐⭐⭐

## Example

```kotlin
open class Animal(
    val name: String,
    val age: Int
)

class Dog(
    name: String,
    age: Int,
    val breed: String
) : Animal(name, age)
```

---

## Object Layout

```text
Dog Instance

+------------------------+
| Object Header          |
| Animal.name            |
| Animal.age             |
| Dog.breed              |
+------------------------+
```

One object contains parent and child fields.

---

## Method Dispatch

```kotlin
open fun speak()

override fun speak()
```

Runtime uses **virtual dispatch**.

---

## Virtual Method Table

```text
Dog

VTable

eat()

sleep()

speak() ---> Dog implementation
```

Used for polymorphism.

---

## Benefits

- Fast dynamic dispatch.
- Runtime polymorphism.
- Shared object layout.

---

## Drawbacks

- Strong coupling.
- Parent changes affect children.
- Deep hierarchies increase complexity.

---

# 43. How Composition Works in JVM ⭐⭐⭐⭐⭐

## Example

```kotlin
class Engine {

    fun start() {}
}

class Car(
    private val engine: Engine
)
```

---

## Memory Layout

```text
Car Object

+-----------------------+
| Engine Reference -----|----+
+-----------------------+    |
                             |
Engine Object                |
+-----------------------+    |
| Object Header         |    |
| Engine Fields         |<---+
+-----------------------+
```

Separate heap objects.

---

## Method Call

```kotlin
car.drive()

↓

engine.start()

↓

Engine.start()
```

Simple object delegation.

---

## Why Flexible?

Replace Engine reference anytime.

```kotlin
Car(PetrolEngine())

Car(ElectricEngine())
```

---

## Android Example

```text
ViewModel

Repository Ref

Analytics Ref

Network Ref
```

All replaceable.

---

# 44. Performance Comparison ⭐⭐⭐⭐⭐

## Story — Flipkart Home Screen

Thousands of product cards load.

Should composition slow the app?

Let's analyze.

---

## Method Dispatch Cost

| Operation | Cost |
|-----------|------|
| Direct Method | Fast |
| Virtual Method | Very Fast |
| Delegated Method | Very Fast |
| Reflection | Slow |

Composition adds one object lookup.

Negligible in most Android apps.

---

## Example

### Inheritance

```kotlin
dog.speak()
```

Virtual dispatch.

---

### Composition

```kotlin
speaker.speak()
```

Reference lookup + method call.

---

## Reality

Modern JVM aggressively optimizes delegation through JIT.

Performance difference is rarely meaningful.

---

## Android Recommendation

Optimize readability and maintainability first.

Micro-optimization rarely matters here.

---

# 45. Memory Comparison ⭐⭐⭐⭐⭐

## Inheritance Memory

```text
Dog Object

Animal Data

Dog Data
```

One object grows larger.

---

## Composition Memory

```text
Dog

↓

Tail Object

↓

Collar Object

↓

FoodBehavior Object
```

Multiple small objects.

---

## Trade-Off Table

| Scenario | Winner |
|----------|--------|
| Few reusable behaviors | Composition |
| Shared immutable hierarchy | Inheritance |
| Runtime swapping | Composition |
| Large enterprise app | Composition |

---

## Memory Cost

Composition introduces references.

References are typically inexpensive compared to maintainability benefits.

---

## Android Example

A ViewModel holding references to Repository and AnalyticsTracker is normal.

---

# 46. Testing Composition vs Inheritance ⭐⭐⭐⭐⭐

## Story — Login Feature Testing

Need to test login without real API.

---

## Inheritance Version

```kotlin
class LoginViewModel :
    LoginRepository()
```

Repository tightly coupled.

Hard to replace.

---

## Composition Version

```kotlin
class LoginViewModel(
    private val repository: LoginRepository
)
```

Inject fake.

---

## Fake Repository

```kotlin
class FakeLoginRepository :
    LoginRepository {

    override suspend fun login(...) =
        LoginResponse(success = true)
}
```

---

## Unit Test

```kotlin
@Test
fun loginSuccess(){

    val vm = LoginViewModel(
        FakeLoginRepository()
    )

    vm.login("a","b")

    assertTrue(vm.state.value.success)
}
```

---

## Fake Analytics

```kotlin
class FakeAnalytics :
    AnalyticsTracker
```

Track events without Firebase.

---

## Why Composition Wins

| Testing Task | Composition |
|--------------|------------|
| Fake API | Easy |
| Fake DAO | Easy |
| Fake Analytics | Easy |
| Fake Preferences | Easy |

---

# 47. Refactoring Inheritance to Composition ⭐⭐⭐⭐⭐

## 🎭 Legacy Android Story

Many legacy Android projects have giant `BaseActivity`.

---

## Before

```text
BaseActivity

500+ methods

↓

LoginActivity

↓

ProfileActivity

↓

SettingsActivity
```

Every screen inherits everything.

---

## Problems

- Unused methods.
- Tight coupling.
- Huge maintenance cost.

---

## Step 1 — Extract Responsibilities

```text
BaseActivity

↓

AnalyticsManager

PermissionManager

ToolbarController

LoadingController
```

---

## Step 2 — Inject Components

```kotlin
class LoginActivity : AppCompatActivity() {

    private val analytics = AnalyticsManager()

    private val loading = LoadingController()
}
```

---

## Step 3 — Reusable Controllers

```kotlin
loading.show()

analytics.track(...)
```

Independent utilities.

---

## Real Android Refactor

### Before

```kotlin
BaseFragment
```

Contains

- Snackbar
- Navigation
- Loading
- Permissions
- Analytics

---

### After

```text
Fragment

HAS

SnackbarManager

Navigator

PermissionHandler

LoadingStateController
```

Much cleaner.

---

# 48. Production Android Patterns ⭐⭐⭐⭐⭐

## Pattern 1 — Analytics Composition

```text
AnalyticsManager

↓

Firebase

Mixpanel

Crashlytics
```

---

## Pattern 2 — Image Loading Composition

```text
ImageLoader

↓

MemoryCache

DiskCache

Decoder

Fetcher
```

Coil and Glide use composition extensively.

---

## Pattern 3 — Authentication Composition

```text
AuthManager

↓

GoogleAuth

PhoneAuth

EmailAuth

FacebookAuth
```

Strategy Pattern.

---

## Pattern 4 — Notification Composition

```text
NotificationManager

↓

PushProvider

SMSProvider

EmailProvider
```

---

## Pattern 5 — Logger Composition

```text
Logger

↓

ConsoleLogger

FirebaseLogger

FileLogger
```

Swap implementation per build type.

---

## Pattern 6 — Payment Composition

```text
PaymentManager

↓

UPI

Card

Wallet

NetBanking
```

Runtime behavior replacement.

---

# 49. Anti Patterns in Android ⭐⭐⭐⭐⭐

## ❌ Massive BaseActivity

Symptoms

- Hundreds of utility methods.
- Shared mutable state.
- Hidden lifecycle behavior.

Prefer independent controllers.

---

## ❌ BaseViewModel Doing Everything

Contains

- Navigation
- Analytics
- API
- Preferences
- Error Handling

Split services.

---

## ❌ Inheritance Explosion

```text
Button

PrimaryButton

RoundedPrimaryButton

LoadingRoundedPrimaryButton

AnimatedLoadingRoundedPrimaryButton
```

Use composable configuration instead.

---

## ❌ Utility Inheritance

```kotlin
class NetworkUtils : BaseUtils()
```

Utilities should not inherit.

---

## ❌ God Repository

Repository containing

- API
- DB
- Preferences
- Analytics
- Validation

Break into services.

---

# 50. Best Practices Checklist ⭐⭐⭐⭐⭐

## ✅ Prefer Composition For

- Services.
- Managers.
- Repositories.
- UseCases.
- Analytics.
- Permissions.
- UI Controllers.
- Image Loaders.
- Payment Systems.

---

## ✅ Keep Inheritance For

- Android lifecycle classes.
- Polymorphic models.
- Sealed hierarchies.
- Framework extension points.

---

## ✅ Inject Interfaces

```kotlin
interface AnalyticsTracker

class FirebaseTracker : AnalyticsTracker
```

---

## ✅ Compose Small Objects

Prefer many focused collaborators instead of one massive parent class.

---

## Decision Matrix

| Question | Recommendation |
|----------|----------------|
| Need runtime behavior change? | Composition |
| True IS-A hierarchy? | Inheritance |
| Need dependency injection? | Composition |
| Need reusable UI pieces? | Composition |
| Extending Android framework class? | Inheritance |

---

# 51. 60+ Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Fundamentals

1. Composition vs Inheritance?
2. IS-A vs HAS-A?
3. Tight coupling vs loose coupling?
4. Why composition is preferred?

## SOLID

5. Composition and SRP?
6. Composition and DIP?
7. Composition and OCP?
8. LSP example with Penguin?

## Kotlin

9. Delegation vs Composition?
10. `by` keyword internals?

## Android

11. Why ViewModel has Repository?
12. Why Repository has ApiService?
13. Why Hilt uses constructor injection?
14. Why Compose uses composition?
15. Why OkHttp uses interceptors?

## Performance

16. JVM memory layout.
17. Virtual dispatch.
18. Object references.
19. Testing differences.
20. Refactoring BaseActivity.

(Continue practicing scenario-based questions rather than memorizing definitions.)

---

# 📋 Ultimate Cheat Sheet

## Inheritance

```kotlin
class Dog : Animal()
```

Relationship

```text
Dog IS-A Animal
```

---

## Composition

```kotlin
class Car(
    val engine: Engine
)
```

Relationship

```text
Car HAS-A Engine
```

---

## Strategy Pattern

```kotlin
Navigator(
    CarNavigation()
)
```

Runtime behavior replacement.

---

## Decorator Pattern

```kotlin
Modifier
    .padding()
    .background()
    .clickable()
```

Behavior layering.

---

## Delegation

```kotlin
class UserPrinter(
    printer: Printer
) : Printer by printer
```

Automatic forwarding.

---

## Dependency Injection

```text
ViewModel

↓

Repository

↓

Api + DAO + Cache
```

---

## Compose Philosophy

```kotlin
Column {

    Header()

    Content()

    Footer()
}
```

Everything is composed.

---

## Refactoring Rule

```text
BaseActivity

↓

AnalyticsManager

PermissionManager

ToolbarManager
```

Extract collaborators.

---

# 📝 Complete Chapter Revision Summary

## What You Learned in Chapter 18

| Part | Topics Covered |
|------|----------------|
| **Part 1** | Fundamentals, IS-A vs HAS-A, Tight vs Loose Coupling, Car & Engine Example. |
| **Part 2** | Strategy Pattern, Decorator Pattern, Kotlin Delegation, Property Delegation, SOLID Principles. |
| **Part 3** | MVVM, Repository, Hilt, Compose, RecyclerView, Navigation, Room, Retrofit, Coroutines, Modular Architecture. |
| **Part 4** | JVM Memory Model, Performance, Testing, Refactoring, Production Patterns, Anti Patterns, Interview Mastery. |

---

# 🎯 Android Interview Takeaways

After completing this chapter, you should be able to explain:

- When to choose **Composition** over **Inheritance**.
- Why Jetpack Compose is fundamentally based on composition.
- How Hilt, Retrofit, Room, Flow, RecyclerView, and MVVM use composition.
- JVM memory and dispatch differences between inheritance and composition.
- How to refactor legacy BaseActivity/BaseFragment architectures into composable, testable components.

---
