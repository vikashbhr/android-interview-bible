# 💜 Kotlin Constructors & Init Blocks — Complete Interview Guide (2026 Edition)

> Master Kotlin constructors from basics to JVM internals, initialization order, default arguments, dependency injection, Compose patterns, and Android interview questions.

**Module:** Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • Razorpay

---

# 📚 Table of Contents

1. What are Constructors?
2. Primary Constructor
3. Constructor Parameters vs Properties
4. Default Arguments
5. Named Arguments
6. Secondary Constructors
7. Constructor Delegation
8. Init Blocks
9. Initialization Order
10. Multiple Init Blocks
11. Property Initialization
12. Backing Properties During Initialization
13. Constructor Visibility
14. Dependency Injection Patterns
15. Compose Examples
16. JVM Bytecode
17. Performance Discussion
18. Best Practices
19. Common Mistakes
20. Interview Questions
21. Cheat Sheet

---

# 1. What are Constructors?

A constructor initializes an object when it is created.

Unlike Java, Kotlin has:

- Primary Constructor (recommended)
- Secondary Constructor (optional)

Example:

```kotlin
class User(val name: String)
```

Object creation:

```kotlin
val user = User("Vikash")
```

The constructor runs automatically.

---

# 2. Primary Constructor

Primary constructor is declared in the class header.

```kotlin
class User(
    val name: String,
    val age: Int
)
```

This single declaration creates:

- Constructor
- Properties
- Getters
- Backing fields

Equivalent Java requires much more code.

---

## Constructor Without Properties

```kotlin
class User(name: String)
```

`name` is only a constructor parameter.

Cannot access outside constructor unless stored.

```kotlin
class User(name: String){
    val username = name
}
```

---

## Constructor with Mixed Parameters

```kotlin
class User(
    val name: String,
    age: Int
){
    val userAge = age
}
```

Useful when not every parameter should become a property.

---

# 3. Constructor Parameters vs Properties

| Declaration | Property Created? |
|-------------|------------------|
| `val name: String` | ✅ Yes |
| `var name: String` | ✅ Yes |
| `name: String` | ❌ No |

Example:

```kotlin
class Person(
    val first: String,
    last: String
){
    val surname = last
}
```

---

## Memory Representation

```text
Constructor Parameter
        │
        ▼
Property (optional)
        │
        ▼
Backing Field
```

Only properties become object state.

---

# 4. Default Arguments

One of Kotlin's biggest improvements over Java.

```kotlin
class User(
    val name: String = "Guest",
    val age: Int = 18
)
```

Now all are valid:

```kotlin
User()

User("Vikash")

User(age = 30)

User("Rahul", 25)
```

No constructor overloading required.

---

## Android Example

Compose Preview.

```kotlin
data class HomeUiState(
    val loading: Boolean = false,
    val users: List<User> = emptyList()
)
```

Allows simple initialization.

---

# 5. Named Arguments

Improves readability.

```kotlin
User(
    age = 29,
    name = "Vikash"
)
```

Especially useful when many parameters exist.

---

## Mix Positional + Named

```kotlin
User(
    "Vikash",
    age = 29
)
```

Allowed.

---

## Not Allowed

```kotlin
User(
    age = 29,
    "Vikash"
)
```

Named arguments cannot be followed by positional ones.

---

# 6. Secondary Constructors

Provide additional initialization paths.

```kotlin
class User{

    var name = ""

    constructor(name: String){
        this.name = name
    }
}
```

---

## Multiple Secondary Constructors

```kotlin
class User{

    constructor()

    constructor(name: String)

    constructor(name: String, age: Int)
}
```

Each constructor defines a different initialization path.

---

## When Should You Use Secondary Constructors?

Use only when:

- Java interoperability.
- Multiple initialization flows.
- Framework requirements.

Otherwise prefer default arguments.

---

# 7. Constructor Delegation

Every secondary constructor must delegate.

## Delegating to Primary Constructor

```kotlin
class User(
    val name: String
){

    constructor() : this("Guest")
}
```

Execution:

```
Secondary Constructor
        │
        ▼
Primary Constructor
        ▼
Init Blocks
```

---

## Delegating Between Secondary Constructors

```kotlin
class User{

    constructor()

    constructor(name: String) : this()
}
```

---

# 8. Init Block

Runs immediately after primary constructor.

```kotlin
class User(
    val name: String
){

    init{
        println("User Created")
    }
}
```

Output during creation.

---

## Validation Pattern

```kotlin
class User(
    val age: Int
){

    init{
        require(age >= 18){
            "User must be adult."
        }
    }
}
```

`require()` throws `IllegalArgumentException`.

---

## check()

```kotlin
init{
    check(users.isNotEmpty())
}
```

Used for internal state validation.

---

# 9. Initialization Order

Very common interview question.

```kotlin
class Demo(
    val name: String
){

    val first = log("First Property")

    init{
        log("Init Block")
    }

    val second = log("Second Property")
}
```

Execution order:

1. Constructor parameters.
2. Property initializers.
3. First init block.
4. Remaining properties.
5. Remaining init blocks.
6. Secondary constructor body.

---

## Visualization

```text
Constructor Parameters
        │
        ▼
Property Initializers
        ▼
Init Block 1
        ▼
Property Initializers
        ▼
Init Block 2
        ▼
Secondary Constructor
```

---

# 10. Multiple Init Blocks

```kotlin
class Demo{

    init{
        println("First")
    }

    init{
        println("Second")
    }

    init{
        println("Third")
    }
}
```

Output

```text
First
Second
Third
```

Executed in declaration order.

---

# 11. Property Initialization

Properties initialize before init blocks if declared earlier.

```kotlin
class User(
    val name: String
){

    val greeting = "Hello $name"

    init{
        println(greeting)
    }
}
```

Output

```text
Hello Vikash
```

---

## Dependent Property Initialization

```kotlin
val fullName = "$firstName $lastName"
```

Safe because constructor parameters are already initialized.

---

# 12. Accessing Properties During Init

Safe.

```kotlin
class User(
    val name: String
){

    init{
        println(name.length)
    }
}
```

Unsafe example.

```kotlin
open class Parent{
    open val message = "Parent"

    init{
        println(message)
    }
}
```

Can invoke overridden property before child initialization.

We'll revisit under inheritance.

---

# 13. Constructor Visibility

Constructors also have visibility modifiers.

## Private Constructor

```kotlin
class Database private constructor()
```

Prevents direct object creation.

---

## Internal Constructor

```kotlin
class Repository internal constructor()
```

Visible within module.

---

## Protected Constructor

Only for inheritance.

```kotlin
open class Base protected constructor()
```

---

# 14. Dependency Injection Pattern

Android best practice.

```kotlin
class UserRepository(
    private val api: ApiService,
    private val dao: UserDao
)
```

Constructor Injection is preferred.

Benefits:

- Immutable dependencies.
- Easy testing.
- Hilt/Koin compatible.

---

## Hilt Example

```kotlin
@HiltViewModel
class HomeViewModel @Inject constructor(
    private val repository: UserRepository
): ViewModel()
```

Primary constructor injection.

---

# 15. Factory Constructor Pattern

Instead of multiple constructors.

```kotlin
class User private constructor(
    val name: String
){

    companion object{

        fun guest() = User("Guest")

        fun admin() = User("Admin")
    }
}
```

Cleaner API.

---

# 16. Compose Examples

## State Holder

```kotlin
class CounterState(
    initial: Int = 0
){

    var count by mutableStateOf(initial)
}
```

Uses constructor defaults.

---

## UI State

```kotlin
data class LoginUiState(
    val email: String = "",
    val password: String = "",
    val loading: Boolean = false
)
```

Default constructor values simplify previews.

---

# 17. JVM Bytecode

Kotlin

```kotlin
class User(
    val name: String,
    val age: Int = 18
)
```

Compiler generates:

- Constructor.
- Synthetic constructor for default values.
- Bitmask parameter.
- DefaultConstructorMarker.

Equivalent JVM contains extra generated constructor.

---

## Synthetic Default Constructor

Kotlin creates something similar to:

```java
User(String name, int age, int mask, DefaultConstructorMarker marker)
```

Bitmask decides which defaults are used.

Senior interview topic.

---

# 18. Memory Diagram

```kotlin
val user = User("Vikash",29)
```

```text
Stack

user
 │
 ▼

Heap

User Object
------------
name = "Vikash"
age = 29
```

Constructor writes values into fields.

---

# 19. Performance Discussion

## Primary Constructor

- Less bytecode.
- Cleaner API.
- Better readability.

## Default Arguments

Avoid multiple overloaded constructors.

## Secondary Constructors

Generate additional methods.

Use sparingly.

---

# 20. Android Best Practices

### Prefer Primary Constructor Injection

```kotlin
class Repository(
    private val api: ApiService
)
```

### Use Default Arguments

Avoid unnecessary overloads.

### Validate Early

Use `require()` in init.

### Keep Constructors Lightweight

Avoid network/database operations inside constructors.

---

# 21. Common Mistakes

### Mistake 1

Business logic inside init.

### Mistake 2

Heavy work in constructor.

### Mistake 3

Too many secondary constructors.

### Mistake 4

Using mutable dependencies.

### Mistake 5

Calling open members inside init.

---

# 22. Real Android Interview Questions

## Basic

1. Primary vs Secondary constructor?
2. What is init block?
3. Constructor parameters vs properties?
4. Named arguments?
5. Default arguments?

## Intermediate

6. Initialization order.
7. Multiple init blocks.
8. Constructor delegation.
9. `require()` vs `check()`.
10. Private constructors.

## Advanced

11. Synthetic constructors generated by Kotlin.
12. Bitmask constructor for default parameters.
13. Why Hilt uses constructor injection?
14. Constructor visibility modifiers.
15. Why avoid open members during initialization?

---

# 23. 2-Minute Interview Answer

> Kotlin primarily uses primary constructors, which combine constructor declaration and property initialization into concise syntax. Default and named arguments eliminate most constructor overloading. Init blocks execute after property initialization and are commonly used for validation with `require()` or `check()`. In Android, constructor injection is the preferred dependency injection pattern because it creates immutable, testable classes compatible with Hilt and Koin.

---

# 24. Cheat Sheet

| Concept | Syntax |
|---------|--------|
| Primary Constructor | `class User(val name: String)` |
| Default Argument | `age: Int = 18` |
| Named Argument | `User(age = 29)` |
| Secondary Constructor | `constructor(name: String)` |
| Init Block | `init {}` |
| Constructor Delegation | `constructor() : this("Guest")` |
| Validation | `require(age > 18)` |
| Internal Constructor | `internal constructor()` |
| Private Constructor | `private constructor()` |

---

# 📝 Revision Summary

- Primary constructors are the Kotlin-first approach.
- Default arguments replace most overloaded constructors.
- Every secondary constructor delegates to another constructor.
- `init` blocks run immediately after property initialization.
- Initialization order is a favorite senior interview topic.
- Constructor injection is the standard Android architecture pattern.
- Kotlin generates synthetic constructors to support default parameters on the JVM.