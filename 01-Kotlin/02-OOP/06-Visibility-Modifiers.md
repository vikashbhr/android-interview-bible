# 💜 Kotlin Visibility Modifiers — Complete Interview Guide (2026 Edition)

> Master Kotlin visibility modifiers from fundamentals to multi-module Android architecture, library design, JVM internals, Compose, KMP, testing, and Staff Engineer interview questions.

**Module:** Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • Razorpay • CRED • Flipkart

---

# 📚 Table of Contents

1. What are Visibility Modifiers?
2. Why Visibility Matters
3. `public`
4. `private`
5. `protected`
6. `internal`
7. Visibility Matrix
8. Visibility in Classes
9. Visibility in Constructors
10. Visibility in Properties & Functions
11. File-Level Visibility
12. Nested Classes Visibility
13. Object & Companion Visibility
14. Multi-Module Android Examples
15. Jetpack Compose Examples
16. Kotlin Multiplatform (KMP)
17. JVM Internals
18. Performance Discussion
19. Best Practices
20. Production Pitfalls
21. Interview Questions
22. Cheat Sheet

---

# 1. What are Visibility Modifiers?

## 🎭 Remember This Story — The Corporate Office Building

Imagine a company building with four levels of access.

| Office Area | Kotlin Modifier |
|-------------|----------------|
| Reception (Everyone) | `public` |
| Employee Cabin | `internal` |
| Team Room | `protected` |
| CEO Locker | `private` |

Not everyone should access everything.

Visibility modifiers control **who can access classes, properties, constructors, and functions.**

---

## Kotlin Visibility Modifiers

| Modifier | Accessible From |
|----------|-----------------|
| `public` | Everywhere |
| `private` | Inside declaring scope only |
| `protected` | Class + subclasses |
| `internal` | Same module only |

Unlike Java, Kotlin's default visibility is **public**.

---

# 2. Why Visibility Matters?

Large Android apps may have:

```text
App
│
├── app
├── core
├── data
├── domain
├── design-system
├── feature-home
├── feature-profile
└── analytics
```

Without visibility control:

- Feature modules access internal APIs.
- Tests modify implementation details.
- Libraries expose unnecessary APIs.
- Refactoring becomes dangerous.

Visibility creates **API boundaries**.

---

# 3. Public Modifier

## Definition

Everything is public by default.

```kotlin
class UserRepository {

    fun users() {}
}
```

Equivalent to:

```kotlin
public class UserRepository {

    public fun users() {}
}
```

---

## Android Example

Public APIs exposed to app module.

```kotlin
public class PaymentSdk {

    fun initialize() {}
}
```

SDK consumers can access it.

---

## When to Use Public?

Use only when the API is intentionally exposed.

Examples:

- SDK APIs
- Public utility functions
- Domain interfaces
- Shared models

---

# 4. Private Modifier

## Definition

Accessible only inside the declaring scope.

```kotlin
class UserRepository {

    private val apiKey = "SECRET"

    private fun authenticate() {}
}
```

Cannot access outside class.

---

## Property Example

```kotlin
private var token = ""
```

Used internally.

---

## Function Example

```kotlin
private fun cacheUsers() {}
```

Hidden implementation detail.

---

## File-Level Private

```kotlin
private const val BASE_URL = "..."
```

Visible only inside the file.

Very common in Android.

---

## 🎭 Story — Password Manager

Your password manager stores encryption keys.

Those keys should never be accessible outside.

`private` protects implementation secrets.

---

# 5. Protected Modifier

## Definition

Accessible only inside class and subclasses.

```kotlin
open class BaseRepository {

    protected val api = ApiService()
}
```

Subclass:

```kotlin
class UserRepository : BaseRepository() {

    fun loadUsers() {
        api.users()
    }
}
```

Outside:

```kotlin
repository.api
```

❌ Compilation Error.

---

## Protected Function

```kotlin
open class BaseActivity {

    protected fun showLoader() {}
}
```

Children reuse loader.

External callers cannot.

---

## Android Example

```kotlin
abstract class BaseViewModel : ViewModel() {

    protected fun launchSafely() {}
}
```

Children access coroutine launcher.

---

# 6. Internal Modifier ⭐⭐⭐⭐⭐

### One of Kotlin's Best Features

Visible only inside the same **module**.

```kotlin
internal class UserCache
```

Accessible inside:

- app module
- core module (if same module)

Not accessible from another Gradle module.

---

## What is a Module?

Examples:

```text
:app

:data

:domain

:analytics

:feature-home
```

Each Gradle module is a Kotlin module.

---

## Android Example

`data` module

```kotlin
internal class UserDatabaseMapper
```

`app` module cannot access it.

Keeps mapping implementation hidden.

---

## Why Internal Is Better Than Public?

Avoid exposing implementation APIs to consumers.

Great for:

- Repositories
- DTO Mappers
- Network Parsers
- Internal Utilities

---

# 7. Visibility Matrix

| Scope | public | private | protected | internal |
|------|--------|---------|-----------|----------|
| Same Class | ✅ | ✅ | ✅ | ✅ |
| Same File | ✅ | ✅ | ❌ | ✅ |
| Subclass | ✅ | ❌ | ✅ | ✅ |
| Same Module | ✅ | ❌ | ❌ | ✅ |
| Different Module | ✅ | ❌ | ❌ | ❌ |

**Memorize this table.**

Google interview favorite.

---

# 8. Visibility on Classes

## Public Class

```kotlin
class User
```

Accessible everywhere.

---

## Internal Class

```kotlin
internal class UserMapper
```

Accessible only inside module.

---

## Private Class

Top-level.

```kotlin
private class JsonParser
```

Only inside file.

Useful helper classes.

---

## Protected Class?

Not allowed at top level.

Protected works only inside classes.

---

# 9. Visibility on Constructors

## Private Constructor

### 🎭 Story — VIP Club

Only management can create VIP memberships.

No one else.

```kotlin
class Database private constructor()
```

Cannot instantiate directly.

---

## Singleton Pattern

```kotlin
class Database private constructor() {

    companion object {
        val instance = Database()
    }
}
```

Only one instance exists.

---

## Internal Constructor

```kotlin
class Repository internal constructor()
```

DI frameworks inside module can create it.

---

## Protected Constructor

```kotlin
open class Base protected constructor()
```

Only subclasses create objects.

---

# 10. Visibility on Properties

## Private Property

```kotlin
class LoginRepository {

    private var token = ""
}
```

---

## Public Getter + Private Setter

Very common pattern.

```kotlin
class Counter {

    var count = 0
        private set
}
```

Anyone reads.

Only class updates.

---

## Android Example

```kotlin
class DownloadManager {

    var progress = 0
        private set

    fun updateProgress(value: Int) {
        progress = value
    }
}
```

Safe encapsulation.

---

# 11. Visibility on Functions

```kotlin
private fun cache() {}

internal fun mapper() {}

protected fun validate() {}

public fun login() {}
```

Each function has different API visibility.

---

## File-Level Utility Function

```kotlin
private fun calculateChecksum()
```

Hidden helper.

---

# 12. File-Level Visibility

Kotlin allows top-level declarations.

```kotlin
private const val API_KEY = "..."

internal fun parseResponse(){}

public fun formatDate(){}
```

---

## Android Constants File

```kotlin
private const val TAG = "UserRepository"

internal const val BASE_URL = "..."
```

Very common.

---

# 13. Nested Classes Visibility

```kotlin
class User {

    private class Parser

    internal class Mapper
}
```

Visibility works independently.

---

## Protected Nested Class

```kotlin
open class Base {

    protected class Helper
}
```

Only subclasses access.

---

# 14. Companion Object Visibility

```kotlin
class Database {

    companion object {

        private const val VERSION = 1

        fun create(){}
    }
}
```

VERSION hidden.

Factory exposed.

---

## Internal Companion Function

```kotlin
internal fun clearCache(){}
```

Visible inside module.

---

# 15. Object Declaration Visibility

```kotlin
internal object Logger
```

Singleton inside module.

---

## Private Object

```kotlin
private object JsonUtil
```

Only current file.

---

# 16. Multi-Module Android Example ⭐⭐⭐⭐⭐

### Project Structure

```text
:app

:data

:domain

:feature-home

:core-ui
```

---

## Domain Interface

```kotlin
public interface UserRepository
```

Exposed.

---

## Data Implementation

```kotlin
internal class UserRepositoryImpl
```

Hidden.

App depends only on interface.

Architecture becomes clean.

---

## Mapper Hidden

```kotlin
internal class UserDtoMapper
```

UI cannot access DTO mapper.

---

# 17. Jetpack Compose Example

## State Exposure Pattern

```kotlin
private val _state =
    MutableStateFlow(HomeUiState())

val state = _state.asStateFlow()
```

Why?

- Mutable internally.
- Immutable externally.

One of the most common Android interview questions.

---

## Compose State Holder

```kotlin
class CounterState {

    var count by mutableStateOf(0)
        private set

    fun increment() {
        count++
    }
}
```

UI reads only.

---

# 18. Kotlin Multiplatform Example

Shared module.

```kotlin
internal class PlatformLogger
```

Only shared module uses it.

Platform modules cannot.

---

## Public Expect/Actual

```kotlin
expect class DeviceInfo
```

Public API exposed.

Implementation hidden.

---

# 19. JVM Internals

### Private Method

Compiler generates private JVM method.

### Internal Method

Compiler generates public JVM method with name mangling to preserve module visibility.

Interesting interview topic.

---

## Decompiled Example

```kotlin
internal fun mapper(){}
```

Decompiler shows mangled name like:

```java
mapper$module()
```

Visibility enforced by metadata.

---

# 20. Performance Discussion

Visibility doesn't significantly affect runtime performance.

Benefits are architectural:

- Smaller public API.
- Easier refactoring.
- Better compiler analysis.
- Better encapsulation.

---

# 21. Android Best Practices

## Prefer Smallest Possible Visibility

Start with `private`.

Increase only when needed.

---

## Repository Pattern

```kotlin
public interface UserRepository

internal class UserRepositoryImpl
```

Ideal architecture.

---

## Hide Mutable State

Expose immutable `StateFlow`.

---

## Hide DTOs

Expose domain models only.

---

## Keep Constants Private

Unless shared intentionally.

---

# 22. ⚠️ Production Pitfalls

## Pitfall 1 — Everything Public

Huge SDK surface.

Breaking changes become impossible.

---

## Pitfall 2 — Mutable State Exposure

```kotlin
val state = MutableStateFlow(...)
```

Anyone modifies state.

Wrong.

---

## Pitfall 3 — Public Mapper Classes

UI begins depending on network DTOs.

Architecture leaks.

---

## Pitfall 4 — Using Protected for Utilities

Prefer composition/private helpers.

---

## Pitfall 5 — Forgetting `internal` in Multi-Module Apps

Implementation accidentally exposed.

---

# 23. Real Android Interview Questions

## Basic

1. What are Kotlin visibility modifiers?
2. Default visibility in Kotlin?
3. Difference between `private` and `protected`?

---

## Intermediate

4. What is `internal`?
5. Module vs package?
6. File-level private.
7. Public getter/private setter.

---

## Advanced

8. `internal` JVM implementation.
9. Why ViewModel exposes immutable StateFlow?
10. Repository interface visibility.
11. SDK API design.
12. Multi-module architecture visibility strategy.

---

# 24. 2-Minute Interview Answer

> Kotlin provides four visibility modifiers: public, private, protected, and internal. Public is the default. Private hides implementation inside the declaring scope, protected exposes members only to subclasses, and internal restricts visibility to the current Gradle module. Modern Android apps heavily use internal for implementation classes and expose immutable public interfaces, especially with repositories and StateFlow.

---

# 25. Cheat Sheet

| Modifier | Use Case |
|----------|----------|
| `public` | Public API / SDK |
| `private` | Internal implementation |
| `protected` | Base class reusable members |
| `internal` | Multi-module architecture |

### Common Android Patterns

```kotlin
private val _state = MutableStateFlow(...)

val state = _state.asStateFlow()

internal class UserRepositoryImpl

public interface UserRepository

var progress = 0
    private set
```

---

# 📝 Revision Summary

- Kotlin has **4 visibility modifiers**.
- `internal` is module-level visibility (unique Kotlin feature).
- Prefer the **smallest visibility possible**.
- Expose immutable state, hide mutable implementation.
- Multi-module Android projects rely heavily on `internal`.
- `private setter` is a common ViewModel and Compose pattern.