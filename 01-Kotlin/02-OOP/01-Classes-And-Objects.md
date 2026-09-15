# 💜 Kotlin Classes & Objects — Complete Interview Guide (2026 Edition)

> Master Kotlin Classes & Objects from fundamentals to JVM memory model, property internals, constructors, getters/setters, Compose state models, and Android architecture.

**Module:** Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • Razorpay • Flipkart

---

# 📚 Table of Contents

1. What are Classes & Objects?
2. Class Declaration
3. Creating Objects
4. Properties vs Fields
5. Primary Constructor
6. Secondary Constructor
7. Init Block
8. Getters & Setters
9. Backing Fields (`field`)
10. Backing Properties
11. Object Memory Model
12. JVM Bytecode
13. Android Examples
14. Compose Examples
15. Performance Discussion
16. Best Practices
17. Common Mistakes
18. Interview Questions
19. Cheat Sheet

---

# 1. What are Classes & Objects?

A **class** is a blueprint that defines the structure and behavior of an object.

An **object** is a real instance created from that blueprint.

### Real World Analogy

| Class | Object |
|-------|--------|
| Car Blueprint | BMW Car |
| User Model | Vikash User |
| Bank Account Template | Your Bank Account |
| Employee Template | Android Developer Employee |

### Kotlin Example

```kotlin
class User

val user = User()
```

- `User` → Class
- `user` → Object (Instance)

---

# Why OOP Matters in Android?

Almost every Android component is a class.

| Android Component | Kotlin Class |
|-------------------|-------------|
| Activity | Class |
| Fragment | Class |
| ViewModel | Class |
| Repository | Class |
| Room Entity | Class |
| Compose State Model | Data Class |

---

# 2. Class Declaration

## Empty Class

```kotlin
class User
```

Equivalent Java:

```java
public final class User {}
```

Notice Kotlin classes are **final by default**.

---

## Class with Properties

```kotlin
class User(
    val name: String,
    val age: Int
)
```

This single line creates:

- Constructor
- Properties
- Getters
- Fields

Much less boilerplate than Java.

---

## Class Body

```kotlin
class User(
    val name: String
) {

    fun greet() {
        println("Hello $name")
    }
}
```

Properties + functions together define behavior.

---

# 3. Creating Objects

## Constructor Call

```kotlin
val user = User("Vikash", 29)
```

Creates an object on the heap.

---

## Memory Diagram

```
Stack

user ────────────────┐
                     ▼

Heap

User
---------------------
name = "Vikash"
age = 29
```

The variable stores a reference.

---

## Multiple Objects

```kotlin
val user1 = User("A",20)

val user2 = User("B",30)
```

Heap contains two different objects.

```
Heap

User("A")
User("B")
```

---

# 4. Properties vs Fields

One of the most important Kotlin interview topics.

## Property

A property is a language feature.

```kotlin
val name: String
```

A property includes:

- Getter
- Setter (if mutable)
- Backing field (optional)

---

## Field

A field is actual JVM storage.

```
Property

↓

Getter

↓

Backing Field
```

Properties don't always generate fields.

---

## Example

```kotlin
class User {

    val fullName: String
        get() = "Android"
}
```

No backing field generated.

---

## Generated Field

```kotlin
class User {

    var age = 20
}
```

Generates:

- Field
- Getter
- Setter

---

# Property vs Field Table

| Property | Field |
|----------|-------|
| Kotlin language concept | JVM storage |
| Can have custom getter | Cannot |
| Can exist without field | Field stores value |
| Public API | Internal implementation |

Interview favorite.

---

# 5. Primary Constructor

The recommended Kotlin constructor.

```kotlin
class User(
    val name: String,
    val age: Int
)
```

---

## Constructor Parameters

```kotlin
class User(
    name: String,
    age: Int
)
```

Parameters aren't properties.

Need assignment.

```kotlin
class User(
    name: String,
    age: Int
){
    val username = name
}
```

---

## Default Values

```kotlin
class User(
    val name: String = "",
    val age: Int = 0
)
```

Useful for Compose previews.

---

## Named Arguments

```kotlin
User(
    age = 29,
    name = "Vikash"
)
```

Improves readability.

---

# 6. Secondary Constructor

Used when multiple initialization paths exist.

```kotlin
class User {

    var name = ""

    constructor(name:String){
        this.name = name
    }
}
```

---

## Multiple Constructors

```kotlin
class User {

    constructor()

    constructor(name:String)

    constructor(name:String, age:Int)
}
```

---

## Delegating to Primary Constructor

```kotlin
class User(
    val name:String
){

    constructor():this("Guest")
}
```

Recommended.

---

# 7. Init Block

Runs immediately after primary constructor.

```kotlin
class User(
    val name:String
){

    init{
        println(name)
    }
}
```

Output during object creation.

---

## Multiple Init Blocks

```kotlin
class User(
    val name:String
){

    init{
        println("First")
    }

    init{
        println("Second")
    }
}
```

Execution order:

```
Primary Constructor

↓

Init Block 1

↓

Init Block 2
```

---

## Validation Example

```kotlin
class User(
    val age:Int
){

    init{
        require(age>=18){
            "Adult Only"
        }
    }
}
```

Very common production pattern.

---

# 8. Initialization Order

One of Google's favorite interview questions.

```kotlin
class User(
    val name:String
){

    val greeting = "Hello $name"

    init{
        println(greeting)
    }
}
```

Execution order:

```
Constructor Parameters

↓

Property Initialization

↓

Init Block

↓

Secondary Constructor
```

---

## Complex Example

```kotlin
class Demo(
    val name:String
){

    val first = log("First Property")

    init{
        log("Init Block")
    }

    val second = log("Second Property")
}
```

Output order matters.

---

# 9. Custom Getters

Getter computes value.

```kotlin
class User(
    val first:String,
    val last:String
){

    val fullName:String
        get() = "$first $last"
}
```

No backing field.

---

## Computed Property

```kotlin
val isAdult:Boolean
    get() = age>=18
```

Preferred over storing duplicate state.

---

# 10. Custom Setters

```kotlin
class User{

    var age:Int = 0

        set(value){
            field = value.coerceAtLeast(0)
        }
}
```

`field` references backing field.

---

## Logging Setter

```kotlin
set(value){
    println("$field -> $value")
    field = value
}
```

Useful for debugging.

---

# 11. Backing Field (`field`)

`field` exists only inside getter/setter.

```kotlin
var name=""

    set(value){
        field=value.trim()
    }
```

Compiler generates storage.

---

## Infinite Recursion Trap

Bad.

```kotlin
set(value){
    name=value
}
```

Calls setter again.

Use `field`.

---

# 12. Backing Properties

Private mutable, public immutable.

```kotlin
private var _token=""

val token:String
    get()=_token
```

Very common Android architecture pattern.

---

## ViewModel Pattern

```kotlin
private val _state =
    MutableStateFlow(HomeUiState())

val state =
    _state.asStateFlow()
```

Encapsulation.

---

# 13. Object Memory Model

### Example

```kotlin
val user = User("Vikash",29)
```

Memory

```
Stack

user
 │
 ▼

Heap

User Object
------------
name
age
hashCode
```

Objects always live on heap.

References live on stack.

---

## Multiple References

```kotlin
val user1 = User("Android",10)

val user2 = user1
```

Memory

```
Stack

user1 ─────┐

user2 ─────┘
            ▼

Heap

User Object
```

Both point to same object.

---

## Mutation

```kotlin
user2.age=30
```

Changes visible through `user1`.

Important interview question.

---

# 14. Copying Objects

Regular classes copy references.

```kotlin
val b = a
```

Same object.

Data classes provide `copy()` (covered later).

---

# 15. JVM Bytecode

Kotlin

```kotlin
class User(
    val name:String
)
```

Decompiler

```java
public final class User{

    private final String name;

    public User(String name){
        this.name=name;
    }

    public String getName(){
        return name;
    }
}
```

Compiler generates getters automatically.

---

## Mutable Property Bytecode

```kotlin
var age:Int=0
```

Generates

```java
private int age;

public int getAge(){}

public void setAge(int value){}
```

---

# 16. Android Examples

## Model Class

```kotlin
class User(
    val id:Int,
    val name:String
)
```

Used for API models.

---

## Repository

```kotlin
class UserRepository(
    private val api:ApiService
){
    suspend fun users()=api.users()
}
```

Constructor Injection.

---

## ViewModel

```kotlin
class HomeViewModel(
    private val repository:UserRepository
):ViewModel()
```

Most DI frameworks use constructor injection.

---

# 17. Jetpack Compose Example

## UI State Model

```kotlin
data class HomeUiState(
    val loading:Boolean=false,
    val users:List<User> = emptyList()
)
```

Immutable properties.

---

## State Holder

```kotlin
class CounterState{

    var count by mutableStateOf(0)
}
```

Class encapsulates mutable state.

---

# 18. Equality of Objects

Regular classes compare references.

```kotlin
class User(val name:String)

val a=User("Android")
val b=User("Android")

println(a==b)
```

Output

```
false
```

Data classes compare contents.

Covered later.

---

# 19. Performance Discussion

### Final Classes

Classes are final by default.

Benefits:

- Faster dispatch.
- Better compiler optimization.
- Safer inheritance.

---

### Getter Optimization

Simple getters often inline.

No runtime penalty.

---

### Object Allocation

Creating objects frequently increases GC pressure.

Prefer immutable reusable models.

---

# 20. Android Best Practices

## Constructor Injection

Preferred.

```kotlin
class LoginRepository(
    private val api:Api
)
```

---

## Immutable Models

Prefer

```kotlin
val
```

inside models.

---

## Computed Properties

Avoid duplicate stored values.

---

## Backing Property Pattern

Expose immutable APIs.

---

# 21. Common Mistakes

### Mistake 1

Using secondary constructors unnecessarily.

Prefer default arguments.

### Mistake 2

Using mutable properties in UI state.

### Mistake 3

Recursive setter.

### Mistake 4

Using regular classes when equality is required.

---

# 22. Real Android Interview Questions

## Basic

1. Difference between class and object?
2. What is a property?
3. What is a backing field?
4. Primary vs Secondary constructor?
5. What is init block?

## Intermediate

6. Initialization order?
7. Getter vs field?
8. Backing property pattern?
9. Constructor injection benefits?
10. Named arguments?

## Advanced

11. JVM bytecode generated for Kotlin class.
12. Why are classes final by default?
13. Heap vs Stack memory for objects.
14. Property without backing field.
15. Compose state holder classes.

---

# 23. 2-Minute Interview Answer

> Kotlin classes are final by default and combine constructor, properties, getters, and setters into concise syntax. Properties are language-level concepts that may generate JVM backing fields. Primary constructors are preferred, while init blocks execute immediately after property initialization. Android applications commonly use constructor injection, immutable properties, and backing properties to expose read-only state from ViewModels.

---

# 24. Cheat Sheet

| Concept | Syntax |
|---------|--------|
| Class | `class User` |
| Object | `User()` |
| Primary Constructor | `class User(val name:String)` |
| Secondary Constructor | `constructor(name:String)` |
| Init Block | `init{}` |
| Getter | `get()` |
| Setter | `set(value)` |
| Backing Field | `field` |
| Backing Property | `_state / state` |
| Named Arguments | `User(age=29,name="Vikash")` |

---

# 📝 Revision Summary

- A class is a blueprint; an object is an instance.
- Kotlin properties automatically generate getters/setters when needed.
- `field` is available only inside custom getters/setters.
- `init` executes after property initialization.
- Constructor injection is the preferred Android architecture pattern.
- Classes are final by default for safety and performance.
- Backing properties (`_state` → `state`) are a core ViewModel and Compose pattern.