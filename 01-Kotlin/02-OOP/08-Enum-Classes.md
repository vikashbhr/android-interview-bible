# 💜 Kotlin Enum Classes — Complete Interview Guide (2026 Edition)

> Master Kotlin Enum Classes from basics to JVM internals, Android architecture, Compose, Serialization, KMP, performance optimization, and Staff Engineer interview questions.

**Module:** Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐☆

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • Razorpay • Flipkart

---

# 📚 Table of Contents

1. What is an Enum Class?
2. Why Enums Exist
3. Creating Enum Classes
4. Enum Constants
5. Properties & Constructors
6. Functions Inside Enum
7. Implementing Interfaces
8. Anonymous Enum Classes
9. `entries`, `values()`, `valueOf()`
10. `ordinal` and `name`
11. `when` with Enums
12. Enum vs Sealed Class
13. Enum vs Object
14. Android Examples
15. Compose Examples
16. Serialization, Room & Retrofit
17. JVM Internals
18. Performance Discussion
19. Best Practices
20. Production Pitfalls
21. Interview Questions
22. Cheat Sheet

---

# 1. What is an Enum Class?

## 🎭 Remember This Story — Traffic Signal

Imagine a traffic signal.

There are only **three valid states**:

- 🔴 RED
- 🟡 YELLOW
- 🟢 GREEN

A traffic light can **never** become BLUE.

The number of possibilities is fixed.

That's an Enum.

---

## Definition

An Enum represents a **fixed set of predefined constants**.

```kotlin
enum class TrafficLight {
    RED,
    YELLOW,
    GREEN
}
```

Only these values can exist.

---

## Why Enums Are Useful?

Use enums when the domain has a **closed list of options**.

Examples:

| Android Feature | Enum Example |
|----------------|--------------|
| Theme | LIGHT / DARK |
| Payment Status | SUCCESS / FAILED / PENDING |
| Network Type | WIFI / MOBILE / OFFLINE |
| User Role | ADMIN / USER / GUEST |
| Build Environment | DEV / QA / PROD |

---

# 2. Creating Enum Classes

Basic syntax.

```kotlin
enum class PaymentStatus {
    SUCCESS,
    FAILED,
    PENDING
}
```

Usage:

```kotlin
val status = PaymentStatus.SUCCESS
```

---

## Access Enum Constant

```kotlin
println(PaymentStatus.SUCCESS)
```

Output

```
SUCCESS
```

---

## Enum Type Safety

```kotlin
fun update(status: PaymentStatus) { }
```

Compiler prevents invalid values.

---

# 3. Enum Constants

Each enum constant is actually an **object instance**.

```kotlin
enum class Direction {
    NORTH,
    SOUTH,
    EAST,
    WEST
}
```

Usage.

```kotlin
Direction.NORTH
Direction.SOUTH
```

---

## Compare Enum Constants

```kotlin
if(status == PaymentStatus.SUCCESS){
    println("Payment Successful")
}
```

Safe comparison.

---

# 4. Enum Constructors

Enums can have constructors.

```kotlin
enum class UserRole(
    val level: Int
){

    GUEST(0),
    USER(1),
    ADMIN(2)
}
```

Usage.

```kotlin
println(UserRole.ADMIN.level)
```

Output

```
2
```

---

## Real Android Example

```kotlin
enum class BuildType(
    val baseUrl: String
){

    DEV("dev.example.com"),

    QA("qa.example.com"),

    PROD("prod.example.com")
}
```

Useful in build configuration.

---

# 5. Properties Inside Enum

Enums can contain properties.

```kotlin
enum class Theme(
    val displayName: String
){

    LIGHT("Light Mode"),

    DARK("Dark Mode")
}
```

Usage.

```kotlin
Theme.DARK.displayName
```

---

## Computed Property

```kotlin
enum class NetworkType {

    WIFI,

    MOBILE;

    val isMetered
        get() = this == MOBILE
}
```

---

# 6. Functions Inside Enum

Enums can contain member functions.

```kotlin
enum class PaymentStatus {

    SUCCESS,

    FAILED,

    PENDING;

    fun canRetry(): Boolean {
        return this == FAILED
    }
}
```

Usage.

```kotlin
status.canRetry()
```

---

## Android Example

```kotlin
enum class ThemeMode {

    LIGHT,

    DARK;

    fun icon() =
        if(this == LIGHT) "☀️" else "🌙"
}
```

---

# 7. Anonymous Enum Classes

Each enum constant can override behavior individually.

```kotlin
enum class Operation {

    ADD{
        override fun apply(a:Int,b:Int)=a+b
    },

    SUBTRACT{
        override fun apply(a:Int,b:Int)=a-b
    };

    abstract fun apply(a:Int,b:Int):Int
}
```

Usage.

```kotlin
Operation.ADD.apply(2,3)
```

Output

```
5
```

---

## 🎭 Story — Different Superheroes

Every superhero belongs to the "Hero" universe.

But each has unique powers.

Enums allow each constant to behave differently.

---

# 8. Enum Implements Interface

Enums can implement interfaces.

```kotlin
interface Logger{
    fun log()
}

enum class LogLevel : Logger{

    DEBUG,

    ERROR;

    override fun log(){
        println(name)
    }
}
```

Very useful for platform abstractions.

---

## Android Example

```kotlin
interface AnalyticsEvent{
    fun eventName(): String
}

enum class AppEvent : AnalyticsEvent{

    LOGIN,

    LOGOUT;

    override fun eventName() = name.lowercase()
}
```

---

# 9. `entries` (Kotlin 1.9+) ⭐⭐⭐⭐⭐

### New Kotlin Feature

Instead of:

```kotlin
PaymentStatus.values()
```

Use:

```kotlin
PaymentStatus.entries
```

---

## Why `entries`?

`values()` creates a new array every call.

`entries` returns a cached immutable list.

Better performance.

---

## Example

```kotlin
PaymentStatus.entries.forEach{
    println(it)
}
```

Output.

```
SUCCESS
FAILED
PENDING
```

---

## Interview Question

**Which is preferred in Kotlin 1.9+?**

Answer:

`entries`

---

# 10. `values()`

Old API.

```kotlin
PaymentStatus.values()
```

Returns:

```kotlin
Array<PaymentStatus>
```

---

## Allocation Discussion

Every call allocates array.

Avoid repeatedly calling in loops.

---

# 11. `valueOf()`

Convert String → Enum.

```kotlin
PaymentStatus.valueOf("SUCCESS")
```

Output.

```
SUCCESS
```

---

## Invalid Value

```kotlin
PaymentStatus.valueOf("DONE")
```

Throws:

```
IllegalArgumentException
```

---

## Safe Parsing Pattern

```kotlin
PaymentStatus.entries.firstOrNull {
    it.name == input
}
```

No exception.

Recommended.

---

# 12. `ordinal`

Every enum has ordinal.

```kotlin
PaymentStatus.SUCCESS.ordinal
```

Output.

```
0
```

---

## Why You Should Avoid Using Ordinal

### ⚠️ Production Pitfall

Initial enum.

```kotlin
SUCCESS
FAILED
PENDING
```

Ordinals.

```
0
1
2
```

Later.

```kotlin
SUCCESS
CANCELLED
FAILED
PENDING
```

Everything changes.

Database corruption.

Never persist ordinal.

---

# 13. `name`

Every enum has name.

```kotlin
PaymentStatus.SUCCESS.name
```

Output.

```
SUCCESS
```

Safe for logging.

---

## Pretty Display

Don't expose `name` directly.

```kotlin
enum class Theme(
    val title:String
){
    LIGHT("Light Mode"),
    DARK("Dark Mode")
}
```

---

# 14. `when` with Enum ⭐⭐⭐⭐⭐

Compiler knows all enum values.

```kotlin
when(status){

    SUCCESS -> ...

    FAILED -> ...

    PENDING -> ...
}
```

No `else` needed.

Exhaustive.

---

## Why This Is Great?

Compiler error when new enum added.

Safer than strings.

---

## Android Example

```kotlin
when(theme){

    LIGHT -> lightColors()

    DARK -> darkColors()
}
```

Compose pattern.

---

# 15. Enum vs Sealed Class

## Biggest Interview Question

<table>
<tr>
<td width="220"><b>Enum</b></td>
<td><b>Sealed Class</b></td>
</tr>

<tr>
<td>Fixed constants only.</td>
<td>Fixed hierarchy of subclasses.</td>
</tr>

<tr>
<td>No state per instance (except constructor properties).</td>
<td>Each subclass can hold different state.</td>
</tr>

<tr>
<td>Singleton instances.</td>
<td>Can create data subclasses.</td>
</tr>

<tr>
<td>Good for finite options.</td>
<td>Good for UI/API states.</td>
</tr>
</table>

---

## Example

### Enum

```kotlin
enum class PaymentStatus{
    SUCCESS,
    FAILED
}
```

### Sealed

```kotlin
sealed class PaymentState{

    object Loading: PaymentState()

    data class Success(val receipt:String): PaymentState()

    data class Error(val message:String): PaymentState()
}
```

Sealed carries data.

---

# 16. Enum vs Object

| Enum | Object |
|------|--------|
| Many predefined singleton instances | One singleton instance |
| `RED`, `GREEN` | `Logger` |
| Finite set | Single global object |

---

## Example

```kotlin
object Logger
```

vs

```kotlin
enum class LogLevel{
    DEBUG,
    INFO,
    ERROR
}
```

---

# 17. Android Architecture Examples

## Theme Mode

```kotlin
enum class ThemeMode{
    SYSTEM,
    LIGHT,
    DARK
}
```

DataStore stores preference.

---

## User Role

```kotlin
enum class UserRole{
    ADMIN,
    USER,
    GUEST
}
```

Permission checks.

---

## Analytics Events

```kotlin
enum class AnalyticsScreen{
    HOME,
    PROFILE,
    SETTINGS
}
```

Strong typing.

---

## Build Environment

```kotlin
enum class Environment(
    val url:String
){
    DEV("..."),
    QA("..."),
    PROD("...")
}
```

---

# 18. Jetpack Compose Examples

## Theme Switcher

```kotlin
enum class ThemeMode{
    LIGHT,
    DARK
}
```

UI state.

```kotlin
when(themeMode){

    LIGHT -> LightTheme()

    DARK -> DarkTheme()
}
```

---

## Filter Tabs

```kotlin
enum class FeedFilter{
    ALL,
    FOLLOWING,
    TRENDING
}
```

Selected tab represented safely.

---

## Settings Screen

```kotlin
enum class FontScale(
    val scale: Float
){
    SMALL(0.8f),
    NORMAL(1f),
    LARGE(1.2f)
}
```

Compose reads scale.

---

# 19. Room Example

Store enum safely.

```kotlin
@Entity
data class UserEntity(
    val status: PaymentStatus
)
```

Requires converter.

```kotlin
class PaymentStatusConverter{

    @TypeConverter
    fun toStatus(value:String)=PaymentStatus.valueOf(value)

    @TypeConverter
    fun fromStatus(status:PaymentStatus)=status.name
}
```

Use `name`, **not ordinal**.

---

# 20. Serialization Example

```kotlin
@Serializable
enum class ThemeMode{
    LIGHT,
    DARK
}
```

JSON.

```json
{
  "theme":"LIGHT"
}
```

---

## Custom Serialized Name

```kotlin
@SerialName("dark")
DARK
```

Useful for APIs.

---

# 21. Retrofit / Moshi / Gson

Enums serialize by name.

```json
SUCCESS
```

Can customize mapping with annotations or adapters.

---

# 22. Kotlin Multiplatform Example

Shared module.

```kotlin
enum class Platform{
    ANDROID,
    IOS,
    DESKTOP
}
```

Common code uses platform safely.

---

# 23. JVM Internals

Enums extend `java.lang.Enum`.

Decompiler.

```java
public final class PaymentStatus
    extends Enum<PaymentStatus>
```

Compiler generates:

- `values()`
- `valueOf()`
- Static instances.
- Static initialization block.

---

## Memory Diagram

```
JVM

SUCCESS

FAILED

PENDING

(All singleton instances)
```

Each enum constant exists only once.

---

# 24. Performance Discussion

### Enum Comparison

Very fast.

Reference comparison.

---

### `entries`

Preferred.

No repeated array allocation.

---

### `valueOf()`

Uses lookup.

Avoid exceptions for parsing unknown values.

---

# 25. Android Best Practices

### Use Enum for Finite Options

Theme.

Language.

Role.

Environment.

### Don't Store UI State in Enum

Use sealed class.

### Persist `name`

Never ordinal.

### Prefer `entries`

Kotlin 1.9+.

---

# 26. ⚠️ Production Pitfalls

### Pitfall 1

Persisting ordinal in Room.

### Pitfall 2

Using enum for loading/error/success UI.

Should be sealed class.

### Pitfall 3

Using strings instead of enums.

Compiler loses safety.

### Pitfall 4

Repeated `values()` allocations.

Use `entries`.

### Pitfall 5

Showing `name` directly to users.

Use display property.

---

# 27. Real Android Interview Questions

## Basic

1. What is an enum?
2. Enum vs constant?
3. Enum constructor?
4. Enum properties?
5. Enum methods?

## Intermediate

6. `entries` vs `values()`.
7. `ordinal` vs `name`.
8. Enum implementing interface.
9. Anonymous enum class.

## Advanced

10. JVM implementation.
11. Why enums are singleton?
12. Enum serialization.
13. Enum in Room.
14. Enum vs sealed class.
15. Performance of enum comparison.

---

# 28. 2-Minute Interview Answer

> Kotlin enums represent a fixed set of singleton constants. They can have constructors, properties, methods, and implement interfaces. Kotlin 1.9 introduced `entries`, which is preferred over `values()` because it avoids repeated array allocation. Enums are ideal for finite options like themes or user roles, while sealed classes are better for state machines carrying different data.

---

# 29. Cheat Sheet

| Feature | Example |
|--------|---------|
| Enum | `enum class ThemeMode` |
| Constructor | `LIGHT("Light")` |
| Property | `val title:String` |
| Function | `fun icon()` |
| entries | `ThemeMode.entries` |
| valueOf | `ThemeMode.valueOf("LIGHT")` |
| ordinal | `ThemeMode.LIGHT.ordinal` |
| name | `ThemeMode.LIGHT.name` |

---

# 📝 Revision Summary

- Enums model **finite, predefined values**.
- Each enum constant is a singleton object.
- Kotlin enums can have constructors, properties, methods, and implement interfaces.
- Use **`entries` instead of `values()`** in Kotlin 1.9+.
- Never persist `ordinal`; persist `name` or a custom value.
- Prefer **sealed classes** over enums for UI states with associated data.