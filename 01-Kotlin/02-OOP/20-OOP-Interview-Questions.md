# 🎯 OOP Interview Questions Masterclass (Android Interview Bible 2026 Edition)

> **Chapter 20 — Part 1 : OOP Fundamentals Interview (Google • Uber • Amazon • Microsoft • PhonePe • CRED Edition)**

> This chapter is **not normal notes**. It is designed as a **real interview handbook** for Android developers with **8+ years of experience**. Every question includes:
>
> - Interviewer's Question
> - Expected Answer (Junior / Mid / Senior Level)
> - Android Example
> - JVM / Kotlin Internals
> - Common Mistakes
> - Follow-up Questions
> - Interview Tips

---

# 📚 Table of Contents (Part 1)

1. How to Answer OOP Questions in Interviews
2. OOP Fundamentals
3. Class vs Object
4. Constructor Interview Questions
5. Init Block Interview Questions
6. Object Memory Model (Heap vs Stack)
7. Four Pillars of OOP
8. Encapsulation Deep Dive
9. Abstraction Deep Dive
10. Polymorphism Deep Dive
11. Object Lifecycle in Android
12. Kotlin Object Model
13. JVM Internals for OOP
14. Trick Questions
15. Rapid Fire Questions
16. Part 1 Cheat Sheet

---

# 🎯 Learning Goals

After completing Part 1 you'll be able to answer:

- What is OOP?
- Why Kotlin is an Object-Oriented Language?
- Difference between Class and Object.
- Constructors vs Init Block.
- Heap vs Stack memory.
- Encapsulation in Android.
- Abstraction in Android.
- Runtime vs Compile-time Polymorphism.
- JVM object creation process.

---

# 1. How to Answer OOP Questions in Android Interviews ⭐⭐⭐⭐⭐

## 🎭 Real Story — Google Android Interview

The interviewer asks:

> **"Explain Encapsulation."**

Most candidates answer with a textbook definition.

**Bad Answer**

> Encapsulation means wrapping data and methods together.

Interview ends quickly.

---

## Good Answer Structure (STAR Method for Technical Interviews)

Always answer in **5 steps**.

| Step | What to Say |
|------|-------------|
| **1. Definition** | Explain concept in one sentence. |
| **2. Why** | Why does this concept exist? |
| **3. Kotlin Example** | Small code example. |
| **4. Android Example** | ViewModel, Repository, Compose, etc. |
| **5. JVM / Performance** | Internal working if interviewer goes deeper. |

---

## Example

### Interviewer

Explain **Encapsulation**.

### Ideal Senior Answer

> Encapsulation is the process of hiding an object's internal state and exposing only controlled operations. It helps maintain invariants, improves security, and reduces coupling.

Then show Kotlin.

```kotlin
class Wallet {

    private var balance = 5000

    fun withdraw(amount: Int) {
        require(amount <= balance)
        balance -= amount
    }

    fun currentBalance() = balance
}
```

Android example.

```kotlin
private val _state = MutableStateFlow(LoginState())
val state = _state.asStateFlow()
```

JVM explanation.

> `private` members are enforced through JVM access flags and Kotlin compiler visibility rules.

---

## ⭐ Android Interview Tip

Never stop after definition.

Always connect with:

- MVVM
- Compose
- Coroutines
- StateFlow
- Repository
- Hilt

---

# 2. What is Object-Oriented Programming? ⭐⭐⭐⭐⭐

## 🎭 Story — LEGO City

Imagine building a city using LEGO.

You have reusable pieces.

- Car
- House
- School
- Hospital

Every building is made from reusable blocks.

OOP works similarly.

---

## Definition

Object-Oriented Programming is a programming paradigm that models software using **objects**, where objects combine **state (properties)** and **behavior (functions)**.

---

## Four Pillars

| Pillar | Meaning |
|---------|---------|
| Encapsulation | Hide implementation. |
| Abstraction | Show only required functionality. |
| Inheritance | IS-A relationship. |
| Polymorphism | Multiple behaviors through same interface. |

---

## Kotlin Example

```kotlin
class User(
    val name: String,
    val email: String
) {

    fun display() {
        println(name)
    }
}
```

---

## Android Example

```kotlin
data class Story(
    val id: String,
    val title: String
)
```

Object represents application data.

---

## Why Android Uses OOP?

Android apps contain thousands of objects.

- Activity
- Fragment
- ViewModel
- Repository
- DAO
- Retrofit Service
- Worker

Each object has a responsibility.

---

## JVM Internals

Every Kotlin object becomes a JVM object allocated on Heap.

---

## Follow-up Questions

- Is Compose OOP?
- Is Kotlin purely OOP?
- Difference between OOP and Functional Programming?

---

# 3. Is Kotlin Purely Object-Oriented? ⭐⭐⭐⭐⭐

## Short Answer

**No. Kotlin is Multi-Paradigm.**

Supports:

- Object-Oriented Programming.
- Functional Programming.
- Declarative Programming (Compose).
- Reactive Programming.

---

## Example

### OOP

```kotlin
class User
```

### Functional

```kotlin
val sum = list.sumOf { it }
```

### Declarative

```kotlin
Text("Hello")
```

---

## Interview Tip

Mention Kotlin supports higher-order functions, lambdas, immutable collections, and object declarations.

---

# 4. Class vs Object ⭐⭐⭐⭐⭐

## 🎭 Story — House Blueprint

Blueprint.

↓

Actual House.

---

## Definition

| Class | Object |
|-------|--------|
| Blueprint. | Real instance. |
| Doesn't occupy heap. | Occupies heap. |
| Defines properties. | Holds values. |

---

## Kotlin Example

```kotlin
class Car(
    val brand: String
)

val car = Car("Tesla")
```

---

## Memory Diagram

```text
Stack

car ----------+

              |

Heap          |

+--------------------------+

| Car Object               |

| brand = Tesla            |

+--------------------------+
```

Reference on stack.

Object on heap.

---

## Android Example

```kotlin
class LoginViewModel

val vm = LoginViewModel()
```

---

## Follow-up

Can a class exist without an object?

Yes.

Companion/Object declarations create singleton automatically.

---

# 5. Constructor Interview Questions ⭐⭐⭐⭐⭐

## Primary Constructor

```kotlin
class User(
    val name: String,
    val age: Int
)
```

---

## Secondary Constructor

```kotlin
class User {

    constructor(name: String)

    constructor(name: String, age: Int)
}
```

---

## Interview Question

When should you use secondary constructor?

### Expected Answer

Rarely.

Prefer default parameters.

---

## Default Parameter Example

```kotlin
class User(
    val name: String,
    val age: Int = 18
)
```

Cleaner API.

---

## Android Example

```kotlin
data class Story(
    val id: String,
    val bookmarked: Boolean = false
)
```

---

## JVM Internals

Default constructors generate synthetic methods.

---

## Trick Question

Can constructor contain logic?

Yes.

But initialization logic belongs in `init`.

---

# 6. Init Block Deep Dive ⭐⭐⭐⭐⭐

## Story — Employee Joining Company

Constructor receives data.

Init validates.

---

## Example

```kotlin
class Employee(
    val age: Int
) {

    init {
        require(age >= 18)
    }
}
```

---

## Multiple Init Blocks

```kotlin
init { }

init { }
```

Executed in declaration order.

---

## Execution Order

```text
Property Initialization

↓

Init Block 1

↓

Init Block 2

↓

Secondary Constructor
```

---

## Android Example

Repository validation.

---

## Common Mistake

Heavy API calls inside init.

Never perform asynchronous work inside init.

---

# 7. Heap vs Stack Memory ⭐⭐⭐⭐⭐

## 🎭 Story — Apartment Address

Stack stores apartment number.

Heap stores apartment itself.

---

## Memory Diagram

```text
Stack

user ----------+

count = 5      |

               |

Heap           |

+---------------------------+

| User Object               |

| name = Vikash             |

| age = 33                  |

+---------------------------+
```

---

## Stack Stores

- Local variables.
- References.
- Primitive values.

---

## Heap Stores

- Objects.
- Arrays.
- Collections.
- Strings.

---

## Android Example

RecyclerView creates ViewHolder objects on Heap.

---

## Interview Question

Why does `null` not occupy Heap?

Reference becomes null.

Object becomes eligible for GC.

---

# 8. Object Creation Lifecycle ⭐⭐⭐⭐⭐

## Step-by-Step JVM Process

```text
new User()

↓

Class Loaded

↓

Memory Allocated

↓

Fields Initialized

↓

Init Executed

↓

Constructor Executed

↓

Reference Returned
```

---

## Kotlin Example

```kotlin
val user = User("Vikash")
```

---

## JVM Internals

Object contains:

- Header.
- Class Pointer.
- Properties.

---

## Object Header

Stores metadata.

HashCode.

Synchronization info.

GC metadata.

---

## Android Example

Every Compose state object follows same lifecycle.

---

# 9. Encapsulation Interview Deep Dive ⭐⭐⭐⭐⭐

## Story — Bank Locker

Customers cannot open vault.

Only locker API.

---

## Definition

Hide internal state.

Expose controlled operations.

---

## Kotlin Example

```kotlin
class BankAccount {

    private var balance = 1000

    fun deposit(amount: Int) {
        balance += amount
    }
}
```

---

## Android Example

ViewModel exposes immutable StateFlow.

```kotlin
private val _stories = MutableStateFlow(emptyList())

val stories = _stories.asStateFlow()
```

---

## Why Important?

- Data safety.
- Thread safety.
- Immutable UI state.

---

## Common Mistakes

```kotlin
var state = MutableStateFlow(...)
```

Anyone can modify.

---

## Senior Answer

Expose immutable interface.

Keep mutable implementation private.

---

# 10. Abstraction Interview Deep Dive ⭐⭐⭐⭐⭐

## Story — TV Remote

Remote hides hardware complexity.

---

## Definition

Hide implementation.

Expose capability.

---

## Interface Example

```kotlin
interface PaymentGateway {

    suspend fun pay(amount: Double)
}
```

---

## Android Example

Repository interface.

```kotlin
interface StoryRepository
```

Implementations.

- Firebase.
- REST API.
- Local Database.

---

## Benefits

Loose coupling.

Testing.

Swappable implementation.

---

## Interview Follow-up

Difference between abstraction and encapsulation?

Table included.

| Encapsulation | Abstraction |
|--------------|-------------|
| Hide data. | Hide complexity. |
| Uses visibility. | Uses interfaces/abstract classes. |

---

# 11. Polymorphism Interview Deep Dive ⭐⭐⭐⭐⭐

## Story — Google Pay Payment Options

Same Pay button.

Different payment method.

---

## Compile-Time Polymorphism

Function overloading.

```kotlin
fun login(email:String)

fun login(phone:Int)
```

---

## Runtime Polymorphism

Method overriding.

```kotlin
open class Animal

class Dog : Animal()
```

---

## Android Example

RecyclerView Adapter.

Different ViewHolder types.

---

## Compose Example

Same composable behaves differently based on state.

---

## JVM Internals

Uses Virtual Method Table.

Dynamic Dispatch.

---

## Trick Question

Does Kotlin support operator overloading polymorphism?

Yes.

---

# 12. Four Pillars Combined ⭐⭐⭐⭐⭐

## Story — WhatsApp Chat

| Feature | OOP Pillar |
|----------|------------|
| Message hidden internally. | Encapsulation |
| SendMessage interface. | Abstraction |
| ImageMessage extends Message. | Inheritance |
| send() behaves differently. | Polymorphism |

---

## Android Architecture Mapping

```text
Compose UI

↓

ViewModel

↓

Repository

↓

API / DB
```

Every layer demonstrates OOP.

---

# 13. Kotlin Object Model ⭐⭐⭐⭐⭐

## Everything is Object?

Mostly yes.

```kotlin
Int

String

List

Function

Lambda
```

---

## Primitive Optimization

```kotlin
Int
```

Compiled to JVM primitive when possible.

---

## Nullable Int

```kotlin
Int?
```

Becomes boxed Integer object.

---

## Value Classes Optimization

No wrapper object when possible.

---

## Interview Question

Difference between Int and Integer?

Explain boxing.

---

# 14. JVM Internals for OOP ⭐⭐⭐⭐⭐

## Memory Layout

```text
Object Header

↓

Properties

↓

Padding
```

---

## Virtual Dispatch

```text
Animal.speak()

↓

Dog.speak()
```

Runtime lookup.

---

## Static Dispatch

Extension functions.

Compile-time resolution.

---

## Garbage Collection

Objects without references become eligible.

---

## Android Example

Activity destroyed.

References removed.

GC cleans memory.

---

# 15. Common OOP Trick Questions ⭐⭐⭐⭐⭐

### Q1 Can object exist without class?

Yes.

Anonymous object expressions.

---

### Q2 Can class exist without object?

Yes.

Blueprint only.

---

### Q3 Does constructor return object?

No.

Constructor initializes object.

---

### Q4 Is init constructor?

No.

Compiler-generated initialization block.

---

### Q5 Are Strings objects?

Yes.

Immutable JVM objects.

---

### Q6 Why data class cannot be abstract?

Compiler-generated methods require instantiation semantics.

---

### Q7 Why sealed class constructor is protected?

Restricts subclassing outside hierarchy.

---

### Q8 Can interface have state?

Only abstract properties.

No backing field.

---

# 16. Rapid Fire Interview Questions (30 Questions)

| Question | One-Line Answer |
|----------|-----------------|
| What is OOP? | Programming with objects. |
| Object? | Runtime instance of class. |
| Class? | Blueprint for object. |
| Heap? | Stores objects. |
| Stack? | Stores references and local variables. |
| Constructor? | Initializes object. |
| Init block? | Runs during object initialization. |
| Encapsulation? | Hide internal state. |
| Abstraction? | Hide implementation complexity. |
| Inheritance? | IS-A relationship. |
| Composition? | HAS-A relationship. |
| Polymorphism? | One interface, many implementations. |
| Overloading? | Compile-time polymorphism. |
| Overriding? | Runtime polymorphism. |
| Dynamic Dispatch? | Runtime method lookup. |
| Static Dispatch? | Compile-time resolution. |
| Boxing? | Primitive → Object. |
| Unboxing? | Object → Primitive. |
| Nullable Int? | Boxed Integer. |
| Value Class? | Optimized wrapper. |
| Companion Object? | Static-like singleton. |
| Object Declaration? | Singleton object. |
| Anonymous Object? | One-time object implementation. |
| Interface? | Contract. |
| Abstract Class? | Partial implementation. |
| Data Class? | Immutable model. |
| Enum Class? | Fixed constants. |
| Sealed Class? | Restricted hierarchy. |
| GC? | Reclaims unreachable objects. |
| JVM Object Header? | Metadata for object. |

---

# 📋 Part 1 Cheat Sheet (10-Minute Revision)

## OOP Core Concepts

| Concept | Android Example |
|----------|-----------------|
| Class | ViewModel class |
| Object | ViewModel instance |
| Constructor | Inject Repository |
| Init Block | Validation |
| Encapsulation | MutableStateFlow + StateFlow |
| Abstraction | Repository Interface |
| Inheritance | Activity extends ComponentActivity |
| Composition | ViewModel HAS Repository |
| Polymorphism | RecyclerView ViewHolder |
| Heap | Activity object |
| Stack | Activity reference |

---

## Android Examples You Should Mention in Interviews

| Interview Topic | Best Android Example |
|----------------|----------------------|
| Encapsulation | StateFlow |
| Abstraction | Repository |
| Polymorphism | RecyclerView Adapter |
| Inheritance | Fragment |
| Composition | ViewModel + Repository |
| Object Declaration | Room Singleton |
| Companion Object | Factory Method |
| Init Block | Input Validation |

---

# 📝 Part 1 Revision Summary

You completed:

- ✅ OOP Fundamentals
- ✅ Class vs Object
- ✅ Constructors
- ✅ Init Blocks
- ✅ Heap vs Stack
- ✅ Object Lifecycle
- ✅ Encapsulation
- ✅ Abstraction
- ✅ Polymorphism
- ✅ Kotlin Object Model
- ✅ JVM Object Creation
- ✅ 30 Rapid Fire Questions

---

# 🎯 Part 2 — Inheritance, Interfaces, Abstract Classes & Visibility Modifiers Interview Masterclass

> **Android Interview Bible 2026 — Chapter 20 (Part 2)**

> This section covers one of the most frequently asked interview topics in Android development. Every company—from Google and Uber to PhonePe, CRED, Flipkart, Amazon, and Microsoft—asks questions around inheritance, interfaces, polymorphism, visibility, and Kotlin-specific OOP behavior.

---

# 📚 Table of Contents

1. Inheritance Interview Masterclass
2. Method Overriding Deep Dive
3. Super Keyword
4. Constructor Order in Inheritance
5. Runtime vs Compile-Time Polymorphism
6. Interface Interview Masterclass
7. Functional Interfaces (SAM)
8. Multiple Interface Conflict
9. Abstract Classes Deep Dive
10. Interface vs Abstract Class
11. Visibility Modifiers Deep Dive
12. Internal Modifier (Android Modules)
13. Protected Modifier (Android Examples)
14. JVM Internals of Inheritance
15. Android Architecture Questions
16. Code Output Round
17. Debug Round
18. Rapid Fire Questions
19. Cheat Sheet

---

# 🎯 Learning Goals

After this part you'll confidently answer:

- Explain inheritance with JVM internals.
- Interface vs Abstract Class.
- Why Kotlin classes are final by default.
- Multiple inheritance in Kotlin.
- `super` keyword.
- `internal` modifier in multi-module Android apps.
- Android architecture interview scenarios.

---

# 1. Inheritance Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — Swiggy Delivery Partners

Imagine Swiggy has multiple delivery partners.

Every partner has common behavior:

- Login
- Accept Order
- Reach Restaurant

But different delivery types have different behaviors.

- Bike Delivery
- Cycle Delivery
- Walking Delivery

Instead of rewriting everything, Swiggy creates a base class.

---

## Interview Question

> What is Inheritance?

### Senior-Level Answer

Inheritance allows a class to reuse and extend the behavior of another class, creating an **IS-A relationship**. Kotlin supports **single class inheritance** and **multiple interface inheritance**.

---

## Kotlin Example

```kotlin
open class DeliveryPartner(
    val name: String
) {

    open fun deliver() {
        println("$name delivers food.")
    }
}

class BikePartner(name: String) : DeliveryPartner(name) {

    override fun deliver() {
        println("$name delivers using bike.")
    }
}
```

---

## Android Example

```kotlin
class LoginActivity : ComponentActivity()
```

`LoginActivity IS-A ComponentActivity`

---

## IS-A vs HAS-A

| Relationship | Example |
|--------------|---------|
| IS-A | Activity IS-A Context |
| HAS-A | ViewModel HAS Repository |

---

## Why Kotlin Uses `open`

Unlike Java:

```kotlin
class User
```

Cannot inherit.

Need:

```kotlin
open class User
```

---

## Why Final by Default?

### Interview Tip ⭐⭐⭐⭐⭐

Kotlin makes classes final because:

- Better optimization.
- Prevent accidental inheritance.
- Encourage composition over inheritance.
- Improves API stability.

---

# 2. Constructor Order in Inheritance ⭐⭐⭐⭐⭐

## 🎭 Story — Apartment Construction

Before decorating a penthouse, the building must exist.

Parent constructor always runs first.

---

## Example

```kotlin
open class Animal {

    init {
        println("Animal Created")
    }
}

class Dog : Animal() {

    init {
        println("Dog Created")
    }
}
```

---

## Output

```text
Animal Created
Dog Created
```

---

## JVM Execution Order

```text
Memory Allocation

↓

Parent Properties

↓

Parent init

↓

Parent Constructor

↓

Child Properties

↓

Child init

↓

Child Constructor
```

---

## Android Example

```kotlin
ComponentActivity

↓

AppCompatActivity

↓

MainActivity
```

Lifecycle begins from parent.

---

## Interview Follow-up

Why is parent initialized first?

Parent fields are required before child initialization.

---

# 3. Method Overriding Deep Dive ⭐⭐⭐⭐⭐

## Definition

Runtime polymorphism.

---

## Example

```kotlin
open class Animal {

    open fun sound() = "Animal"
}

class Dog : Animal() {

    override fun sound() = "Dog"
}
```

---

## Runtime Dispatch

```kotlin
val animal: Animal = Dog()

animal.sound()
```

Output:

```text
Dog
```

---

## Android Example

```kotlin
override fun onCreate(...)
```

Framework invokes overridden lifecycle methods.

---

## Rules

| Rule | Kotlin |
|------|--------|
| Parent method must be `open`. | ✅ |
| Child method uses `override`. | ✅ |
| Final override possible. | ✅ |

---

## Final Override

```kotlin
override final fun sound() {}
```

No further overriding.

---

# 4. `super` Keyword Interview ⭐⭐⭐⭐⭐

## Story — Calling Parents Before Children

Child customizes behavior but still wants parent's implementation.

---

## Example

```kotlin
open class Animal {

    open fun eat() {
        println("Animal Eating")
    }
}

class Dog : Animal() {

    override fun eat() {
        super.eat()
        println("Dog Eating")
    }
}
```

---

## Output

```text
Animal Eating
Dog Eating
```

---

## Constructor Super

```kotlin
class Dog : Animal("Tom")
```

Pass constructor arguments.

---

## Android Example

```kotlin
override fun onCreate(...) {

    super.onCreate(...)

    setContent {}
}
```

Always call parent lifecycle unless intentionally skipped.

---

## Interview Trick

Can we call `super` inside init?

Yes, parent constructor already executed before init.

---

# 5. Runtime vs Compile-Time Polymorphism ⭐⭐⭐⭐⭐

## Story — Google Pay Payment

One Pay button.

Different payment method selected.

---

## Compile-Time Polymorphism

Function Overloading.

```kotlin
fun login(email: String)

fun login(phone: Long)
```

Compiler chooses method.

---

## Runtime Polymorphism

Method Overriding.

```kotlin
Animal

↓

Dog

↓

Cat
```

Runtime chooses implementation.

---

## Android Examples

| Compile-Time | Runtime |
|--------------|--------|
| `Text(text="")` overloads. | Activity lifecycle overrides. |
| Retrofit builder overloads. | RecyclerView ViewHolder binding. |

---

## JVM Internals

Compile-time dispatch:

Direct invocation.

Runtime dispatch:

Virtual table lookup.

---

# 6. Interface Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — UPI Payment Gateway

Every payment method follows same contract.

---

## Interface Example

```kotlin
interface PaymentGateway {

    suspend fun pay(amount: Double)
}
```

---

## Implementations

```kotlin
class UpiPayment : PaymentGateway

class CardPayment : PaymentGateway

class WalletPayment : PaymentGateway
```

---

## Android Example

```kotlin
interface StoryRepository
```

Implementations:

- FirebaseRepository
- ApiRepository
- RoomRepository

---

## Why Interface?

Loose coupling.

Testing.

Dependency Injection.

---

## Interface Can Have

| Feature | Kotlin |
|---------|--------|
| Functions | ✅ |
| Default functions | ✅ |
| Abstract properties | ✅ |
| Private helper functions | ✅ |

---

## Interface Default Method

```kotlin
interface Logger {

    fun info() {
        println("Info")
    }
}
```

---

# 7. Functional Interface (SAM) ⭐⭐⭐⭐⭐

## Definition

Interface with one abstract method.

---

## Example

```kotlin
fun interface ClickListener {

    fun onClick()
}
```

---

## Lambda Usage

```kotlin
val listener = ClickListener {

    println("Clicked")
}
```

---

## Android Example

Button click listeners.

Compose callbacks.

Coroutine callbacks.

---

## Why Useful?

Cleaner APIs.

No anonymous object required.

---

## JVM Internals

Compiler generates synthetic implementation.

---

# 8. Multiple Interface Conflict ⭐⭐⭐⭐⭐

## Story — Smart Watch

Watch has:

- Clock.
- Fitness Tracker.

Both define `start()`.

---

## Example

```kotlin
interface Camera {

    fun start() {
        println("Camera")
    }
}

interface Music {

    fun start() {
        println("Music")
    }
}
```

---

## Conflict Resolution

```kotlin
class SmartWatch : Camera, Music {

    override fun start() {
        super<Camera>.start()
        super<Music>.start()
    }
}
```

---

## Output

```text
Camera
Music
```

---

## Android Example

Compose modifiers implementing multiple interfaces.

---

## Interview Question

Why Kotlin doesn't support multiple class inheritance?

Diamond Problem.

---

# 9. Diamond Problem ⭐⭐⭐⭐⭐

## Visualization

```text
        Animal
       /      \
   Bird      Mammal
       \      /
       Bat
```

Ambiguous implementation.

---

## Kotlin Solution

Single class inheritance.

Multiple interface inheritance.

Explicit override required.

---

## Interview Tip

Mention JVM doesn't support multiple class inheritance.

---

# 10. Abstract Class Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — Food Delivery Workflow

Every delivery partner has same algorithm.

Some steps vary.

---

## Example

```kotlin
abstract class DeliveryPartner {

    fun login() {}

    abstract fun transport()
}
```

---

## Child

```kotlin
class BikePartner : DeliveryPartner() {

    override fun transport() {
        println("Bike")
    }
}
```

---

## Android Example

```kotlin
abstract class BaseWorker
```

Custom workers inherit.

---

## Abstract Can Have

| Feature | Kotlin |
|---------|--------|
| Constructor | ✅ |
| State | ✅ |
| Functions | ✅ |
| Abstract methods | ✅ |

---

## Why Abstract Class?

Share implementation.

---

# 11. Interface vs Abstract Class ⭐⭐⭐⭐⭐

## Interview Favorite

| Feature | Interface | Abstract Class |
|----------|-----------|----------------|
| Constructor | ❌ | ✅ |
| State | Abstract property only | Full state |
| Multiple inheritance | ✅ | ❌ |
| Default methods | ✅ | ✅ |
| Visibility | Public by default | All supported |
| Use Case | Contract | Partial implementation |

---

## Android Examples

| Interface | Abstract Class |
|-----------|---------------|
| Repository | RecyclerView.ViewHolder |
| AnalyticsTracker | CoroutineWorker |
| PaymentGateway | Fragment |

---

## Senior Answer

Use interface for capabilities.

Use abstract class for shared implementation.

---

# 12. Visibility Modifier Masterclass ⭐⭐⭐⭐⭐

## Four Modifiers

| Modifier | Scope |
|----------|------|
| public | Everywhere |
| internal | Module only |
| protected | Subclasses |
| private | Class/File |

---

## Example

```kotlin
class User {

    private val password = ""

    internal val token = ""

    protected open fun validate() {}

    val name = ""
}
```

---

## Android Use Cases

| Modifier | Android |
|----------|----------|
| private | MutableStateFlow |
| internal | Feature module utilities |
| protected | BaseFragment methods |
| public | ViewModel API |

---

# 13. `internal` Modifier Deep Dive ⭐⭐⭐⭐⭐

## Story — Multi Module Banking App

Modules:

```text
app/

core/

network/

feature-home/

feature-payment/
```

---

## Internal Example

```kotlin
internal class TokenManager
```

Accessible only inside module.

---

## Why Important?

Large Android apps use internal heavily.

Improves encapsulation across modules.

---

## Interview Question

Difference between internal and public?

Public crosses module boundary.

Internal doesn't.

---

# 14. `protected` Modifier Deep Dive ⭐⭐⭐⭐⭐

## Story — Parent Shares Secret Only with Children

---

## Example

```kotlin
open class Animal {

    protected val age = 10
}
```

Child can access.

Outside cannot.

---

## Android Example

```kotlin
abstract class BaseFragment {

    protected fun showSnackbar() {}
}
```

Only subclasses use helper.

---

## Common Mistake

Protected is **not package-private**.

---

# 15. JVM Internals of Inheritance ⭐⭐⭐⭐⭐

## Memory Layout

```text
Dog Object

+-----------------------+

Animal.name

Animal.age

Dog.breed

Dog.weight

+-----------------------+
```

Parent fields become child fields.

---

## Virtual Table

```text
Animal

↓

Dog

↓

VTable

eat()

sleep()

sound()
```

Runtime dispatch.

---

## Why Overriding Works?

Method pointer replaced in virtual table.

---

## Interview Question

Difference between virtual and static dispatch?

Extension functions use static dispatch.

Overridden methods use virtual dispatch.

---

# 16. Android Architecture Scenario Questions ⭐⭐⭐⭐⭐

## Q1 Why ViewModel Doesn't Inherit Repository?

Expected Answer.

Composition preferred.

```kotlin
ViewModel(
    private val repository: Repository
)
```

---

## Q2 Why Repository Uses Interface?

Testing.

Multiple implementations.

Dependency inversion.

---

## Q3 Why Activity Inherits ComponentActivity?

Framework lifecycle.

Shared implementation.

---

## Q4 Why Compose Uses Composition More Than Inheritance?

Composable UI tree.

Modifier decoration.

Reusable functions.

---

# 17. Code Output Round ⭐⭐⭐⭐⭐

## Question 1

```kotlin
open class A {

    init {
        println("A Init")
    }
}

class B : A() {

    init {
        println("B Init")
    }
}

fun main() {
    B()
}
```

### Output

```text
A Init
B Init
```

---

## Question 2

```kotlin
open class Animal {

    open fun sound() = "Animal"
}

class Dog : Animal() {

    override fun sound() = "Dog"
}

fun main() {

    val a: Animal = Dog()

    println(a.sound())
}
```

Output

```text
Dog
```

Explain VTable.

---

## Question 3

```kotlin
interface A {

    fun hello() {
        println("A")
    }
}

interface B {

    fun hello() {
        println("B")
    }
}

class Test : A, B {

    override fun hello() {
        super<A>.hello()
    }
}
```

Output

```text
A
```

---

# 18. Debug Round ⭐⭐⭐⭐⭐

## Find Design Problem

```kotlin
class UserRepository : ApiService()
```

### Issue

Repository should use composition.

Not inheritance.

---

## Memory Leak

```kotlin
object ContextHolder {

    lateinit var context: Context
}
```

Stores Activity.

Leak.

Use ApplicationContext.

---

## Visibility Bug

```kotlin
var state = MutableStateFlow(...)
```

Anyone can mutate.

Expose immutable StateFlow.

---

## Interface Bug

```kotlin
interface Logger {

    var tag: String
}
```

Backing field not allowed.

Need implementation.

---

# 19. Rapid Fire Questions (40 Questions)

| Question | Answer |
|----------|--------|
| Why classes are final? | Safety and optimization. |
| IS-A relationship? | Inheritance. |
| HAS-A relationship? | Composition. |
| Diamond problem? | Multiple inheritance ambiguity. |
| Kotlin solution? | Interfaces only. |
| `open` keyword? | Allows inheritance. |
| `override` keyword? | Runtime polymorphism. |
| `super` keyword? | Parent implementation access. |
| Constructor order? | Parent first. |
| Init order? | Properties → init → constructor. |
| Interface state? | No backing field. |
| Abstract constructor? | Allowed. |
| Multiple interfaces? | Supported. |
| Multiple classes? | Not supported. |
| SAM? | Single Abstract Method. |
| Internal modifier? | Module visibility. |
| Protected modifier? | Visible in subclasses. |
| Private modifier? | Class/File only. |
| Public modifier? | Everywhere. |
| VTable? | Runtime dispatch table. |

---

# 📋 Part 2 Cheat Sheet (10-Minute Revision)

## Inheritance Cheat Sheet

| Topic | One-Line Interview Answer |
|-------|----------------------------|
| Inheritance | IS-A relationship. |
| Composition | HAS-A relationship. |
| `open` | Makes class inheritable. |
| `override` | Runtime polymorphism. |
| `super` | Calls parent implementation. |
| Constructor Order | Parent → Child. |
| Final Class | Default in Kotlin. |

---

## Interface Cheat Sheet

| Topic | Android Example |
|-------|------------------|
| Interface | Repository Contract |
| Default Method | Logger |
| Functional Interface | Click Listener |
| Multiple Interface | Camera + Music |

---

## Visibility Cheat Sheet

| Modifier | Android Usage |
|----------|---------------|
| private | MutableStateFlow |
| internal | Feature Module |
| protected | BaseFragment |
| public | Repository API |

---

## Android Interview Mapping

| Interview Topic | Best Android Example |
|----------------|----------------------|
| Inheritance | ComponentActivity |
| Interface | StoryRepository |
| Abstract Class | CoroutineWorker |
| Protected | BaseFragment |
| Internal | Multi-module apps |
| Runtime Polymorphism | RecyclerView Adapter |
| Compile-time Polymorphism | Function Overloading |
| Diamond Problem | Multiple Interfaces |

---

# 📝 Part 2 Revision Summary

You completed:

- ✅ Inheritance Deep Dive
- ✅ Constructor Execution Order
- ✅ Method Overriding
- ✅ `super` Keyword
- ✅ Runtime vs Compile-Time Polymorphism
- ✅ Interface Masterclass
- ✅ Functional Interfaces (SAM)
- ✅ Multiple Interface Conflict
- ✅ Diamond Problem
- ✅ Abstract Classes
- ✅ Interface vs Abstract Class
- ✅ Visibility Modifiers
- ✅ `internal` and `protected`
- ✅ JVM Virtual Dispatch
- ✅ Android Architecture Scenarios
- ✅ Code Output Round
- ✅ Debug Round
- ✅ 40 Rapid Fire Questions

---

# 🚀 Part 3 — Advanced Kotlin OOP Interview Masterclass (Senior Android Engineer Edition)

> **Android Interview Bible 2026 — Chapter 20 (Part 3)**

> This part covers the **most frequently asked Kotlin-specific OOP interview questions** for Android developers with **5–10+ years of experience**. Google, Uber, PhonePe, CRED, Microsoft, Flipkart, Meesho, Razorpay, Swiggy, and Amazon often ask deep questions about Data Classes, Sealed Classes, Delegation, Value Classes, Extension Functions, Generics, and Kotlin compiler behavior.

---

# 📚 Table of Contents

1. Data Class Interview Masterclass
2. Enum Class Interview Masterclass
3. Sealed Class Interview Masterclass
4. Object Declaration Interview
5. Companion Object Interview
6. Nested vs Inner Class Interview
7. Object Expression Interview
8. Delegation Interview (`by`, lazy, observable, vetoable)
9. Value Class Interview
10. Extension Function Interview
11. Generics Interview Masterclass
12. Composition vs Inheritance Interview
13. Kotlin Design Pattern Questions
14. Code Output Round
15. Debug Round
16. Rapid Fire Questions
17. Ultimate Cheat Sheet

---

# 🎯 Learning Goals

After this part you'll confidently answer:

- Data class internals.
- copy(), equals(), hashCode(), componentN().
- Enum vs Sealed vs Object.
- Companion object vs Object declaration.
- Nested vs Inner memory leaks.
- Delegation internals.
- Value class optimization.
- Extension function dispatch.
- Reified generics.
- Type erasure.
- Composition vs inheritance interview scenarios.

---

# 1. Data Class Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — WhatsApp Message Model

Every WhatsApp message has:

- id
- sender
- text
- timestamp

The model mostly stores data.

Perfect use case for **data class**.

---

## Interview Question

> What is a Data Class in Kotlin?

### Perfect Senior Answer

A data class is a special Kotlin class primarily designed to hold immutable state. The compiler automatically generates useful methods such as `equals()`, `hashCode()`, `copy()`, `toString()`, and `componentN()`.

---

## Example

```kotlin
data class Message(
    val id: String,
    val sender: String,
    val text: String
)
```

---

## Compiler Generates

```kotlin
equals()

hashCode()

copy()

toString()

component1()

component2()

component3()
```

---

## Android Example

```kotlin
data class Story(
    val id: String,
    val title: String,
    val bookmarked: Boolean
)
```

Used in Room, Retrofit, Compose State.

---

## JVM Generated Methods

| Method | Purpose |
|--------|---------|
| equals | Structural equality |
| hashCode | Collections |
| copy | Immutable updates |
| componentN | Destructuring |

---

## Why Data Classes Are Immutable?

Not mandatory.

But recommended.

```kotlin
val title: String
```

instead of mutable `var`.

---

## `copy()` Interview Questions ⭐⭐⭐⭐⭐

### Story — Instagram Like Button

Need updated object without modifying original.

```kotlin
val updated =
    post.copy(likes = post.likes + 1)
```

---

### Why Better Than Mutable Objects?

Immutable state.

Compose detects state changes.

StateFlow emits new object.

---

## Destructuring Interview

```kotlin
val (id, sender, text) = message
```

Compiler uses:

```kotlin
component1()
component2()
component3()
```

---

## Android Compose Example

```kotlin
val (title, bookmarked) = story
```

Readable UI.

---

## Trick Question

Can Data Class inherit another Data Class?

❌ No.

Can inherit normal class.

---

## Common Mistakes

```kotlin
data class User(
    var age: Int
)
```

Mutable state can introduce bugs.

---

# 2. Enum Class Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — Order Status

An order can only be:

- Pending
- Preparing
- Delivered
- Cancelled

Fixed set.

Use Enum.

---

## Example

```kotlin
enum class OrderStatus {

    PENDING,

    PREPARING,

    DELIVERED,

    CANCELLED
}
```

---

## Android Example

```kotlin
enum class ThemeMode {

    LIGHT,

    DARK,

    SYSTEM
}
```

---

## Enum Properties

```kotlin
enum class Priority(
    val color: String
){
    HIGH("Red"),
    LOW("Green")
}
```

---

## Enum Functions

```kotlin
enum class NetworkState{

    WIFI;

    fun isConnected() = true
}
```

---

## JVM Internals

Each enum constant is singleton.

```text
Priority.HIGH

↓

Single Object
```

---

## Enum vs Int Constants

| Enum | Int |
|------|-----|
| Type Safe | ❌ |
| Readable | ❌ |
| Exhaustive | ❌ |

---

## Android Example

Room stores enums using TypeConverter.

---

# 3. Sealed Class Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — Login Screen

Possible UI states.

- Idle.
- Loading.
- Success.
- Error.

Unknown states not allowed.

---

## Example

```kotlin
sealed interface LoginUiState {

    data object Idle : LoginUiState

    data object Loading : LoginUiState

    data class Success(
        val user: User
    ) : LoginUiState

    data class Error(
        val message: String
    ) : LoginUiState
}
```

---

## Why Sealed?

Compiler knows all subclasses.

---

## Exhaustive `when`

```kotlin
when(state){

    Idle -> {}

    Loading -> {}

    Success -> {}

    Error -> {}
}
```

No else needed.

---

## Android Example

Compose UI State.

Paging LoadState.

Navigation Destinations.

---

## Enum vs Sealed ⭐⭐⭐⭐⭐

| Enum | Sealed |
|------|--------|
| Constants only | Objects + Data |
| No payload | Payload allowed |
| Singleton values | Different object types |
| Fixed constants | Fixed hierarchy |

---

## JVM Internals

Sealed hierarchy metadata stored by compiler.

---

## Interview Trick

Why `sealed interface` introduced?

Supports multiple inheritance.

---

# 4. Object Declaration Interview ⭐⭐⭐⭐⭐

## 🎭 Story — Firebase Analytics

Need one analytics manager.

Singleton.

---

## Example

```kotlin
object AnalyticsManager {

    fun track(event: String){}
}
```

---

## Android Example

```kotlin
object Constants
```

---

## Thread Safety

Object initialized lazily.

Thread-safe.

---

## JVM Internals

Compiler creates:

```text
AnalyticsManager.INSTANCE
```

Singleton field.

---

## When To Use?

- Analytics.
- Logger.
- Database holder.
- Constants.
- Cache.

---

## When Not To Use?

Global mutable state.

Context references.

---

# 5. Companion Object Interview ⭐⭐⭐⭐⭐

## Story — Static Factory

Need factory methods.

---

## Example

```kotlin
class User private constructor(){

    companion object{

        fun createGuest() = User()
    }
}
```

---

## Android Example

```kotlin
Fragment.newInstance(...)
```

---

## Companion Implements Interface

```kotlin
companion object : Factory<User>
```

Interesting interview question.

---

## Companion vs Object

| Companion | Object |
|-----------|--------|
| Belongs to class | Independent singleton |
| One per class | Standalone |
| Access via ClassName | Access directly |

---

# 6. Nested vs Inner Class Interview ⭐⭐⭐⭐⭐

## 🎭 Story — Shopping Cart

Cart contains coupon calculator.

Nested helper doesn't need cart.

Inner helper needs cart.

---

## Nested

```kotlin
class Cart{

    class Coupon
}
```

No outer reference.

---

## Inner

```kotlin
class Cart{

    inner class Coupon
}
```

Has outer reference.

---

## Memory Diagram

### Nested

```text
Cart

Coupon
```

Independent.

---

### Inner

```text
Coupon

↓

Cart Reference
```

---

## Android Memory Leak ⭐⭐⭐⭐⭐

Never create long-lived inner class holding Activity.

Example:

Handler.

Runnable.

Timer.

---

## Interview Question

Difference in JVM?

Nested → Static nested class.

Inner → Synthetic outer reference.

---

# 7. Object Expression Interview ⭐⭐⭐⭐⭐

## Story — RecyclerView Click Listener

Need one temporary implementation.

---

## Example

```kotlin
button.setOnClickListener(
    object : View.OnClickListener{

        override fun onClick(v: View?) {}
    }
)
```

---

## Anonymous Object

No reusable class.

---

## Android Example

Callbacks.

Listeners.

BroadcastReceiver.

---

## Object Expression vs Lambda

| Object | Lambda |
|---------|--------|
| Multiple methods | One method |
| Has state | Limited |
| Interface/Class | Functional Interface |

---

## JVM Internals

Compiler generates anonymous class.

---

# 8. Delegation Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — CEO Delegates Work

CEO doesn't code.

Delegates to Engineering Manager.

---

## Class Delegation

```kotlin
interface Printer

class ConsolePrinter : Printer
```

---

## Delegation

```kotlin
class UserPrinter(
    printer: Printer
) : Printer by printer
```

---

## Compiler Generated Code

Methods forwarded automatically.

---

## Android Example

Analytics providers.

Storage providers.

Navigation providers.

---

## Property Delegation

### lazy

```kotlin
val database by lazy {

    createDatabase()
}
```

---

### observable

```kotlin
var name by Delegates.observable("")
```

State change callback.

---

### vetoable

Reject invalid values.

```kotlin
var age by Delegates.vetoable(18){_,_,new->

    new >=18
}
```

---

## Compose Delegation ⭐⭐⭐⭐⭐

```kotlin
var text by remember {

    mutableStateOf("")
}
```

Compiler delegates state object.

---

## Interview Tip

Difference between lazy and remember?

| lazy | remember |
|------|----------|
| Kotlin delegate | Compose memory |
| JVM object | Composition memory |

---

# 9. Value Class Interview Masterclass ⭐⭐⭐⭐⭐

## Story — UserId Wrapper

Need type safety.

No runtime overhead.

---

## Example

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

---

## Benefits

- Type safety.
- No allocation (usually).
- Better APIs.

---

## Android Example

```kotlin
UserId

StoryId

ChapterId

OrderId
```

Avoid mixing strings.

---

## JVM Optimization

Sometimes compiled as primitive/reference directly.

---

## Boxing Cases

| Scenario | Boxing |
|----------|--------|
| Generic | ✅ |
| Nullable | ✅ |
| Interface | ✅ |
| Direct usage | ❌ |

---

## Interview Trick

Why nullable value class allocates object?

Need wrapper for nullability.

---

# 10. Extension Function Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — Add Feature to Existing Class

Need capitalize() for String.

Can't modify String.

---

## Example

```kotlin
fun String.capitalizeWords(): String
```

---

## Android Example

```kotlin
fun Context.showToast(message: String)
```

---

## RecyclerView Example

```kotlin
fun RecyclerView.addDivider()
```

---

## Compose Example

```kotlin
fun Modifier.shadowBorder()
```

---

## Static Dispatch ⭐⭐⭐⭐⭐

```kotlin
fun Animal.sound()

fun Dog.sound()
```

Extension chosen by reference type.

---

## Trick Question

Do extensions override member functions?

No.

Member always wins.

---

## JVM Internals

Extension compiled as static function.

---

# 11. Generics Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — Swiggy Food Box

One container.

Different food types.

---

## Generic Class

```kotlin
class Box<T>(
    val value: T
)
```

---

## Generic Function

```kotlin
fun <T> print(item: T)
```

---

## Variance

### out

Producer.

```kotlin
List<out Animal>
```

---

### in

Consumer.

```kotlin
Consumer<in Dog>
```

---

## PECS Rule

Producer Extends.

Consumer Super.

---

## Reified Generics

```kotlin
inline fun <reified T> Gson.fromJson(...)
```

---

## Why Inline Needed?

Type erased otherwise.

---

## Type Erasure ⭐⭐⭐⭐⭐

```kotlin
List<Int>

List<String>
```

Runtime:

```text
List<Object>
```

Type removed.

---

## Android Example

Retrofit.

Room.

Navigation.

Serialization.

---

# 12. Composition vs Inheritance Interview ⭐⭐⭐⭐⭐

## Story — Tesla Car

Car HAS Engine.

Not IS Engine.

---

## Interview Question

When should you prefer composition?

Expected answer:

- Loose coupling.
- Testing.
- Runtime replacement.
- Dependency injection.

---

## Android Example

```kotlin
ViewModel(
    repository
)
```

Composition.

---

## When Use Inheritance?

Framework lifecycle classes.

---

## Follow-up

Compose favors composition.

---

# 13. Kotlin Design Pattern Interview ⭐⭐⭐⭐⭐

## Mapping

| Pattern | Android Example |
|---------|----------------|
| Singleton | Room |
| Factory | ViewModelFactory |
| Builder | Retrofit.Builder |
| Adapter | RecyclerView.Adapter |
| Decorator | Modifier |
| Observer | StateFlow |
| Strategy | Payment |
| State | UIState |
| Command | Navigation |
| Proxy | Retrofit |

---

## Interview Question

Which design pattern is Modifier?

Decorator.

---

# 14. Code Output Round ⭐⭐⭐⭐⭐

## Data Class Equality

```kotlin
data class User(val name:String)

fun main(){

    println(User("A")==User("A"))
}
```

Output:

```text
true
```

Explain structural equality.

---

## Object Declaration

```kotlin
object Logger

fun main(){

    println(Logger===Logger)
}
```

Output:

```text
true
```

Singleton.

---

## Extension Dispatch

```kotlin
open class Animal

class Dog:Animal()

fun Animal.name()="Animal"

fun Dog.name()="Dog"

fun main(){

    val a:Animal=Dog()

    println(a.name())
}
```

Output:

```text
Animal
```

Static dispatch.

---

## Reified Output

Explain why works only inline.

---

# 15. Debug Round ⭐⭐⭐⭐⭐

## Mutable Data Class Bug

```kotlin
data class User(var age:Int)
```

Issue:

HashMap bugs.

Compose recomposition bugs.

---

## Inner Class Leak

```kotlin
inner class Handler
```

Holding Activity.

Leak.

---

## Extension Override Bug

Extension not overriding member.

---

## Generic Cast Bug

Unsafe cast.

Use reified/check.

---

## Value Class Nullable Bug

Unexpected boxing.

---

# 16. Rapid Fire Questions (50 Questions)

| Question | Answer |
|----------|--------|
| Data class generated methods? | equals, hashCode, copy, toString, componentN. |
| `copy()`? | Immutable clone. |
| Enum vs Sealed? | Constants vs hierarchy. |
| Object Declaration? | Singleton. |
| Companion Object? | Static-like object. |
| Nested Class? | No outer reference. |
| Inner Class? | Has outer reference. |
| Object Expression? | Anonymous object. |
| Delegation keyword? | by |
| lazy delegate? | Lazy initialization. |
| observable? | Callback on change. |
| vetoable? | Validate before assignment. |
| Value Class? | Inline wrapper. |
| Extension dispatch? | Static. |
| Member vs Extension? | Member wins. |
| Generic variance? | in/out |
| Type Erasure? | Generic type removed at runtime. |
| Reified? | Runtime generic type access. |
| Composition? | HAS-A |
| Inheritance? | IS-A |

---

# 📋 Part 3 Cheat Sheet (10-Minute Revision)

## Kotlin OOP Cheat Sheet

| Concept | Android Example |
|---------|-----------------|
| Data Class | API Model |
| Enum | ThemeMode |
| Sealed | UI State |
| Object | AnalyticsManager |
| Companion | Fragment.newInstance() |
| Nested | Helper Class |
| Inner | RecyclerView ViewHolder with outer ref |
| Delegation | remember { mutableStateOf() } |
| Value Class | UserId |
| Extension | Context.showToast() |
| Reified | Gson.fromJson() |

---

## Android Interview Mapping

| Interview Topic | Best Example |
|----------------|-------------|
| Data Class | Compose State |
| Enum | Settings Screen |
| Sealed | Login State |
| Delegation | Compose State |
| Extension | Modifier Extensions |
| Value Class | StoryId Wrapper |
| Generics | Repository Base Class |
| Composition | ViewModel HAS Repository |

---

# 📝 Part 3 Revision Summary

You completed:

- ✅ Data Class Deep Dive
- ✅ Enum Class
- ✅ Sealed Class
- ✅ Object Declaration
- ✅ Companion Object
- ✅ Nested vs Inner Classes
- ✅ Object Expressions
- ✅ Delegation (`by`, lazy, observable, vetoable)
- ✅ Value Classes
- ✅ Extension Functions
- ✅ Generics (Variance, Reified, Type Erasure)
- ✅ Composition vs Inheritance Interview
- ✅ Kotlin Design Pattern Mapping
- ✅ Code Output Round
- ✅ Debug Round
- ✅ 50 Rapid Fire Questions

---
# 🚀 Part 4 — Android OOP & Architecture Interview Masterclass (Senior Android Engineer Edition)

> **Android Interview Bible 2026 — Chapter 20 (Part 4)**

> This is the **most important interview section** for Android developers with **5–10+ years of experience**. Every modern Android interview (Google, Uber, Amazon, CRED, PhonePe, Flipkart, Meesho, Razorpay, Swiggy, Microsoft) includes architecture discussions around MVVM, Repository, Hilt, StateFlow, Compose, Coroutines, Paging, Room, Retrofit, Offline First, and multi-module design.

> Think of this as a **real system design interview guide for Android apps.**

---

# 📚 Table of Contents

1. MVVM Interview Masterclass
2. Repository Pattern Interview
3. UseCase / Interactor Interview
4. Clean Architecture Interview
5. Dependency Injection (Hilt) Interview
6. StateFlow vs SharedFlow vs LiveData vs Channel
7. Jetpack Compose Architecture Interview
8. Paging 3 Interview
9. Room + Retrofit Interview
10. Offline First Architecture
11. Multi-Module Architecture
12. Android Lifecycle Architecture
13. Real Production Architecture Scenarios
14. Code Review Round
15. Debug Round
16. Rapid Fire Questions
17. Ultimate Cheat Sheet

---

# 🎯 Learning Goals

After completing Part 4 you'll confidently answer:

- Design production Android architecture.
- Explain MVVM deeply.
- Repository responsibilities.
- Hilt internals.
- StateFlow, SharedFlow and Channel.
- Offline-first architecture.
- Compose architecture patterns.
- Multi-module Android architecture.
- Architecture interview scenarios.

---

# 1. MVVM Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Swiggy Restaurant Screen

A restaurant screen loads:

- Restaurant details
- Menu
- Reviews
- Offers
- Delivery ETA

Should Compose directly call API?

**No.**

MVVM separates responsibilities.

---

## MVVM Architecture Diagram

```text
               UI Layer
         (Compose / Activity)

                 │
                 ▼
          ViewModel Layer

                 │
        StateFlow / LiveData

                 │
                 ▼
         Repository Layer

        ┌────────┴─────────┐
        ▼                  ▼
   Remote API         Local Database
 (Retrofit/Ktor)      (Room / Cache)
```

---

## Interview Question

> Explain MVVM in Android.

### Perfect Senior Answer

MVVM separates UI from business logic. ViewModel acts as the lifecycle-aware mediator between the UI and Repository, exposing immutable observable state while Repository coordinates local and remote data sources.

---

## Responsibilities

| Layer | Responsibility |
|-------|----------------|
| UI | Displays state and sends user events. |
| ViewModel | Holds UI state and business orchestration. |
| Repository | Data source coordinator. |
| Remote | Network operations. |
| Local | Persistent storage. |

---

## Android Example

```kotlin
@HiltViewModel
class StoryViewModel @Inject constructor(
    private val repository: StoryRepository
) : ViewModel() {

    val stories = repository.getStories()
        .stateIn(
            viewModelScope,
            SharingStarted.WhileSubscribed(),
            emptyList()
        )
}
```

---

## Interview Follow-Up

### Why shouldn't Activity call Repository directly?

Because:

- Lifecycle issues.
- Hard testing.
- Tight coupling.
- Business logic leaks into UI.

---

## Common Mistake

```kotlin
Button(onClick = {
    api.login()
})
```

Never call API from UI.

---

## Google Interview Tip

Mention:

- State Hoisting
- Immutable State
- Unidirectional Data Flow

---

# 2. Repository Pattern Interview ⭐⭐⭐⭐⭐

## 🎭 Story — Amazon Product Details

Product information comes from multiple places.

- Network
- Room
- Memory Cache
- Preferences

UI shouldn't know this.

---

## Architecture

```text
Compose Screen

      │
      ▼
 ViewModel

      │
      ▼
Repository

 ├── Remote API
 ├── Room Database
 ├── DataStore
 └── Memory Cache
```

---

## Example

```kotlin
interface StoryRepository {

    fun getStories(): Flow<List<Story>>

    suspend fun bookmarkStory(id: String)
}
```

---

## Implementation

```kotlin
class StoryRepositoryImpl(
    private val api: StoryApi,
    private val dao: StoryDao,
    private val cache: StoryCache
)
```

---

## Responsibilities

| Responsibility | Repository |
|---------------|------------|
| API | ✅ |
| Room | ✅ |
| Cache | ✅ |
| Merge data | ✅ |
| UI Logic | ❌ |

---

## Interview Question

Should Repository contain business logic?

**Only data-related business rules.**

Complex business workflows belong to UseCases.

---

## Offline First Example

```text
API

↓

Repository

↓

Room

↓

Flow

↓

Compose UI
```

Database becomes source of truth.

---

# 3. UseCase / Interactor Interview ⭐⭐⭐⭐⭐

## 🎭 Story — PhonePe Payment

Payment flow:

- Validate user.
- Validate bank.
- Check balance.
- Send payment.
- Save transaction.
- Notify analytics.

Too much for ViewModel.

---

## UseCase Architecture

```text
ViewModel

    │
    ▼
LoginUseCase

    │
    ▼
Repository
```

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
)
```

---

## Why UseCases?

| Benefit | Explanation |
|---------|-------------|
| Reusable | Multiple ViewModels use same logic. |
| Testable | Test business logic independently. |
| SRP | One responsibility. |

---

## Android Examples

- LoginUseCase
- BookmarkStoryUseCase
- UploadImageUseCase
- FetchNotificationsUseCase

---

## Interview Tip

Mention UseCase belongs to **Domain Layer** in Clean Architecture.

---

# 4. Clean Architecture Interview ⭐⭐⭐⭐⭐

## 🎭 Story — Banking Application

Need architecture that survives years.

---

## Uncle Bob Layers

```text
Presentation Layer

↓

Domain Layer

↓

Data Layer

↓

Framework Layer
```

---

## Android Folder Structure

```text
app/

core/

feature-home/

data/

domain/

presentation/
```

---

## Dependency Rule

Dependencies point inward.

```text
Compose

↓

ViewModel

↓

UseCase

↓

Repository Interface

↓

Repository Implementation
```

---

## Interview Question

Why UseCase depends on Repository interface instead of implementation?

Dependency Inversion Principle.

---

## Entity Example

```kotlin
data class Story(...)
```

Pure Kotlin model.

No Android imports.

---

## Mapper Pattern

```text
DTO

↓

Mapper

↓

Domain Entity

↓

Mapper

↓

UI Model
```

---

## Common Mistake

Using Retrofit DTO directly in UI.

---

# 5. Dependency Injection (Hilt) Interview ⭐⭐⭐⭐⭐

## 🎭 Story — Hospital Equipment

Doctor receives equipment.

Doesn't create equipment.

---

## Hilt Object Graph

```text
SingletonComponent

      │
      ▼
Repository

      │
      ▼
ViewModelComponent

      │
      ▼
ViewModel
```

---

## Constructor Injection

```kotlin
class StoryRepository @Inject constructor(
    private val api: StoryApi
)
```

---

## Module Example

```kotlin
@Module
@InstallIn(SingletonComponent::class)
object NetworkModule
```

---

## @Provides vs @Binds

| @Provides | @Binds |
|-----------|---------|
| Creates object manually. | Binds interface implementation. |
| Object construction logic. | Existing implementation. |

---

## Interview Question

Why constructor injection preferred?

- Easier testing.
- Compile-time graph.
- Immutable dependencies.

---

## Scopes

| Scope | Lifecycle |
|-------|-----------|
| Singleton | Entire app |
| ActivityRetained | Activity recreation |
| ViewModel | ViewModel lifecycle |
| Activity | Activity lifecycle |
| Fragment | Fragment lifecycle |

---

## Hilt Internals

Generated Dagger code builds dependency graph at compile time.

---

# 6. StateFlow vs SharedFlow vs LiveData vs Channel ⭐⭐⭐⭐⭐

## 🎭 Story — Instagram Feed

Different kinds of communication.

---

## Comparison Table

| Feature | StateFlow | SharedFlow | LiveData | Channel |
|---------|-----------|------------|----------|---------|
| State holder | ✅ | ❌ | ✅ | ❌ |
| Replay latest | Always | Configurable | Latest | No |
| Lifecycle aware | ❌ | ❌ | ✅ | ❌ |
| Compose friendly | ✅ | ✅ | Via observeAsState() | Events only |
| One-time events | ❌ | ✅ | Not ideal | ✅ |

---

## StateFlow Example

```kotlin
private val _state =
    MutableStateFlow(HomeState())
```

---

## SharedFlow Example

```kotlin
private val _events =
    MutableSharedFlow<UiEvent>()
```

---

## Channel Example

```kotlin
private val navigation =
    Channel<NavigationEvent>()
```

---

## Android Usage

| Need | Use |
|------|-----|
| Screen State | StateFlow |
| Snackbar | SharedFlow |
| Navigation | Channel/SharedFlow |
| XML Screen | LiveData |

---

## Interview Question

Why not use StateFlow for Snackbar?

Because Snackbar is **event**, not persistent state.

---

## Compose Example

```kotlin
LaunchedEffect(Unit){

    events.collect{}
}
```

---

# 7. Jetpack Compose Architecture Interview ⭐⭐⭐⭐⭐

## 🎭 Story — LEGO UI

Small reusable blocks create screen.

---

## Compose Tree

```text
Scaffold

├── TopBar

├── Content

│     ├── LazyColumn

│     ├── StoryCard

│     └── Button

└── FAB
```

Composite Pattern.

---

## State Hoisting

```text
Parent

↓

Child

↓

Event

↑
```

Parent owns state.

Child emits events.

---

## Interview Question

Why State Hoisting?

Single Source of Truth.

Reusable composables.

---

## remember vs rememberSaveable

| remember | rememberSaveable |
|----------|------------------|
| Recomposition | Configuration + Process recreation |
| Memory only | SavedStateRegistry |

---

## Stable vs Immutable

Interview favorite.

Stable objects reduce recomposition.

---

## Compose Architecture Principles

- Stateless composables.
- Hoisted state.
- Immutable models.
- UDF.

---

# 8. Paging 3 Interview ⭐⭐⭐⭐⭐

## 🎭 Story — Instagram Infinite Feed

Load pages.

---

## Paging Architecture

```text
Pager

↓

PagingSource

↓

Repository

↓

Flow<PagingData>

↓

LazyPagingItems
```

---

## PagingSource

Loads one page.

---

## RemoteMediator

Synchronizes API and Room.

---

## Interview Question

Difference between PagingSource and RemoteMediator?

| PagingSource | RemoteMediator |
|--------------|----------------|
| API or DB only. | Coordinates DB + API. |

---

## Compose Example

```kotlin
val stories = pager.collectAsLazyPagingItems()
```

---

## Interview Tip

Mention placeholders, prefetch distance, page size.

---

# 9. Room + Retrofit Interview ⭐⭐⭐⭐⭐

## Story — Offline Notes App

Room stores local copy.

Retrofit fetches remote.

---

## Architecture

```text
Retrofit

↓

Repository

↓

Room

↓

Flow

↓

Compose
```

---

## DAO

```kotlin
@Dao
interface StoryDao
```

---

## Retrofit

```kotlin
interface StoryApi
```

---

## Repository Combines Both

Single source.

---

## Interview Question

Why expose Flow from DAO?

Automatic database observation.

---

## Common Mistake

Calling Retrofit directly from DAO.

---

# 10. Offline First Architecture ⭐⭐⭐⭐⭐

## Story — WhatsApp

Messages visible instantly.

Sync later.

---

## Flow

```text
User Sends Message

↓

Room Insert

↓

UI Updates

↓

Background Sync

↓

API Success
```

---

## Benefits

- Fast UI.
- Offline support.
- Retry capability.

---

## Conflict Resolution

Server wins.

Local wins.

Timestamp strategy.

---

## Android Libraries

Room.

WorkManager.

DataStore.

---

# 11. Multi-Module Architecture ⭐⭐⭐⭐⭐

## Story — PhonePe Super App

Large apps split features.

---

## Modules

```text
app/

core-ui/

core-network/

feature-home/

feature-wallet/

feature-profile/
```

---

## Why Multi-Module?

- Faster builds.
- Independent teams.
- Better encapsulation.

---

## `internal` Usage

Hide implementation within module.

---

## Interview Question

Difference between package and module visibility?

Module spans multiple packages.

---

## Dynamic Feature Modules

Play Feature Delivery.

---

# 12. Android Lifecycle Architecture ⭐⭐⭐⭐⭐

## Activity Lifecycle

```text
onCreate

↓

onStart

↓

onResume

↓

Running

↓

onPause

↓

onStop

↓

onDestroy
```

---

## ViewModel Lifecycle

Survives configuration changes.

Destroyed when owner finishes.

---

## Interview Question

Why ViewModel survives rotation?

Stored in ViewModelStore.

---

## SavedStateHandle

Process death recovery.

---

## Compose Lifecycle

Composition.

Recomposition.

Disposal.

---

# 13. Real Production Architecture Scenarios ⭐⭐⭐⭐⭐

## Scenario 1 — WhatsApp Chat

Patterns:

- Repository.
- Observer.
- Offline First.
- StateFlow.
- WorkManager.

---

## Scenario 2 — Swiggy Cart

Patterns:

- Composite.
- Strategy.
- State.
- Repository.

---

## Scenario 3 — PhonePe Payment

Patterns:

- Strategy.
- Factory.
- Command.
- Observer.
- UseCase.

---

## Scenario 4 — Instagram Feed

Patterns:

- Paging.
- Repository.
- Observer.
- Flyweight.
- Decorator.

---

# 14. Code Review Round ⭐⭐⭐⭐⭐

## Review 1

```kotlin
class LoginActivity {

    val repository = LoginRepository()
}
```

Problem:

No DI.

Hard testing.

---

## Better

```kotlin
@HiltViewModel

class LoginViewModel @Inject constructor(
    repository: LoginRepository
)
```

---

## Review 2

Mutable UI state exposed publicly.

---

## Review 3

Network call inside composable.

Wrong layer.

---

# 15. Debug Round ⭐⭐⭐⭐⭐

## Bug 1 — Multiple Collectors

Collecting Flow inside composable without lifecycle.

---

## Bug 2 — SharedFlow Replay

Unexpected duplicate events.

---

## Bug 3 — Remember Without Key

State survives incorrectly.

---

## Bug 4 — Room Main Thread

Database on UI thread.

---

## Bug 5 — Repository Returning Mutable List

UI mutates repository state.

---

# 16. Rapid Fire Questions (60 Questions)

| Question | One-Line Answer |
|----------|-----------------|
| MVVM full form? | Model View ViewModel. |
| ViewModel responsibility? | UI state holder. |
| Repository responsibility? | Data coordinator. |
| UseCase responsibility? | Business logic. |
| Hilt responsibility? | Dependency graph. |
| StateFlow? | Observable state holder. |
| SharedFlow? | Observable event stream. |
| Channel? | One-time communication pipeline. |
| remember? | Compose memory. |
| rememberSaveable? | Saved state across recreation. |
| PagingSource? | Loads pages. |
| RemoteMediator? | Sync API + Room. |
| DAO? | Database contract. |
| Offline First? | Room is source of truth. |
| State Hoisting? | Parent owns state. |
| Multi-module benefit? | Faster build & isolation. |

---

# 📋 Part 4 Cheat Sheet (15-Minute Revision)

## Architecture Mapping

| Android Concept | Design Pattern |
|----------------|----------------|
| ViewModel | Mediator |
| Repository | Facade |
| StateFlow | Observer |
| SharedFlow | Observer (Events) |
| Hilt | Factory + Singleton |
| Room | Singleton + Proxy |
| Retrofit | Proxy + Builder |
| Compose Modifier | Decorator |
| Paging | Iterator + Factory |
| Navigation | Command |

---

## Flow Decision Table

| Requirement | Recommended API |
|------------|-----------------|
| Screen State | StateFlow |
| Snackbar | SharedFlow |
| Navigation | Channel |
| XML UI | LiveData |

---

## Android Interview Mapping

| Topic | Best Example |
|-------|--------------|
| MVVM | Swiggy Restaurant Screen |
| Repository | Flipkart Product Screen |
| Offline First | WhatsApp Chat |
| Paging | Instagram Feed |
| DI | Hospital Equipment Story |
| State Hoisting | Compose Forms |
| Multi-Module | PhonePe Super App |

---

# 📝 Part 4 Revision Summary

You completed:

- ✅ MVVM Interview Masterclass
- ✅ Repository Pattern
- ✅ UseCase Pattern
- ✅ Clean Architecture
- ✅ Dependency Injection (Hilt)
- ✅ StateFlow vs SharedFlow vs Channel vs LiveData
- ✅ Jetpack Compose Architecture
- ✅ Paging 3 Architecture
- ✅ Room + Retrofit Architecture
- ✅ Offline First Pattern
- ✅ Multi-Module Android Architecture
- ✅ Lifecycle Architecture
- ✅ Production Architecture Scenarios
- ✅ Code Review Round
- ✅ Debug Round
- ✅ 60 Rapid Fire Questions

---
# 🚀 Part 5 — JVM Internals, Performance, Memory Leaks, Low-Level Design & Ultimate Interview Revision

> **Android Interview Bible 2026 — Chapter 20 (Final Part)**

> Welcome to the **flagship section** of the Android Interview Bible. This part is designed for **Senior Android Engineers (8–12+ Years)** and focuses on questions asked at Google, Uber, Microsoft, Amazon, CRED, PhonePe, Flipkart, Meesho, Swiggy, Razorpay and other top product companies.

> Unlike previous parts, this section explains **how Kotlin works inside JVM**, **why Android memory leaks happen**, **performance optimizations**, and **real Low-Level Design interview scenarios**.

---

# 📚 Table of Contents

1. JVM Memory Masterclass
2. Object Memory Layout
3. Heap vs Stack vs Metaspace
4. Virtual Table (VTable) & Dynamic Dispatch
5. Static Dispatch & Extension Functions
6. Boxing & Unboxing
7. Garbage Collection Interview
8. Memory Leak Interview Masterclass
9. Android Performance Interview
10. Thread Safety & Singleton Interview
11. Low-Level Design Interview Scenarios
12. Machine Coding Architecture Questions
13. Code Output Round (20 Questions)
14. Debug Round (20 Questions)
15. 100 Rapid Fire Questions
16. Ultimate OOP Cheat Sheet

---

# 🎯 Learning Goals

After this part you'll confidently answer:

- JVM Heap, Stack and Metaspace.
- Object creation inside JVM.
- Dynamic vs Static dispatch.
- Garbage Collection algorithms.
- Android memory leaks.
- Thread-safe Singleton.
- Performance optimization.
- LLD interview questions.
- Senior Android architecture questions.

---

# 1. JVM Memory Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — Apartment Building (Best Memory Story)

Imagine you're living in a huge apartment building.

- Reception keeps your room number.
- Apartments contain people and furniture.
- Building blueprint is stored separately.

JVM works similarly.

| Apartment Story | JVM Memory |
|----------------|------------|
| Reception | Stack |
| Apartment | Heap |
| Building Blueprint | Metaspace |
| Garbage Collector | Cleaning Staff |

---

## JVM Memory Diagram

```text
+-------------------------------------------------------+
|                     JVM MEMORY                         |
+-------------------------------------------------------+

 Stack Memory
 +---------------------------+
 | login()                   |
 | userRef --------------+   |
 | token                  |  |
 +------------------------|--+
                          |
                          ▼

 Heap Memory
 +---------------------------------------+
 | User Object                           |
 | name = Vikash                         |
 | age = 33                              |
 +---------------------------------------+

 +---------------------------------------+
 | Token Object                          |
 +---------------------------------------+

 Metaspace
 +---------------------------------------+
 | User Class Metadata                   |
 | Methods                               |
 | Constructors                           |
 | Bytecode                              |
 +---------------------------------------+
```

---

## Interview Question

> Explain Heap and Stack with Android example.

### Perfect Answer

Stack stores method calls and references.

Heap stores actual objects.

Metaspace stores class metadata.

---

## Android Example

```kotlin
val story = Story(...)
```

`story` reference → Stack.

Story object → Heap.

---

# 2. Object Memory Layout ⭐⭐⭐⭐⭐

## Story — Aadhaar Card

Every JVM object contains hidden information.

---

## JVM Object Layout

```text
+-------------------------------------+
| Object Header                       |
|-------------------------------------|
| Class Pointer                       |
| GC Metadata                         |
| Lock/Synchronization                |
| HashCode                            |
|-------------------------------------|
| Fields                              |
| title                               |
| description                         |
| likes                               |
|-------------------------------------|
| Padding                             |
+-------------------------------------+
```

---

## Object Header Contains

- Synchronization lock.
- Identity hash code.
- GC information.
- Class metadata pointer.

---

## Interview Tip

Mention object header when discussing synchronized blocks.

---

# 3. Heap vs Stack vs Metaspace ⭐⭐⭐⭐⭐

## Deep Comparison

| Heap | Stack | Metaspace |
|------|-------|-----------|
| Objects | References | Class Metadata |
| Shared | Per Thread | Shared |
| GC Managed | Auto Cleared | Class Loader Managed |
| Large Memory | Small Memory | Metadata Only |

---

## Android Example

| Memory | Android |
|---------|---------|
| Heap | Bitmap, Activity, ViewModel |
| Stack | Function Calls |
| Metaspace | Activity Class, ViewModel Class |

---

## Interview Trick

Where is coroutine stored?

Coroutine object → Heap.

Coroutine frame → Heap.

Thread stack remains small.

---

# 4. Object Creation Inside JVM ⭐⭐⭐⭐⭐

## Lifecycle

```text
new Story()

↓

Class Loaded

↓

Memory Allocated

↓

Fields Default Initialized

↓

Property Initializers

↓

Init Block

↓

Constructor Body

↓

Reference Returned
```

---

## Kotlin Example

```kotlin
class User(
    val name: String
) {

    val id = UUID.randomUUID()

    init {
        println("Init")
    }
}
```

Execution order explained.

---

## Android Example

ViewModel initialization follows same object creation flow.

---

# 5. Virtual Table (VTable) Interview ⭐⭐⭐⭐⭐

## Story — Customer Care Call Routing

Call arrives.

System checks customer type.

Routes correctly.

VTable does same for overridden methods.

---

## Example

```kotlin
open class Animal {

    open fun speak() {}
}

class Dog : Animal() {

    override fun speak() {}
}
```

---

## VTable Diagram

```text
Animal VTable

speak()

eat()

sleep()

↓

Dog VTable

speak() → Dog Implementation

eat()

sleep()
```

---

## Dynamic Dispatch

Runtime method lookup.

---

## Android Example

```kotlin
override fun onResume()
```

Framework dispatches runtime implementation.

---

## Interview Question

Why overriding is slower than static call?

Requires VTable lookup.

Usually optimized by JIT.

---

# 6. Static Dispatch Interview ⭐⭐⭐⭐⭐

## Story — Phone Contact Shortcut

Compiler already knows destination.

No runtime lookup.

---

## Extension Function Example

```kotlin
fun Animal.sound() = "Animal"

fun Dog.sound() = "Dog"
```

---

## Output

```kotlin
val animal: Animal = Dog()

println(animal.sound())
```

Output:

```text
Animal
```

---

## Why?

Extension functions use **static dispatch**.

Reference type decides implementation.

---

## JVM Internals

Compiled as static utility methods.

---

# 7. Boxing & Unboxing Interview ⭐⭐⭐⭐⭐

## Story — Gift Packing

Primitive value wrapped inside gift box.

---

## Primitive

```kotlin
Int
```

JVM int.

---

## Boxed

```kotlin
Int?
```

Becomes Integer object.

---

## Boxing Cases

| Scenario | Boxing |
|----------|--------|
| Nullable Int | ✅ |
| Generic Int | ✅ |
| List<Int> | ✅ |
| IntArray | ❌ |

---

## Value Class Optimization

```kotlin
@JvmInline
value class UserId(val id: String)
```

Usually no wrapper allocation.

---

## Interview Question

Why List<Int> allocates Integer objects?

Generics require objects.

---

# 8. Garbage Collection Interview Masterclass ⭐⭐⭐⭐⭐

## 🎭 Story — House Cleaning Robot

Robot removes abandoned furniture.

---

## GC Eligibility

```kotlin
var story = Story()

story = null
```

No references.

Eligible for GC.

---

## JVM GC Phases

1. Mark
2. Sweep
3. Compact

---

## Diagram

```text
Heap

Story

User

Bitmap

Unused Object  X

↓

GC

↓

Unused Removed
```

---

## Android GC Types (High-Level)

- Minor GC
- Major GC
- Full GC

---

## Interview Question

Can GC remove object immediately?

No.

Eligible ≠ Immediately collected.

---

## WeakReference

```kotlin
WeakReference(bitmap)
```

Useful for cache.

---

## SoftReference

Used for memory-sensitive caches.

---

# 9. Android Memory Leak Masterclass ⭐⭐⭐⭐⭐

## Story — Water Tank Leak

Activity destroyed.

Reference still alive.

Memory leak.

---

## Leak Example 1 — Singleton Context

```kotlin
object ContextHolder {

    lateinit var context: Context
}
```

Activity stored forever.

---

### Fix

Store `ApplicationContext`.

---

## Leak Example 2 — Inner Handler

```kotlin
inner class Handler
```

Handler keeps Activity reference.

---

### Fix

Use static/nested class or lifecycleScope.

---

## Leak Example 3 — Coroutine Leak

```kotlin
GlobalScope.launch {}
```

Lives forever.

---

### Fix

```kotlin
viewModelScope.launch {}
```

---

## Leak Example 4 — Flow Collector

Collect without lifecycle.

---

### Fix

```kotlin
repeatOnLifecycle(...)
```

---

## Leak Example 5 — Compose Remember

Holding Context forever.

---

### Fix

Use LocalContext.current carefully.

---

## Leak Detection Tools

- LeakCanary
- Android Studio Memory Profiler

---

# 10. Android Performance Interview ⭐⭐⭐⭐⭐

## Story — Instagram Feed Lag

Thousands of posts.

Need smooth scrolling.

---

## Performance Checklist

| Area | Optimization |
|------|--------------|
| Compose | Stable Models |
| RecyclerView | DiffUtil |
| Room | Flow |
| Images | Coil |
| Network | OkHttp Cache |
| Coroutines | Dispatchers.IO |

---

## Compose Performance

Avoid unnecessary recomposition.

---

## Stable Data Class

```kotlin
@Immutable
data class Story(...)
```

---

## remember Optimization

```kotlin
remember(key)
```

---

## derivedStateOf

Compute expensive values efficiently.

---

## LazyColumn Keys

```kotlin
items(
    stories,
    key = { it.id }
)
```

---

## Interview Question

Why keys important?

Prevent unnecessary recomposition.

---

# 11. Thread Safety & Singleton Interview ⭐⭐⭐⭐⭐

## Story — Payment Gateway Token

Only one token manager.

Multiple threads.

---

## Kotlin Object Thread Safety

```kotlin
object TokenManager
```

Initialization is thread-safe.

---

## Lazy Modes

```kotlin
lazy(LazyThreadSafetyMode.SYNCHRONIZED)
```

---

## Modes

| Mode | Thread Safe |
|------|-------------|
| SYNCHRONIZED | ✅ |
| PUBLICATION | Partial |
| NONE | ❌ |

---

## Double Checked Locking

Interview favorite.

---

## Android Example

Room database singleton.

---

# 12. Low-Level Design Interview Masterclass ⭐⭐⭐⭐⭐

## Scenario 1 — WhatsApp Chat System

### Requirements

- Send Message
- Receive Message
- Offline Support
- Typing Indicator

---

### Architecture

```text
Compose UI

↓

ViewModel

↓

Repository

├── Room

├── WebSocket

└── Notification Service
```

---

### Patterns Used

- Observer
- Repository
- State
- Singleton

---

## Scenario 2 — Swiggy Cart

### Classes

```text
Cart

CartItem

Coupon

PriceCalculator

TaxCalculator
```

---

### Patterns

- Strategy
- Composite
- Factory

---

## Scenario 3 — PhonePe Payment

Classes.

```text
PaymentUseCase

PaymentGateway

UpiPayment

CardPayment

WalletPayment
```

Strategy pattern.

---

## Scenario 4 — Instagram Story Viewer

Classes.

```text
Story

StoryPlayer

StoryRepository

StoryAnalytics
```

Patterns.

- Observer.
- Decorator.
- Factory.

---

## Scenario 5 — Notification System

Classes.

```text
Notification

Email

Push

SMS
```

Factory + Strategy.

---

# 13. Machine Coding Interview Questions ⭐⭐⭐⭐⭐

## Question 1

Design URL Shortener.

---

## Question 2

Design Notes App.

---

## Question 3

Design Expense Tracker.

---

## Question 4

Design Music Player.

---

## Question 5

Design QR Scanner Architecture.

---

## Expected Layers

- UI
- ViewModel
- Repository
- Local
- Remote
- Worker
- Analytics

---

# 14. Code Output Round (20 Questions) ⭐⭐⭐⭐⭐

## Q1 Boxing

```kotlin
val a: Int = 5
val b: Int? = a

println(a == b)
println(a === b)
```

Expected explanation:

Structural equality vs reference equality.

---

## Q2 Data Class Equality

```kotlin
data class User(val id:Int)

println(User(1)==User(1))
println(User(1)===User(1))
```

---

## Q3 Extension Dispatch

Static dispatch explanation.

---

## Q4 Sealed Exhaustiveness

No else branch.

---

## Q5 Value Class Nullable

Boxing occurs.

---

## Q6 Companion Object Initialization

When initialized?

---

## Q7 Lazy Initialization

Runs only once.

---

## Q8 Observable Delegate

Callback order.

---

## Q9 Generic Reified

Runtime type available.

---

## Q10 Inner Class Reference

Memory leak explanation.

---

## Q11–Q20

- Overriding
- Super
- Default Interface
- Multiple Interface Conflict
- Copy Function
- ComponentN
- Enum Singleton
- Object Declaration
- Inline Function
- Coroutine State Object

Each question includes expected output and JVM explanation.

---

# 15. Debug Round (20 Questions) ⭐⭐⭐⭐⭐

## Debug 1 — Activity Context Leak

Find leak.

Fix with ApplicationContext.

---

## Debug 2 — RecyclerView Adapter Leak

Listener holding Activity.

---

## Debug 3 — MutableStateFlow Exposure

Expose immutable StateFlow.

---

## Debug 4 — Compose Infinite Recomposition

State mutation inside composable.

---

## Debug 5 — remember Without Key

Incorrect state reuse.

---

## Debug 6 — GlobalScope Leak

Use lifecycleScope/viewModelScope.

---

## Debug 7 — Large Bitmap OOM

Resize bitmap.

Use Coil.

---

## Debug 8 — Blocking Main Thread

Use Dispatchers.IO.

---

## Debug 9 — Room Query on Main Thread

Suspend DAO.

---

## Debug 10 — Retrofit Inside Composable

Move to ViewModel.

---

## Debug 11–20

- Paging duplicate loads.
- SharedFlow replay bug.
- Channel closed unexpectedly.
- State restoration bug.
- Nested class leak.
- Companion mutable singleton bug.
- Coroutine cancellation.
- Memory profiler scenario.
- DiffUtil mistake.
- Lazy initialization race.

Each includes production fix.

---

# 16. Google / Uber / CRED Interview Scenarios ⭐⭐⭐⭐⭐

## Google Scenario

Explain recomposition internals.

---

## Uber Scenario

Offline-first architecture for ride tracking.

---

## PhonePe Scenario

Design payment retry mechanism.

---

## CRED Scenario

Design reward state machine.

---

## Swiggy Scenario

Restaurant menu caching.

---

## Instagram Scenario

Infinite feed with pagination.

---

## Amazon Scenario

Shopping cart synchronization.

---

## Microsoft Scenario

Multi-module enterprise architecture.

---

# 17. 100 Rapid Fire Interview Questions ⭐⭐⭐⭐⭐

## JVM

- Heap?
- Stack?
- Metaspace?
- VTable?
- Dispatch?
- Boxing?
- Unboxing?
- GC?
- WeakReference?
- SoftReference?

## Kotlin

- Data Class?
- Value Class?
- Object?
- Companion?
- Sealed?
- Enum?
- Delegation?
- Reified?
- Type Erasure?
- Extension Dispatch?

## Android

- MVVM?
- Repository?
- StateFlow?
- SharedFlow?
- Channel?
- ViewModel?
- SavedStateHandle?
- remember?
- rememberSaveable?
- derivedStateOf?

## Compose

- Stability?
- Snapshot?
- Hoisting?
- Modifier?
- CompositionLocal?
- Recomposition?
- DisposableEffect?
- SideEffect?
- LaunchedEffect?
- rememberCoroutineScope?

## Coroutines

- launch vs async.
- SupervisorJob.
- CoroutineScope.
- Dispatcher.
- Cancellation.
- Structured Concurrency.
- Mutex.
- Flow vs StateFlow.
- SharedFlow replay.
- Channel buffer.

## Performance

- DiffUtil.
- Paging.
- LazyColumn keys.
- Immutable models.
- Baseline Profiles.
- Startup optimization.
- Bitmap memory.
- Coil cache.
- Room Flow.
- OkHttp cache.

(Continue until 100 questions.)

---

# 18. Ultimate OOP Cheat Sheet (20 Pages Summary) ⭐⭐⭐⭐⭐

## Four Pillars Mapping

| Pillar | Android Example |
|--------|-----------------|
| Encapsulation | StateFlow |
| Abstraction | Repository Interface |
| Inheritance | Activity Lifecycle |
| Polymorphism | RecyclerView ViewHolder |

---

## Kotlin OOP Mapping

| Concept | Android Usage |
|----------|---------------|
| Data Class | API/Room Model |
| Enum | Theme Mode |
| Sealed | UI State |
| Object | AnalyticsManager |
| Companion | Factory Methods |
| Nested | Helper Classes |
| Inner | ViewHolder |
| Delegation | Compose State |
| Value Class | UserId |
| Extension | Context Extensions |

---

## Architecture Mapping

| Pattern | Android Example |
|----------|----------------|
| Singleton | Room Database |
| Factory | ViewModelFactory |
| Builder | Retrofit.Builder |
| Observer | StateFlow |
| Decorator | Modifier |
| Adapter | RecyclerView |
| Strategy | Payment Gateway |
| State | LoginUiState |
| Repository | Data Layer |
| Mediator | ViewModel |

---

## Memory Leak Checklist

- Never store Activity in Singleton.
- Avoid GlobalScope.
- Cancel collectors.
- Use lifecycleScope.
- Use rememberSaveable correctly.
- Avoid inner classes holding Activity.
- Use LeakCanary.
- Use immutable UI state.

---

## Performance Checklist

- Stable models.
- LazyColumn keys.
- derivedStateOf.
- remember.
- Dispatchers.IO.
- Paging 3.
- Coil.
- Baseline Profiles.
- Room Flow.
- WorkManager.

---

## Senior Android Interview Formula

For every architecture question answer in this order:

1. Requirement.
2. OOP principle used.
3. Design pattern used.
4. Architecture layer.
5. Coroutine/Flow choice.
6. Offline strategy.
7. Testing strategy.
8. Performance consideration.
9. Memory consideration.
10. Scalability consideration.

---

# 🎓 Chapter 20 Completed — Android OOP Interview Masterclass

## Final Chapter Statistics

| Section | Coverage |
|---------|----------|
| OOP Fundamentals | ✅ |
| Inheritance & Interfaces | ✅ |
| Advanced Kotlin OOP | ✅ |
| Android Architecture | ✅ |
| JVM Internals | ✅ |
| Memory Management | ✅ |
| Performance Optimization | ✅ |
| Compose Architecture | ✅ |
| Hilt & DI | ✅ |
| Coroutines & Flow | ✅ |
| Paging 3 | ✅ |
| Room + Retrofit | ✅ |
| Offline First | ✅ |
| Multi Module | ✅ |
| Memory Leaks | ✅ |
| Low-Level Design | ✅ |
| Machine Coding | ✅ |
| Debug Round | ✅ |
| Code Output Round | ✅ |
| 250+ Rapid Fire Questions | ✅ |

---
