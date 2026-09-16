# 💎 Value Classes — Android Interview Bible (2026 Edition)

> Complete Kotlin Value Classes guide for Android Developers (8+ Years Experience). Learn `@JvmInline value class`, boxing & unboxing, JVM internals, Compose, Room, Retrofit, KMP, Serialization, performance, best practices, production pitfalls, and interview questions.

**Module:** Kotlin OOP

**File:** `15-Value-Classes.md`

**Difficulty:** Intermediate → Staff Android Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • Amazon • Microsoft • PhonePe • CRED • Flipkart • Meesho

---

# 📚 Chapter Roadmap

## Part 1 — Fundamentals

1. What is a Value Class?
2. Why Kotlin Introduced Value Classes?
3. Value Class vs Data Class vs Type Alias.
4. Creating Value Classes.
5. Rules & Limitations.
6. Methods, Validation & Business Logic.
7. Equality, HashCode & toString.
8. Android Domain Modeling Examples.

## Part 2 — JVM Internals

9. `@JvmInline` Explained.
10. JVM Representation.
11. Boxing vs Unboxing.
12. Nullable Value Classes.
13. Generics & Boxing.
14. Interface Boxing.
15. JVM Bytecode Deep Dive.

## Part 3 — Android Production Usage

16. UserId, Email, PhoneNumber.
17. Money & Currency.
18. Auth Tokens.
19. URLs & Product IDs.
20. Navigation Compose.
21. Compose Stability.
22. `mutableStateOf` + Value Classes.
23. Room Integration.
24. Retrofit Integration.
25. Kotlinx Serialization.
26. Parcelable.

## Part 4 — Kotlin Multiplatform

27. expect/actual.
28. Swift Interop.
29. Shared Domain Models.
30. Serialization Across Platforms.

## Part 5 — Performance & Interview Mastery

31. Allocation Benchmarks.
32. GC Analysis.
33. Memory Model.
34. Production Pitfalls.
35. Best Practices.
36. 40+ Interview Questions.
37. Ultimate Cheat Sheet.
38. Revision Summary.

# 💎 Value Classes — Android Interview Bible (2026 Edition)

> **Part 1 — Fundamentals & Domain Modeling**
>
> Complete Kotlin Value Classes guide for Android Developers (8+ Years Experience). Learn `@JvmInline value class`, domain modeling, validation, equality, rules, Android production examples, best practices, and interview questions.

---

## 📌 Module Information

| Property | Value |
|----------|-------|
| **Module** | Kotlin OOP |
| **File** | `15-Value-Classes.md` |
| **Level** | Intermediate → Senior Android Engineer |
| **Interview Frequency** | ⭐⭐⭐⭐⭐ Very High |
| **Companies** | Google, Uber, Amazon, Microsoft, PhonePe, CRED, Flipkart, Meesho |

---

# 📚 Chapter Roadmap

## Part 1 — Fundamentals (This File)

1. What is a Value Class?
2. Why Kotlin Introduced Value Classes
3. Primitive Obsession Problem
4. Value Class vs Data Class vs Type Alias
5. Creating Value Classes
6. Rules & Limitations
7. Constructors, Validation & Business Logic
8. Properties, Methods & Companion Objects
9. Equality, HashCode & toString
10. Android Domain Modeling Examples
11. Best Practices
12. Common Pitfalls
13. Interview Questions
14. Cheat Sheet

> **Upcoming Parts**
>
> - Part 2 → JVM Internals, Boxing, Unboxing & Bytecode
> - Part 3 → Compose, Room, Retrofit, Serialization & KMP
> - Part 4 → Performance, Memory, Testing & Interview Mastery

---

# 1️⃣ What is a Value Class? ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Google Pay UPI ID

Imagine you're building **Google Pay**.

A user enters a UPI ID.

```text
vikash@oksbi
```

Technically it's just a `String`.

Now look at this API.

```kotlin
fun sendMoney(
    receiver: String,
    amount: Double
)
```

Everything works...

Until someone accidentally passes an email.

```kotlin
sendMoney(
    receiver = "vikash@gmail.com",
    amount = 100.0
)
```

The compiler allows it.

But an **Email is NOT a UPI ID**.

A **Phone Number is NOT an Account Number**.

A **JWT Token is NOT a User ID**.

All of them are `String`.

This is called **Primitive Obsession**.

Kotlin Value Classes solve this problem.

---

## Definition

A **Value Class** creates a **new domain-specific type** around a single immutable value.

```kotlin
@JvmInline
value class UpiId(
    val value: String
)
```

Now your API becomes:

```kotlin
fun sendMoney(
    receiver: UpiId,
    amount: Double
)
```

Now only `UpiId` is accepted.

---

## Why "Value" Class?

A Value Class has **no meaningful identity**.

Only the wrapped value matters.

Unlike regular wrapper classes, Kotlin can optimize it into the underlying value.

---

## Basic Example

```kotlin
@JvmInline
value class UserId(
    val value: String
)

fun getUser(id: UserId) {
    println("Fetching user ${id.value}")
}

fun main() {

    val id = UserId("USR_1001")

    getUser(id)
}
```

### Output

```text
Fetching user USR_1001
```

---

## Why Android Developers Should Care?

Use Value Classes for:

- UserId
- ProductId
- OrderId
- Email
- PhoneNumber
- AuthToken
- CurrencyCode
- Url
- OTP
- Password

This dramatically improves **compile-time safety**.

---

# 2️⃣ Why Kotlin Introduced Value Classes? ⭐⭐⭐⭐⭐

## The Primitive Obsession Problem

Everything becomes a primitive.

```kotlin
fun login(
    email: String,
    password: String
)
```

Looks fine.

Now a mistake happens.

```kotlin
login(
    email = "password123",
    password = "vikash@gmail.com"
)
```

Compiler accepts it.

Runtime bug.

---

## Value Class Solution

```kotlin
@JvmInline
value class Email(val value: String)

@JvmInline
value class Password(val value: String)

fun login(
    email: Email,
    password: Password
)
```

Now this won't compile.

---

## Another Android Example — Swiggy

```kotlin
fun assignOrder(
    orderId: String,
    partnerId: String
)
```

Accidental swap.

```kotlin
assignOrder(partnerId, orderId)
```

Compiler accepts.

### Better

```kotlin
@JvmInline
value class OrderId(val value: String)

@JvmInline
value class DeliveryPartnerId(val value: String)

fun assignOrder(
    orderId: OrderId,
    partnerId: DeliveryPartnerId
)
```

Compiler catches mistakes immediately.

---

## Benefits

| Benefit | Why Important |
|---------|---------------|
| Compile-time Safety | Prevents mixing different String values. |
| Better Readability | API becomes self-documenting. |
| Domain Modeling | Represents business concepts. |
| Performance | Wrapper allocation often removed. |
| Maintainability | Fewer runtime bugs. |

---

# 3️⃣ Primitive Obsession — Deep Dive ⭐⭐⭐⭐⭐

Primitive Obsession is a common software design smell.

Instead of modeling business concepts...

Developers use primitive values.

### Bad Example

```kotlin
fun registerUser(
    username: String,
    phone: String,
    otp: String
)
```

Every parameter is `String`.

---

## Better Domain Model

```kotlin
@JvmInline
value class Username(val value: String)

@JvmInline
value class PhoneNumber(val value: String)

@JvmInline
value class OtpCode(val value: String)

fun registerUser(
    username: Username,
    phone: PhoneNumber,
    otp: OtpCode
)
```

Now the compiler understands business intent.

---

## Clean Architecture Example

```text
Presentation
      │
      ▼
UseCase
      │
      ▼
Domain
 ├── UserId
 ├── Email
 ├── PhoneNumber
 ├── CurrencyCode
 └── Money
```

Value Classes belong inside the **Domain Layer**.

---

# 4️⃣ Value Class vs Data Class vs Type Alias ⭐⭐⭐⭐⭐

One of the most frequently asked Kotlin interview questions.

## Complete Comparison

| Feature | Value Class | Data Class | Type Alias |
|--------|-------------|------------|------------|
| Creates New Type | ✅ Yes | ✅ Yes | ❌ No |
| Multiple Properties | ❌ No | ✅ Yes | Depends |
| Runtime Wrapper Object | Usually No | Yes | No |
| Identity | No | Yes | Original Type |
| Copy Function | No | Yes | No |
| Destructuring | No | Yes | Same as Original Type |
| Type Safety | Strong | Strong | Weak |
| Performance | Excellent | Moderate | Excellent |

---

## Type Alias Example

```kotlin
typealias UserId = String

fun fetchUser(id: UserId)
```

Compiler sees only:

```kotlin
fun fetchUser(id: String)
```

No new type created.

---

## Data Class Example

```kotlin
data class UserId(
    val value: String
)
```

Creates wrapper object.

Supports:

- copy()
- component1()
- equals()
- hashCode()
- toString()

---

## Value Class Example

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

Creates a new type.

Compiler optimizes away wrapper object in many JVM scenarios.

---

## Visual Comparison

### Type Alias

```text
UserId
   │
   ▼
String
```

Exactly same type.

### Data Class

```text
UserId Object
      │
      ▼
String
```

Wrapper object exists.

### Value Class

```text
UserId
Compiler Optimization
      │
      ▼
String
```

Wrapper often removed.

---

# 5️⃣ Creating Value Classes ⭐⭐⭐⭐⭐

## Basic Syntax

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

---

## Int Example

```kotlin
@JvmInline
value class Age(
    val value: Int
)
```

---

## Long Example

```kotlin
@JvmInline
value class Timestamp(
    val value: Long
)
```

---

## Double Example

```kotlin
@JvmInline
value class Price(
    val value: Double
)
```

---

## Boolean Example

```kotlin
@JvmInline
value class IsPremium(
    val value: Boolean
)
```

---

## UUID Example

```kotlin
@JvmInline
value class SessionId(
    val value: UUID
)
```

Useful for backend-driven Android apps.

---

# 6️⃣ Rules & Limitations ⭐⭐⭐⭐⭐

These rules are very common interview questions.

---

## Rule 1 — Exactly One Property

✅ Valid

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

❌ Invalid

```kotlin
@JvmInline
value class User(
    val id: String,
    val name: String
)
```

Compilation error.

---

## Rule 2 — Property Must Be `val`

```kotlin
@JvmInline
value class Email(
    val value: String
)
```

`var` isn't allowed.

---

## Rule 3 — Cannot Extend Classes

```kotlin
open class Person

@JvmInline
value class UserId(
    val value: String
) : Person()
```

Compilation error.

---

## Rule 4 — Can Implement Interfaces

```kotlin
interface Printable {
    fun print()
}

@JvmInline
value class UserId(
    val value: String
) : Printable {

    override fun print() {
        println(value)
    }
}
```

---

## Rule 5 — Can Have Companion Objects

```kotlin
@JvmInline
value class Currency(
    val value: String
){

    companion object {
        val INR = Currency("INR")
        val USD = Currency("USD")
    }
}
```

---

## Rule 6 — Can Have Business Logic

```kotlin
@JvmInline
value class Email(
    val value: String
){

    fun domain(): String =
        value.substringAfter("@")
}
```

---

## Rule 7 — Can Have `init` Block

```kotlin
@JvmInline
value class PinCode(
    val value: String
){

    init {
        require(value.length == 6)
    }
}
```

Validation happens during object creation.

---

# 7️⃣ Constructors, Validation & Business Logic ⭐⭐⭐⭐⭐

## 🎭 Story — Banking Application

An account number must always contain **12 digits**.

Instead of validating everywhere...

Validate once inside the Value Class.

---

## Account Number Example

```kotlin
@JvmInline
value class AccountNumber(
    val value: String
){

    init {
        require(
            value.matches(Regex("\\d{12}"))
        ) {
            "Account number must contain exactly 12 digits."
        }
    }
}
```

Usage

```kotlin
val account = AccountNumber("123456789012")
```

---

## Email Validation

```kotlin
@JvmInline
value class Email(
    val value: String
){

    init {
        require("@" in value) {
            "Invalid email address."
        }
    }
}
```

---

## OTP Validation

```kotlin
@JvmInline
value class OtpCode(
    val value: String
){

    init {
        require(value.length == 6)
    }
}
```

---

## Money Validation

```kotlin
@JvmInline
value class Money(
    val amount: Double
){

    init {
        require(amount >= 0)
    }
}
```

---

## GPS Coordinates

```kotlin
@JvmInline
value class Latitude(
    val value: Double
){

    init {
        require(value in -90.0..90.0)
    }
}

@JvmInline
value class Longitude(
    val value: Double
){

    init {
        require(value in -180.0..180.0)
    }
}
```

Useful in Google Maps apps.

---

# 8️⃣ Properties, Methods & Companion Objects ⭐⭐⭐⭐⭐

Value Classes can contain helper methods.

---

## Phone Number Formatting

```kotlin
@JvmInline
value class PhoneNumber(
    val value: String
){

    fun formatted() = "+91 $value"
}
```

Usage

```kotlin
PhoneNumber("9876543210").formatted()
```

Output

```text
+91 9876543210
```

---

## Masking Account Number

```kotlin
@JvmInline
value class AccountNumber(
    val value: String
){

    fun masked() =
        "XXXXXX${value.takeLast(6)}"
}
```

Output

```text
XXXXXX789012
```

---

## Computed Property

```kotlin
@JvmInline
value class Currency(
    val code: String
){

    val symbol: String
        get() = when(code){
            "INR" -> "₹"
            "USD" -> "$"
            "EUR" -> "€"
            else -> code
        }
}
```

---

## Extension Function

```kotlin
fun UserId.isGuest(): Boolean {
    return value.startsWith("GUEST")
}
```

Usage

```kotlin
UserId("GUEST_123").isGuest()
```

---

## Companion Object Example

```kotlin
@JvmInline
value class Currency(
    val value: String
){

    companion object {

        val INR = Currency("INR")
        val USD = Currency("USD")
        val EUR = Currency("EUR")
    }
}
```

Usage

```kotlin
Currency.INR
Currency.USD
```

---

## Factory Function

```kotlin
@JvmInline
value class Percentage(
    val value: Int
){

    companion object {

        fun fromDecimal(decimal: Double): Percentage {
            return Percentage((decimal * 100).toInt())
        }
    }
}
```

Usage

```kotlin
Percentage.fromDecimal(0.85)
```

Output

```text
85
```

---

# 9️⃣ Equality, HashCode & toString ⭐⭐⭐⭐⭐

## Equality

```kotlin
@JvmInline
value class UserId(
    val value: String
)

val first = UserId("101")
val second = UserId("101")

println(first == second)
```

Output

```text
true
```

Equality is based on the underlying value.

---

## Identity Check

```kotlin
println(first === second)
```

Compilation error.

Value Classes don't have reference identity.

---

## HashCode

```kotlin
println(first.hashCode())
```

Delegates to the wrapped value.

---

## toString

```kotlin
println(UserId("USR_101"))
```

Output

```text
UserId(value=USR_101)
```

Useful for debugging.

---

# 🔟 Android Domain Modeling Examples ⭐⭐⭐⭐⭐

This is where Value Classes shine in production Android apps.

---

## UserId

```kotlin
@JvmInline
value class UserId(val value: String)
```

Repository

```kotlin
interface UserRepository {

    suspend fun getUser(id: UserId): User
}
```

---

## OrderId

```kotlin
@JvmInline
value class OrderId(val value: String)
```

---

## ProductId

```kotlin
@JvmInline
value class ProductId(val value: String)
```

---

## AuthToken

```kotlin
@JvmInline
value class AuthToken(val value: String)
```

Interceptor

```kotlin
request.newBuilder()
    .header("Authorization", token.value)
```

---

## CurrencyCode

```kotlin
@JvmInline
value class CurrencyCode(val value: String)
```

Usage

```kotlin
getPrice(
    currency = CurrencyCode("INR")
)
```

---

## PhoneNumber

```kotlin
@JvmInline
value class PhoneNumber(val value: String)
```

---

## ProductSku

```kotlin
@JvmInline
value class ProductSku(val value: String)
```

Great for inventory systems.

---

## Clean Architecture Folder Structure

```text
domain/
├── model/
│   ├── UserId.kt
│   ├── OrderId.kt
│   ├── ProductId.kt
│   ├── Email.kt
│   ├── PhoneNumber.kt
│   ├── AuthToken.kt
│   ├── Money.kt
│   ├── CurrencyCode.kt
│   └── Percentage.kt
```

---

## UseCase Example

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
){

    suspend operator fun invoke(
        email: Email,
        password: Password
    ): AuthToken {
        return repository.login(email, password)
    }
}
```

Domain APIs become self-documenting.

---

# 1️⃣1️⃣ Best Practices ⭐⭐⭐⭐⭐

## ✅ Use Value Classes For

- UserId
- ProductId
- OrderId
- Email
- Password
- Phone Number
- OTP
- CurrencyCode
- Money
- Distance
- Temperature
- Latitude
- Longitude
- Product SKU
- AuthToken

---

## ❌ Avoid Value Classes For

- RecyclerView UI Models.
- Compose Screen State.
- Network DTOs with multiple fields.
- Mutable objects.
- Objects with more than one property.

Use **Data Classes** instead.

---

## Android Recommendation

Use Value Classes primarily inside the **Domain Layer** of Clean Architecture.

---

# 1️⃣2️⃣ Common Production Pitfalls

## Pitfall 1 — Using Data Class Instead

```kotlin
data class UserId(
    val value: String
)
```

Unnecessary wrapper allocation.

---

## Pitfall 2 — Using TypeAlias for Domain Types

```kotlin
typealias UserId = String
```

No compile-time safety.

---

## Pitfall 3 — Multiple String Parameters

```kotlin
fun createOrder(
    userId: String,
    productId: String
)
```

Easy to swap values.

---

## Better

```kotlin
fun createOrder(
    userId: UserId,
    productId: ProductId
)
```

Compiler prevents mistakes.

---

# 1️⃣3️⃣ Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Basics

1. What is a Value Class?
2. Why was `@JvmInline` introduced?
3. Difference between Value Class and Data Class?
4. Difference between Value Class and Type Alias?

## Rules

5. Can Value Classes have multiple properties?
6. Can they have methods?
7. Can they have companion objects?
8. Can they extend classes?
9. Can they implement interfaces?
10. Can they have `init` blocks?

## Domain Modeling

11. Why use Value Classes for IDs?
12. How do Value Classes improve API readability?
13. Where should Value Classes live in Clean Architecture?

## Android

14. Where would you use Value Classes in Android apps?
15. Would you use them for Room entities?
16. Would you use them for Compose state?

---

# 📋 Cheat Sheet (Part 1)

## Basic Syntax

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

---

## Validation

```kotlin
init {
    require(value.isNotBlank())
}
```

---

## Method

```kotlin
fun masked() =
    "XXXX$value"
```

---

## Companion Object

```kotlin
companion object {
    val INR = Currency("INR")
}
```

---

## Comparison Table

| Feature | Value Class | Data Class | Type Alias |
|--------|-------------|------------|------------|
| New Type | ✅ | ✅ | ❌ |
| Wrapper Object | Usually No | Yes | No |
| Multiple Fields | ❌ | ✅ | Depends |
| Type Safety | Strong | Strong | Weak |
| Performance | Excellent | Moderate | Excellent |

---

## Android Use Cases

| Use Case | Recommendation |
|----------|----------------|
| User ID | ✅ Value Class |
| Email | ✅ Value Class |
| Phone Number | ✅ Value Class |
| Auth Token | ✅ Value Class |
| Currency Code | ✅ Value Class |
| GPS Coordinates | ✅ Value Class |
| UI State Model | ❌ Data Class |
| Network DTO | ⚠️ Depends |

---

# 📝 Revision Summary

In this chapter you learned:

- Why Kotlin introduced **Value Classes**.
- The **Primitive Obsession** problem.
- Value Class vs Data Class vs Type Alias.
- Syntax, rules, and limitations.
- Validation using `init`.
- Business logic inside Value Classes.
- Equality, `hashCode()`, and `toString()`.
- Real Android domain modeling examples.
- Best practices for Clean Architecture.
- Common production pitfalls.
- Frequently asked Android interview questions.

---

---

# Part 2 — JVM Internals, Boxing vs Unboxing & Bytecode Mastery

> This section explains how Kotlin Value Classes are represented inside the JVM, when wrapper objects are created, how boxing/unboxing works, nullable behavior, generics, interfaces, and bytecode generation.

---

# 📚 Table of Contents

15. `@JvmInline` Explained
16. JVM Representation
17. Boxing vs Unboxing
18. Nullable Value Classes
19. Generic Boxing
20. Interface Boxing
21. Collections Behaviour
22. Reflection Behaviour
23. Java Interoperability
24. JVM Bytecode Deep Dive
25. Performance Discussion
26. Best Practices
27. Common Pitfalls
28. Interview Questions
29. Cheat Sheet

---

# 15. `@JvmInline` Explained ⭐⭐⭐⭐⭐

## 🎭 Story — Shipping Label Without a Box

Imagine Amazon ships millions of products every day.

Sometimes a package only contains a **shipping label**.

Instead of sending a separate box, Amazon prints only the label.

That saves:

- Packaging material.
- Storage.
- Shipping cost.

Value Classes work similarly.

Instead of creating a wrapper object, Kotlin tries to send **only the underlying value**.

---

## Why `@JvmInline`?

`value class` exists at the Kotlin language level.

`@JvmInline` tells the JVM compiler:

> Optimize this class as its underlying value whenever possible.

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

---

## Without `@JvmInline`

Older Kotlin versions used **inline classes**.

```kotlin
inline class UserId(val value: String)
```

This syntax is deprecated.

Modern Kotlin uses:

```kotlin
@JvmInline
value class UserId(val value: String)
```

---

## Compiler Optimization

```kotlin
fun printUser(id: UserId)
```

Compiler often generates:

```java
void printUser(String id)
```

No wrapper object.

---

## Why Android Developers Should Care?

This optimization reduces:

- Heap allocation.
- Garbage Collection pressure.
- Memory usage.
- CPU overhead.

Especially useful in RecyclerView, Compose, Flow, and networking layers.

---

# 16. JVM Representation ⭐⭐⭐⭐⭐

## How JVM Sees Value Classes

Kotlin source:

```kotlin
@JvmInline
value class UserId(
    val value: String
)

fun fetch(id: UserId) {}
```

---

### Kotlin View

```text
UserId
 └── String value
```

---

### JVM Optimized View

```text
fetch(String id)
```

The wrapper disappears.

---

## Heap Visualization

### Data Class Representation

```text
Heap

UserId Object
     │
     ▼
 String Object
```

Two objects exist.

---

### Value Class Representation

```text
Heap

String Object
```

Only underlying value.

---

## Primitive Value Example

```kotlin
@JvmInline
value class Age(val value: Int)
```

Compiler can represent it directly as JVM `int`.

---

### Memory Comparison

| Type | Heap Allocation |
|------|-----------------|
| `Int` | No |
| `Age` (Unboxed) | No |
| `Age?` | Yes |
| `Age` inside `Any` | Yes |

---

## Multiple Examples

### Double

```kotlin
@JvmInline
value class Price(val value: Double)
```

Optimized to JVM `double`.

---

### Long

```kotlin
@JvmInline
value class Timestamp(val value: Long)
```

Optimized to JVM `long`.

---

# 17. Boxing vs Unboxing ⭐⭐⭐⭐⭐

This is one of Google's favorite interview questions.

---

## What is Unboxing?

Compiler removes wrapper.

```kotlin
@JvmInline
value class UserId(val value: String)

fun fetch(id: UserId)
```

JVM receives:

```text
String
```

No allocation.

---

## What is Boxing?

Compiler creates wrapper object.

Example:

```kotlin
val any: Any = UserId("USR1")
```

Needs object.

---

## Boxing Visualization

```text
Any
 │
 ▼
UserId Wrapper
 │
 ▼
String
```

Wrapper created.

---

## When Does Boxing Happen?

| Scenario | Boxing? |
|----------|----------|
| Function Parameter | ❌ Usually No |
| Return Type | ❌ Usually No |
| Generic Type | ✅ Yes |
| Nullable Type | ✅ Yes |
| Interface Reference | ✅ Yes |
| `Any` | ✅ Yes |
| Reflection | ✅ Yes |

Remember this table.

---

## Example — Any

```kotlin
val id = UserId("USR101")

val value: Any = id
```

Wrapper allocated.

---

## Example — List

```kotlin
val ids = listOf(UserId("A"), UserId("B"))
```

Generic collection.

Wrapper objects created.

---

## Example — Generic Function

```kotlin
fun <T> printValue(value: T) {
    println(value)
}

printValue(UserId("123"))
```

Compiler boxes `UserId`.

---

# 18. Nullable Value Classes ⭐⭐⭐⭐⭐

## Why Nullable Causes Boxing

Nullable values need an object representation.

```kotlin
val id: UserId? = UserId("USR101")
```

Cannot be represented as plain String because `null` must also exist.

---

## Visualization

```text
Nullable Wrapper

UserId?

 ├── String Value
 └── null
```

---

## Example

```kotlin
fun printId(id: UserId?) {
    println(id)
}
```

Parameter becomes boxed.

---

## Best Practice

Prefer non-null value classes whenever possible.

Instead of

```kotlin
UserId?
```

Consider

```kotlin
UserId.EMPTY
```

Factory constant.

---

# 19. Generic Boxing ⭐⭐⭐⭐⭐

Generics erase type information.

---

## Generic Example

```kotlin
fun <T> cache(item: T) {}
```

Calling

```kotlin
cache(UserId("123"))
```

Boxes value.

---

## Generic List

```kotlin
val users: List<UserId> = listOf(...)
```

Every element boxed.

---

## MutableList Example

```kotlin
val ids = mutableListOf<UserId>()
```

Wrapper stored inside collection.

---

## Why?

JVM generics work with objects.

Primitive optimization isn't possible here.

---

## Interview Note ⭐⭐⭐⭐⭐

> Value Classes lose their allocation optimization when used as generic type arguments.

---

# 20. Interface Boxing ⭐⭐⭐⭐⭐

Value Classes can implement interfaces.

```kotlin
interface Printable {
    fun print()
}

@JvmInline
value class UserId(
    val value: String
) : Printable {

    override fun print() {
        println(value)
    }
}
```

---

## Interface Reference

```kotlin
val printable: Printable = UserId("USR101")
```

Compiler boxes value.

---

## Why?

Interface references require objects.

---

## Android Example

```kotlin
val logger: Logger = UserId("USR1")
```

Boxing occurs.

---

# 21. Collections Behaviour ⭐⭐⭐⭐

Collections always use generics.

---

## List

```kotlin
val ids = listOf(
    UserId("A"),
    UserId("B")
)
```

Wrapper objects inside list.

---

## Set

```kotlin
val users = hashSetOf(UserId("A"))
```

Boxed.

---

## Map

```kotlin
val map = mapOf(
    UserId("101") to "Vikash"
)
```

Boxed key.

---

## Sequence

```kotlin
sequenceOf(UserId("1"))
```

Still boxed.

---

## Recommendation

Collections prioritize flexibility over allocation optimization.

---

# 22. Reflection Behaviour ⭐⭐⭐⭐

Reflection requires metadata.

---

## Example

```kotlin
UserId::class
```

Reflection creates wrapper metadata.

---

## Property Reflection

```kotlin
UserId::value
```

Works because wrapper metadata exists.

---

## Why Boxing?

Reflection APIs operate on objects.

---

# 23. Java Interoperability ⭐⭐⭐⭐⭐

Very important Android interview topic.

---

## Kotlin

```kotlin
@JvmInline
value class UserId(val value: String)

fun fetch(id: UserId)
```

---

## Java View

```java
void fetch(String id)
```

---

## Calling Kotlin from Java

```java
fetch("USR101");
```

Java doesn't know Value Class type.

---

## Java Limitation

No compile-time safety in Java.

---

## Android SDK Interop

Retrofit, Room, and Android APIs see underlying value after compiler adaptation.

---

# 24. JVM Bytecode Deep Dive ⭐⭐⭐⭐⭐

## Kotlin Source

```kotlin
@JvmInline
value class UserId(
    val value: String
)

fun printUser(id: UserId) {
    println(id)
}
```

---

## Generated Methods

Compiler generates synthetic methods.

```text
UserId

box-impl()

unbox-impl()

equals-impl()

hashCode-impl()

toString-impl()
```

---

## Simplified Decompiled Java

```java
public final class UserId {

    private final String value;

    private UserId(String value){
        this.value = value;
    }

    public static String unbox_impl(UserId id){
        return id.value;
    }

    public static UserId box_impl(String value){
        return new UserId(value);
    }
}
```

---

## Important Synthetic Methods

| Method | Purpose |
|--------|---------|
| `box-impl()` | Creates wrapper. |
| `unbox-impl()` | Returns underlying value. |
| `equals-impl()` | Equality. |
| `hashCode-impl()` | Hash calculation. |
| `toString-impl()` | String representation. |

---

## Interview Tip

> Compiler generates helper methods instead of exposing constructors directly.

---

# 25. Performance Discussion ⭐⭐⭐⭐⭐

## Story — RecyclerView with 20,000 Items

Imagine every product has:

- ProductId
- SellerId
- CategoryId
- OrderId

Using Data Classes.

```text
80,000 Wrapper Objects
```

Using Value Classes.

```text
Underlying primitive/string values whenever possible.
```

Huge memory improvement.

---

## Allocation Comparison

| Representation | Allocation |
|---------------|------------|
| Data Class | Always |
| Value Class (Direct) | Usually None |
| Value Class (Generic) | Yes |
| Value Class (Nullable) | Yes |
| Value Class (`Any`) | Yes |

---

## Why Faster?

- Less heap allocation.
- Better CPU cache locality.
- Less GC.
- Smaller memory footprint.

---

## Android Use Cases

- Compose state models.
- Navigation IDs.
- Repository IDs.
- Network tokens.
- Paging IDs.

---

# 26. Best Practices ⭐⭐⭐⭐⭐

## ✅ Prefer Unboxed Usage

```kotlin
fun getUser(id: UserId)
```

---

## Avoid Nullable

```kotlin
UserId?
```

Use sentinel values if appropriate.

---

## Avoid Generics in Performance-Critical Paths

```kotlin
List<UserId>
```

Boxes values.

---

## Keep Value Classes Immutable

Always `val`.

---

## Use Domain Types

- UserId
- ProductId
- Email
- CurrencyCode
- AuthToken

---

# 27. Common Production Pitfalls

## Pitfall 1 — Nullable Boxing

```kotlin
UserId?
```

Allocates wrapper.

---

## Pitfall 2 — Generic APIs

```kotlin
fun <T> save(item: T)
```

Boxes Value Class.

---

## Pitfall 3 — Interface References

```kotlin
Printable = UserId(...)
```

Boxing.

---

## Pitfall 4 — Expecting Java Type Safety

Java sees underlying type.

---

# 28. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## JVM

1. What does `@JvmInline` do?
2. What is boxing?
3. What is unboxing?
4. When are Value Classes boxed?
5. Why do nullable Value Classes allocate objects?

## Performance

6. Why are Value Classes faster than Data Classes?
7. Do Value Classes always avoid allocation?
8. What happens inside collections?

## Android

9. Does Room store Value Classes directly?
10. Does Retrofit understand Value Classes?
11. Can Compose optimize Value Classes?

## Bytecode

12. What synthetic methods does compiler generate?
13. What is `box-impl()`?
14. What is `unbox-impl()`?

---

# 📋 Cheat Sheet (Part 2)

## Boxing Rules

| Scenario | Boxed |
|----------|--------|
| Direct Function Parameter | ❌ |
| Return Type | ❌ |
| Generic | ✅ |
| Nullable | ✅ |
| Interface | ✅ |
| Any | ✅ |
| Reflection | ✅ |

---

## Generated Compiler Methods

```text
box-impl()
unbox-impl()
equals-impl()
hashCode-impl()
toString-impl()
```

---

## JVM Representation

```text
Kotlin Source

UserId("101")

        │

Compiler

        ▼

String ("101")
```

---

## Performance Summary

- Value Classes are optimized away in many JVM scenarios.
- Boxing happens only when object semantics are required.
- Avoid nullable and generic usage in performance-critical code.

---

# 📝 Revision Summary

In this part you learned:

- Why `@JvmInline` exists.
- JVM representation of Value Classes.
- Boxing vs unboxing rules.
- Nullable behavior.
- Generic boxing.
- Interface boxing.
- Reflection behavior.
- Java interoperability.
- JVM bytecode generated by Kotlin compiler.
- Performance characteristics.
- Common interview questions and best practices.


# Part 3 — Android Integration (Compose, Room, Retrofit, Serialization, Parcelable & KMP)

> Learn how Kotlin Value Classes integrate with **Jetpack Compose, Room, Retrofit, OkHttp, Navigation Compose, Kotlinx Serialization, Parcelable, and Kotlin Multiplatform (KMP)**. This section focuses entirely on **real Android production usage**.

---

# 📚 Table of Contents

15. Value Classes in Android Architecture
16. Jetpack Compose Integration
17. Compose Stability & Recomposition
18. `mutableStateOf()` with Value Classes
19. Navigation Compose
20. Room Database Integration
21. Retrofit Integration
22. OkHttp & Authentication Tokens
23. Kotlinx Serialization
24. Parcelable Support
25. Kotlin Multiplatform (KMP)
26. Production Domain Modeling
27. Android Best Practices
28. Common Production Pitfalls
29. Senior Interview Questions
30. Cheat Sheet

---

# 15. Value Classes in Android Architecture ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Building PhonePe

Imagine you're building **PhonePe**.

Every feature has its own identifiers.

```text
Authentication
├── UserId
├── SessionId
└── AuthToken

Payments
├── TransactionId
├── UpiId
└── MerchantId

Commerce
├── OrderId
├── ProductId
└── CouponCode
```

Without Value Classes, every ID becomes a `String`.

That makes accidental mistakes easy.

```kotlin
fun fetchOrder(
    userId: String,
    orderId: String
)
```

This compiles.

```kotlin
fetchOrder(
    userId = orderId,
    orderId = userId
)
```

Wrong values. No compiler error.

---

## Strongly Typed API

```kotlin
@JvmInline
value class UserId(val value: String)

@JvmInline
value class OrderId(val value: String)

fun fetchOrder(
    userId: UserId,
    orderId: OrderId
)
```

Now the compiler prevents accidental parameter swapping.

---

## Recommended Clean Architecture Structure

```text
domain/
│
├── model/
│   ├── UserId.kt
│   ├── OrderId.kt
│   ├── ProductId.kt
│   ├── MerchantId.kt
│   ├── AuthToken.kt
│   ├── Email.kt
│   ├── PhoneNumber.kt
│   ├── Money.kt
│   ├── CurrencyCode.kt
│   └── Percentage.kt
│
├── repository/
├── usecase/
└── mapper/
```

---

## Example Repository

```kotlin
interface PaymentRepository {

    suspend fun createPayment(
        merchantId: MerchantId,
        amount: Money,
        currency: CurrencyCode
    ): TransactionId
}
```

This API is self-documenting.

---

# 16. Jetpack Compose Integration ⭐⭐⭐⭐⭐

Jetpack Compose works extremely well with Value Classes because they are immutable.

## Basic Compose Example

```kotlin
@JvmInline
value class Username(
    val value: String
)

@Composable
fun ProfileHeader(
    username: Username
) {
    Text(text = username.value)
}
```

Usage

```kotlin
ProfileHeader(
    username = Username("vikash")
)
```

---

## Why Not Use String?

```kotlin
@Composable
fun ProfileHeader(
    username: String
)
```

Nothing prevents passing an email.

```kotlin
ProfileHeader("vikash@gmail.com")
```

Using `Username` prevents this.

---

## Compose UI State

```kotlin
data class ProfileUiState(

    val userId: UserId,

    val username: Username,

    val isPremium: IsPremium
)
```

Domain concepts remain strongly typed even inside UI.

---

# 17. Compose Stability & Recomposition ⭐⭐⭐⭐⭐

One of the most frequently asked senior Compose interview questions.

## 🎭 Story — Instagram Feed

100 product cards are displayed.

Only Product #25 changes.

Compose should only recompose that card.

Immutable Value Classes help Compose compare values efficiently.

---

## Stable Parameter Example

```kotlin
@JvmInline
value class ProductId(
    val value: String
)

@Composable
fun ProductCard(
    productId: ProductId
){
    Text(productId.value)
}
```

Compose treats immutable parameters efficiently.

---

## Why Value Classes Are Good for Compose

- Immutable.
- No mutable internal state.
- Easy equality comparison.
- Small memory footprint.

---

## Recommendation

Use Value Classes for identifiers and domain values inside Compose state models.

---

# 18. mutableStateOf() with Value Classes ⭐⭐⭐⭐⭐

This is how you'll use Value Classes daily.

## Username State

```kotlin
@JvmInline
value class Username(
    val value: String
)

@Composable
fun UsernameEditor(){

    var username by remember {
        mutableStateOf(
            Username("")
        )
    }

    TextField(
        value = username.value,
        onValueChange = {
            username = Username(it)
        }
    )
}
```

---

## Email State

```kotlin
var email by remember {
    mutableStateOf(
        Email("")
    )
}
```

Validation stays inside `Email`.

---

## Money State

```kotlin
var amount by remember {
    mutableStateOf(Money(0.0))
}
```

Usage

```kotlin
amount = Money(500.0)
```

---

## Why Better?

Instead of exposing primitive values throughout UI, Compose works with business types.

---

# 19. Navigation Compose ⭐⭐⭐⭐⭐

Navigation arguments are strings.

Convert them immediately into Value Classes.

---

## Navigate

```kotlin
navController.navigate(
    "profile/${userId.value}"
)
```

---

## Destination

```kotlin
composable("profile/{userId}") { entry ->

    val userId = UserId(
        entry.arguments!!.getString("userId")!!
    )

    ProfileScreen(userId)
}
```

---

## Extension Function

```kotlin
fun NavBackStackEntry.userId(): UserId {
    return UserId(
        arguments!!.getString("userId")!!
    )
}
```

Usage

```kotlin
val userId = backStackEntry.userId()
```

Cleaner navigation layer.

---

## Best Practice

**Boundary Rule**

```text
Navigation Route

        ↓

String Argument

        ↓

Value Class

        ↓

Domain Layer
```

Only convert once.

---

# 20. Room Database Integration ⭐⭐⭐⭐⭐

Room stores primitive values.

Use `TypeConverter`.

---

## Entity Example

```kotlin
@Entity
data class UserEntity(

    @PrimaryKey
    val id: UserId,

    val email: Email,

    val phone: PhoneNumber
)
```

This won't compile without converters.

---

## TypeConverter

```kotlin
class UserConverters {

    @TypeConverter
    fun fromUserId(id: UserId): String {
        return id.value
    }

    @TypeConverter
    fun toUserId(value: String): UserId {
        return UserId(value)
    }

    @TypeConverter
    fun fromEmail(email: Email): String {
        return email.value
    }

    @TypeConverter
    fun toEmail(value: String): Email {
        return Email(value)
    }

    @TypeConverter
    fun fromPhone(phone: PhoneNumber): String {
        return phone.value
    }

    @TypeConverter
    fun toPhone(value: String): PhoneNumber {
        return PhoneNumber(value)
    }
}
```

---

## Register Converter

```kotlin
@Database(
    entities = [UserEntity::class],
    version = 1
)
@TypeConverters(UserConverters::class)
abstract class AppDatabase : RoomDatabase()
```

---

## Money Converter

```kotlin
class MoneyConverter {

    @TypeConverter
    fun fromMoney(money: Money): Double = money.value

    @TypeConverter
    fun toMoney(value: Double): Money = Money(value)
}
```

---

## Why Keep Value Classes in Entity?

Keeps database layer strongly typed.

---

# 21. Retrofit Integration ⭐⭐⭐⭐⭐

A production networking pattern.

## DTO Layer

```kotlin
@Serializable
data class UserResponse(

    val id: String,

    val email: String
)
```

---

## Domain Mapper

```kotlin
data class User(

    val id: UserId,

    val email: Email
)

fun UserResponse.toDomain() = User(
    id = UserId(id),
    email = Email(email)
)
```

---

## Request DTO

```kotlin
@Serializable
data class LoginRequest(
    val email: String,
    val password: String
)
```

---

## Domain Input

```kotlin
data class LoginInput(
    val email: Email,
    val password: Password
)

fun LoginInput.toRequest() =
    LoginRequest(
        email.value,
        password.value
    )
```

---

## Why Mapper Layer?

Keep networking independent from domain modeling.

---

## API Interface

```kotlin
interface AuthApi {

    @POST("login")
    suspend fun login(
        @Body request: LoginRequest
    ): LoginResponse
}
```

---

# 22. OkHttp & Authentication Tokens ⭐⭐⭐⭐⭐

Authentication tokens are an excellent Value Class.

## AuthToken

```kotlin
@JvmInline
value class AuthToken(
    val value: String
)
```

---

## RefreshToken

```kotlin
@JvmInline
value class RefreshToken(
    val value: String
)
```

Separate types prevent accidental misuse.

---

## Auth Interceptor

```kotlin
class AuthInterceptor(
    private val tokenProvider: () -> AuthToken
) : Interceptor {

    override fun intercept(
        chain: Interceptor.Chain
    ): Response {

        val request = chain.request()
            .newBuilder()
            .header(
                "Authorization",
                "Bearer ${tokenProvider().value}"
            )
            .build()

        return chain.proceed(request)
    }
}
```

---

## Refresh Token Request

```kotlin
data class RefreshRequest(
    val refreshToken: RefreshToken
)
```

Compile-time distinction between access and refresh tokens.

---

# 23. Kotlinx Serialization ⭐⭐⭐⭐⭐

Value Classes serialize as their underlying values.

---

## Serializable Value Class

```kotlin
@Serializable
@JvmInline
value class UserId(
    val value: String
)
```

---

## DTO Example

```kotlin
@Serializable
data class UserDto(

    val id: UserId,

    val email: Email
)
```

---

## JSON Output

```json
{
  "id": "USR101",
  "email": "vikash@gmail.com"
}
```

No wrapper object appears.

---

## Payment Example

```kotlin
@Serializable
data class PaymentRequest(

    val merchantId: MerchantId,

    val amount: Money,

    val currency: CurrencyCode
)
```

---

## Nested Serialization

Works automatically when nested inside serializable models.

---

# 24. Parcelable Support ⭐⭐⭐⭐⭐

Perfect for Navigation and Intents.

---

## Parcelize Example

```kotlin
@Parcelize
@JvmInline
value class UserId(
    val value: String
) : Parcelable
```

---

## Intent Example

```kotlin
intent.putExtra(
    "USER_ID",
    UserId("USR101")
)
```

Retrieve

```kotlin
val id =
    intent.getParcelableExtra<UserId>("USER_ID")
```

---

## SavedStateHandle Example

```kotlin
savedStateHandle["userId"] =
    UserId("USR101")
```

Great for ViewModels.

---

## Compose Navigation Saved State

```kotlin
savedStateHandle.get<UserId>("userId")
```

Works naturally.

---

# 25. Kotlin Multiplatform (KMP) ⭐⭐⭐⭐⭐

Value Classes are supported across platforms.

---

## Shared Module

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

Shared between:

- Android
- iOS
- Desktop
- Web

---

## Shared Domain Model

```kotlin
data class User(

    val id: UserId,

    val email: Email
)
```

Single source of truth.

---

## Swift Interoperability

Swift sees:

```swift
let id = UserId(value: "USR101")
```

Still strongly typed.

---

## Shared Repository

```kotlin
interface UserRepository {

    suspend fun user(
        id: UserId
    ): User
}
```

Shared business logic.

---

## Serialization Across Platforms

Value Classes serialize consistently using Kotlin Serialization.

Perfect for Ktor clients.

---

# 26. Production Domain Modeling ⭐⭐⭐⭐⭐

## Money

```kotlin
@JvmInline
value class Money(
    val value: Double
){

    operator fun plus(other: Money) =
        Money(value + other.value)

    operator fun minus(other: Money) =
        Money(value - other.value)
}
```

Usage

```kotlin
val total =
    Money(200.0) + Money(50.0)
```

---

## Percentage

```kotlin
@JvmInline
value class Percentage(
    val value: Int
){

    init {
        require(value in 0..100)
    }
}
```

---

## CouponCode

```kotlin
@JvmInline
value class CouponCode(
    val value: String
)
```

---

## Url

```kotlin
@JvmInline
value class Url(
    val value: String
){

    init {
        require(value.startsWith("https://"))
    }
}
```

---

## Distance

```kotlin
@JvmInline
value class DistanceKm(
    val value: Double
){

    fun meters() = value * 1000
}
```

---

## MerchantId

```kotlin
@JvmInline
value class MerchantId(
    val value: String
)
```

Useful for payment gateways.

---

# 27. Android Best Practices ⭐⭐⭐⭐⭐

## ✅ Use Value Classes For

| Domain Concept | Recommendation |
|---------------|----------------|
| UserId | ✅ |
| ProductId | ✅ |
| OrderId | ✅ |
| Email | ✅ |
| PhoneNumber | ✅ |
| AuthToken | ✅ |
| CurrencyCode | ✅ |
| Money | ✅ |
| Url | ✅ |
| GPS Coordinates | ✅ |

---

## Mapper Pattern

```text
Network DTO

      ↓

Mapper

      ↓

Value Class Domain Model

      ↓

UI State
```

Always convert DTOs into domain models.

---

## Compose Recommendation

Use immutable Value Classes inside immutable state.

---

## Room Recommendation

Always register `TypeConverter`s.

---

# 28. Common Production Pitfalls

## ❌ Pitfall 1 — Passing Strings Throughout App

Convert immediately to Value Class.

---

## ❌ Pitfall 2 — Using Value Classes Directly as DTOs Everywhere

Prefer mapping layer.

---

## ❌ Pitfall 3 — Missing Room TypeConverters

Room compilation fails.

---

## ❌ Pitfall 4 — Business Logic Inside DTO

Keep validation inside Value Classes or domain layer.

---

# 29. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Compose

1. Why are Value Classes good for Compose?
2. Are Value Classes stable?
3. Can `mutableStateOf()` hold Value Classes?

## Navigation

4. How should Navigation arguments be converted?
5. Why avoid raw Strings beyond navigation layer?

## Room

6. Why does Room need TypeConverters?
7. Can Room store Value Classes automatically?

## Retrofit

8. Should Retrofit DTOs use Value Classes?
9. Why use mapper layer?

## Serialization

10. How does Kotlin Serialization serialize Value Classes?

## KMP

11. Are Value Classes supported in shared modules?
12. How are they represented on iOS?

---

# 📋 Cheat Sheet (Part 3)

## Compose State

```kotlin
var username by remember {
    mutableStateOf(Username(""))
}
```

---

## Navigation

```kotlin
val userId = UserId(
    backStackEntry.arguments!!.getString("userId")!!
)
```

---

## Room Converter

```kotlin
@TypeConverter
fun fromUserId(id: UserId) = id.value

@TypeConverter
fun toUserId(value: String) = UserId(value)
```

---

## Retrofit Mapper

```kotlin
fun UserDto.toDomain() = User(
    UserId(id),
    Email(email)
)
```

---

## Serialization

```kotlin
@Serializable
@JvmInline
value class UserId(val value: String)
```

---

## Parcelable

```kotlin
@Parcelize
@JvmInline
value class UserId(val value: String) : Parcelable
```

---

## KMP

```kotlin
@JvmInline
value class UserId(val value: String)
```

Shared across Android, iOS, Desktop and Web.

---

# 📝 Revision Summary

In this part you learned:

- Value Classes in Clean Architecture.
- Jetpack Compose integration.
- Stability and recomposition.
- `mutableStateOf()` with Value Classes.
- Navigation Compose argument mapping.
- Room `TypeConverter`s.
- Retrofit mapper strategy.
- OkHttp authentication tokens.
- Kotlinx Serialization.
- Parcelable support.
- Kotlin Multiplatform support.
- Production-ready domain modeling patterns.
- Android best practices and interview questions.

---
# Part 4 — Performance, Memory Model, Testing & Interview Mastery

> The final part of the Value Classes chapter. Learn JVM memory optimization, garbage collection, benchmarking, testing, Domain-Driven Design (DDD), migration strategies, production patterns, pitfalls, and senior Android interview questions.

---

# 📚 Table of Contents

31. Performance Benchmarks
32. JVM Memory Model Deep Dive
33. Garbage Collection Analysis
34. Benchmarking Value Classes
35. Testing Value Classes
36. Domain-Driven Design (DDD)
37. Migration from Data Class to Value Class
38. Production Design Patterns
39. Common Production Bugs
40. Best Practices Checklist
41. 50+ Senior Android Interview Questions
42. Ultimate Cheat Sheet
43. Chapter Revision Summary

---

# 31. Performance Benchmarks ⭐⭐⭐⭐⭐

## 🎭 Real World Story — Flipkart Big Billion Day

Imagine Flipkart loads **50,000 products** during Big Billion Day.

Every product contains:

- ProductId
- SellerId
- CategoryId
- Price
- CurrencyCode

If each ID is wrapped in a **Data Class**, millions of additional objects are created.

Value Classes reduce unnecessary object allocation and improve runtime performance.

---

## Allocation Comparison

| Representation | Heap Allocation |
|---------------|-----------------|
| `String` | 1 Object |
| `Data Class` Wrapper | 2 Objects |
| `Value Class` (Unboxed) | 1 Object |
| `Value Class?` (Nullable) | 2 Objects |

---

## Data Class Benchmark

```kotlin
data class ProductId(
    val value: String
)

fun main() {
    repeat(1_000_000) {
        ProductId("P$it")
    }
}
```

**Result:** One million wrapper objects are allocated.

---

## Value Class Benchmark

```kotlin
@JvmInline
value class ProductId(
    val value: String
)

fun main() {
    repeat(1_000_000) {
        ProductId("P$it")
    }
}
```

**Result:** The compiler can eliminate wrapper allocations in many situations.

---

## Memory Visualization

### Data Class

```text
Heap Memory

+---------------------+
| ProductId Wrapper   |
+---------------------+
          │
          ▼
+---------------------+
| String Object       |
+---------------------+
```

### Value Class

```text
Heap Memory

+---------------------+
| String Object       |
+---------------------+
```

Only the underlying value remains.

---

## Why Value Classes Are Faster

- Less heap allocation.
- Reduced garbage collection.
- Better CPU cache locality.
- Smaller memory footprint.
- Faster parameter passing.

---

# 32. JVM Memory Model Deep Dive ⭐⭐⭐⭐⭐

## How JVM Stores Objects

Every JVM object contains:

```text
Object Header
Identity Hash
Monitor Information
Fields
Padding
```

A Data Class adds another object header.

---

## Memory Layout Comparison

### Data Class

```text
+----------------------+
| Object Header        |
| value : String Ref   |
+----------------------+
          │
          ▼
+----------------------+
| String Header        |
| Characters           |
+----------------------+
```

### Value Class

```text
+----------------------+
| String Header        |
| Characters           |
+----------------------+
```

No wrapper object.

---

## Primitive Value Example

```kotlin
@JvmInline
value class Age(
    val value: Int
)
```

When possible, JVM stores it as a primitive `int`.

---

## Stack vs Heap

| Value | Stored In |
|-------|-----------|
| `Int` | Stack / Registers |
| `Age` | Optimized Primitive |
| `Age?` | Heap Wrapper |
| `Any` | Heap Wrapper |

---

## Why It Matters in Android

Large lists, Compose recomposition, Paging, and Flow all benefit from fewer allocations.

---

# 33. Garbage Collection Analysis ⭐⭐⭐⭐⭐

## 🎭 Story — Instagram Reels Feed

Scrolling through reels creates thousands of temporary models.

Every wrapper object increases GC pressure.

---

## Data Class Example

```kotlin
data class StoryId(
    val value: String
)
```

Every object is allocated on the heap.

---

## Value Class Example

```kotlin
@JvmInline
value class StoryId(
    val value: String
)
```

Compiler often removes wrapper allocation.

---

## GC Comparison

| Scenario | Garbage Collection |
|----------|--------------------|
| Millions of Data Class wrappers | High |
| Millions of Value Classes | Low |
| Nullable Value Classes | Medium |
| Generic Collections | Medium |

---

## Android Benefits

Lower GC means:

- Smoother RecyclerView scrolling.
- Better Compose FPS.
- Reduced jank.
- Lower battery usage.

---

# 34. Benchmarking Value Classes ⭐⭐⭐⭐⭐

## Kotlin Benchmark

```kotlin
import kotlin.system.measureNanoTime

@JvmInline
value class UserId(val value: String)

fun main() {

    val time = measureNanoTime {

        repeat(1_000_000) {
            UserId("USR$it")
        }
    }

    println("Execution Time = $time ns")
}
```

---

## Compare With Data Class

```kotlin
data class UserId(val value: String)
```

Run the same benchmark.

---

## Android Jetpack Benchmark

```kotlin
@RunWith(AndroidJUnit4::class)
class ValueClassBenchmark {

    @get:Rule
    val benchmarkRule = BenchmarkRule()

    @Test
    fun benchmarkCreation() {
        benchmarkRule.measureRepeated {
            UserId("USR101")
        }
    }
}
```

---

## What Should You Measure?

- Allocation Count.
- Execution Time.
- Memory Usage.
- Garbage Collection Count.

---

# 35. Testing Value Classes ⭐⭐⭐⭐⭐

## Unit Test — Validation

```kotlin
@JvmInline
value class Email(
    val value: String
) {
    init {
        require("@" in value)
    }
}
```

### Valid Email

```kotlin
@Test
fun validEmail() {

    val email = Email("vikash@gmail.com")

    assertEquals(
        "vikash@gmail.com",
        email.value
    )
}
```

---

### Invalid Email

```kotlin
@Test
fun invalidEmail() {

    assertFailsWith<IllegalArgumentException> {
        Email("vikash")
    }
}
```

---

## Equality Test

```kotlin
@Test
fun equalityTest() {

    assertEquals(
        UserId("101"),
        UserId("101")
    )
}
```

---

## HashCode Test

```kotlin
@Test
fun hashCodeTest() {

    assertEquals(
        UserId("101").hashCode(),
        UserId("101").hashCode()
    )
}
```

---

## Compose UI Test

```kotlin
composeTestRule.setContent {

    ProfileHeader(
        Username("vikash")
    )
}

composeTestRule
    .onNodeWithText("vikash")
    .assertExists()
```

---

## Room Converter Test

```kotlin
@Test
fun converterTest() {

    val converter = UserConverters()

    val id = UserId("USR101")

    val value = converter.fromUserId(id)

    assertEquals("USR101", value)
}
```

---

# 36. Domain-Driven Design (DDD) ⭐⭐⭐⭐⭐

## 🎭 Story — Banking Domain

Every business concept has meaning.

```text
Money
AccountNumber
TransactionId
CustomerId
CurrencyCode
```

These are **Value Objects**.

Kotlin Value Classes are the perfect DDD implementation.

---

## Money

```kotlin
@JvmInline
value class Money(
    val value: Double
) {

    operator fun plus(other: Money) =
        Money(value + other.value)

    operator fun minus(other: Money) =
        Money(value - other.value)
}
```

---

## Account Number

```kotlin
@JvmInline
value class AccountNumber(
    val value: String
) {

    init {
        require(value.length == 12)
    }
}
```

---

## Currency

```kotlin
@JvmInline
value class CurrencyCode(
    val value: String
)
```

---

## DDD Folder Structure

```text
domain/
│
├── valueobject/
│   ├── UserId.kt
│   ├── Email.kt
│   ├── Money.kt
│   ├── AccountNumber.kt
│   ├── CurrencyCode.kt
│   └── PhoneNumber.kt
│
├── entity/
├── repository/
└── usecase/
```

---

## Why This Is Powerful

Business validation stays inside domain objects instead of UI or repository code.

---

# 37. Migration from Data Class to Value Class ⭐⭐⭐⭐

## Before Migration

```kotlin
data class UserId(
    val value: String
)
```

---

## After Migration

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

---

## Migration Checklist

| Layer | Required Change |
|-------|------------------|
| Room | Add TypeConverter |
| Retrofit | Use Mapper |
| Compose | Usually No Changes |
| Parcelable | Add `@Parcelize` |
| Navigation | Convert Route String |

---

## Safe Migration Strategy

```text
Network DTO
      │
      ▼
Mapper
      │
      ▼
Value Class Domain Model
      │
      ▼
UI State
```

---

# 38. Production Design Patterns ⭐⭐⭐⭐⭐

## Pattern 1 — Authentication

```kotlin
@JvmInline
value class AuthToken(val value: String)

@JvmInline
value class RefreshToken(val value: String)
```

Compiler prevents mixing token types.

---

## Pattern 2 — Commerce

```kotlin
@JvmInline
value class ProductId(val value: String)

@JvmInline
value class CouponCode(val value: String)
```

---

## Pattern 3 — Payments

```kotlin
@JvmInline
value class MerchantId(val value: String)

@JvmInline
value class TransactionId(val value: String)
```

---

## Pattern 4 — Maps

```kotlin
@JvmInline
value class Latitude(val value: Double)

@JvmInline
value class Longitude(val value: Double)
```

---

## Pattern 5 — Analytics

```kotlin
@JvmInline
value class EventName(val value: String)

@JvmInline
value class ScreenName(val value: String)
```

Perfect for Firebase Analytics.

---

# 39. Common Production Bugs ⭐⭐⭐⭐⭐

## Bug 1 — Nullable Value Class

```kotlin
val id: UserId? = UserId("101")
```

Creates boxed object.

---

## Bug 2 — Generic Collection

```kotlin
val users = listOf(UserId("1"))
```

Generic collection causes boxing.

---

## Bug 3 — Reflection

```kotlin
UserId::class
```

Reflection requires wrapper metadata.

---

## Bug 4 — Java Interoperability

Java sees the underlying primitive or String.

Compile-time safety is lost in Java.

---

## Bug 5 — Missing Validation

```kotlin
@JvmInline
value class Email(val value: String)
```

Without validation, invalid emails are possible.

---

# 40. Best Practices Checklist ⭐⭐⭐⭐⭐

## ✅ Do

- Use Value Classes for IDs and Tokens.
- Keep them immutable.
- Validate inside `init`.
- Use mapper layer between DTO and Domain.
- Register Room TypeConverters.
- Keep business logic inside domain layer.

---

## ❌ Don't

- Store multiple properties.
- Use mutable state inside Value Classes.
- Use Value Classes as large UI models.
- Depend on nullable Value Classes unnecessarily.

---

## Naming Convention

| Business Concept | Recommended Name |
|------------------|------------------|
| User Identifier | `UserId` |
| Product Identifier | `ProductId` |
| Merchant Identifier | `MerchantId` |
| Authentication Token | `AuthToken` |
| Refresh Token | `RefreshToken` |
| Currency | `CurrencyCode` |
| Latitude | `Latitude` |
| Longitude | `Longitude` |

---

# 41. 50+ Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Fundamentals

1. What is a Value Class?
2. Why use `@JvmInline`?
3. Difference between Value Class and Data Class?
4. Difference between Value Class and TypeAlias?
5. Why are Value Classes immutable?

## JVM Internals

6. What is boxing?
7. What is unboxing?
8. When does boxing happen?
9. Why does nullable boxing happen?
10. Why do generics box Value Classes?
11. What happens when Value Classes implement interfaces?
12. What synthetic methods are generated?

## Compose

13. Are Value Classes stable?
14. How do they affect recomposition?
15. Can `mutableStateOf()` store Value Classes?
16. Are Value Classes recommended in UI state?

## Room

17. Why are TypeConverters required?
18. Can Value Classes be Primary Keys?
19. How should Room store them?

## Retrofit

20. Should Retrofit DTOs use Value Classes?
21. Why use mapper layers?
22. How are Value Classes serialized?

## Parcelable

23. Can Value Classes implement Parcelable?
24. Can SavedStateHandle store them?

## KMP

25. Are Value Classes supported in KMP?
26. How does Swift represent them?

## Performance

27. Memory allocation difference?
28. GC improvements?
29. Object header savings?
30. CPU cache locality?

## Architecture

31. Domain-Driven Design usage?
32. Value Objects vs Entities?
33. Repository best practices?
34. UseCase best practices?

## Testing

35. Validation testing.
36. Equality testing.
37. Benchmark testing.
38. Compose testing.

## Advanced

39. Java interoperability.
40. Reflection behavior.
41. Collections boxing.
42. `Any` boxing.
43. Escape analysis.
44. Migration strategy.
45. Serialization pitfalls.
46. Nullable pitfalls.
47. Generic pitfalls.
48. Room migration.
49. Performance trade-offs.
50. When NOT to use Value Classes?

---

# 📋 Ultimate Cheat Sheet ⭐⭐⭐⭐⭐

## Basic Syntax

```kotlin
@JvmInline
value class UserId(
    val value: String
)
```

---

## Validation

```kotlin
init {
    require(value.isNotBlank())
}
```

---

## Companion Object

```kotlin
companion object {
    val EMPTY = UserId("")
}
```

---

## Compose State

```kotlin
var id by remember {
    mutableStateOf(UserId(""))
}
```

---

## Room Converter

```kotlin
@TypeConverter
fun fromUserId(id: UserId) = id.value

@TypeConverter
fun toUserId(value: String) = UserId(value)
```

---

## Retrofit Mapper

```kotlin
fun UserDto.toDomain() = User(
    id = UserId(id),
    email = Email(email)
)
```

---

## Parcelable

```kotlin
@Parcelize
@JvmInline
value class UserId(
    val value: String
) : Parcelable
```

---

## Serialization

```kotlin
@Serializable
@JvmInline
value class UserId(val value: String)
```

---

## Boxing Rules

| Scenario | Boxing |
|----------|--------|
| Function Parameter | ❌ Usually No |
| Return Value | ❌ Usually No |
| Nullable | ✅ Yes |
| Generic | ✅ Yes |
| `Any` | ✅ Yes |
| Interface | ✅ Yes |
| Reflection | ✅ Yes |

---

## Data Class vs Value Class

| Feature | Value Class | Data Class |
|---------|-------------|------------|
| Wrapper Allocation | Usually No | Always Yes |
| Multiple Properties | ❌ | ✅ |
| `copy()` | ❌ | ✅ |
| `componentN()` | ❌ | ✅ |
| Performance | Better | More Allocation |
| Domain IDs | ✅ Recommended | ⚠️ Less Efficient |

---

# 📝 Chapter Revision Summary ⭐⭐⭐⭐⭐

## Complete Chapter Coverage

| Part | Topics Covered |
|------|----------------|
| **Part 1** | Fundamentals, Syntax, Rules, Validation, Domain Modeling |
| **Part 2** | JVM Internals, Boxing vs Unboxing, Bytecode, Java Interop |
| **Part 3** | Compose, Room, Retrofit, Serialization, Parcelable, KMP |
| **Part 4** | Performance, Memory Model, GC, Benchmarking, Testing, DDD, Migration, Best Practices, 50 Interview Questions |

---

# 🎯 Android Interview Takeaways

After completing this chapter, you should be able to explain:

- Why Kotlin introduced **Value Classes**.
- How **`@JvmInline`** works internally.
- JVM **boxing vs unboxing** behavior.
- Memory optimization and garbage collection impact.
- Using Value Classes with **Compose, Room, Retrofit, Navigation, Parcelable, Serialization, and KMP**.
- Production-ready **Domain-Driven Design** using Value Classes.
- Senior-level interview questions asked at companies like Google, Uber, Amazon, PhonePe, CRED, and Flipkart.

---


