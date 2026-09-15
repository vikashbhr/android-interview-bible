# 💜 Kotlin Data Classes — Complete Interview Guide (2026 Edition)

> Master Kotlin Data Classes from basics to immutability, copy semantics, JVM generated methods, Compose recomposition, Room entities, Serialization, KMP, and Staff Engineer interview questions.

**Module:** Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • CRED • Razorpay • Flipkart

---

# 📚 Table of Contents

1. What is a Data Class?
2. Why Kotlin Introduced Data Classes
3. Generated Functions
4. `equals()` and `hashCode()`
5. `toString()`
6. `copy()` Function
7. `componentN()` & Destructuring
8. Immutability & Mutable Properties
9. Nested Data Classes
10. Deep Copy vs Shallow Copy
11. Data Classes in Android
12. Data Classes in Jetpack Compose
13. Room + Serialization + Retrofit
14. JVM Internals
15. Performance Discussion
16. Best Practices
17. Production Pitfalls
18. Interview Questions
19. Cheat Sheet

---

# 1. What is a Data Class?

## 🎭 Remember This Story — Aadhaar Card

Imagine every Indian citizen has an Aadhaar card.

The card's identity is determined entirely by its **data**:

- Name
- DOB
- Aadhaar Number
- Address

Nobody cares **how** it prints itself.

Two Aadhaar cards with identical information represent the same person.

A Data Class is exactly that: an object whose identity is based on its **data**, not its behavior.

---

## Definition

A data class is a class primarily designed to **hold data**.

```kotlin
data class User(
    val id: Int,
    val name: String,
    val age: Int
)
```

With just one line Kotlin automatically generates:

- `equals()`
- `hashCode()`
- `toString()`
- `copy()`
- `component1()...componentN()`

---

# 2. Why Kotlin Introduced Data Classes?

### Java Version

```java
class User {

    int id;
    String name;

    // Constructor
    // Getter
    // Setter
    // equals()
    // hashCode()
    // toString()
}
```

Lots of boilerplate.

### Kotlin Version

```kotlin
data class User(
    val id: Int,
    val name: String
)
```

Zero boilerplate.

---

## Android Usage Everywhere

| Android Feature | Data Class |
|-----------------|-----------|
| UI State | ✅ |
| API Response | ✅ |
| Room Entity | ✅ |
| Firestore Model | ✅ |
| Paging Item | ✅ |
| Navigation Arguments | ✅ |
| Serialization DTO | ✅ |
| MVI State | ✅ |

---

# 3. Generated Functions

Given:

```kotlin
data class User(
    val id: Int,
    val name: String
)
```

Compiler generates:

| Function | Purpose |
|----------|---------|
| `equals()` | Value equality |
| `hashCode()` | Hash collections |
| `toString()` | Debug output |
| `copy()` | Clone object |
| `component1()` | Destructuring |
| `component2()` | Destructuring |

---

## Decompiled Java (Simplified)

```java
public final class User {

    private final int id;
    private final String name;

    public User copy(int id, String name){}

    public String toString(){}

    public boolean equals(Object other){}

    public int hashCode(){}

    public int component1(){}

    public String component2(){}
}
```

Interview favorite.

---

# 4. equals() — Value Equality

## Regular Class

```kotlin
class User(val name: String)

val a = User("Vikash")
val b = User("Vikash")

println(a == b)
```

Output

```
false
```

Different references.

---

## Data Class

```kotlin
data class User(val name: String)

val a = User("Vikash")
val b = User("Vikash")

println(a == b)
```

Output

```
true
```

Values are equal.

---

## Why Important?

Compose.

DiffUtil.

Room.

StateFlow.

List comparison.

---

# 5. hashCode()

Objects stored in:

- HashMap
- HashSet
- LinkedHashMap

Need consistent hash codes.

### Data Class

```kotlin
val user = User(1,"Vikash")

println(user.hashCode())
```

Generated automatically.

---

## Contract

If

```kotlin
a == b
```

Then

```kotlin
a.hashCode() == b.hashCode()
```

Always.

---

# 6. toString()

Generated automatically.

```kotlin
println(User(1,"Vikash"))
```

Output

```text
User(id=1, name=Vikash)
```

Excellent for debugging.

---

## Android Log Example

```kotlin
Log.d("User", user.toString())
```

Readable logs.

---

# 7. copy() Function ⭐⭐⭐⭐⭐

### 🎭 Remember This Story — Photocopy with One Change

Imagine you photocopy a document.

Everything stays identical.

You only change one field.

That's `copy()`.

---

## Example

```kotlin
val user = User(1,"Vikash",29)

val updated = user.copy(age = 30)
```

Original untouched.

New object created.

---

## Output

```text
Original: 29

Updated : 30
```

Immutable update.

---

## Multiple Changes

```kotlin
user.copy(
    name = "Harshita",
    age = 25
)
```

---

## Copy Without Changes

```kotlin
val clone = user.copy()
```

Creates identical object.

---

# 8. Why copy() Matters in Compose

### UI State Update

```kotlin
_state.update {
    it.copy(
        loading = false,
        users = response
    )
}
```

No mutation.

New immutable state.

---

## Why Compose Loves This

Compose detects new object.

Triggers recomposition correctly.

---

# 9. Destructuring (`componentN()`)

Generated automatically.

```kotlin
val user = User(1,"Vikash",29)

val(id,name,age)=user
```

Output

```
1
Vikash
29
```

---

## Ignore Values

```kotlin
val(_,name)=User(1,"Android")
```

Useful in parsing.

---

## Loop Destructuring

```kotlin
users.forEach { (id,name) ->
    println(name)
}
```

---

# 10. componentN() Internals

Compiler generates:

```kotlin
component1()

component2()

component3()
```

Used only for destructuring syntax.

---

# 11. Immutability

### Preferred

```kotlin
data class User(
    val name:String
)
```

Immutable.

---

### Mutable Data Class

```kotlin
data class User(
    var name:String
)
```

Allowed.

Usually discouraged.

---

## Why Prefer val?

Predictable state.

Compose optimization.

Thread safety.

---

# 12. Mutable Property Pitfall

```kotlin
val user = User("Vikash")

user.name = "Harshita"
```

Changes object.

Can create unexpected UI updates.

---

## Compose Best Practice

Always use immutable state models.

---

# 13. Nested Data Classes

```kotlin
data class Address(
    val city:String
)

data class User(
    val name:String,
    val address:Address
)
```

Perfect for API models.

---

# 14. Shallow Copy ⭐⭐⭐⭐⭐

Huge interview topic.

```kotlin
data class User(
    val name:String,
    val address:Address
)
```

Copy.

```kotlin
val copy = user.copy()
```

Address reference is shared.

```
User A
   │
   ▼
Address

User B
   │
   ▼
Same Address
```

---

## Demonstration

```kotlin
copy.address.city = "Delhi"
```

Original changes too.

---

# 15. Deep Copy

Need to copy nested objects manually.

```kotlin
val copy = user.copy(
    address = user.address.copy()
)
```

Now objects independent.

---

## Deep Copy Diagram

```
User A
   │
   ▼
Address A

User B
   │
   ▼
Address B
```

---

# 16. Data Classes in Android Architecture

## API Response

```kotlin
data class UserResponse(
    val id:Int,
    val name:String
)
```

Retrofit parses directly.

---

## Domain Model

```kotlin
data class User(
    val id:Int,
    val name:String
)
```

Domain layer.

---

## UI State

```kotlin
data class HomeUiState(
    val loading:Boolean=false,
    val users:List<User> = emptyList(),
    val error:String?=null
)
```

Compose standard.

---

# 17. Room Entity Example

```kotlin
@Entity
data class UserEntity(

    @PrimaryKey
    val id:Int,

    val name:String
)
```

Room works beautifully with immutable data classes.

---

# 18. Retrofit Example

```kotlin
data class LoginRequest(
    val email:String,
    val password:String
)
```

Serialization becomes automatic.

---

# 19. Kotlin Serialization

```kotlin
@Serializable
data class User(
    val id:Int,
    val name:String
)
```

Shared between Android/iOS in KMP.

---

# 20. Firestore Example

```kotlin
data class User(
    val name:String="",
    val age:Int=0
)
```

Default values required for deserialization.

---

# 21. Compose State Example ⭐⭐⭐⭐⭐

```kotlin
data class ProfileUiState(
    val loading:Boolean=false,
    val profile:User?=null,
    val error:String?=null
)
```

Update.

```kotlin
_state.update{
    it.copy(
        loading=false,
        profile=user
    )
}
```

Immutable architecture.

---

# 22. List Update Pattern

```kotlin
val updatedUsers =
    users.map {

        if(it.id==1)
            it.copy(name="Updated")
        else
            it
    }
```

Very common interview question.

---

# 23. JVM Internals

Data classes are final.

Decompiler generates:

- constructor
- getters
- equals
- hashCode
- toString
- copy
- componentN

All automatically.

---

## Why Final?

Immutable models should not be inherited.

Prevents equality bugs.

---

# 24. Memory Diagram

```kotlin
val user = User(1,"Vikash")
```

```
Stack

user
 │
 ▼

Heap

User
id
name
```

Copy.

```
user

copy

↓

Two separate User objects
```

---

# 25. Performance Discussion

### equals()

O(number of properties)

### copy()

Shallow copy only.

### Immutable Models

Less accidental mutation.

Better Compose recomposition.

---

## Large Data Classes

Very large objects increase comparison cost.

Keep UI state focused.

---

# 26. Android Best Practices

### Prefer Immutable Data Classes

Use `val`.

### Separate Layers

DTO

↓

Domain Model

↓

UI State

Don't reuse same class everywhere.

### Keep UI State Immutable

Use `copy()`.

---

# 27. ⚠️ Production Pitfalls

## Pitfall 1

Mutable nested objects with shallow copy.

## Pitfall 2

Huge UI state objects causing unnecessary recomposition.

## Pitfall 3

Reusing API DTO in UI layer.

## Pitfall 4

Using `var` everywhere.

## Pitfall 5

Ignoring `copy()` and mutating state directly.

---

# 28. Real Android Interview Questions

## Basic

1. What is a data class?
2. Generated methods?
3. Why final?

## Intermediate

4. copy() behavior.
5. equals vs regular class.
6. componentN().
7. Destructuring.

## Advanced

8. Shallow vs deep copy.
9. Compose recomposition with copy().
10. Data class in Room.
11. Serialization default values.
12. JVM generated methods.

---

# 29. 2-Minute Interview Answer

> Kotlin data classes automatically generate equals, hashCode, toString, copy, and componentN functions based on constructor properties. They are ideal for immutable state models, API DTOs, Room entities, and Compose UI state. The generated copy() performs a shallow copy, so nested mutable objects require manual deep copying. Modern Android architecture heavily relies on immutable data classes with copy() for predictable state updates.

---

# 30. Cheat Sheet

| Feature | Example |
|---------|---------|
| Data Class | `data class User(...)` |
| Copy | `user.copy(age=30)` |
| Destructuring | `val(id,name)=user` |
| Equality | `user1 == user2` |
| toString | `User(id=1,name=...)` |
| HashCode | Generated automatically |
| Deep Copy | `user.copy(address=user.address.copy())` |

---

# 📝 Revision Summary

- Data classes are optimized for **holding immutable data**.
- Kotlin generates six important methods automatically.
- `copy()` creates **shallow copies**.
- Prefer `val` properties for Compose and thread safety.
- Use separate data classes for DTOs, domain models, and UI state.
- `copy()` + immutable state is the foundation of modern Compose architecture.