# 💜 Kotlin Sealed Classes & Sealed Interfaces — Android Interview Bible (2026 Edition)

> Master Sealed Classes from beginner to Staff Android Engineer level with MVI architecture, Jetpack Compose state management, API result wrappers, Navigation events, KMP, JVM internals, and interview questions from Google, Uber, PhonePe, CRED, and Amazon.

**Module:** Kotlin OOP

**Difficulty:** Intermediate → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐ (Top 3 Kotlin Interview Topic)

**Companies:** Google • Uber • PhonePe • Amazon • Microsoft • CRED • Flipkart • Meesho • Razorpay

---

# 📚 Table of Contents (Full Chapter)

## Part 1 — Foundations (This file section)

1. What are Sealed Classes?
2. Why Kotlin Introduced Sealed Classes
3. Sealed Class vs Enum
4. Sealed Class vs Abstract Class
5. Sealed Interface
6. Object vs Data Class inside Sealed
7. Exhaustive `when`
8. Constructor Rules
9. Visibility Rules
10. Package Rules

## Part 2 — Android Architecture

11. API Result Wrapper
12. Repository Pattern
13. MVI State Management
14. UI Events
15. Navigation Events
16. Authentication State Machine
17. Payment State Machine
18. Download Manager Example

## Part 3 — Jetpack Compose + KMP

19. Compose State
20. Compose Events
21. Side Effects
22. Snackbar Events
23. Bottom Sheet States
24. KMP Shared States
25. Serialization

## Part 4 — JVM + Interview Mastery

26. JVM Internals
27. Performance
28. Best Practices
29. Production Pitfalls
30. 50+ Interview Questions
31. Cheat Sheet

---

# 🎯 Why This Chapter Matters

Sealed Classes are the **foundation of modern Android architecture**.

You'll find them in:

| Area | Usage |
|------|-------|
| Jetpack Compose | UI State |
| MVVM | Screen State |
| MVI | Intent / State / Effect |
| Navigation | Navigation Events |
| Coroutines | Result Wrappers |
| Paging | Load State |
| KMP | Shared Platform State |
| Networking | Success / Error / Loading |
| Authentication | Login State Machine |

> If Data Classes represent **what data is**, Sealed Classes represent **what state the app is currently in**.

---

# 1. What are Sealed Classes?

## 🎭 Remember This Story — Airport Security Check

Imagine you're entering an airport.

Every passenger must be in **exactly one** of these states:

- Waiting
- Security Check
- Boarding
- On Flight
- Cancelled

No other state exists.

The airport system knows **all possible passenger states in advance**.

That is a **Sealed Class hierarchy**.

<svg viewBox="0 0 640 220" xmlns="http://www.w3.org/2000/svg">
  <rect x="180" y="10" width="280" height="42" rx="10" fill="none" stroke="currentColor" stroke-width="1.5"/>
  <text x="320" y="36" text-anchor="middle" font-size="16" fill="currentColor">PassengerState</text>

  <path d="M320 52 V70 M110 70 H530" stroke="currentColor" stroke-width="1.5" fill="none"/>

  {#each [
      {x:30,label:"Waiting"},
      {x:170,label:"Security"},
      {x:310,label:"Boarding"},
      {x:450,label:"Flying"},
      {x:560,label:"Cancelled"}
    ] as s}
    <path d={`M${s.x+40} 70 V88`} stroke="currentColor" stroke-width="1.5"/>
    <rect x={s.x} y="88" width="80" height="36" rx="8" fill="none" stroke="currentColor"/>
    <text x={s.x+40} y="111" text-anchor="middle" font-size="11" fill="currentColor">{s.label}</text>
  {/each}
</svg>

Only these states are allowed.

Compiler knows every possible state.

---

## Definition

A **sealed class represents a closed hierarchy of subclasses**.

```kotlin
sealed class PaymentState
```

Children:

```kotlin
sealed class PaymentState {

    object Loading : PaymentState()

    data class Success(
        val transactionId: String
    ) : PaymentState()

    data class Error(
        val message: String
    ) : PaymentState()
}
```

This hierarchy is **closed**.

No unknown subclasses outside the allowed scope.

---

## Why Is It Called "Sealed"?

Think of a sealed envelope.

Once sealed...

Nobody can insert another letter.

Similarly...

A sealed class hierarchy is "sealed" from unexpected subclasses.

---

# 2. Why Kotlin Introduced Sealed Classes

Before sealed classes...

Developers modeled states like this:

```kotlin
enum class Status {
    LOADING,
    SUCCESS,
    ERROR
}
```

Where is the success data?

Need another variable.

```kotlin
data class UiState(
    val status: Status,
    val data: List<User>?,
    val error: String?
)
```

Problem:

Impossible combinations become possible.

```kotlin
status = SUCCESS
error = "Network Error"
```

Invalid UI.

---

## Sealed Class Fix

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

Impossible states disappear.

Each state owns its own data.

---

## Why Google Loves Sealed Classes

Because they eliminate **invalid state combinations**.

Huge architecture improvement.

---

# 3. Anatomy of a Sealed Class

```kotlin
sealed class ApiResult {

    object Loading : ApiResult()

    data class Success(
        val users: List<User>
    ) : ApiResult()

    data class Failure(
        val error: Throwable
    ) : ApiResult()
}
```

Hierarchy:

<svg viewBox="0 0 640 220" xmlns="http://www.w3.org/2000/svg">
  <rect x="220" y="10" width="200" height="40" rx="10" fill="none" stroke="currentColor"/>
  <text x="320" y="35" text-anchor="middle" font-size="16" fill="currentColor">ApiResult</text>

  <path d="M320 50 V70 M100 70 H540" stroke="currentColor" stroke-width="1.5" fill="none"/>

  {#each [
      {x:40,label:"Loading",w:120},
      {x:230,label:"Success(users)",w:180},
      {x:450,label:"Failure(error)",w:150}
    ] as c}
    <path d={`M${c.x+c.w/2} 70 V90`} stroke="currentColor" stroke-width="1.5"/>
    <rect x={c.x} y="90" width={c.w} height="44" rx="10" fill="none" stroke="currentColor"/>
    <text x={c.x+c.w/2} y="116" text-anchor="middle" font-size="12" fill="currentColor">{c.label}</text>
  {/each}
</svg>

---

## Three Types of Children

<table>
<tr>
<td width="180"><b>Child Type</b></td>
<td><b>Use Case</b></td>
</tr>

<tr>
<td>`object`</td>
<td>Singleton state (Loading)</td>
</tr>

<tr>
<td>`data class`</td>
<td>State carrying data (Success)</td>
</tr>

<tr>
<td>Regular class</td>
<td>Rare custom behavior</td>
</tr>
</table>

---

# 4. Sealed Class vs Enum ⭐⭐⭐⭐⭐

This is probably the **#1 interview comparison question**.

<table>
<tr>
<td width="220"><b>Enum</b></td>
<td><b>Sealed Class</b></td>
</tr>

<tr>
<td>Finite constants.</td>
<td>Finite hierarchy.</td>
</tr>

<tr>
<td>Every constant is singleton.</td>
<td>Can have singleton or data subclasses.</td>
</tr>

<tr>
<td>No unique data per instance.</td>
<td>Each subclass carries different data.</td>
</tr>

<tr>
<td>Same shape for all constants.</td>
<td>Each state has different structure.</td>
</tr>

<tr>
<td>Great for categories.</td>
<td>Great for state machines.</td>
</tr>
</table>

---

## Example — Payment

### Enum

```kotlin
enum class PaymentStatus {
    SUCCESS,
    FAILED
}
```

Need separate receipt.

---

### Sealed

```kotlin
sealed class PaymentStatus {

    data class Success(
        val receiptId: String
    ) : PaymentStatus()

    data class Failed(
        val reason: String
    ) : PaymentStatus()
}
```

Cleaner.

Safer.

---

## Android Decision Tree

<table>
<tr>
<td width="240"><b>Need?</b></td>
<td><b>Use</b></td>
</tr>

<tr>
<td>Theme (Light/Dark)</td>
<td>Enum</td>
</tr>

<tr>
<td>User Role</td>
<td>Enum</td>
</tr>

<tr>
<td>API Result</td>
<td>Sealed Class</td>
</tr>

<tr>
<td>Login State</td>
<td>Sealed Class</td>
</tr>

<tr>
<td>Navigation Event</td>
<td>Sealed Class</td>
</tr>
</table>

---

# 5. Sealed Class vs Abstract Class

Another favorite.

<table>
<tr>
<td width="220"><b>Abstract Class</b></td>
<td><b>Sealed Class</b></td>
</tr>

<tr>
<td>Open hierarchy.</td>
<td>Closed hierarchy.</td>
</tr>

<tr>
<td>Unlimited subclasses.</td>
<td>Known subclasses only.</td>
</tr>

<tr>
<td>Compiler doesn't know children.</td>
<td>Compiler knows all children.</td>
</tr>

<tr>
<td>`when` needs `else`.</td>
<td>Exhaustive `when` without `else`.</td>
</tr>
</table>

---

## Example

### Abstract

```kotlin
abstract class Animal

class Dog : Animal()

class Cat : Animal()

class Tiger : Animal()
```

Compiler cannot know future subclasses.

Need `else`.

---

### Sealed

```kotlin
sealed class Animal {

    object Dog : Animal()

    object Cat : Animal()
}
```

Compiler knows hierarchy completely.

---

# 6. Sealed Interfaces (Kotlin 1.5+)

A newer Kotlin feature.

```kotlin
sealed interface NetworkState
```

Children.

```kotlin
object Connected : NetworkState

object Disconnected : NetworkState

data class Limited(
    val speedMbps: Int
) : NetworkState
```

---

## Why Sealed Interface?

Supports multiple inheritance.

```kotlin
sealed interface UiState

sealed interface AnalyticsState

data class HomeLoaded(
    val users: List<User>
) : UiState, AnalyticsState
```

Impossible with sealed classes.

---

## Sealed Class vs Sealed Interface

<table>
<tr>
<td width="220"><b>Sealed Class</b></td>
<td><b>Sealed Interface</b></td>
</tr>

<tr>
<td>Single inheritance.</td>
<td>Multiple inheritance.</td>
</tr>

<tr>
<td>Can have constructor.</td>
<td>No constructor.</td>
</tr>

<tr>
<td>Can hold state.</td>
<td>Contract only.</td>
</tr>

<tr>
<td>Great for state hierarchies.</td>
<td>Great for capability hierarchies.</td>
</tr>
</table>

---

# 7. Object vs Data Class Inside Sealed

### Loading State

```kotlin
object Loading : UiState()
```

Singleton.

No data.

---

### Success State

```kotlin
data class Success(
    val users: List<User>
) : UiState()
```

Carries payload.

---

### Error State

```kotlin
data class Error(
    val message: String
) : UiState()
```

Carries error details.

---

## Memory Visualization

<svg viewBox="0 0 640 180" xmlns="http://www.w3.org/2000/svg">
  <rect x="20" y="40" width="170" height="100" rx="12" fill="none" stroke="currentColor"/>
  <text x="105" y="65" text-anchor="middle" font-size="14" fill="currentColor">Loading Object</text>
  <text x="105" y="90" text-anchor="middle" font-size="12" fill="currentColor">Singleton</text>

  <rect x="240" y="25" width="170" height="130" rx="12" fill="none" stroke="currentColor"/>
  <text x="325" y="50" text-anchor="middle" font-size="14" fill="currentColor">Success Object</text>
  <text x="325" y="75" text-anchor="middle" font-size="12" fill="currentColor">users = [...]</text>
  <text x="325" y="95" text-anchor="middle" font-size="12" fill="currentColor">New instance each success</text>

  <rect x="450" y="25" width="170" height="130" rx="12" fill="none" stroke="currentColor"/>
  <text x="535" y="50" text-anchor="middle" font-size="14" fill="currentColor">Error Object</text>
  <text x="535" y="75" text-anchor="middle" font-size="12" fill="currentColor">message = "Timeout"</text>
</svg>

---

# 8. Exhaustive `when` ⭐⭐⭐⭐⭐

## The Biggest Advantage

```kotlin
when(state){

    is Loading -> showLoading()

    is Success -> showUsers(state.users)

    is Error -> showError(state.message)
}
```

No `else`.

---

## Compiler Safety

Add new state.

```kotlin
object Empty : UiState()
```

Compiler immediately shows error.

> `'when' expression must be exhaustive.`

This prevents production bugs.

---

## Why This Is Incredible

Without sealed classes...

New states silently ignored.

With sealed classes...

Compiler forces handling.

---

# 9. `when` Smart Casting

Inside each branch...

Compiler knows subtype.

```kotlin
when(state){

    is Success -> state.users

    is Error -> state.message
}
```

No manual cast.

---

## Compose Example

```kotlin
@Composable
fun HomeScreen(state: HomeUiState){

    when(state){

        is Loading -> LoadingView()

        is Success -> UserList(state.users)

        is Error -> ErrorView(state.message)
    }
}
```

Compose code becomes extremely clean.

---

# 10. Constructors in Sealed Classes

Sealed classes can have constructors.

```kotlin
sealed class NetworkResult(
    open val timestamp: Long
)
```

Children pass values.

```kotlin
data class Success(
    val users: List<User>,
    override val timestamp: Long
) : NetworkResult(timestamp)
```

Useful for shared metadata.

---

## Shared Property Example

```kotlin
sealed class DownloadState(
    open val progress: Int
)
```

Children inherit progress field if needed.

---

# 11. Visibility Rules

Sealed class constructor defaults to `protected`.

```kotlin
sealed class Result
```

Cannot instantiate directly.

Only subclasses instantiate.

---

## Private Constructor

```kotlin
sealed class State private constructor()
```

Rare advanced use.

Restricts creation even further.

---

# 12. Package Rules (Kotlin 1.5+)

Originally...

Subclasses had to be in same file.

Now they can be in **same package and module**.

```text
ui/state/
    UiState.kt
    Loading.kt
    Success.kt
    Error.kt
```

Cleaner project structure.

---

## Android Recommended Structure

```text
feature-home/

ui/

state/
    HomeUiState.kt
    HomeUiEvent.kt
    HomeUiEffect.kt
```

Very common in MVI projects.

---

# 13. When Should You Choose Sealed Classes?

## Use Sealed Classes For

- UI State
- Network Result
- Authentication State
- Navigation Event
- Download State
- Payment State
- Bluetooth State
- Camera Permission State

---

## Don't Use Sealed Classes For

- User Roles
- Theme Modes
- Languages
- Build Types
- Countries
- Fixed categories

Enums are better there.

---

# 14. Android Production Pattern Preview

This is what many production apps use.

```kotlin
sealed interface HomeUiState {

    object Loading : HomeUiState

    object Empty : HomeUiState

    data class Success(
        val users: List<User>
    ) : HomeUiState

    data class Error(
        val message: String
    ) : HomeUiState
}
```

We'll build this architecture in Part 2.

---

# 🧠 Interview Summary (Part 1)

## Top Questions Covered

1. What is a sealed class?
2. Why is it called "sealed"?
3. Sealed Class vs Enum.
4. Sealed Class vs Abstract Class.
5. Sealed Interface vs Sealed Class.
6. Why exhaustive `when` is important.
7. Why Compose prefers sealed UI states.
8. Constructor and visibility rules.
9. Same package rule (modern Kotlin).

---

# 📝 Revision Summary

- Sealed classes create a **closed hierarchy**.
- Compiler knows every subclass.
- Exhaustive `when` eliminates many runtime bugs.
- Use `object` for singleton states and `data class` for states carrying data.
- Sealed Interfaces enable multiple inheritance and are increasingly common in Compose and KMP architectures.
- Modern Android apps use sealed classes for nearly every state machine.

---

# Part 2 — Sealed Classes in Android Architecture (Production Level)

> This section teaches how Sealed Classes are used in real Android applications built with MVVM, MVI, Compose, Coroutines, Flow, Hilt, and Clean Architecture.

---

# 15. API Result Wrapper — The Most Common Android Pattern ⭐⭐⭐⭐⭐

## 🎭 Remember This Story — Ordering Food on Swiggy

You order food.

Your screen goes through exactly these states:

<svg viewBox="0 0 720 120" xmlns="http://www.w3.org/2000/svg">
  <rect x="20" y="30" width="140" height="50" rx="12" fill="none" stroke="currentColor"/>
  <text x="90" y="60" text-anchor="middle" fill="currentColor">🍔 Ordering</text>

  <path d="M160 55 L200 55" stroke="currentColor" stroke-width="2"/>

  <rect x="200" y="30" width="140" height="50" rx="12" fill="none" stroke="currentColor"/>
  <text x="270" y="60" text-anchor="middle" fill="currentColor">🧑‍🍳 Cooking</text>

  <path d="M340 55 L380 55" stroke="currentColor" stroke-width="2"/>

  <rect x="380" y="30" width="140" height="50" rx="12" fill="none" stroke="currentColor"/>
  <text x="450" y="60" text-anchor="middle" fill="currentColor">🚚 Delivery</text>

  <path d="M520 55 L560 55" stroke="currentColor" stroke-width="2"/>

  <rect x="560" y="30" width="140" height="50" rx="12" fill="none" stroke="currentColor"/>
  <text x="630" y="60" text-anchor="middle" fill="currentColor">✅ Delivered</text>
</svg>

Possible outcomes:

- Loading
- Success
- Error

No other state.

This is a sealed class.

---

## Step 1 — Create Generic Result Wrapper

```kotlin
sealed interface Result<out T> {

    data object Loading : Result<Nothing>

    data class Success<T>(
        val data: T
    ) : Result<T>

    data class Error(
        val exception: Throwable,
        val message: String
    ) : Result<Nothing>
}
```

### Why `out T`?

Covariance allows:

```kotlin
Result<List<User>>
```

to be used where `Result<Any>` is expected.

We'll cover variance deeply in Generics.

---

## Why `Nothing`?

`Loading` and `Error` don't carry data.

`Nothing` is Kotlin's bottom type.

```kotlin
Loading : Result<Nothing>
```

Very common interview question.

---

# Repository Layer Example

```kotlin
class UserRepository(
    private val api: ApiService
){

    suspend fun getUsers(): Result<List<User>>{

        return try{

            Result.Success(api.getUsers())

        }catch(e: Exception){

            Result.Error(
                exception = e,
                message = "Unable to fetch users."
            )
        }
    }
}
```

Repository never throws.

It always returns **state**.

---

# ViewModel Layer Example

```kotlin
@HiltViewModel
class HomeViewModel @Inject constructor(
    private val repository: UserRepository
): ViewModel(){

    private val _state =
        MutableStateFlow<Result<List<User>>>(
            Result.Loading
        )

    val state = _state.asStateFlow()

    fun loadUsers(){

        viewModelScope.launch{

            _state.value = Result.Loading

            _state.value = repository.getUsers()
        }
    }
}
```

Beautiful.

No booleans.

No nullable lists.

No nullable error.

---

# Compose Screen Example

```kotlin
@Composable
fun HomeScreen(
    state: Result<List<User>>
){

    when(state){

        Result.Loading -> LoadingScreen()

        is Result.Success ->
            UserList(state.data)

        is Result.Error ->
            ErrorScreen(state.message)
    }
}
```

### Why This Is Better Than Flags?

Bad approach:

```kotlin
loading = false
users = null
error = "Timeout"
```

Impossible combinations.

Sealed classes eliminate them.

---

# 16. Generic API Result Wrapper (Production Ready)

A reusable wrapper for the entire project.

```kotlin
sealed interface NetworkResult<out T>{

    data object Loading : NetworkResult<Nothing>

    data class Success<T>(
        val body:T,
        val code:Int
    ):NetworkResult<T>

    data class Failure(
        val code:Int?,
        val message:String
    ):NetworkResult<Nothing>

    data class Exception(
        val throwable:Throwable
    ):NetworkResult<Nothing>
}
```

Now distinguish between:

<table>
<tr>
<td width="220"><b>Failure Type</b></td>
<td><b>Example</b></td>
</tr>

<tr>
<td>HTTP Error</td>
<td>404 / 500</td>
</tr>

<tr>
<td>Exception</td>
<td>No Internet</td>
</tr>

<tr>
<td>Loading</td>
<td>Progress indicator</td>
</tr>

<tr>
<td>Success</td>
<td>Actual API payload</td>
</tr>
</table>

Senior Android architecture pattern.

---

# 17. Mapping API Result to UI State

Clean Architecture says:

```
API DTO

↓

Repository

↓

Domain Result

↓

UI State
```

---

## DTO

```kotlin
data class UserDto(
    val id:Int,
    val name:String
)
```

---

## Domain Model

```kotlin
data class User(
    val id:Int,
    val name:String
)
```

---

## UI State

```kotlin
sealed interface HomeUiState{

    data object Loading:HomeUiState

    data object Empty:HomeUiState

    data class Success(
        val users:List<User>
    ):HomeUiState

    data class Error(
        val message:String
    ):HomeUiState
}
```

---

## Mapper in ViewModel

```kotlin
_state.value =
    when(val result = repository.users()){

        Result.Loading ->
            HomeUiState.Loading

        is Result.Success ->

            if(result.data.isEmpty())
                HomeUiState.Empty
            else
                HomeUiState.Success(result.data)

        is Result.Error ->
            HomeUiState.Error(result.message)
    }
```

Notice UI doesn't know network errors.

---

# 18. MVVM + Sealed Classes Architecture

<svg viewBox="0 0 720 320" xmlns="http://www.w3.org/2000/svg">
  <rect x="250" y="10" width="220" height="50" rx="12" fill="none" stroke="currentColor"/>
  <text x="360" y="40" text-anchor="middle" fill="currentColor">Compose Screen</text>

  <path d="M360 60 V90" stroke="currentColor" stroke-width="2"/>

  <rect x="210" y="90" width="300" height="60" rx="12" fill="none" stroke="currentColor"/>
  <text x="360" y="118" text-anchor="middle" fill="currentColor">StateFlow&lt;HomeUiState&gt;</text>
  <text x="360" y="138" text-anchor="middle" font-size="12" fill="currentColor">Loading • Success • Error • Empty</text>

  <path d="M360 150 V180" stroke="currentColor" stroke-width="2"/>

  <rect x="240" y="180" width="240" height="55" rx="12" fill="none" stroke="currentColor"/>
  <text x="360" y="212" text-anchor="middle" fill="currentColor">HomeViewModel</text>

  <path d="M360 235 V265" stroke="currentColor" stroke-width="2"/>

  <rect x="220" y="265" width="280" height="45" rx="12" fill="none" stroke="currentColor"/>
  <text x="360" y="293" text-anchor="middle" fill="currentColor">Repository → NetworkResult</text>
</svg>

---

## Why This Architecture Wins Interviews

- Single source of truth.
- Immutable state.
- Exhaustive rendering.
- Easy testing.
- Easy preview.

---

# 19. MVI Architecture — Intent, State, Effect ⭐⭐⭐⭐⭐

One of Google's favorite architecture discussions.

## 🎭 Remember This Story — Restaurant Waiter

Customer doesn't directly cook food.

Workflow:

<svg viewBox="0 0 720 120" xmlns="http://www.w3.org/2000/svg">
  {#each [
      {x:20,label:"Customer",emoji:"🙋"},
      {x:200,label:"Waiter",emoji:"🧑‍🍳"},
      {x:380,label:"Kitchen",emoji:"🍳"},
      {x:560,label:"Table",emoji:"🍽️"}
    ] as step, i}
    <rect x={step.x} y="30" width="120" height="50" rx="12" fill="none" stroke="currentColor"/>
    <text x={step.x+60} y="52" text-anchor="middle" fill="currentColor">{step.emoji}</text>
    <text x={step.x+60} y="68" text-anchor="middle" font-size="12" fill="currentColor">{step.label}</text>
    {#if i < 3}
      <path d={`M${step.x+120} 55 L${step.x+180} 55`} stroke="currentColor" stroke-width="2"/>
    {/if}
  {/each}
</svg>

Customer sends **Intent**.

Kitchen updates **State**.

Waiter triggers **Effects**.

Exactly MVI.

---

# Intent

User actions.

```kotlin
sealed interface HomeIntent{

    data object Refresh:HomeIntent

    data object Retry:HomeIntent

    data class Search(
        val query:String
    ):HomeIntent

    data class UserClicked(
        val id:Int
    ):HomeIntent
}
```

Every user interaction becomes a sealed event.

---

# State

Everything UI needs.

```kotlin
sealed interface HomeUiState{

    data object Loading:HomeUiState

    data class Success(
        val users:List<User>,
        val search:String
    ):HomeUiState

    data class Error(
        val message:String
    ):HomeUiState
}
```

Immutable.

---

# Effect

One-time events.

```kotlin
sealed interface HomeEffect{

    data class ShowToast(
        val message:String
    ):HomeEffect

    data class Navigate(
        val route:String
    ):HomeEffect

    data object ShowLogoutDialog:HomeEffect
}
```

Effects should never stay in state.

---

# Full MVI Flow Diagram

<svg viewBox="0 0 720 260" xmlns="http://www.w3.org/2000/svg">
  <rect x="20" y="100" width="150" height="60" rx="12" fill="none" stroke="currentColor"/>
  <text x="95" y="135" text-anchor="middle" fill="currentColor">UI Intent</text>

  <path d="M170 130 L230 130" stroke="currentColor" stroke-width="2"/>

  <rect x="230" y="85" width="220" height="90" rx="12" fill="none" stroke="currentColor"/>
  <text x="340" y="110" text-anchor="middle" fill="currentColor">ViewModel</text>
  <text x="340" y="130" text-anchor="middle" font-size="12" fill="currentColor">Reducer + Business Logic</text>
  <text x="340" y="150" text-anchor="middle" font-size="12" fill="currentColor">Repository Calls</text>

  <path d="M340 175 V215" stroke="currentColor" stroke-width="2"/>

  <rect x="220" y="215" width="240" height="35" rx="12" fill="none" stroke="currentColor"/>
  <text x="340" y="238" text-anchor="middle" fill="currentColor">HomeUiState / HomeEffect</text>

  <path d="M460 130 L520 130" stroke="currentColor" stroke-width="2"/>

  <rect x="520" y="100" width="180" height="60" rx="12" fill="none" stroke="currentColor"/>
  <text x="610" y="135" text-anchor="middle" fill="currentColor">Repository</text>
</svg>

---

# 20. Reducer Pattern

Reducers convert **State + Intent → New State**.

```kotlin
private fun reduce(
    state:HomeUiState,
    intent:HomeIntent
):HomeUiState{

    return when(intent){

        HomeIntent.Refresh ->
            HomeUiState.Loading

        is HomeIntent.Search ->
            HomeUiState.Success(
                users = filter(intent.query),
                search = intent.query
            )

        HomeIntent.Retry ->
            HomeUiState.Loading

        is HomeIntent.UserClicked ->
            state
    }
}
```

Pure function.

Easy testing.

---

# 21. Authentication State Machine

## 🎭 Story — Netflix Login

Possible states only:

<svg viewBox="0 0 720 120" xmlns="http://www.w3.org/2000/svg">
  {#each [
      {x:20,label:"Logged Out"},
      {x:190,label:"Loading"},
      {x:360,label:"Logged In"},
      {x:530,label:"Session Expired"}
    ] as s, i}
    <rect x={s.x} y="30" width="150" height="50" rx="12" fill="none" stroke="currentColor"/>
    <text x={s.x+75} y="60" text-anchor="middle" font-size="12" fill="currentColor">{s.label}</text>
    {#if i < 3}
      <path d={`M${s.x+150} 55 L${s.x+170} 55`} stroke="currentColor" stroke-width="2"/>
    {/if}
  {/each}
</svg>

---

## Kotlin Implementation

```kotlin
sealed interface AuthState{

    data object LoggedOut:AuthState

    data object Loading:AuthState

    data class LoggedIn(
        val user:User,
        val token:String
    ):AuthState

    data object SessionExpired:AuthState
}
```

---

## Compose Rendering

```kotlin
when(state){

    AuthState.Loading ->
        LoadingScreen()

    AuthState.LoggedOut ->
        LoginScreen()

    is AuthState.LoggedIn ->
        Dashboard(state.user)

    AuthState.SessionExpired ->
        SessionExpiredDialog()
}
```

Every screen guaranteed.

---

# 22. Payment State Machine

<svg viewBox="0 0 720 140" xmlns="http://www.w3.org/2000/svg">
  {#each [
      {x:20,label:"Initiated"},
      {x:180,label:"Processing"},
      {x:340,label:"Success"},
      {x:500,label:"Failed"}
    ] as s, i}
    <rect x={s.x} y="35" width="140" height="50" rx="12" fill="none" stroke="currentColor"/>
    <text x={s.x+70} y="65" text-anchor="middle" font-size="12" fill="currentColor">{s.label}</text>
    {#if i < 3}
      <path d={`M${s.x+140} 60 L${s.x+160} 60`} stroke="currentColor" stroke-width="2"/>
    {/if}
  {/each}
</svg>

```kotlin
sealed interface PaymentState{

    data object Initiated:PaymentState

    data object Processing:PaymentState

    data class Success(
        val receipt:Receipt
    ):PaymentState

    data class Failed(
        val reason:String
    ):PaymentState
}
```

Production-grade payment flow.

---

# 23. Download State Machine

Downloads have progress.

```kotlin
sealed interface DownloadState{

    data object Waiting:DownloadState

    data class Progress(
        val percentage:Int
    ):DownloadState

    data class Success(
        val filePath:String
    ):DownloadState

    data class Failed(
        val error:String
    ):DownloadState
}
```

---

## Compose Progress Example

```kotlin
when(state){

    is DownloadState.Progress ->
        LinearProgressIndicator(
            progress = state.percentage / 100f
        )

    is DownloadState.Success ->
        OpenFileButton(state.filePath)

    is DownloadState.Failed ->
        RetryButton()
}
```

Very realistic interview example.

---

# 24. Search Screen State Machine

Instead of booleans.

```kotlin
sealed interface SearchState{

    data object Idle:SearchState

    data object Loading:SearchState

    data class Results(
        val items:List<Product>
    ):SearchState

    data object NoResults:SearchState

    data class Error(
        val message:String
    ):SearchState
}
```

Impossible combinations disappear.

---

# 25. StateFlow + Sealed Classes

The modern Android combination.

```kotlin
private val _state =
    MutableStateFlow<HomeUiState>(
        HomeUiState.Loading
    )

val state = _state.asStateFlow()
```

Observe.

```kotlin
val uiState by viewModel.state.collectAsStateWithLifecycle()
```

Render.

```kotlin
when(uiState){
    ...
}
```

This is the standard architecture in Compose apps.

---

# 26. Why Not Use Boolean Flags?

Bad.

```kotlin
data class HomeUiState(

    val loading:Boolean,

    val error:String?,

    val users:List<User>
)
```

Possible invalid states:

<table>
<tr>
<td width="260"><b>State</b></td>
<td><b>Valid?</b></td>
</tr>

<tr>
<td>loading=true + users filled</td>
<td>❌</td>
</tr>

<tr>
<td>loading=false + error + users</td>
<td>❌</td>
</tr>

<tr>
<td>loading=false + no users + no error</td>
<td>❓</td>
</tr>
</table>

---

## Sealed Fix

Each state becomes exclusive.

Compiler guarantees correctness.

---

# 27. Repository + ViewModel + Compose Complete Example

<svg viewBox="0 0 720 260" xmlns="http://www.w3.org/2000/svg">
  <rect x="250" y="10" width="220" height="45" rx="12" fill="none" stroke="currentColor"/>
  <text x="360" y="38" text-anchor="middle" fill="currentColor">Compose Screen</text>

  <path d="M360 55 V80" stroke="currentColor" stroke-width="2"/>

  <rect x="180" y="80" width="360" height="60" rx="12" fill="none" stroke="currentColor"/>
  <text x="360" y="105" text-anchor="middle" fill="currentColor">StateFlow&lt;HomeUiState&gt;</text>
  <text x="360" y="125" text-anchor="middle" font-size="12" fill="currentColor">Loading / Success / Error / Empty</text>

  <path d="M360 140 V165" stroke="currentColor" stroke-width="2"/>

  <rect x="220" y="165" width="280" height="45" rx="12" fill="none" stroke="currentColor"/>
  <text x="360" y="193" text-anchor="middle" fill="currentColor">ViewModel</text>

  <path d="M360 210 V235" stroke="currentColor" stroke-width="2"/>

  <rect x="190" y="235" width="340" height="25" rx="10" fill="none" stroke="currentColor"/>
  <text x="360" y="252" text-anchor="middle" font-size="12" fill="currentColor">Repository → NetworkResult → Mapper → UiState</text>
</svg>

This is a production-ready Compose architecture.

---

# 28. Best Practices for Android Architecture

### ✅ Use Sealed Classes For

- UI State
- API Result
- Navigation Events
- Authentication
- Download Progress
- Payment Flow
- Bluetooth State
- Camera Permission Flow

### ❌ Don't Use Sealed Classes For

- Theme
- Language
- User Role
- Country
- Gender
- Currency Type

Enums fit those better.

---

# ⚠️ Production Pitfalls

### Pitfall 1 — Boolean Explosion

```kotlin
loading
error
empty
refreshing
offline
```

Five booleans create **32 possible combinations**.

Use sealed classes.

---

### Pitfall 2 — Error Stored in Success State

```kotlin
Success(users,error)
```

Impossible architecture.

---

### Pitfall 3 — Mutable UI State

Never mutate sealed state objects.

Always emit a **new state**.

---

### Pitfall 4 — One-Time Events Stored in State

Navigation inside state causes repeated navigation after configuration changes.

We'll solve this in **Part 3** with **Effects**.

---

# 🧠 Senior Android Interview Questions (Part 2)

### Basic

1. Why use sealed classes for API responses?
2. Why not return nullable data from repositories?
3. Loading vs Empty state?

### Intermediate

4. How does MVI use sealed classes?
5. Intent vs State vs Effect?
6. Why StateFlow pairs well with sealed classes?

### Advanced

7. Design a payment state machine.
8. Design a download manager state machine.
9. Explain impossible states and how sealed classes eliminate them.
10. Why Google recommends immutable UI state?

---

# 📝 Revision Summary (Part 2)

- Sealed classes are the **foundation of MVVM/MVI UI state management**.
- `Result<T>` wrappers replace exceptions and nullable returns.
- UI renders sealed states with exhaustive `when`.
- MVI separates **Intent**, **State**, and **Effect** using sealed hierarchies.
- State machines (authentication, payment, download, search) are naturally modeled using sealed classes.
- `StateFlow + Sealed Class + Compose` is the standard modern Android architecture.

---

# Part 3 — Jetpack Compose, Navigation, Side Effects & KMP

> Learn the production architecture used by Google, PhonePe, CRED, Meesho, Uber, and modern Compose applications.

---

# 29. UiState vs UiEvent vs UiEffect ⭐⭐⭐⭐⭐

## 🎭 Remember This Story — Zomato Food Ordering

Imagine you're ordering food on Zomato.

There are **three different kinds of things** happening.

<table>
<tr>
<td width="200"><b>Concept</b></td>
<td><b>Zomato Example</b></td>
</tr>

<tr>
<td>**State**</td>
<td>Screen is Loading / Restaurant Loaded / Error.</td>
</tr>

<tr>
<td>**Event**</td>
<td>User clicks "Order Now".</td>
</tr>

<tr>
<td>**Effect**</td>
<td>Show Toast: "Order Placed Successfully". Navigate to Tracking Screen.</td>
</tr>
</table>

These three should **never be mixed**.

---

## The Architecture

<svg viewBox="0 0 720 280" xmlns="http://www.w3.org/2000/svg">
  <rect x="20" y="110" width="160" height="60" rx="12" fill="none" stroke="currentColor"/>
  <text x="100" y="145" text-anchor="middle" fill="currentColor">Compose UI</text>

  <path d="M180 140 L250 140" stroke="currentColor" stroke-width="2"/>

  <rect x="250" y="30" width="200" height="70" rx="12" fill="none" stroke="currentColor"/>
  <text x="350" y="55" text-anchor="middle" fill="currentColor">UiEvent</text>
  <text x="350" y="75" text-anchor="middle" font-size="12" fill="currentColor">Button Click • Search • Retry</text>

  <rect x="250" y="110" width="200" height="70" rx="12" fill="none" stroke="currentColor"/>
  <text x="350" y="135" text-anchor="middle" fill="currentColor">ViewModel</text>
  <text x="350" y="155" text-anchor="middle" font-size="12" fill="currentColor">Business Logic</text>

  <rect x="250" y="200" width="200" height="70" rx="12" fill="none" stroke="currentColor"/>
  <text x="350" y="225" text-anchor="middle" fill="currentColor">UiState</text>
  <text x="350" y="245" text-anchor="middle" font-size="12" fill="currentColor">Loading • Success • Error</text>

  <path d="M450 140 L520 140" stroke="currentColor" stroke-width="2"/>

  <rect x="520" y="110" width="180" height="70" rx="12" fill="none" stroke="currentColor"/>
  <text x="610" y="135" text-anchor="middle" fill="currentColor">UiEffect</text>
  <text x="610" y="155" text-anchor="middle" font-size="12" fill="currentColor">Toast • Navigate • Snackbar</text>
</svg>

**Golden Rule**

- State → Persistent UI.
- Event → User Input.
- Effect → One-time action.

---

# 30. UiState — Persistent Screen State

Everything needed to draw the screen.

```kotlin
sealed interface ProfileUiState{

    data object Loading : ProfileUiState

    data object Empty : ProfileUiState

    data class Success(
        val profile: User,
        val posts: List<Post>
    ) : ProfileUiState

    data class Error(
        val message: String
    ) : ProfileUiState
}
```

---

## Why State Is Persistent?

If user rotates screen...

Screen should still know:

- Loaded profile.
- Posts.
- Error.

State survives configuration changes.

---

## Compose Rendering

```kotlin
@Composable
fun ProfileScreen(state: ProfileUiState){

    when(state){

        ProfileUiState.Loading ->
            LoadingScreen()

        ProfileUiState.Empty ->
            EmptyScreen()

        is ProfileUiState.Success ->
            ProfileContent(state.profile, state.posts)

        is ProfileUiState.Error ->
            ErrorScreen(state.message)
    }
}
```

Simple.

Compiler-safe.

---

# 31. UiEvent — User Intent

Events are actions from UI.

```kotlin
sealed interface ProfileUiEvent{

    data object Refresh : ProfileUiEvent

    data object Retry : ProfileUiEvent

    data object Logout : ProfileUiEvent

    data class Search(
        val query:String
    ) : ProfileUiEvent

    data class FollowClicked(
        val userId:Int
    ) : ProfileUiEvent
}
```

---

## Event Flow

```
User Click

↓

UiEvent

↓

ViewModel
```

---

## Compose Example

```kotlin
Button(
    onClick = {
        onEvent(ProfileUiEvent.Refresh)
    }
){
    Text("Refresh")
}
```

Screen doesn't know ViewModel implementation.

Very testable.

---

# 32. ViewModel Handling Events

```kotlin
fun onEvent(event: ProfileUiEvent){

    when(event){

        ProfileUiEvent.Refresh ->
            loadProfile()

        ProfileUiEvent.Logout ->
            logout()

        is ProfileUiEvent.Search ->
            search(event.query)

        is ProfileUiEvent.FollowClicked ->
            follow(event.userId)

        ProfileUiEvent.Retry ->
            retry()
    }
}
```

One entry point for UI actions.

Google interview favorite.

---

# 33. UiEffect — One-Time Events ⭐⭐⭐⭐⭐

## 🎭 Remember This Story — OTP Sent Toast

When OTP is sent...

Toast appears.

Rotate screen...

Toast **should not appear again**.

Why?

Toast is an **effect**, not state.

---

## Examples of Effects

<table>
<tr>
<td width="240"><b>Effect</b></td>
<td><b>Persistent?</b></td>
</tr>

<tr>
<td>Toast</td>
<td>❌</td>
</tr>

<tr>
<td>Snackbar</td>
<td>❌</td>
</tr>

<tr>
<td>Navigation</td>
<td>❌</td>
</tr>

<tr>
<td>Open BottomSheet</td>
<td>❌</td>
</tr>

<tr>
<td>Show Dialog</td>
<td>Usually ❌</td>
</tr>

<tr>
<td>Play Sound</td>
<td>❌</td>
</tr>
</table>

---

## Define Effect

```kotlin
sealed interface ProfileUiEffect{

    data class ShowToast(
        val message:String
    ) : ProfileUiEffect

    data class Navigate(
        val route:String
    ) : ProfileUiEffect

    data class ShowSnackbar(
        val message:String
    ) : ProfileUiEffect

    data object LogoutCompleted : ProfileUiEffect
}
```

---

# 34. SharedFlow for Effects

Use `SharedFlow`.

```kotlin
private val _effect =
    MutableSharedFlow<ProfileUiEffect>()

val effect = _effect.asSharedFlow()
```

Emit.

```kotlin
_effect.emit(
    ProfileUiEffect.ShowToast("Saved")
)
```

---

## Why Not StateFlow?

`StateFlow` replays latest value.

Toast repeats.

Bad UX.

`SharedFlow` emits one-time events.

---

# 35. Collecting Effects in Compose

```kotlin
@Composable
fun ProfileScreen(
    viewModel: ProfileViewModel
){

    val snackbarHostState =
        remember { SnackbarHostState() }

    LaunchedEffect(Unit){

        viewModel.effect.collect{ effect ->

            when(effect){

                is ProfileUiEffect.ShowToast -> {
                    // Toast
                }

                is ProfileUiEffect.ShowSnackbar ->
                    snackbarHostState.showSnackbar(effect.message)

                is ProfileUiEffect.Navigate -> {
                    // Navigation
                }

                ProfileUiEffect.LogoutCompleted -> {}
            }
        }
    }
}
```

This is production Compose architecture.

---

# 36. Navigation Events ⭐⭐⭐⭐⭐

Navigation should **never** be part of UiState.

---

## Wrong

```kotlin
data class HomeUiState(
    val navigateToProfile:Boolean
)
```

Rotate screen.

Navigates again.

---

## Correct

```kotlin
sealed interface HomeUiEffect{

    data class NavigateToProfile(
        val id:Int
    ) : HomeUiEffect

    data object NavigateBack : HomeUiEffect

    data object OpenSettings : HomeUiEffect
}
```

---

## ViewModel Emits Navigation

```kotlin
_effect.emit(
    HomeUiEffect.NavigateToProfile(user.id)
)
```

---

## Compose Collects Navigation

```kotlin
when(effect){

    is HomeUiEffect.NavigateToProfile ->
        navController.navigate("profile/${effect.id}")

    HomeUiEffect.NavigateBack ->
        navController.popBackStack()

    HomeUiEffect.OpenSettings ->
        navController.navigate("settings")
}
```

Clean separation.

---

# 37. Snackbar Architecture

## Snackbar Effect

```kotlin
sealed interface SnackbarEffect{

    data class Success(
        val message:String
    ) : SnackbarEffect

    data class Error(
        val message:String
    ) : SnackbarEffect
}
```

---

## ViewModel

```kotlin
_effect.emit(
    SnackbarEffect.Success("Profile Updated")
)
```

---

## Compose

```kotlin
snackbarHostState.showSnackbar(
    effect.message
)
```

One-time only.

---

# 38. Dialog State vs Dialog Effect

### When Dialog Is State

Dialog remains visible until user closes it.

```kotlin
sealed interface DialogState{

    data object Hidden : DialogState

    data class Confirmation(
        val title:String,
        val description:String
    ) : DialogState
}
```

Persistent UI.

---

### When Dialog Is Effect

Dialog shown only once.

```kotlin
sealed interface DialogEffect{

    data class ShowError(
        val message:String
    ) : DialogEffect
}
```

Choose based on lifecycle.

---

# 39. Bottom Sheet State

Compose Material3 example.

```kotlin
sealed interface BottomSheetState{

    data object Hidden : BottomSheetState

    data class ShareSheet(
        val url:String
    ) : BottomSheetState

    data class FilterSheet(
        val filters:List<String>
    ) : BottomSheetState
}
```

Only one sheet active.

---

# 40. Authentication Flow in Compose

<svg viewBox="0 0 720 140" xmlns="http://www.w3.org/2000/svg">
  {#each [
      {x:20,label:"Splash"},
      {x:170,label:"Login"},
      {x:320,label:"OTP"},
      {x:470,label:"Dashboard"},
      {x:620,label:"Expired"}
    ] as s, i}
    <rect x={s.x} y="35" width="110" height="50" rx="12" fill="none" stroke="currentColor"/>
    <text x={s.x+55} y="63" text-anchor="middle" font-size="12" fill="currentColor">{s.label}</text>
    {#if i < 4}
      <path d={`M${s.x+110} 60 L${s.x+150} 60`} stroke="currentColor" stroke-width="2"/>
    {/if}
  {/each}
</svg>

```kotlin
sealed interface AuthUiState{

    data object Splash : AuthUiState

    data object Login : AuthUiState

    data object OtpVerification : AuthUiState

    data class Dashboard(
        val user:User
    ) : AuthUiState

    data object SessionExpired : AuthUiState
}
```

---

## Render Authentication

```kotlin
when(state){

    AuthUiState.Splash -> SplashScreen()

    AuthUiState.Login -> LoginScreen()

    AuthUiState.OtpVerification -> OtpScreen()

    is AuthUiState.Dashboard ->
        Dashboard(state.user)

    AuthUiState.SessionExpired ->
        SessionExpiredScreen()
}
```

---

# 41. Search Screen — State + Event + Effect

### State

```kotlin
sealed interface SearchState{

    data object Idle : SearchState

    data object Loading : SearchState

    data class Results(
        val products:List<Product>
    ) : SearchState

    data object NoResults : SearchState
}
```

---

### Events

```kotlin
sealed interface SearchEvent{

    data class QueryChanged(
        val query:String
    ) : SearchEvent

    data object Retry : SearchEvent

    data object ClearSearch : SearchEvent
}
```

---

### Effects

```kotlin
sealed interface SearchEffect{

    data class ShowToast(
        val message:String
    ) : SearchEffect

    data class NavigateToProduct(
        val productId:Int
    ) : SearchEffect
}
```

This is a perfect MVI interview example.

---

# 42. Compose Recomposition with Sealed Classes ⭐⭐⭐⭐⭐

Why immutable sealed states help Compose.

```kotlin
_state.value =
    HomeUiState.Success(users)
```

Entire state changes.

Compose recomposes affected UI.

---

## Stable State Flow

```kotlin
val uiState by viewModel.state.collectAsStateWithLifecycle()
```

---

## Why Better Than Mutable Fields?

Mutable fields create partial updates.

Immutable sealed states create predictable recomposition boundaries.

---

# 43. Kotlin Multiplatform (KMP)

Shared module.

```kotlin
sealed interface NetworkState{

    data object Connected : NetworkState

    data object Offline : NetworkState

    data class SlowConnection(
        val speedMbps:Int
    ) : NetworkState
}
```

Android UI.

```kotlin
when(state){...}
```

iOS Swift receives equivalent hierarchy.

Huge KMP advantage.

---

# 44. Shared Error Hierarchy

```kotlin
sealed interface AppError{

    data object Network : AppError

    data object Unauthorized : AppError

    data object Server : AppError

    data class Validation(
        val field:String
    ) : AppError
}
```

Shared across Android and iOS.

---

# 45. Serialization with Sealed Classes

Kotlin Serialization supports polymorphism.

```kotlin
@Serializable
sealed class PaymentState
```

Children.

```kotlin
@Serializable
data class Success(...)
```

JSON includes type information.

Useful for offline caching.

---

# 46. Testing UiState

```kotlin
@Test
fun success_state_contains_users(){

    val state =
        HomeUiState.Success(listOf(User(...)))

    assertTrue(state is HomeUiState.Success)
}
```

---

## Testing StateFlow

```kotlin
viewModel.state.test{

    assertEquals(
        HomeUiState.Loading,
        awaitItem()
    )

    assertTrue(awaitItem() is HomeUiState.Success)
}
```

Very common Turbine interview topic.

---

# 47. Best Practices for Compose + Sealed Classes

### ✅ Keep UiState Immutable

### ✅ Keep UiEvents Stateless

### ✅ Keep UiEffects One-Time

### ✅ Collect Effects in `LaunchedEffect`

### ✅ Never Put Navigation in UiState

### ✅ Never Put Snackbar in UiState

### ✅ Use StateFlow for State

### ✅ Use SharedFlow for Effects

---

# ⚠️ Production Pitfalls

### Pitfall 1 — Navigation Stored in State

Causes repeated navigation after rotation.

---

### Pitfall 2 — Toast Stored in State

Toast repeats after recomposition.

---

### Pitfall 3 — Mutable Lists Inside State

Use immutable lists whenever possible.

---

### Pitfall 4 — Multiple Boolean Flags

Replace with sealed state hierarchy.

---

### Pitfall 5 — Collecting SharedFlow Without Lifecycle

Use `LaunchedEffect` + lifecycle-aware collection.

---

# 🧠 Senior Android Interview Questions (Compose)

### Basic

1. Difference between UiState and UiEvent?
2. Difference between UiState and UiEffect?

### Intermediate

3. Why SharedFlow for navigation?
4. Why StateFlow for state?

### Advanced

5. Design MVI architecture using sealed classes.
6. Snackbar as State or Effect?
7. Dialog as State or Effect?
8. Navigation event architecture.
9. Compose recomposition with immutable sealed states.
10. KMP shared sealed hierarchy.

---

# 📝 Revision Summary (Part 3)

- Modern Compose apps separate **State**, **Event**, and **Effect**.
- `StateFlow<UiState>` drives the UI.
- `SharedFlow<UiEffect>` delivers one-time actions like navigation and snackbars.
- Sealed classes make MVI reducers exhaustive and type-safe.
- KMP shares sealed hierarchies across Android and iOS.
- This architecture is considered production-grade for large Compose applications.

---

# Part 4 — JVM Internals, Performance, Testing & Interview Mastery

> Understand how sealed classes work under the hood, how Compose optimizes them, testing patterns, production pitfalls, and the complete interview preparation section.

---

# 48. JVM Internals — How Sealed Classes Work ⭐⭐⭐⭐⭐

This is a favorite discussion in **Google, Uber, Amazon and Microsoft** interviews.

## Kotlin Code

```kotlin
sealed class Result {

    object Loading : Result()

    data class Success(val users: List<User>) : Result()

    data class Error(val message: String) : Result()
}
```

---

## Simplified Decompiled Java

```java
public abstract class Result {

    private Result() {}

    public static final class Loading extends Result {
        public static final Loading INSTANCE = new Loading();
    }

    public static final class Success extends Result {
        private final List<User> users;
    }

    public static final class Error extends Result {
        private final String message;
    }
}
```

### Things to Notice

- Sealed class becomes an **abstract class**.
- `object` becomes a singleton (`INSTANCE`).
- `data class` becomes a normal final class extending the sealed parent.
- Constructor is protected/private.

---

# 49. Memory Model — Object vs Data Class

## Loading Object

```kotlin
object Loading : UiState
```

Memory:

<svg viewBox="0 0 540 160" xmlns="http://www.w3.org/2000/svg">
  <rect x="140" y="20" width="260" height="110" rx="12"
        fill="none" stroke="currentColor"/>
  <text x="270" y="50" text-anchor="middle" fill="currentColor">Loading Singleton</text>
  <text x="270" y="80" text-anchor="middle" font-size="13" fill="currentColor">Only one object exists.</text>
  <text x="270" y="100" text-anchor="middle" font-size="13" fill="currentColor">Shared across the app.</text>
</svg>

Every screen references the same object.

---

## Success Data Class

```kotlin
Success(users)
```

Memory:

<svg viewBox="0 0 540 180" xmlns="http://www.w3.org/2000/svg">
  <rect x="40" y="30" width="180" height="120" rx="12"
        fill="none" stroke="currentColor"/>
  <text x="130" y="55" text-anchor="middle" fill="currentColor">Success #1</text>
  <text x="130" y="85" text-anchor="middle" font-size="13" fill="currentColor">users=[A,B,C]</text>

  <rect x="320" y="30" width="180" height="120" rx="12"
        fill="none" stroke="currentColor"/>
  <text x="410" y="55" text-anchor="middle" fill="currentColor">Success #2</text>
  <text x="410" y="85" text-anchor="middle" font-size="13" fill="currentColor">users=[A,B,C,D]</text>
</svg>

Each emission creates a new immutable object.

---

## Why Compose Likes This

Compose compares state references and values.

Immutable new objects are predictable.

---

# 50. Exhaustive `when` Compiler Internals

```kotlin
when(state){

    Loading -> ...

    is Success -> ...

    is Error -> ...
}
```

Compiler checks all subclasses.

If new subclass added...

```kotlin
object Empty : UiState()
```

Compiler immediately fails.

```
'when' expression must be exhaustive.
```

---

## Why This Prevents Bugs

Without exhaustive checking...

New state might never render.

With sealed classes...

Impossible to forget.

---

# 51. Sealed Class Equality

### Object Equality

```kotlin
Loading == Loading
```

True.

Same singleton.

---

### Data Equality

```kotlin
Success(listOf(1)) == Success(listOf(1))
```

True.

Generated `equals()`.

---

### Error Equality

```kotlin
Error("Timeout") == Error("Timeout")
```

True.

Useful for testing.

---

# 52. Sealed Interfaces JVM Difference

```kotlin
sealed interface UiState
```

Decompiler.

```java
public interface UiState
```

Children implement interface instead of extending class.

Benefits:

- Multiple inheritance.
- Better capability modeling.

---

# 53. Performance — Compose Recomposition ⭐⭐⭐⭐⭐

## Immutable State Emission

```kotlin
_state.value = Success(users)
```

Compose sees new state.

Only affected composables recompose.

---

## Mutable State Example (Bad)

```kotlin
users.add(newUser)
```

List reference unchanged.

Compose may miss updates depending on state holder.

---

## Correct Pattern

```kotlin
_state.value =
    Success(
        users + newUser
    )
```

New list.

New state.

Predictable recomposition.

---

# 54. Stable vs Unstable Types in Compose

Compose prefers immutable state.

```kotlin
data class UiState(
    val users: List<User>
)
```

Better.

Avoid:

```kotlin
MutableList<User>
HashMap
ArrayList
```

Inside UI state.

---

## Production Recommendation

Use immutable collections where possible.

---

# 55. Nested Sealed Hierarchies

Large apps often nest state machines.

```kotlin
sealed interface HomeState {

    data object Loading : HomeState

    data class Success(
        val profile: ProfileState,
        val feed: FeedState
    ) : HomeState
}
```

---

## Nested Example

```kotlin
sealed interface ProfileState

sealed interface FeedState
```

Composable independent rendering.

---

# 56. Sealed Class Composition

Instead of giant hierarchy...

Compose smaller state machines.

```kotlin
HomeState

↓

ProfileState

↓

PaymentState

↓

DownloadState
```

More scalable.

---

# 57. Generic Sealed Classes

```kotlin
sealed interface Resource<out T>{

    data object Loading : Resource<Nothing>

    data class Success<T>(
        val data:T
    ):Resource<T>

    data class Error(
        val message:String
    ):Resource<Nothing>
}
```

Reusable everywhere.

---

## Usage

```kotlin
Resource<User>

Resource<List<Post>>

Resource<Boolean>
```

Very common architecture.

---

# 58. Combining Multiple StateFlows

```kotlin
combine(
    profileState,
    feedState
){ profile, feed ->

    HomeState.Success(
        profile,
        feed
    )
}
```

Compose renders combined sealed state.

---

# 59. State Machine Pattern ⭐⭐⭐⭐⭐

## 🎭 Story — Amazon Order Lifecycle

<svg viewBox="0 0 720 160" xmlns="http://www.w3.org/2000/svg">
  {#each [
      {x:20,label:"Placed"},
      {x:150,label:"Packed"},
      {x:280,label:"Shipped"},
      {x:410,label:"Out For Delivery"},
      {x:570,label:"Delivered"}
    ] as s, i}
    <rect x={s.x} y="50" width="120" height="50" rx="12"
          fill="none" stroke="currentColor"/>
    <text x={s.x+60} y="80" text-anchor="middle" font-size="12" fill="currentColor">{s.label}</text>
    {#if i < 4}
      <path d={`M${s.x+120} 75 L${s.x+130} 75`} stroke="currentColor" stroke-width="2"/>
    {/if}
  {/each}
</svg>

Every transition is explicit.

```kotlin
sealed interface OrderState {

    object Placed : OrderState

    object Packed : OrderState

    object Shipped : OrderState

    object OutForDelivery : OrderState

    data class Delivered(
        val date:String
    ) : OrderState
}
```

This is called a **Finite State Machine (FSM).**

Huge interview topic.

---

# 60. Reducer Pattern (Advanced MVI)

Reducers produce next state.

```kotlin
fun reduce(
    current: HomeState,
    event: HomeEvent
): HomeState
```

Properties:

- Pure function.
- No side effects.
- Deterministic.
- Easy to unit test.

Google & CRED architecture.

---

# 61. Side Effect Isolation

Business logic emits:

```kotlin
_state.value = Success(users)

_effect.emit(
    Navigate("profile")
)
```

State and effect separated.

Avoids duplicate navigation.

---

# 62. Sealed Classes with Coroutines Flow

Repository.

```kotlin
fun users(): Flow<Resource<List<User>>> = flow {

    emit(Resource.Loading)

    emit(Resource.Success(api.users()))
}
```

Compose collects.

Very common architecture.

---

# 63. Retry Architecture Pattern

```kotlin
sealed interface NetworkEvent{

    object Retry : NetworkEvent

    object Refresh : NetworkEvent
}
```

Reducer handles retry.

State changes.

Effect optional.

---

# 64. Offline-First Architecture

```kotlin
sealed interface SyncState{

    object Syncing : SyncState

    object Synced : SyncState

    data class Offline(
        val pending:Int
    ) : SyncState

    data class Failed(
        val reason:String
    ) : SyncState
}
```

Great senior interview discussion.

---

# 65. Testing Sealed Classes

## Unit Testing Equality

```kotlin
assertEquals(
    Success(users),
    Success(users)
)
```

Works automatically.

---

## Testing Reducers

```kotlin
@Test
fun search_event_updates_state(){

    val state =
        reduce(
            Loading,
            Search("Android")
        )

    assertTrue(state is Success)
}
```

Reducers are extremely testable.

---

# 66. Testing StateFlow with Turbine

```kotlin
viewModel.state.test{

    assertEquals(
        Loading,
        awaitItem()
    )

    val success = awaitItem()

    assertTrue(success is Success)
}
```

Very common interview topic.

---

# 67. Snapshot Testing Compose State

Each sealed state corresponds to one preview.

```kotlin
@Preview
fun SuccessPreview(){...}

@Preview
fun LoadingPreview(){...}

@Preview
fun ErrorPreview(){...}
```

Excellent design workflow.

---

# 68. Sealed Classes in Navigation Graphs

Instead of strings.

```kotlin
sealed interface Screen{

    object Home : Screen

    object Settings : Screen

    data class Profile(
        val id:Int
    ) : Screen
}
```

Type-safe navigation model.

---

# 69. Serialization Internals

Sealed hierarchies serialize polymorphically.

Useful for:

- Offline cache.
- SavedStateHandle.
- KMP shared models.
- IPC models.

---

# 70. Production Best Practices ⭐⭐⭐⭐⭐

## ✅ Do

- Keep states immutable.
- Use `object` for singleton states.
- Use `data class` for payload states.
- Use `StateFlow` for UI state.
- Use `SharedFlow` for effects.
- Use exhaustive `when`.
- Separate Event, State, and Effect.

---

## ❌ Don't

- Put navigation inside state.
- Put snackbar inside state.
- Use boolean flags for screen states.
- Mutate lists inside UI state.
- Reuse API DTO as UI state.

---

# 71. Production Pitfalls

## Pitfall 1 — Boolean Explosion

```kotlin
loading
error
empty
refreshing
offline
```

32 combinations.

---

## Pitfall 2 — Mutable List

```kotlin
users.add(...)
```

Unexpected recomposition.

---

## Pitfall 3 — Navigation Boolean

```kotlin
navigate = true
```

Repeats after rotation.

---

## Pitfall 4 — One Giant Sealed Class

300-line hierarchy.

Split by feature.

---

## Pitfall 5 — Using Enum for UI State

Enums cannot carry payload.

Use sealed classes.

---

# 72. Real Production Folder Structure

```text
feature-home/

ui/

state/
    HomeUiState.kt
    HomeUiEvent.kt
    HomeUiEffect.kt

viewmodel/
    HomeViewModel.kt

reducer/
    HomeReducer.kt

repository/
    HomeRepository.kt
```

Scalable architecture.

---

# 73. Google / Uber Interview Questions (50+)

## Beginner

1. What is a sealed class?
2. Sealed class vs enum.
3. Sealed class vs abstract class.
4. Sealed interface.
5. Why exhaustive `when`?

## Intermediate

6. API Result wrapper.
7. Why `Nothing`?
8. Object vs data class inside sealed.
9. SharedFlow vs StateFlow.
10. Navigation events.

## Compose

11. UiState vs UiEvent.
12. UiState vs UiEffect.
13. Snackbar architecture.
14. Dialog architecture.
15. Bottom sheet state.

## Architecture

16. MVI with sealed classes.
17. Reducer pattern.
18. Authentication state machine.
19. Payment state machine.
20. Offline sync state machine.

## Coroutines

21. Flow<Resource<T>>.
22. Retry architecture.
23. Error propagation.
24. Loading lifecycle.
25. Combining StateFlows.

## KMP

26. Shared sealed hierarchies.
27. Serialization.
28. Platform-specific UI.

## JVM

29. Decompiled sealed classes.
30. Object singleton generation.
31. Exhaustive compiler checks.
32. Memory model.

## Performance

33. Immutable state.
34. Recomposition optimization.
35. Stable vs unstable state.
36. Object allocation.

## Testing

37. Testing reducers.
38. Turbine testing.
39. Preview testing.
40. Equality testing.

## Staff Engineer

41. State machine design.
42. Payment architecture.
43. Navigation architecture.
44. Offline-first architecture.
45. Multi-module sealed hierarchy.
46. SDK design.
47. Compose scalability.
48. State ownership.
49. Effect lifecycle.
50. When NOT to use sealed classes.

---

# 74. 2-Minute Interview Answer ⭐⭐⭐⭐⭐

> Sealed classes model a closed hierarchy of states known at compile time. They enable exhaustive `when` expressions, eliminate impossible UI states, and work naturally with immutable architecture. In modern Android, sealed classes are used for UI state, API results, navigation events, authentication flows, payment flows, and MVI reducers. `StateFlow` typically exposes sealed UI states, while `SharedFlow` delivers one-time effects like navigation and snackbars.

---

# 75. Cheat Sheet

## Decision Matrix

| Requirement | Use |
|-------------|-----|
| Fixed options | Enum |
| UI state with data | Sealed Class |
| Multiple inheritance | Sealed Interface |
| Shared implementation | Abstract Class |
| Singleton | Object |

---

## Android Patterns

| Pattern | Sealed Type |
|---------|-------------|
| UiState | Sealed Interface |
| UiEvent | Sealed Interface |
| UiEffect | Sealed Interface |
| Resource Wrapper | Generic Sealed Interface |
| Navigation | Sealed Interface |
| Payment Flow | Sealed Interface |
| Download Flow | Sealed Interface |

---

## StateFlow vs SharedFlow

| StateFlow | SharedFlow |
|-----------|------------|
| Persistent state | One-time events |
| Replays latest value | No replay by default |
| Screen rendering | Navigation, Snackbar, Toast |

---

## Golden Architecture

```text
Compose UI

↓ (UiEvent)

ViewModel

↓ (Reducer)

StateFlow<UiState>

↓

Compose UI

↓

SharedFlow<UiEffect>

↓

Navigation / Snackbar / Toast
```

---

# 📝 Complete Chapter Revision

### You learned

- Sealed Classes fundamentals.
- Sealed Interfaces.
- Exhaustive `when`.
- API Result wrappers.
- MVI architecture.
- UiState / UiEvent / UiEffect.
- Navigation architecture.
- Snackbar/Dialog patterns.
- Payment & Authentication state machines.
- Compose recomposition.
- KMP shared hierarchies.
- JVM internals.
- Performance optimization.
- Testing with Turbine.
- Production best practices.
- 50+ interview questions.

---

# ⭐ Senior Android Interview Takeaway

If someone asks **"Design a modern Compose screen architecture"**, the expected answer is:

- **UiState** → Sealed class exposed via `StateFlow`.
- **UiEvent** → Sealed class sent from Compose to ViewModel.
- **UiEffect** → Sealed class emitted through `SharedFlow` for navigation/snackbars.
- **Reducer** → Pure function that converts Event + Current State → New State.
- **Repository** → Returns `Resource<T>` sealed wrapper instead of nullable data or exceptions.