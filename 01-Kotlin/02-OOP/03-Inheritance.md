# 💜 Kotlin Inheritance — Complete Interview Guide (2026 Edition)

> Master Kotlin inheritance from basics to polymorphism, initialization pitfalls, JVM dispatch, Android architecture, Compose, and Staff Engineer interview questions.

**Module:** Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • CRED • Razorpay

---

# 📚 Table of Contents

1. What is Inheritance?
2. Why Kotlin Classes Are Final by Default
3. `open` Keyword
4. Extending Classes
5. Constructor Inheritance
6. Method Overriding
7. Property Overriding
8. `super` Keyword
9. Initialization Order (Important Pitfall)
10. Polymorphism
11. Dynamic Dispatch
12. Multiple Inheritance via Interfaces
13. Abstract vs Open Classes
14. Android Architecture Examples
15. Compose Examples
16. JVM Internals
17. Performance Discussion
18. Best Practices
19. Common Mistakes
20. Interview Questions
21. Cheat Sheet

---

# 1. What is Inheritance?

## 🏢 Story: The Family Business

Imagine a family owns a successful restaurant called **FoodHub**.

The father created the original restaurant with common rules:

- Every restaurant has a name.
- Every restaurant opens at 9 AM.
- Every restaurant closes at 11 PM.

Now three children open their own branches:

- FoodHub Bangalore
- FoodHub Delhi
- FoodHub Mumbai

Each branch inherits the common behavior but customizes some features.

**That's inheritance.**

A child class reuses code from a parent class while adding or changing behavior.

---

## Kotlin Example

```kotlin
open class Restaurant {

    fun openStore() {
        println("Opening at 9 AM")
    }
}

class BangaloreRestaurant : Restaurant()
```

```
Restaurant
      ▲
      │
BangaloreRestaurant
```

---

## Real Android Examples

| Parent Class | Child Class |
|--------------|-------------|
| `ViewModel` | `HomeViewModel` |
| `RecyclerView.Adapter` | `UserAdapter` |
| `Activity` | `MainActivity` |
| `Fragment` | `ProfileFragment` |
| `Exception` | `IOException` |

Inheritance is everywhere in Android.

---

# 2. Why Kotlin Classes Are Final by Default?

### Interview Favorite ⭐⭐⭐⭐⭐

Unlike Java...

```java
class User { }
```

Every class is inheritable.

In Kotlin...

```kotlin
class User
```

Cannot inherit.

```
This type is final, so it cannot be inherited from.
```

---

## Why Did Kotlin Do This?

Because inheritance is powerful **but dangerous**.

### Problem in Java

Developers accidentally inherit classes that were never designed for extension.

This can break encapsulation.

### Kotlin Philosophy

> **Composition first. Inheritance only when intentional.**

A class must explicitly opt into inheritance.

---

# 3. `open` Keyword

To allow inheritance...

```kotlin
open class Animal {

    fun eat() {
        println("Eating...")
    }
}

class Dog : Animal()
```

Now inheritance works.

---

## `open` Means

> "I designed this class to be extended."

Everything is final unless marked `open`.

---

## Memory Relationship

```text
Animal Object
      ▲
      │
Dog Object
```

Dog object contains Animal members.

---

# 4. Extending Classes

Basic syntax.

```kotlin
open class Animal(
    val name: String
)

class Dog : Animal("Bruno")
```

---

## Constructor Arguments

```kotlin
open class Animal(
    val name: String
)

class Dog(name: String) : Animal(name)
```

Parent constructor executes first.

---

## Android Example

```kotlin
open class BaseRepository(
    protected val api: ApiService
)

class UserRepository(api: ApiService)
    : BaseRepository(api)
```

Very common Clean Architecture pattern.

---

# 5. Constructor Inheritance

### Story: Apartment Construction

Imagine building a penthouse.

Before decorating the penthouse...

- Foundation is built.
- Building floors are built.
- Plumbing is installed.

Only then the penthouse is initialized.

Parent constructor is the **foundation**.

---

## Example

```kotlin
open class Animal(name: String) {

    init {
        println("Animal Created")
    }
}

class Dog(name: String) : Animal(name) {

    init {
        println("Dog Created")
    }
}
```

Output

```text
Animal Created
Dog Created
```

Parent first.

---

# 6. Overriding Methods

Parent behavior can be customized.

```kotlin
open class Animal {

    open fun sound() {
        println("Animal Sound")
    }
}

class Dog : Animal() {

    override fun sound() {
        println("Bark")
    }
}
```

Output

```text
Bark
```

---

## Rules

| Parent | Child |
|--------|-------|
| `open fun` | `override fun` |
| `final fun` | Cannot override |

---

## Final Override

```kotlin
open class Animal {

    open fun sound(){}
}

class Dog : Animal() {

    final override fun sound(){}
}
```

Further subclasses cannot override.

---

# 7. Overriding Properties

Properties behave similarly.

```kotlin
open class Animal {

    open val legs = 4
}

class Spider : Animal() {

    override val legs = 8
}
```

---

## `var` vs `val`

### Allowed

```kotlin
open class Animal {
    open val name = ""
}

class Dog : Animal() {
    override var name = "Bruno"
}
```

`val` → `var` is allowed.

---

### Not Allowed

`var` cannot become `val`.

Reason:

Would remove setter functionality.

---

## Custom Getter Override

```kotlin
override val name: String
    get() = "Dog"
```

Computed property.

---

# 8. `super` Keyword

Call parent implementation.

```kotlin
open class Animal {

    open fun sound() {
        println("Animal Sound")
    }
}

class Dog : Animal() {

    override fun sound() {

        super.sound()

        println("Bark")
    }
}
```

Output

```text
Animal Sound
Bark
```

---

## Access Parent Property

```kotlin
open class Animal {

    open val legs = 4
}

class Dog : Animal() {

    override val legs = 4

    fun printLegs() {
        println(super.legs)
    }
}
```

---

# 9. Initialization Pitfall ⭐⭐⭐⭐⭐

### Story: Calling Your Child Before They're Born

Imagine a parent constructor calls a child method before the child finishes initializing.

That's exactly what happens here.

```kotlin
open class Parent {

    open val message = "Parent"

    init {
        println(message)
    }
}

class Child : Parent() {

    override val message = "Child"
}
```

What prints?

```
null
```

or unexpected value depending on initialization.

---

## Why?

Initialization order:

```
Parent Constructor

↓

Parent Init

↓

Child Property Initialization

↓

Child Init
```

Child property isn't initialized yet.

---

## Best Practice

**Never call open properties or methods from constructors/init blocks.**

Google interview favorite.

---

# 10. Polymorphism

### Story: Swiggy Delivery Partner

You don't care whether food comes via:

- Bike
- Cycle
- Car

You simply call `deliver()`.

Different behavior.

Same interface.

---

## Example

```kotlin
open class Animal {

    open fun sound() {}
}

class Dog : Animal() {

    override fun sound() {
        println("Bark")
    }
}

class Cat : Animal() {

    override fun sound() {
        println("Meow")
    }
}
```

---

## Runtime Behavior

```kotlin
val animal: Animal = Dog()

animal.sound()
```

Output

```
Bark
```

Reference type differs.

Runtime object decides behavior.

---

## Why Important?

Polymorphism enables:

- Repository abstraction.
- ViewModel abstraction.
- Navigation abstraction.
- DI.

---

# 11. Dynamic Dispatch

JVM decides overridden implementation **at runtime**.

```kotlin
val animal: Animal = Dog()
```

Method lookup:

```
Animal Reference

↓

Dog Object

↓

Dog.sound()
```

---

## Static Dispatch

Extension functions are statically dispatched.

Covered later.

---

# 12. Multiple Inheritance?

Kotlin **doesn't allow multiple class inheritance**.

❌ Invalid

```kotlin
class Dog : Animal(), Pet()
```

if both are classes.

---

## Why?

Diamond Problem.

```
        Animal
       /      \
 PetAnimal   WildAnimal
       \      /
         Dog
```

Which implementation wins?

---

## Kotlin Solution

Multiple **interfaces**.

```kotlin
interface Pet

interface Wild

class Dog : Pet, Wild
```

Safe multiple inheritance.

---

# 13. Abstract vs Open

| `open` | `abstract` |
|--------|------------|
| Can instantiate | Cannot instantiate |
| Provides implementation | May force implementation |
| Optional override | Mandatory override |

---

## Example

```kotlin
abstract class Animal {

    abstract fun sound()
}
```

Must override.

---

# 14. Android Architecture Example

## Base ViewModel

```kotlin
open class BaseViewModel : ViewModel() {

    fun log(message: String) {}
}
```

Child.

```kotlin
class HomeViewModel : BaseViewModel()
```

Shared functionality.

---

## Base Activity

```kotlin
abstract class BaseActivity : AppCompatActivity() {

    fun showLoader(){}
}
```

Every activity inherits loader logic.

---

## Repository Pattern

```kotlin
open class NetworkRepository(
    protected val api: ApiService
)
```

Children.

- UserRepository
- ProductRepository
- PaymentRepository

---

# 15. Compose Example

## UI State Hierarchy

```kotlin
sealed class UiState {

    object Loading : UiState()

    data class Success(
        val users: List<User>
    ) : UiState()

    data class Error(
        val message: String
    ) : UiState()
}
```

Technically inheritance through sealed classes.

Compose loves this pattern.

---

## Component Hierarchy

```kotlin
open class UiComponent

class ButtonComponent : UiComponent()

class CardComponent : UiComponent()
```

Reusable UI models.

---

# 16. JVM Internals

Kotlin

```kotlin
open class Animal {

    open fun sound(){}
}

class Dog : Animal(){

    override fun sound(){}
}
```

Java Decompiler

```java
public class Animal{

    public void sound(){}
}

public final class Dog extends Animal{

    @Override
    public void sound(){}
}
```

Uses normal JVM inheritance.

---

## Virtual Method Table (vtable)

```
Animal

sound()

eat()

sleep()

↓

Dog

sound() → overridden

eat() → inherited

sleep() → inherited
```

Runtime dispatch uses vtable.

Senior interview topic.

---

# 17. Performance Discussion

## Final Methods

Compiler can inline.

Faster dispatch.

---

## Open Methods

Require virtual lookup.

Slight runtime overhead.

Usually negligible.

---

## Inheritance Depth

Deep inheritance chains reduce readability.

Prefer composition after 2–3 levels.

---

# 18. Android Best Practices

### Prefer Composition

Instead of

```kotlin
Car : Engine : Fuel : Battery
```

Use

```kotlin
class Car(
    private val engine: Engine
)
```

---

### Keep Base Classes Small

Base classes should expose reusable behavior only.

---

### Don't Put Business Logic in Base Activity

Prefer composition + delegation.

---

### Use Sealed Classes for UI State

Better than large inheritance hierarchies.

---

# 19. Common Mistakes

### Mistake 1

Forgetting `open`.

### Mistake 2

Calling open members inside init.

### Mistake 3

Deep inheritance chains.

### Mistake 4

Using inheritance where composition is better.

### Mistake 5

Overriding mutable state carelessly.

---

# 20. Real Android Interview Questions

## Basic

1. Why are Kotlin classes final by default?
2. What does `open` mean?
3. How do you override a method?
4. Difference between `super` and `this`?
5. Parent constructor execution order?

---

## Intermediate

6. Property overriding rules.
7. Dynamic dispatch.
8. Why can't Kotlin inherit multiple classes?
9. Diamond Problem.
10. Initialization pitfalls.

---

## Advanced

11. Virtual method table.
12. Open vs final performance.
13. Why avoid open methods in constructors?
14. Abstract vs open classes.
15. Composition vs inheritance in Android architecture.

---

# 21. 2-Minute Interview Answer

> Kotlin classes are final by default to encourage composition over inheritance. A class becomes inheritable only with the `open` keyword. Parent constructors and init blocks execute before child initialization, which is why calling open members during construction is unsafe. Runtime polymorphism is implemented using JVM virtual dispatch, allowing parent references to invoke child implementations dynamically.

---

# 22. Cheat Sheet

| Concept | Syntax |
|---------|--------|
| Inheritance | `class Dog : Animal()` |
| Open Class | `open class Animal` |
| Open Method | `open fun sound()` |
| Override | `override fun sound()` |
| Parent Call | `super.sound()` |
| Property Override | `override val legs = 4` |
| Final Override | `final override fun sound()` |

---

# 📝 Revision Summary

- Kotlin classes are **final by default**.
- `open` explicitly allows inheritance.
- Parent constructors execute before child initialization.
- `override` changes inherited behavior.
- `super` accesses parent implementation.
- Dynamic dispatch chooses the implementation based on the runtime object.
- Prefer **composition over inheritance** in modern Android architecture.