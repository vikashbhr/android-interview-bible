# 🎭 Object Expressions — Android Interview Bible (2026 Edition)

> Complete Kotlin Object Expressions guide for Android Developers (8+ Years Experience). Learn anonymous objects, callbacks, listeners, RecyclerView, Retrofit, Compose, Coroutines, JVM internals, performance, testing, and interview questions.

**Module:** Kotlin OOP

**File:** `13-Object-Expressions.md`

**Difficulty:** Intermediate → Senior Android Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies:** Google • Uber • Amazon • Microsoft • PhonePe • CRED • Flipkart • Meesho

---

# 📚 Chapter Roadmap

## Part 1 — Fundamentals (This File)

1. What is Object Expression?
2. Why Kotlin Introduced Object Expressions?
3. Anonymous Objects
4. Object Expression vs Object Declaration
5. Object Expression vs Anonymous Function
6. Implementing Interfaces
7. Extending Classes
8. Implementing Multiple Interfaces
9. Android Listeners
10. RecyclerView Callbacks
11. JVM Internals
12. Memory Allocation
13. Best Practices
14. Interview Questions
15. Cheat Sheet

---

# 1. What is an Object Expression?

## 🎭 Real World Story — Temporary Employee at Swiggy

Imagine Swiggy launches a **Big Festival Sale**.

For just one weekend, Swiggy hires temporary delivery partners.

Those delivery partners:

- Work only for one task.
- Don't become permanent employees.
- Disappear after the work finishes.

Kotlin Object Expressions work exactly the same way.

Instead of creating a reusable class, Kotlin creates an object instantly for temporary work.

## Definition

An **Object Expression** creates an anonymous object immediately.

```kotlin
val logger = object {

    fun log(message: String) {
        println(message)
    }
}
```

Characteristics:

- No class name.
- Created immediately.
- Exists only within its scope.
- Can contain properties and functions.

## Usage

```kotlin
logger.log("Welcome to Android Interview Bible")
```

Output:

```text
Welcome to Android Interview Bible
```

---

# 2. Why Kotlin Introduced Object Expressions?

Without Object Expressions, we would create a class for every small callback.

## Traditional Kotlin

```kotlin
class Logger {

    fun log(message: String) {
        println(message)
    }
}

val logger = Logger()
```

This is unnecessary when the object is used only once.

## Kotlin Object Expression

```kotlin
val logger = object {

    fun log(message: String) {
        println(message)
    }
}
```

Less boilerplate.

Cleaner code.

Perfect for callbacks and listeners.

---

# 3. Anonymous Objects

Anonymous objects are unnamed objects created using Object Expressions.

## Example

```kotlin
val currentUser = object {

    val id = 101

    val name = "Vikash"

    val isPremium = true
}
```

Usage

```kotlin
println(currentUser.id)
println(currentUser.name)
println(currentUser.isPremium)
```

Output

```text
101
Vikash
true
```

---

## Anonymous Object with Methods

```kotlin
val calculator = object {

    fun square(value: Int): Int {
        return value * value
    }

    fun cube(value: Int): Int {
        return value * value * value
    }
}
```

Usage

```kotlin
println(calculator.square(5))
println(calculator.cube(3))
```

Output

```text
25
27
```

---

## Anonymous Object with Mutable State

```kotlin
val counter = object {

    var value = 0

    fun increment() {
        value++
    }
}

counter.increment()
counter.increment()

println(counter.value)
```

Output

```text
2
```

Useful for temporary state.

---

# 4. Object Expression vs Object Declaration ⭐⭐⭐⭐⭐

## Object Declaration

```kotlin
object Logger {

    fun log(message: String) {
        println(message)
    }
}
```

Singleton.

One JVM instance.

## Object Expression

```kotlin
val logger = object {

    fun log(message: String) {
        println(message)
    }
}
```

Creates a new anonymous object.

## Comparison Table

| Object Declaration | Object Expression |
|--------------------|------------------|
| Singleton | New object each execution |
| Global scope | Local scope |
| Named object | Anonymous object |
| Lazy initialized | Immediately initialized |
| One instance in JVM | Unlimited instances |

---

## Demonstration

```kotlin
repeat(3) {

    val listener = object {}

    println(listener.hashCode())
}
```

Output

```text
18234
29877
39118
```

Different objects.

---

## Singleton Demonstration

```kotlin
repeat(3) {
    println(Logger.hashCode())
}
```

Output

```text
9900
9900
9900
```

Same object every time.

---

# 5. Object Expression vs Anonymous Function ⭐⭐⭐⭐⭐

## Anonymous Function

```kotlin
val multiply = fun(a: Int, b: Int): Int {
    return a * b
}
```

Usage

```kotlin
println(multiply(4,5))
```

Output

```text
20
```

---

## Object Expression

```kotlin
val calculator = object {

    fun multiply(a: Int, b: Int): Int {
        return a * b
    }
}
```

Usage

```kotlin
println(calculator.multiply(4,5))
```

---

## Difference Table

| Anonymous Function | Object Expression |
|--------------------|------------------|
| Represents a function | Represents an object |
| Cannot store properties | Can store properties |
| Function Type | Anonymous Class |
| Used for callbacks | Used for listeners and callbacks |

---

# 6. Implementing Interfaces ⭐⭐⭐⭐⭐

One of the most common Android interview questions.

## Interface

```kotlin
interface PaymentListener {

    fun onSuccess()

    fun onFailure()
}
```

---

## Object Expression Implementation

```kotlin
val paymentListener = object : PaymentListener {

    override fun onSuccess() {
        println("Payment Successful")
    }

    override fun onFailure() {
        println("Payment Failed")
    }
}
```

Usage

```kotlin
paymentListener.onSuccess()
```

Output

```text
Payment Successful
```

---

## Why Object Expression?

Implementation is needed only once.

No reusable class required.

---

# 7. Extending Classes ⭐⭐⭐⭐⭐

Object Expressions can extend classes.

## Example

```kotlin
open class Animal {

    open fun sound() {
        println("Animal Sound")
    }
}
```

Anonymous subclass.

```kotlin
val dog = object : Animal() {

    override fun sound() {
        println("Bark Bark")
    }
}
```

Usage

```kotlin
dog.sound()
```

Output

```text
Bark Bark
```

---

## Multiple Overrides

```kotlin
open class Shape {

    open fun draw() {}

    open fun area(): Int = 0
}

val rectangle = object : Shape() {

    override fun draw() {
        println("Drawing Rectangle")
    }

    override fun area(): Int {
        return 100
    }
}
```

---

# 8. Implementing Multiple Interfaces ⭐⭐⭐⭐

Object Expressions support multiple interface implementation.

## Example

```kotlin
interface ClickListener {

    fun onClick()
}

interface SwipeListener {

    fun onSwipe()
}

val callback = object : ClickListener, SwipeListener {

    override fun onClick() {
        println("Item Clicked")
    }

    override fun onSwipe() {
        println("Item Swiped")
    }
}
```

Usage

```kotlin
callback.onClick()
callback.onSwipe()
```

---

## Why Useful?

Android gesture handling often requires multiple callbacks.

Examples:

- Swipe
- Click
- Long Click
- Drag

---

# 9. Android Listeners ⭐⭐⭐⭐⭐

The most common Object Expression usage in Android Views.

## Button Click Listener

```kotlin
button.setOnClickListener(

    object : View.OnClickListener {

        override fun onClick(v: View?) {

            Toast.makeText(
                context,
                "Clicked",
                Toast.LENGTH_SHORT
            ).show()
        }
    }
)
```

---

## Why Anonymous Object?

Listener is needed only for this button.

Creating a separate class would add unnecessary boilerplate.

---

## Long Click Listener

```kotlin
button.setOnLongClickListener(

    object : View.OnLongClickListener {

        override fun onLongClick(v: View?): Boolean {

            println("Long Click")

            return true
        }
    }
)
```

---

# 10. RecyclerView Callback ⭐⭐⭐⭐⭐

## Story — Amazon Product List

Every product card needs click handling.

Instead of creating multiple listener classes...

Use Object Expressions.

---

## Interface

```kotlin
interface UserClickListener {

    fun onClick(user: User)
}
```

---

## Adapter Usage

```kotlin
val adapter = UserAdapter(

    object : UserClickListener {

        override fun onClick(user: User) {

            navigateToProfile(user.id)
        }
    }
)
```

Clean.

Scoped.

Readable.

---

## ItemTouchHelper Callback

Very common Android interview question.

```kotlin
ItemTouchHelper(

    object : ItemTouchHelper.Callback() {

        override fun onMove(
            recyclerView: RecyclerView,
            viewHolder: RecyclerView.ViewHolder,
            target: RecyclerView.ViewHolder
        ): Boolean {
            return true
        }

        override fun onSwiped(
            viewHolder: RecyclerView.ViewHolder,
            direction: Int
        ) {

            println("Item Deleted")
        }
    }
)
```

Object Expression extends an abstract class.

---

# 11. JVM Internals ⭐⭐⭐⭐⭐

Kotlin Object Expression

```kotlin
val listener = object : ClickListener {

    override fun onClick() {
        println("Clicked")
    }
}
```

Compiler generates something similar to Java anonymous inner classes.

```java
ClickListener listener = new ClickListener() {

    @Override
    public void onClick() {
        System.out.println("Clicked");
    }
};
```

Generated class name typically looks like:

```text
MainActivity$listener$1.class
```

Compiler creates a synthetic anonymous class.

---

## What Gets Generated?

- Anonymous class.
- Constructor.
- Overridden methods.
- Captured variables become fields.

---

# 12. Memory Allocation ⭐⭐⭐⭐

Every Object Expression allocates memory on the heap.

```kotlin
repeat(5) {

    val callback = object {}
}
```

Result:

- 5 different heap objects.
- 5 different references.

---

## Heap Visualization

```text
Heap Memory

Object #1
Object #2
Object #3
Object #4
Object #5
```

Unlike singleton objects.

---

## Singleton Comparison

```kotlin
object Logger
```

Only one heap allocation.

---

# 13. Best Practices ⭐⭐⭐⭐⭐

## ✅ Use Object Expressions For

- Button listeners.
- RecyclerView callbacks.
- ItemTouchHelper callbacks.
- BroadcastReceiver implementations.
- Retrofit interceptors.
- Gesture listeners.
- Temporary fake implementations in tests.

---

## ❌ Avoid Object Expressions For

- Large reusable classes.
- Repository implementations.
- Business logic.
- Global utilities.

Use normal classes or singleton objects instead.

---

## Android Recommendation

Prefer lambdas when an interface has only one abstract method.

Use Object Expressions for:

- Multiple methods.
- Abstract classes.
- Stateful temporary implementations.

---

# 14. Common Production Pitfalls

## Pitfall 1 — Creating Objects Inside Loops

```kotlin
repeat(1000) {

    val obj = object {}
}
```

Creates 1000 objects.

Avoid unnecessary allocations.

---

## Pitfall 2 — Object Expressions Inside Compose Without remember

```kotlin
@Composable
fun Screen() {

    val callback = object : Listener {}
}
```

New object every recomposition.

Will fix later using `remember`.

---

## Pitfall 3 — Capturing Large Objects

Anonymous objects capture surrounding variables.

Avoid capturing Activity or Fragment unnecessarily.

---

# 15. Interview Questions (Part 1)

## Basic

1. What is an Object Expression?
2. Difference between Object Expression and Object Declaration?
3. What is an Anonymous Object?
4. Can Object Expressions have properties?

## Intermediate

5. Object Expression vs Anonymous Function.
6. Implement interface using Object Expression.
7. Extend abstract class using Object Expression.
8. Implement multiple interfaces.

## Android

9. Why RecyclerView callbacks use Object Expressions?
10. Why ItemTouchHelper.Callback uses Object Expressions?
11. Why listeners don't require separate classes?

## JVM

12. What bytecode does Kotlin generate?
13. Does Object Expression create a singleton?
14. Where is memory allocated?

---

# 📋 Cheat Sheet

## Object Expression Syntax

```kotlin
val obj = object {

}
```

---

## Interface Implementation

```kotlin
val obj = object : Interface {

    override fun method() {}
}
```

---

## Class Extension

```kotlin
val obj = object : BaseClass() {

    override fun method() {}
}
```

---

## Multiple Interfaces

```kotlin
val obj = object : A, B {

}
```

---

## Comparison Table

| Feature | Object Expression | Object Declaration |
|--------|-------------------|--------------------|
| Named | ❌ | ✅ |
| Singleton | ❌ | ✅ |
| New Instance Every Time | ✅ | ❌ |
| Local Scope | ✅ | ❌ |
| Extends Class | ✅ | ✅ |
| Implements Interface | ✅ | ✅ |

---

# 📝 Revision Summary

- Object Expressions create anonymous objects immediately.
- Every execution creates a new heap object.
- They can implement interfaces and extend classes.
- Android uses Object Expressions extensively for callbacks, listeners, RecyclerView, and ItemTouchHelper.
- Kotlin compiles Object Expressions into anonymous inner classes on the JVM.
- Prefer Object Expressions when behavior is temporary and scoped to one location.

---

# Part 2 — Android Framework APIs (TextWatcher, BroadcastReceiver, Retrofit, Coroutines & Compose)

> Learn how Object Expressions are used throughout the Android framework and Jetpack Compose. This section focuses on production Android examples that are frequently asked in senior Android interviews.

---

# 📚 Table of Contents

16. TextWatcher
17. BroadcastReceiver
18. Retrofit Interceptor
19. OkHttp Authenticator
20. Animator Listener
21. CoroutineScope Object Expression
22. GestureDetector
23. ItemTouchHelper
24. NestedScrollConnection (Compose)
25. PointerInput (Compose)
26. AccessibilityDelegate
27. Android Best Practices
28. Interview Questions

---

# 16. TextWatcher ⭐⭐⭐⭐⭐

## 🎭 Story — OTP Screen

Imagine an OTP verification screen.

As the user types each digit:

- Validate input.
- Move cursor.
- Enable Verify button.

The listener needs multiple callbacks.

This is why Android uses **Object Expressions**.

## What is TextWatcher?

`TextWatcher` is an interface with three abstract methods.

```kotlin
interface TextWatcher {

    fun beforeTextChanged(...)

    fun onTextChanged(...)

    fun afterTextChanged(...)
}
```

---

## Android Example

```kotlin
editText.addTextChangedListener(

    object : TextWatcher {

        override fun beforeTextChanged(
            s: CharSequence?,
            start: Int,
            count: Int,
            after: Int
        ) {
        }

        override fun onTextChanged(
            s: CharSequence?,
            start: Int,
            before: Int,
            count: Int
        ) {
            verifyButton.isEnabled = s?.length == 6
        }

        override fun afterTextChanged(s: Editable?) {
        }
    }
)
```

---

## Why Object Expression?

`TextWatcher` contains **three methods**.

A lambda cannot implement multiple abstract methods.

---

## Interview Tip

> `TextWatcher` is a classic example where Object Expressions are preferred over lambdas.

---

# 17. BroadcastReceiver ⭐⭐⭐⭐⭐

## 🎭 Story — Phone Charger

Android needs to know when:

- Charger connected.
- WiFi changed.
- Battery low.
- Airplane mode enabled.

BroadcastReceiver listens for system events.

---

## Dynamic Receiver

```kotlin
val receiver = object : BroadcastReceiver() {

    override fun onReceive(
        context: Context,
        intent: Intent
    ) {
        println(intent.action)
    }
}
```

Register receiver.

```kotlin
registerReceiver(
    receiver,
    IntentFilter(Intent.ACTION_POWER_CONNECTED)
)
```

---

## Unregister Receiver

```kotlin
override fun onDestroy() {
    unregisterReceiver(receiver)
}
```

Important lifecycle practice.

---

## Why Object Expression?

Receiver exists only inside Activity.

No reusable class needed.

---

## Memory Tip

Always unregister dynamic receivers.

Otherwise Activity leaks.

---

# 18. Retrofit Interceptor ⭐⭐⭐⭐⭐

## 🎭 Story — Security Guard at Office

Every API request enters the office.

Security guard:

- Adds token.
- Adds language.
- Adds app version.

Interceptor behaves like that guard.

---

## Production Example

```kotlin
val client = OkHttpClient.Builder()

    .addInterceptor(

        object : Interceptor {

            override fun intercept(
                chain: Interceptor.Chain
            ): Response {

                val request = chain.request()
                    .newBuilder()
                    .addHeader("Authorization", token)
                    .build()

                return chain.proceed(request)
            }
        }
    )

    .build()
```

---

## Why Object Expression?

Interceptor implementation is needed once during client creation.

---

## Real Headers

```text
Authorization: Bearer xxxx
Accept-Language: en
App-Version: 2.1.0
Device-Type: Android
```

---

# 19. OkHttp Authenticator ⭐⭐⭐⭐

Authenticator refreshes expired tokens.

```kotlin
OkHttpClient.Builder()

    .authenticator(

        object : Authenticator {

            override fun authenticate(
                route: Route?,
                response: Response
            ): Request? {

                val newToken = refreshToken()

                return response.request
                    .newBuilder()
                    .header("Authorization", newToken)
                    .build()
            }
        }
    )
```

---

## Difference

| Interceptor | Authenticator |
|-------------|---------------|
| Before request | After 401 response |
| Adds headers | Refreshes token |
| Runs for every request | Runs only on authentication failure |

---

# 20. Animator Listener ⭐⭐⭐⭐

Android animations expose many callbacks.

```kotlin
animator.addListener(

    object : AnimatorListenerAdapter() {

        override fun onAnimationStart(animation: Animator) {
            println("Started")
        }

        override fun onAnimationEnd(animation: Animator) {
            println("Finished")
        }
    }
)
```

---

## Why AnimatorListenerAdapter?

Instead of implementing all listener methods manually.

Adapter provides default implementations.

Object Expression overrides only required methods.

---

# 21. CoroutineScope Object Expression ⭐⭐⭐⭐

Sometimes custom coroutine scope is required.

```kotlin
val customScope = object : CoroutineScope {

    override val coroutineContext =
        Dispatchers.IO + SupervisorJob()
}
```

Usage

```kotlin
customScope.launch {
    println("Background Task")
}
```

---

## Production Usage

Useful for:

- SDKs.
- Background workers.
- Library code.

---

## Recommendation

For Android screens prefer:

- `viewModelScope`
- `lifecycleScope`

---

# 22. GestureDetector ⭐⭐⭐⭐⭐

GestureDetector handles:

- Single Tap.
- Double Tap.
- Scroll.
- Fling.
- Long Press.

---

## Example

```kotlin
val detector = GestureDetector(

    context,

    object : GestureDetector.SimpleOnGestureListener() {

        override fun onDoubleTap(e: MotionEvent): Boolean {
            println("Double Tap")
            return true
        }

        override fun onLongPress(e: MotionEvent) {
            println("Long Press")
        }
    }
)
```

---

## Why Object Expression?

Only required gesture methods are overridden.

---

# 23. ItemTouchHelper.Callback ⭐⭐⭐⭐⭐

RecyclerView swipe.

```kotlin
val callback = object : ItemTouchHelper.Callback() {

    override fun onMove(
        recyclerView: RecyclerView,
        viewHolder: RecyclerView.ViewHolder,
        target: RecyclerView.ViewHolder
    ): Boolean {
        return true
    }

    override fun onSwiped(
        viewHolder: RecyclerView.ViewHolder,
        direction: Int
    ) {
        deleteItem(viewHolder.bindingAdapterPosition)
    }
}
```

Attach.

```kotlin
ItemTouchHelper(callback).attachToRecyclerView(recyclerView)
```

---

## Interview Question

Why Object Expression instead of lambda?

Because Callback is an abstract class with multiple methods.

---

# 24. NestedScrollConnection (Jetpack Compose) ⭐⭐⭐⭐⭐

## 🎭 Story — Instagram Feed

User scrolls feed.

Toolbar collapses.

Bottom bar hides.

Compose intercepts scrolling.

---

## Example

```kotlin
val connection = remember {

    object : NestedScrollConnection {

        override fun onPreScroll(
            available: Offset,
            source: NestedScrollSource
        ): Offset {

            println(available.y)

            return Offset.Zero
        }
    }
}
```

Usage

```kotlin
Modifier.nestedScroll(connection)
```

---

## Why remember?

Without remember a new object is created on every recomposition.

Huge Compose interview topic.

---

# 25. PointerInput (Jetpack Compose)

Detect gestures.

```kotlin
Modifier.pointerInput(Unit) {

    detectTapGestures(

        onDoubleTap = {

        },

        onLongPress = {

        }
    )
}
```

Internally Compose uses object expressions for gesture handling.

---

## Simplified Internal Idea

```kotlin
object : PointerInputModifierNode {

    override fun onPointerEvent(...) {
    }
}
```

Compose uses anonymous objects heavily.

---

# 26. AccessibilityDelegate ⭐⭐⭐⭐

Custom accessibility behavior.

```kotlin
ViewCompat.setAccessibilityDelegate(

    button,

    object : AccessibilityDelegateCompat() {

        override fun onInitializeAccessibilityNodeInfo(
            host: View,
            info: AccessibilityNodeInfoCompat
        ) {

            super.onInitializeAccessibilityNodeInfo(host, info)

            info.text = "Play Audio"
        }
    }
)
```

---

## Why Important?

Accessibility questions appear in senior Android interviews.

---

# 27. Android Best Practices ⭐⭐⭐⭐⭐

## Use Object Expressions For

- TextWatcher
- BroadcastReceiver
- ItemTouchHelper.Callback
- GestureDetector
- Retrofit Interceptor
- Authenticator
- AnimatorListener
- AccessibilityDelegate
- NestedScrollConnection
- Temporary coroutine scopes
- Fake implementations in tests

---

## Prefer Lambdas For

- Button clicks.
- Compose `onClick`.
- Flow operators.
- Coroutine callbacks with a single function.
- Functional APIs.

---

## Compose Recommendation

Always remember stateful object expressions.

```kotlin
val connection = remember {
    object : NestedScrollConnection {}
}
```

---

# 28. Common Production Pitfalls

## Pitfall 1 — Creating Object Every Recomposition

```kotlin
@Composable
fun Screen() {

    val connection = object : NestedScrollConnection {}
}
```

Bad.

Creates new object every recomposition.

---

## Pitfall 2 — Forgetting unregisterReceiver()

Leaks Activity.

---

## Pitfall 3 — Holding Activity in BroadcastReceiver

Avoid storing Activity references.

---

## Pitfall 4 — Long-Lived Interceptor Holding Context

Use Application Context.

---

# 29. Senior Android Interview Questions

## Android Framework

1. Why TextWatcher uses Object Expression?
2. Why BroadcastReceiver often uses anonymous objects?
3. Difference between Interceptor and Authenticator?
4. Why AnimatorListenerAdapter exists?
5. Why ItemTouchHelper.Callback cannot use lambda?

## Compose

6. Why `remember { object{} }`?
7. What is NestedScrollConnection?
8. Why Compose creates scope objects?

## Architecture

9. Where should Interceptors be created?
10. Can BroadcastReceiver leak Activity?

---

# 📋 Quick Cheat Sheet

## Android APIs That Use Object Expressions

| API | Why Object Expression? |
|------|------------------------|
| TextWatcher | Multiple callback methods |
| BroadcastReceiver | Temporary receiver implementation |
| ItemTouchHelper.Callback | Abstract class with many methods |
| GestureDetector | Override selected gesture callbacks |
| AnimatorListenerAdapter | Override only required animation methods |
| Interceptor | One-time request modification |
| Authenticator | Token refresh callback |
| NestedScrollConnection | Scroll callback object |
| AccessibilityDelegateCompat | Accessibility customization |

---

# 📝 Revision Summary

- Android Framework uses Object Expressions extensively for listeners and callbacks.
- Object Expressions are required whenever multiple methods or abstract classes need implementation.
- Compose relies on Object Expressions for scroll, gesture, and modifier APIs.
- `remember { object{} }` is essential in Compose to avoid repeated allocations.
- BroadcastReceivers and callbacks must follow lifecycle rules to prevent memory leaks.

---

# Part 3 — Lambdas vs Object Expressions, SAM Conversion, Performance & Interview Mastery

> Learn the JVM internals behind Object Expressions, understand SAM conversion, performance optimization, memory leaks, Compose best practices, testing strategies, and master every Android interview question related to Object Expressions.

---

# 📚 Table of Contents

30. Object Expressions vs Lambdas
31. SAM Conversion
32. Closures & Variable Capturing
33. Object Expressions in Compose
34. Memory Leaks
35. Performance & Allocation
36. JVM Bytecode Deep Dive
37. Testing with Object Expressions
38. Best Practices
39. Production Pitfalls
40. Senior Interview Questions
41. Ultimate Cheat Sheet

---

# 30. Object Expressions vs Lambdas ⭐⭐⭐⭐⭐

## 🎭 Story — Hiring an Employee vs Calling a Freelancer

Imagine you're building a startup.

### Option 1 — Freelancer

You hire someone for **one single task**.

They finish the work.

That's a **Lambda**.

### Option 2 — Temporary Employee

You hire someone for multiple responsibilities.

They have identity, state, and methods.

That's an **Object Expression**.

---

## Lambda Example

```kotlin
button.setOnClickListener {
    println("Clicked")
}
```

---

## Object Expression Example

```kotlin
button.setOnClickListener(

    object : View.OnClickListener {

        override fun onClick(v: View?) {
            println("Clicked")
        }
    }
)
```

---

## Comparison Table

| Lambda | Object Expression |
|--------|-------------------|
| Function only | Object with state |
| Single method | Multiple methods |
| Supports SAM | Supports abstract classes & interfaces |
| Less allocation (often) | New object allocation |
| Cleaner syntax | More flexible |

---

## Which One Should You Use?

### Use Lambda

- Button clicks.
- Compose callbacks.
- Flow operators.
- Coroutine callbacks.

### Use Object Expression

- TextWatcher.
- BroadcastReceiver.
- ItemTouchHelper.
- GestureDetector.
- Retrofit Interceptor.
- Abstract classes.

---

# 31. SAM Conversion ⭐⭐⭐⭐⭐

## What is SAM?

**SAM = Single Abstract Method**

An interface with exactly **one abstract method**.

---

## Example

```kotlin
fun interface ClickListener {

    fun onClick()
}
```

---

## Object Expression

```kotlin
val listener = object : ClickListener {

    override fun onClick() {
        println("Clicked")
    }
}
```

---

## Lambda Equivalent

```kotlin
val listener = ClickListener {
    println("Clicked")
}
```

Cleaner.

---

## JVM Behind the Scenes

Compiler converts lambda into an implementation of `ClickListener`.

Conceptually:

```java
new ClickListener() {
    @Override
    public void onClick() {
        System.out.println("Clicked");
    }
};
```

---

## Android SAM Examples

### OnClickListener

```kotlin
button.setOnClickListener {
    println("Click")
}
```

### Runnable

```kotlin
Thread {
    println("Running")
}.start()
```

### Comparator

```kotlin
list.sortedWith { a, b ->
    a.age - b.age
}
```

---

## Interfaces That Cannot Use SAM

```kotlin
interface DownloadListener {

    fun start()

    fun progress()

    fun finish()
}
```

Must use Object Expression.

---

# 32. Closures & Variable Capturing ⭐⭐⭐⭐⭐

## Story — Delivery Boy Carries Address

The delivery partner carries customer address with him.

Similarly Object Expressions carry surrounding variables.

---

## Capturing Variables

```kotlin
var clicks = 0

val listener = object : ClickListener {

    override fun onClick() {
        clicks++
    }
}
```

Usage

```kotlin
listener.onClick()

println(clicks)
```

Output

```text
1
```

---

## Capturing Objects

```kotlin
val user = User("Vikash")

val callback = object {

    fun printUser() {
        println(user.name)
    }
}
```

Captured object reference becomes a field.

---

## JVM Visualization

```text
Anonymous Object

+---------------------+
| user reference ---->|
| clicks reference -->|
+---------------------+
```

---

## Why Important?

Captured Activity or Fragment can cause memory leaks.

---

# 33. Object Expressions in Jetpack Compose ⭐⭐⭐⭐⭐

Compose uses Object Expressions internally.

---

## Wrong Example

```kotlin
@Composable
fun HomeScreen() {

    val connection = object : NestedScrollConnection {}
}
```

Every recomposition creates a new object.

---

## Correct Example

```kotlin
@Composable
fun HomeScreen() {

    val connection = remember {

        object : NestedScrollConnection {}
    }
}
```

Object created only once.

---

## NestedScrollConnection

```kotlin
val connection = remember {

    object : NestedScrollConnection {

        override fun onPreScroll(
            available: Offset,
            source: NestedScrollSource
        ): Offset {

            return Offset.Zero
        }
    }
}
```

---

## Compose Gesture Example

```kotlin
Modifier.pointerInput(Unit) {

    detectTapGestures(

        onTap = { },

        onDoubleTap = { },

        onLongPress = { }
    )
}
```

Internally uses anonymous objects.

---

## Compose Recommendation

Always wrap stateful object expressions with `remember`.

---

# 34. Memory Leaks ⭐⭐⭐⭐⭐

## Story — Activity Destroyed but Callback Lives

Activity finishes.

Callback still running.

Activity cannot be garbage collected.

---

## Leak Example

```kotlin
class MainActivity : AppCompatActivity() {

    val receiver = object : BroadcastReceiver() {

        override fun onReceive(
            context: Context,
            intent: Intent
        ) {
            println(title)
        }
    }
}
```

Receiver captures Activity.

---

## Fix

Register/unregister properly.

```kotlin
override fun onDestroy() {
    unregisterReceiver(receiver)
}
```

---

## Leak Example — Handler

```kotlin
val handler = Handler()

handler.postDelayed({

}, 10000)
```

Delayed lambda captures Activity.

---

## Better

Use lifecycle-aware APIs.

```kotlin
lifecycleScope.launch {

}
```

---

## Leak Example — Object Expression in Singleton

```kotlin
object Analytics {

    lateinit var callback: Callback
}
```

If callback references Activity → Leak.

---

# 35. Performance & Allocation ⭐⭐⭐⭐⭐

## Heap Allocation

```kotlin
repeat(1000) {

    val listener = object {}
}
```

Creates 1000 objects.

---

## Lambda Allocation

Stateless lambda.

```kotlin
val click = { println("Click") }
```

Compiler often reuses singleton instance.

---

## Stateful Lambda

```kotlin
val user = User("Vikash")

val click = {
    println(user.name)
}
```

Captures variable.

Requires allocation.

---

## Allocation Comparison

| Feature | Allocation |
|--------|------------|
| Stateless Lambda | Usually singleton |
| Stateful Lambda | Allocation |
| Object Expression | Allocation every creation |
| Object Declaration | Single allocation |

---

## Compose Performance

Bad

```kotlin
LazyColumn {

    items(users) {

        val callback = object : Listener {}
    }
}
```

Creates callback for every item.

---

## Better

```kotlin
val callback = remember {

    object : Listener {}
}
```

Reuse callback.

---

# 36. JVM Bytecode Deep Dive ⭐⭐⭐⭐⭐

## Kotlin Code

```kotlin
val listener = object : ClickListener {

    override fun onClick() {}
}
```

Generated Java

```java
ClickListener listener =
    new ClickListener() {

        @Override
        public void onClick() {}
    };
```

---

## Generated Class Name

```text
MainActivity$listener$1
```

Anonymous synthetic class.

---

## Captured Variables Become Fields

```kotlin
val name = "Vikash"

val obj = object {

    fun printName() {
        println(name)
    }
}
```

Decompiler

```java
final String name;

public void printName() {
    System.out.println(name);
}
```

---

## Constructor Receives Captured Values

Compiler passes captured variables into constructor.

---

## Important Interview Question

**Where are captured variables stored?**

Answer:

> As synthetic fields inside the generated anonymous class.

---

# 37. Testing with Object Expressions ⭐⭐⭐⭐

## Fake Repository

```kotlin
val fakeRepository = object : UserRepository {

    override suspend fun users(): List<User> {
        return emptyList()
    }
}
```

Very common in unit tests.

---

## Fake Analytics

```kotlin
val fakeAnalytics = object : Analytics {

    val events = mutableListOf<String>()

    override fun log(event: String) {
        events.add(event)
    }
}
```

Verify calls.

---

## Fake Callback

```kotlin
val callback = object : PaymentListener {

    var successCalled = false

    override fun onSuccess() {
        successCalled = true
    }

    override fun onFailure() {}
}
```

Useful for testing ViewModels.

---

# 38. Android Best Practices ⭐⭐⭐⭐⭐

## Use Object Expressions For

- Multiple callback methods.
- Abstract class implementation.
- Temporary fake implementations.
- Retrofit interceptors.
- Gesture detectors.
- RecyclerView callbacks.
- Compose scroll/gesture connections.

---

## Prefer Lambdas For

- Single callback methods.
- Compose button clicks.
- Flow operators.
- Collection operations.
- Runnable.
- Coroutine callbacks.

---

## Compose Rule

Always `remember` long-lived object expressions.

---

# 39. Production Pitfalls

## Pitfall 1 — Anonymous Object in Recomposition

Creates unnecessary allocations.

---

## Pitfall 2 — Capturing Activity

Leads to leaks.

---

## Pitfall 3 — Object Expression Inside Adapter Bind

```kotlin
override fun onBindViewHolder(...) {

    holder.button.setOnClickListener(

        object : View.OnClickListener {}
    )
}
```

New object for every bind.

---

## Better

```kotlin
holder.button.setOnClickListener {
    onItemClick(item)
}
```

---

## Pitfall 4 — Heavy Anonymous Object

Large business logic inside object expression.

Create dedicated class instead.

---

# 40. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Basics

1. What is an Object Expression?
2. Difference between Object Expression and Object Declaration?
3. Anonymous Object vs Lambda?

## Intermediate

4. What is SAM Conversion?
5. Why TextWatcher cannot use lambda?
6. Why ItemTouchHelper uses Object Expression?
7. Can Object Expressions extend classes?

## Compose

8. Why `remember { object{} }`?
9. NestedScrollConnection architecture?
10. PointerInput internals?

## JVM

11. How compiler generates anonymous objects?
12. Where are captured variables stored?
13. Why generated class names end with `$1`?

## Performance

14. Lambda allocation vs Object Expression allocation.
15. Stateless lambda optimization.
16. Allocation inside LazyColumn.

## Architecture

17. Testing fake implementations.
18. BroadcastReceiver lifecycle.
19. Interceptor vs Authenticator.
20. Memory leak examples.

---

# 41. Ultimate Cheat Sheet

## Object Expression Syntax

### Anonymous Object

```kotlin
val obj = object {}
```

### Interface Implementation

```kotlin
val obj = object : ClickListener {

    override fun onClick() {}
}
```

### Abstract Class Implementation

```kotlin
val obj = object : AnimatorListenerAdapter() {}
```

### Multiple Interfaces

```kotlin
val obj = object : A, B {}
```

---

## Android Usage Matrix

| Android API | Recommendation |
|-------------|---------------|
| OnClickListener | Lambda |
| TextWatcher | Object Expression |
| BroadcastReceiver | Object Expression |
| ItemTouchHelper.Callback | Object Expression |
| AnimatorListenerAdapter | Object Expression |
| GestureDetector | Object Expression |
| NestedScrollConnection | `remember { object{} }` |
| Retrofit Interceptor | Object Expression |
| Authenticator | Object Expression |

---

## Lambda vs Object Expression

| Requirement | Use |
|-------------|-----|
| Single callback | Lambda |
| Multiple callbacks | Object Expression |
| Abstract class | Object Expression |
| Temporary state | Object Expression |
| Functional API | Lambda |

---

## Memory Rules

- Stateless lambda → Usually singleton.
- Capturing lambda → Allocation.
- Object Expression → New allocation every creation.
- `remember` object expressions in Compose.
- Don't capture Activity in long-lived objects.

---

# 🎯 Complete Chapter Revision

You learned:

- Object Expressions fundamentals.
- Anonymous Objects.
- Interface implementation.
- Class extension.
- Multiple interface implementation.
- Android listeners.
- RecyclerView callbacks.
- TextWatcher.
- BroadcastReceiver.
- Retrofit Interceptor.
- Authenticator.
- GestureDetector.
- AnimatorListenerAdapter.
- NestedScrollConnection.
- Compose PointerInput.
- SAM Conversion.
- Lambda vs Object Expression.
- Closures.
- Variable capturing.
- JVM bytecode generation.
- Memory allocation.
- Compose performance.
- Memory leaks.
- Testing fake implementations.
- 20+ senior interview questions.
- Android best practices and cheat sheet.
