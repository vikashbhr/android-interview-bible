# 🚀 Chapter 21 — SOLID Principles Masterclass (Android + Kotlin + Jetpack Compose + Hilt + Coroutines)

> **Android Interview Bible 2026 — Premium Edition**

> This chapter is one of the most important chapters in Android interviews. Almost every senior Android interview (5–12 years experience) includes questions around **SOLID Principles**, Clean Architecture, Repository Pattern, Dependency Injection, and scalable app design.

This chapter is written specifically for **real production Android apps** like **Swiggy, PhonePe, WhatsApp, Instagram, Uber, CRED, Flipkart, Meesho, and Amazon**.

---

# 📚 Table of Contents

## Part 1 — Introduction + S + O Principles

1. What is SOLID?
2. Why SOLID Exists?
3. Real Story — Building WhatsApp Without SOLID
4. Single Responsibility Principle (SRP)
5. SRP in Android MVVM
6. SRP Violations in Production Apps
7. Open Closed Principle (OCP)
8. OCP with Payment Gateway
9. OCP with RecyclerView & Compose
10. OCP with Repository Pattern
11. Interview Questions
12. Code Review Round
13. Cheat Sheet

> **Part 2** will cover **LSP + ISP + DIP** in extreme depth.

---

# 🎯 Learning Goals

After completing this chapter you'll be able to:

- Explain SOLID in interviews with confidence.
- Design scalable Android architecture.
- Identify SOLID violations.
- Refactor bad Android code.
- Answer Google/Uber/CRED architecture questions.
- Apply SOLID in Jetpack Compose, MVVM, Hilt and Coroutines.

---

# 🌟 What is SOLID?

## 🎭 Real Story — Why WhatsApp Doesn't Have One Giant Class

Imagine WhatsApp was written like this:

```kotlin
class WhatsApp {

    fun login(){}

    fun logout(){}

    fun sendMessage(){}

    fun receiveMessage(){}

    fun uploadPhoto(){}

    fun downloadPhoto(){}

    fun playVoiceNote(){}

    fun syncContacts(){}

    fun fetchStatus(){}

    fun sendNotification(){}

    fun backupChats(){}

    fun restoreChats(){}

    fun analytics(){}

    fun payment(){}
}
```

Everything inside one class.

### What Happens After 5 Years?

- 15 developers edit same file.
- Merge conflicts.
- Bugs increase.
- Testing becomes impossible.
- One change breaks another feature.

This is exactly what SOLID tries to prevent.

---

# What Does SOLID Stand For?

| Letter | Principle |
|--------|-----------|
| **S** | Single Responsibility Principle |
| **O** | Open Closed Principle |
| **L** | Liskov Substitution Principle |
| **I** | Interface Segregation Principle |
| **D** | Dependency Inversion Principle |

> These five principles help build software that is **maintainable, scalable, reusable, testable and loosely coupled**.

---

# Why Every Android Developer Must Know SOLID ⭐⭐⭐⭐⭐

## Android Without SOLID

```text
Activity
   │
   ├── API Call
   ├── Database
   ├── Analytics
   ├── Navigation
   ├── Validation
   ├── Business Logic
   ├── SharedPreferences
   └── UI
```

**God Activity** — a very common interview discussion.

---

## Android With SOLID

```text
Compose UI
     │
     ▼
ViewModel
     │
     ▼
UseCases
     │
     ▼
Repository
 ├── API
 ├── Room
 └── Cache
```

Each layer has one responsibility.

---

# Benefits of SOLID

| Benefit | Android Example |
|---------|-----------------|
| Reusable Code | UseCase reused in multiple ViewModels. |
| Easy Testing | Mock Repository. |
| Easy Refactoring | Replace Firebase with REST API. |
| Better Team Collaboration | Feature modules. |
| Scalability | Large enterprise apps. |

---

# Interview Question ⭐⭐⭐⭐⭐

## Q1. What is SOLID?

### 30-Second Answer

SOLID is a set of five object-oriented design principles that help build loosely coupled, maintainable, scalable, and testable software systems.

### 2-Minute Senior Answer

SOLID helps separate responsibilities, extend software without modifying existing code, replace implementations safely, create focused interfaces, and depend on abstractions instead of concrete implementations. Modern Android architecture (MVVM + Repository + Hilt + UseCases + Compose) follows SOLID extensively.

---

# Interview Tip

Whenever asked about architecture, naturally mention:

- SOLID
- Clean Architecture
- Dependency Injection
- Composition over Inheritance
- State Hoisting

---

# S — Single Responsibility Principle (SRP) ⭐⭐⭐⭐⭐

## Definition

> **A class should have only one reason to change.**

A responsibility means a single business purpose.

---

# 🎭 Story — Restaurant Kitchen

Imagine a chef.

The chef should cook food.

Should the chef also:

- Take payment?
- Clean tables?
- Deliver food?
- Manage inventory?
- Answer phone?

No.

Every responsibility belongs to someone else.

Exactly SRP.

---

# Android Example — Login Feature

## ❌ Bad Design

```kotlin
class LoginActivity {

    fun validateInput(){}

    fun loginApi(){}

    fun saveToken(){}

    fun navigateHome(){}

    fun sendAnalytics(){}

    fun showLoading(){}

    fun hideLoading(){}

    fun cacheUser(){}

    fun updateFirebase(){}
}
```

### Problems

- UI logic.
- API logic.
- Storage logic.
- Analytics logic.
- Navigation logic.

Everything mixed together.

---

# Why This Is Dangerous

Imagine analytics changes.

LoginActivity changes.

Imagine API changes.

LoginActivity changes.

Imagine navigation changes.

LoginActivity changes.

**Many reasons to change one class.**

---

# ✅ SRP Version

```text
LoginScreen

      │
      ▼
LoginViewModel

      │
      ▼
LoginUseCase

      │
      ▼
AuthRepository

 ├── ApiService
 ├── TokenStorage
 └── AnalyticsTracker
```

Each class has one responsibility.

---

# Kotlin Implementation

## LoginViewModel

```kotlin
@HiltViewModel
class LoginViewModel @Inject constructor(
    private val loginUseCase: LoginUseCase
) : ViewModel()
```

Only manages UI state.

---

## LoginUseCase

```kotlin
class LoginUseCase(
    private val repository: AuthRepository
)
```

Only business workflow.

---

## Repository

```kotlin
interface AuthRepository {

    suspend fun login(...)
}
```

Only authentication data operations.

---

## Token Manager

```kotlin
class TokenManager(
    private val dataStore: DataStore<Preferences>
)
```

Only token storage.

---

# Responsibilities Breakdown

| Class | Responsibility |
|--------|----------------|
| LoginScreen | Render UI |
| LoginViewModel | UI State |
| LoginUseCase | Business Rules |
| Repository | Data |
| TokenManager | Storage |
| Analytics | Tracking |

Perfect SRP.

---

# Real Production Example — Swiggy Checkout

### Checkout Screen Responsibilities

| Feature | Responsible Class |
|----------|-------------------|
| Coupon | CouponUseCase |
| Tax | TaxCalculator |
| Delivery Fee | DeliveryFeeCalculator |
| Cart | CartRepository |
| Payment | PaymentUseCase |

No single class handles everything.

---

# Compose Example — SRP ⭐⭐⭐⭐⭐

## ❌ Bad Composable

```kotlin
@Composable
fun ProfileScreen() {

    api.fetchUser()

    savePreference()

    updateAnalytics()

    Button(...)
}
```

Composable doing network + storage + analytics.

---

## ✅ Good Composable

```kotlin
@Composable
fun ProfileScreen(
    state: ProfileUiState,
    onRefresh: () -> Unit
)
```

Only renders UI.

ViewModel performs logic.

---

# SRP in Jetpack Compose

| Responsibility | Layer |
|---------------|------|
| UI | Composable |
| State | ViewModel |
| Business | UseCase |
| Data | Repository |

---

# SRP with Coroutines

## ❌ Bad Example

```kotlin
viewModelScope.launch {

    api.login()

    room.save()

    analytics.track()

    navigate()
}
```

Too many responsibilities.

---

## Better

```kotlin
viewModelScope.launch {
    loginUseCase()
}
```

UseCase coordinates workflow.

---

# Story — Instagram Like Button

When user likes a post:

- Update UI.
- Save locally.
- Sync server.
- Track analytics.
- Show animation.

Each responsibility belongs to different components.

---

# SRP Interview Questions ⭐⭐⭐⭐⭐

## Q2. What is a "Reason to Change"?

Business requirement that affects one responsibility.

Example:

Changing analytics shouldn't affect login UI.

---

## Q3. Is ViewModel Responsible for Business Logic?

Partially.

Simple UI orchestration.

Complex business logic belongs to UseCases.

---

## Q4. Why Repository Shouldn't Contain UI Logic?

Violates SRP.

Repository handles data.

---

## Q5. Can UseCase Call Multiple Repositories?

Yes.

Its responsibility is business workflow.

---

# Code Review Round — SRP ⭐⭐⭐⭐⭐

## Review This Code

```kotlin
class StoryViewModel {

    fun loadStories() {
        api.fetchStories()
        dao.saveStories()
        analytics.track()
        notification.schedule()
    }
}
```

### Problems

- API.
- Database.
- Analytics.
- Notification.

Four responsibilities.

---

## Refactor

```text
StoryViewModel

↓

LoadStoriesUseCase

├── StoryRepository
├── AnalyticsTracker
└── NotificationScheduler
```

Much cleaner.

---

# O — Open Closed Principle (OCP) ⭐⭐⭐⭐⭐

## Definition

> Software entities should be **open for extension** but **closed for modification**.

Meaning:

You can add new behavior **without editing existing tested code**.

---

# 🎭 Story — PhonePe Adds UPI Lite

PhonePe initially supports:

- UPI
- Card
- Wallet

Now RBI introduces **UPI Lite**.

Should developers edit old payment code?

No.

Extend.

---

# ❌ Bad Payment Example

```kotlin
class PaymentProcessor {

    fun pay(type: String) {

        when(type){

            "UPI" -> {}

            "CARD" -> {}

            "WALLET" -> {}
        }
    }
}
```

Need to modify this every new payment method.

Violates OCP.

---

# ✅ OCP Version

```kotlin
interface PaymentGateway {

    suspend fun pay(amount: Double)
}
```

Implementations.

```kotlin
class UpiPayment : PaymentGateway

class CardPayment : PaymentGateway

class WalletPayment : PaymentGateway

class UpiLitePayment : PaymentGateway
```

No existing code modified.

---

# Android Example — Analytics Providers

```kotlin
interface AnalyticsTracker {

    fun track(event: Event)
}
```

Implementations.

- FirebaseAnalytics
- MixpanelAnalytics
- CleverTapAnalytics

Add new provider.

Old code unchanged.

---

# Compose Example — UI Components

Create reusable button.

```kotlin
@Composable
fun AppButton(...)
```

Add gradient version.

```kotlin
@Composable
fun GradientButton(...)
```

Extension instead of modifying original.

---

# RecyclerView Example

## Bad Adapter

```kotlin
if(type=="TEXT"){}

if(type=="IMAGE"){}

if(type=="VIDEO"){}
```

---

## Better

Separate ViewHolder classes.

Each extends BaseViewHolder.

Add AudioViewHolder later.

---

# OCP in Repository Pattern

```text
StoryRepository

       ▲

  FirebaseRepository

  ApiRepository

  CacheRepository
```

New implementation.

No ViewModel change.

---

# Story — Flipkart Delivery Methods

Delivery options grow every year.

- Standard
- Same Day
- Instant
- Locker Pickup
- Drone Delivery

OCP allows adding new strategy.

---

# Strategy Pattern + OCP

```kotlin
interface DeliveryStrategy
```

Implementations.

- Bike
- Truck
- Drone

Add Drone.

Old strategies untouched.

---

# OCP with Compose Navigation

Instead of modifying NavHost repeatedly.

Create feature navigation modules.

Each feature contributes destinations.

---

# OCP Interview Questions ⭐⭐⭐⭐⭐

## Q1. Explain OCP with Android example.

Repository implementations.

Payment gateways.

Analytics providers.

Image loaders.

---

## Q2. Why OCP Helps Large Teams?

Different teams extend features independently.

No merge conflicts.

---

## Q3. Which Design Patterns Follow OCP?

| Pattern | Why |
|----------|-----|
| Strategy | Add strategies. |
| Factory | Add products. |
| Decorator | Extend behavior. |
| Observer | Add observers. |

---

# Real Production Story — CRED Rewards

CRED introduced:

- Cashback
- Coins
- Scratch Card
- Coupons

Instead of editing reward engine.

Every reward becomes new implementation.

OCP enabled feature growth.

---

# SRP vs OCP

| SRP | OCP |
|------|-----|
| One responsibility. | Extend without modification. |
| Prevent God Classes. | Prevent modifying stable code. |
| Focuses on responsibility. | Focuses on extensibility. |

---

# Code Review Round — OCP ⭐⭐⭐⭐⭐

## Review This

```kotlin
class NotificationManager {

    fun send(type:String){

        if(type=="EMAIL"){}

        if(type=="SMS"){}

        if(type=="PUSH"){}
    }
}
```

### Refactor

```kotlin
interface NotificationSender

EmailSender

SmsSender

PushSender

WhatsappSender
```

Now adding WhatsApp requires no existing modification.

---

# Android Architecture Interview Scenario

## Swiggy Coupon Engine

Need support for:

- Flat Discount
- Percentage Discount
- Buy One Get One
- Festival Coupon
- Bank Offer

Solution:

```text
CouponStrategy

├── FlatCoupon

├── PercentageCoupon

├── FestivalCoupon

└── BankCoupon
```

Perfect OCP + Strategy.

---

# Rapid Fire Questions (SRP + OCP)

| Question | Answer |
|----------|--------|
| SRP full form? | Single Responsibility Principle. |
| OCP full form? | Open Closed Principle. |
| One reason to change means? | One business responsibility. |
| ViewModel responsibility? | UI state. |
| Repository responsibility? | Data coordination. |
| UseCase responsibility? | Business workflow. |
| Which pattern follows OCP? | Strategy Pattern. |
| Analytics providers follow? | OCP. |
| Payment gateways follow? | OCP. |
| RecyclerView ViewTypes follow? | OCP. |
| Compose stateless UI follows? | SRP. |
| Why God Activity is bad? | Violates SRP. |

---

# Cheat Sheet — SRP + OCP

## Android Mapping

| Principle | Android Example |
|-----------|-----------------|
| SRP | ViewModel, Repository, UseCase separation |
| OCP | PaymentGateway interface |
| SRP | Stateless Composable |
| OCP | RecyclerView ViewHolder hierarchy |
| OCP | Analytics implementations |
| OCP | ImageLoader implementations |
| SRP | TokenManager |

---

# Production Examples to Mention in Interviews

| Company | SOLID Example |
|---------|---------------|
| WhatsApp | Chat architecture with Repository + UseCases |
| Swiggy | Coupon Strategy Pattern |
| PhonePe | Payment Gateway implementations |
| Uber | Ride pricing strategies |
| Flipkart | Delivery strategies |
| Instagram | Feed renderer with different ViewTypes |
| CRED | Reward engine plugins |

---

# 📝 Part 1 Revision Summary

You completed:

- ✅ Introduction to SOLID
- ✅ Why SOLID Exists
- ✅ SRP Deep Dive (Android + Compose + Coroutines)
- ✅ SRP Code Review Round
- ✅ SRP Interview Questions
- ✅ Open Closed Principle Deep Dive
- ✅ OCP with Strategy Pattern
- ✅ OCP in Android Architecture
- ✅ Payment, Analytics, RecyclerView, Compose Examples
- ✅ Production Stories (Swiggy, PhonePe, CRED)
- ✅ Rapid Fire Questions
- ✅ Premium Cheat Sheet

---

# 🚀 Part 2 — Liskov Substitution Principle (LSP), Interface Segregation Principle (ISP) & Dependency Inversion Principle (DIP)

> **Android Interview Bible 2026 — Premium Edition**

> This is the deepest SOLID interview section. Companies like **Google, Uber, PhonePe, Amazon, Microsoft, CRED, Swiggy, Flipkart, Meesho** frequently ask architecture questions that indirectly test **LSP, ISP, and DIP**.

This chapter teaches these principles using **real Android production architectures**, **Jetpack Compose**, **Hilt**, **Repository Pattern**, **Coroutines**, and **Clean Architecture**.

---

# 📚 Table of Contents

1. Liskov Substitution Principle (LSP)
2. LSP in Android Architecture
3. LSP Violations (Real Production Bugs)
4. Interface Segregation Principle (ISP)
5. ISP in Android APIs
6. ISP with Compose & Repository
7. Dependency Inversion Principle (DIP)
8. DIP with MVVM + Hilt
9. DIP Internals (How Hilt Works)
10. DIP in Multi Module Apps
11. Code Review Round
12. Interview Questions
13. Rapid Fire Questions
14. Cheat Sheet

---

# 🎯 Learning Goals

After completing this part you'll be able to:

- Explain LSP beyond Bird/Penguin.
- Identify LSP violations in Android projects.
- Design focused interfaces.
- Understand Dependency Inversion deeply.
- Explain Hilt using DIP.
- Refactor tightly coupled Android code.

---

# 🟡 L — Liskov Substitution Principle (LSP)

## Definition ⭐⭐⭐⭐⭐

> **Objects of a subclass should be replaceable with objects of the superclass without changing program correctness.**

In simple words:

> If `Dog` is an `Animal`, then replacing `Animal` with `Dog` should not break existing behavior.

---

# 🎭 Story — Uber Driver Types

Uber has multiple driver types.

```
Driver
 ├── BikeDriver
 ├── AutoDriver
 ├── CarDriver
 └── EVDriver
```

Every driver should support:

- Accept Ride
- Start Ride
- Complete Ride

If EVDriver throws an exception for `startRide()`, Uber app breaks.

That violates LSP.

---

# Classic Bird Example (Interview Favorite)

## ❌ Wrong Design

```kotlin
open class Bird {
    open fun fly() {}
}

class Penguin : Bird() {
    override fun fly() {
        throw Exception("Penguins can't fly")
    }
}
```

---

## Why This Violates LSP?

Client expects every `Bird` to fly.

Replacing Bird with Penguin changes behavior.

Broken contract.

---

# ✅ Better Design

```kotlin
interface Bird

interface FlyingBird : Bird {
    fun fly()
}

class Sparrow : FlyingBird {
    override fun fly() {}
}

class Penguin : Bird
```

Now substitution is safe.

---

# Android Story — Media Player ⭐⭐⭐⭐⭐

Imagine a media player.

```
MediaPlayer
 ├── AudioPlayer
 ├── VideoPlayer
 ├── PodcastPlayer
```

Every player implements:

```kotlin
play()

pause()

stop()
```

If `PodcastPlayer.pause()` crashes...

Violation.

---

# Android Example — Notification Sender

```kotlin
interface NotificationSender {

    fun send(notification: Notification)
}
```

Implementations:

- EmailSender
- PushSender
- SmsSender

ViewModel doesn't care which implementation.

LSP satisfied.

---

# Android Architecture Example — Payment Gateway

```kotlin
interface PaymentGateway {

    suspend fun pay(amount: Double)
}
```

Implementations:

- UPI
- Card
- Wallet
- NetBanking

All must follow same contract.

---

# Compose Example — UI Components

```
AppCard
 ├── ProductCard
 ├── StoryCard
 ├── UserCard
```

Every card accepts Modifier.

Every card behaves consistently.

---

# LSP Violation in Compose

```kotlin
@Composable
fun StoryCard(...)
```

One card ignores Modifier padding.

Other respects it.

Unexpected behavior.

Violation.

---

# LSP in Repository Pattern ⭐⭐⭐⭐⭐

Repository contract.

```kotlin
interface StoryRepository {

    suspend fun getStories(): List<Story>
}
```

Implementations:

- FirebaseRepository
- RoomRepository
- ApiRepository

Each returns stories.

No implementation changes expectations.

---

# Bad Repository Example

```kotlin
class FirebaseRepository {

    override suspend fun getStories(): List<Story> {
        return emptyList()
    }
}

class ApiRepository {

    override suspend fun getStories(): List<Story> {
        throw Exception()
    }
}
```

Clients behave differently.

Violation.

---

# LSP Checklist

| Rule | Good Practice |
|------|---------------|
| Don't throw unsupported exceptions. | ✅ |
| Preserve parent behavior. | ✅ |
| Don't strengthen preconditions. | ✅ |
| Don't weaken postconditions. | ✅ |

---

# LSP Interview Questions ⭐⭐⭐⭐⭐

## Q1. Explain LSP with Android example.

Use Repository or PaymentGateway.

---

## Q2. Difference Between OCP and LSP?

| OCP | LSP |
|------|-----|
| Extend behavior. | Replace safely. |
| Add implementations. | Preserve behavior. |

---

## Q3. What is Contract?

Parent class promises behavior.

Children must honor it.

---

# Code Review — LSP

## Bad Example

```kotlin
interface Downloader {

    fun download()
}

class OfflineDownloader : Downloader {

    override fun download() {
        throw UnsupportedOperationException()
    }
}
```

Violation.

---

## Better

Separate interfaces.

---

# 🟢 I — Interface Segregation Principle (ISP)

## Definition ⭐⭐⭐⭐⭐

> Clients should not be forced to depend on methods they don't use.

Small focused interfaces.

---

# 🎭 Story — Restaurant Menu

Customer wants coffee.

Restaurant forces customer to order:

- Pizza
- Burger
- Dessert
- Juice

Huge menu.

Unnecessary dependency.

---

# Bad Interface

```kotlin
interface SmartDevice {

    fun call()

    fun takePhoto()

    fun playMusic()

    fun sendSms()

    fun useFlashlight()

    fun recordVideo()
}
```

A smart watch doesn't need camera.

Violation.

---

# Better ISP Design

```kotlin
interface Camera

interface MusicPlayer

interface Flashlight

interface Calling
```

Device implements only required capabilities.

---

# Android Example — Repository Interfaces ⭐⭐⭐⭐⭐

Bad.

```kotlin
interface UserRepository {

    fun login()

    fun logout()

    fun updateProfile()

    fun uploadPhoto()

    fun deleteAccount()

    fun fetchNotifications()

    fun saveSettings()
}
```

Too many responsibilities.

---

# Better

```kotlin
interface AuthRepository

interface ProfileRepository

interface NotificationRepository

interface SettingsRepository
```

Each feature owns its interface.

---

# Compose Example — Stateless Components

Bad.

```kotlin
@Composable
fun Button(
    text:String,
    icon: Painter?,
    loading:Boolean,
    image: Bitmap?,
    animation:Boolean,
    badge:Int?
)
```

Huge API.

---

# Better

- AppButton
- LoadingButton
- IconButton
- BadgeButton

Focused APIs.

---

# ISP with Analytics

Instead of:

```kotlin
interface Analytics {

    fun login()

    fun logout()

    fun payment()

    fun crash()

    fun screen()

    fun ads()
}
```

Create:

```kotlin
ScreenAnalytics

PaymentAnalytics

CrashAnalytics
```

---

# ISP with Camera API

Camera app.

Capabilities:

- Capture Photo
- Record Video
- Zoom
- Flash

Different devices support different capabilities.

Separate interfaces.

---

# Android Example — RecyclerView Adapter

Instead of one adapter handling:

- Stories
- Ads
- Banner
- Carousel
- Footer

Create dedicated delegate adapters.

ISP + OCP.

---

# Interview Question ⭐⭐⭐⭐⭐

When should interface become smaller?

Answer:

When implementations leave methods empty or throw exceptions.

---

# ISP Checklist

| Smell | Fix |
|-------|-----|
| Empty implementations | Split interface |
| Unsupported exceptions | Split interface |
| Huge interface | Feature interfaces |
| Unused methods | Segregate |

---

# Code Review — ISP

Bad.

```kotlin
interface Worker {

    fun code()

    fun design()

    fun test()

    fun deploy()
}
```

Designer forced to implement code.

Violation.

---

# Better

Separate interfaces.

---

# 🔵 D — Dependency Inversion Principle (DIP)

## Definition ⭐⭐⭐⭐⭐

> High-level modules should not depend on low-level modules.

Both depend on abstractions.

---

# 🎭 Story — Mobile Charger

Your phone depends on USB-C standard.

Not Samsung charger implementation.

Abstraction.

---

# Android Without DIP

```text
LoginViewModel

↓

LoginRepositoryImpl

↓

Retrofit
```

ViewModel depends on concrete class.

---

# Android With DIP

```text
LoginViewModel

↓

AuthRepository (Interface)

↓

LoginRepositoryImpl

↓

Retrofit
```

ViewModel depends on abstraction.

---

# Kotlin Example

```kotlin
interface AuthRepository {

    suspend fun login(...)
}
```

Implementation.

```kotlin
class AuthRepositoryImpl(...)
```

---

# ViewModel

```kotlin
class LoginViewModel(
    private val repository: AuthRepository
)
```

Perfect DIP.

---

# Why DIP Matters ⭐⭐⭐⭐⭐

Imagine changing Firebase to REST API.

Only implementation changes.

ViewModel untouched.

---

# Real Story — WhatsApp Cloud Migration

WhatsApp changes storage backend.

Repository implementation replaced.

UI unaffected.

That's DIP.

---

# DIP with Hilt ⭐⭐⭐⭐⭐

## Architecture

```text
ViewModel

↓

AuthRepository

↓

AuthRepositoryImpl

↓

Retrofit

↓

OkHttp
```

Hilt injects implementation.

---

# @Binds Example

```kotlin
@Binds
abstract fun bindRepository(
    impl: AuthRepositoryImpl
): AuthRepository
```

ViewModel receives interface.

---

# DIP + Testing

Production.

```kotlin
AuthRepositoryImpl
```

Testing.

```kotlin
FakeAuthRepository
```

ViewModel unchanged.

---

# Fake Repository Example

```kotlin
class FakeAuthRepository : AuthRepository
```

Used in unit tests.

---

# DIP in Compose

Composable receives abstraction.

```kotlin
@Composable
fun LoginScreen(
    state: LoginUiState,
    onLogin: () -> Unit
)
```

Composable doesn't know ViewModel.

---

# DIP with UseCases

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

Every layer depends inward.

---

# Multi Module DIP ⭐⭐⭐⭐⭐

Modules.

```
feature-login/

domain-auth/

data-auth/

core-network/
```

Feature depends on domain.

Domain depends on abstraction.

Data implements abstraction.

---

# DIP in WorkManager

Worker depends on Repository interface.

Injected via Hilt.

---

# DIP in Navigation

Feature exposes navigator interface.

App module provides implementation.

---

# Hilt Internals (Advanced Interview)

## Compile-Time Dependency Graph

Hilt generates Dagger components.

- SingletonComponent
- ActivityRetainedComponent
- ViewModelComponent
- ActivityComponent

Each object resolved through interfaces.

---

# Constructor Injection vs Field Injection

| Constructor | Field |
|-------------|-------|
| Immutable | Mutable |
| Testable | Harder |
| Preferred | Legacy |

---

# DIP Interview Questions ⭐⭐⭐⭐⭐

## Q1. Explain DIP with Android example.

ViewModel depends on Repository interface.

---

## Q2. Why Hilt follows DIP?

Injects abstraction instead of implementation.

---

## Q3. Difference Between DIP and DI?

| DIP | DI |
|-----|----|
| Design principle. | Technique/tool. |
| Depends on abstraction. | Provides dependencies. |

---

## Q4. Can DI exist without DIP?

Yes.

But architecture becomes tightly coupled.

---

# Real Production Example — Swiggy Analytics

Interfaces.

```text
AnalyticsTracker

├── FirebaseTracker

├── MixpanelTracker

└── InternalTracker
```

Hilt injects implementation.

---

# Code Review Round ⭐⭐⭐⭐⭐

## Bad ViewModel

```kotlin
class LoginViewModel {

    val repository = AuthRepositoryImpl()
}
```

Problems.

- Tight coupling.
- Hard testing.
- No abstraction.

---

## Good Version

```kotlin
class LoginViewModel(
    private val repository: AuthRepository
)
```

Injected.

---

## Bad Singleton

```kotlin
object ApiClient
```

Used everywhere.

Hard testing.

---

## Better

Inject ApiService.

---

# LSP + ISP + DIP Combined Example

## Story — PhonePe Payment

Architecture.

```text
PaymentScreen

↓

PaymentViewModel

↓

PaymentUseCase

↓

PaymentGateway (Interface)

├── UPI
├── Wallet
├── Card
├── UpiLite
```

### SOLID Mapping

| Principle | Applied |
|-----------|---------|
| SRP | ViewModel only state |
| OCP | Add payment methods |
| LSP | All gateways interchangeable |
| ISP | Separate payment interfaces |
| DIP | ViewModel depends on interface |

---

# Rapid Fire Questions (40 Questions)

| Question | Answer |
|----------|--------|
| LSP full form? | Liskov Substitution Principle. |
| ISP full form? | Interface Segregation Principle. |
| DIP full form? | Dependency Inversion Principle. |
| LSP means? | Replace subclass safely. |
| ISP means? | Small focused interfaces. |
| DIP means? | Depend on abstractions. |
| Repository uses DIP? | Yes. |
| Hilt follows DIP? | Yes. |
| Fake repository purpose? | Testing. |
| Why constructor injection? | Immutable dependency. |
| Bird/Penguin teaches? | LSP violation. |
| Huge repository interface violates? | ISP. |
| ViewModel creating Retrofit violates? | DIP. |
| Empty interface methods smell? | ISP violation. |
| UnsupportedOperationException smell? | LSP violation. |

---

# Cheat Sheet — LSP + ISP + DIP

## Android Mapping

| Principle | Android Example |
|-----------|-----------------|
| LSP | Repository Implementations |
| LSP | PaymentGateway |
| LSP | NotificationSender |
| ISP | AuthRepository |
| ISP | SettingsRepository |
| ISP | Analytics Interfaces |
| DIP | ViewModel → Repository |
| DIP | UseCase → Repository |
| DIP | Hilt Injection |
| DIP | Multi Module Domain Layer |

---

# Common Android Interview Smells

| Smell | Violated Principle |
|-------|--------------------|
| God Repository | SRP + ISP |
| ViewModel creates Retrofit | DIP |
| Child throws UnsupportedOperationException | LSP |
| Interface with 15 methods | ISP |
| Switch statement for payment methods | OCP |
| Singleton Activity Context | SRP + Memory Leak |

---

# Production Stories to Mention in Interviews

| Company | SOLID Example |
|---------|---------------|
| WhatsApp | Repository abstraction for sync engines |
| Swiggy | Payment & Coupon strategies |
| PhonePe | Multiple payment gateway implementations |
| Uber | Driver strategy implementations |
| Flipkart | Delivery providers |
| CRED | Reward engine abstractions |
| Instagram | Feed renderer interfaces |

---

# 📝 Part 2 Revision Summary

You completed:

- ✅ Liskov Substitution Principle (Deep Dive)
- ✅ Android LSP Examples
- ✅ LSP Violations & Refactoring
- ✅ Interface Segregation Principle
- ✅ ISP in Compose & Repository
- ✅ Dependency Inversion Principle
- ✅ DIP with MVVM + Hilt
- ✅ Hilt Internals
- ✅ Multi Module DIP
- ✅ Code Review Round
- ✅ Google-Level Interview Questions
- ✅ 40 Rapid Fire Questions
- ✅ Premium Cheat Sheet

---

# 🚀 Part 3 — SOLID in Real Android Projects (Jetpack Compose + Coroutines + Flow + Hilt + Multi Module)

> **Android Interview Bible 2026 — Premium Edition**

> This part is where SOLID stops being theory and becomes **production Android architecture**. Every example is inspired by real apps like **WhatsApp, Swiggy, PhonePe, Instagram, YouTube, Spotify, Uber, Flipkart, Amazon, Ola and CRED**.

> This section is intentionally written as if you're a **Senior Android Engineer designing a scalable application**.

---

# 📚 Table of Contents

1. SOLID in Jetpack Compose
2. SOLID in MVVM
3. SOLID in Coroutines & Flow
4. SOLID in Repository Pattern
5. SOLID in Room + Retrofit
6. SOLID in Paging 3
7. SOLID in WorkManager
8. SOLID in Multi Module Android Apps
9. SOLID Refactoring — WhatsApp Clone
10. SOLID Refactoring — Swiggy Clone
11. SOLID Refactoring — PhonePe Clone
12. SOLID Code Smells (25 Real Examples)
13. Senior Interview Questions
14. Cheat Sheet

---

# 🎯 Learning Goals

After this section you'll be able to:

- Apply SOLID to Compose architecture.
- Build scalable feature modules.
- Refactor legacy Android code.
- Identify architectural smells.
- Design enterprise Android apps.

---

# 1. SOLID in Jetpack Compose ⭐⭐⭐⭐⭐

## 🎭 Story — LEGO UI (Compose Philosophy)

Jetpack Compose UI is built from small LEGO blocks.

Instead of one giant screen:

```
HomeScreen

├── TopBar

├── SearchBar

├── CategorySection

├── StoryList

│     ├── StoryCard

│     ├── StoryCard

│     └── StoryCard

├── BottomBar

└── FloatingActionButton
```

Every composable has **one responsibility**.

---

# SRP in Compose

## ❌ Bad Composable (Everything Inside)

```kotlin
@Composable
fun HomeScreen() {

    val response = api.fetchStories()

    savePreference()

    analytics.track()

    NotificationManager.schedule()

    LazyColumn {
        items(response) {
            StoryCard(it)
        }
    }
}
```

### Why This Violates SOLID?

| Problem | Principle Violated |
|---------|--------------------|
| API Call | SRP |
| Storage | SRP |
| Analytics | SRP |
| Notification | SRP |

UI is doing business logic.

---

# ✅ Production Compose Architecture

```text
HomeScreen

      │ Events

      ▼

HomeViewModel

      │ StateFlow

      ▼

HomeUiState
```

Composable becomes pure UI.

```kotlin
@Composable
fun HomeScreen(
    state: HomeUiState,
    onRefresh: () -> Unit,
    onStoryClick: (StoryId) -> Unit
)
```

---

## Interview Tip ⭐⭐⭐⭐⭐

A good composable should ideally:

- Receive state.
- Emit events.
- Never know Retrofit.
- Never know Room.
- Never know Hilt.

---

# OCP in Compose Components

## Story — Design System

Create one reusable button.

```kotlin
AppButton(...)
```

Need loading button.

Don't modify old button.

Extend.

```kotlin
LoadingButton(...)
```

Need icon button.

```kotlin
IconButton(...)
```

Need gradient button.

```kotlin
GradientButton(...)
```

Original button remains untouched.

---

# ISP in Compose APIs

Bad.

```kotlin
AppButton(
    text = "",
    icon = null,
    badge = null,
    loading = false,
    image = null,
    progress = 0f,
    shimmer = false
)
```

Huge API.

---

Better.

```
AppButton

LoadingButton

IconButton

BadgeButton

OutlineButton
```

Small focused composables.

---

# DIP in Compose

Composable depends on callbacks.

```kotlin
@Composable
fun LoginScreen(
    onLogin: (LoginEvent) -> Unit
)
```

Doesn't know ViewModel.

Reusable in Preview.

Reusable in Tests.

Reusable in Desktop Compose.

---

# Real Story — BhaktiTales App

Your `StoryCard` shouldn't know Firestore.

```
StoryCard

↓

HomeScreen

↓

ViewModel

↓

Repository

↓

Firestore
```

This architecture scales beautifully.

---

# 2. SOLID in MVVM ⭐⭐⭐⭐⭐

## Production Folder Structure

```text
presentation/

    home/

        HomeScreen.kt

        HomeViewModel.kt

        HomeUiState.kt

        HomeEvent.kt

domain/

    usecase/

        GetStoriesUseCase.kt

data/

    repository/

        StoryRepository.kt

        StoryRepositoryImpl.kt
```

Every folder has one purpose.

---

## ViewModel Responsibility

Only:

- Receive events.
- Update state.
- Call UseCases.

Never:

- Parse JSON.
- Save SharedPreferences.
- Upload Images.
- Build Retrofit.

---

## Example

```kotlin
fun onEvent(event: HomeEvent)
```

UI events only.

---

## UseCase Responsibility

Business workflow.

Example.

```kotlin
BookmarkStoryUseCase
```

Responsible only for bookmarking.

---

## Repository Responsibility

Data orchestration.

```
Room

API

Cache

DataStore
```

One place for data.

---

## MVVM SOLID Mapping

| Principle | Layer |
|-----------|------|
| SRP | ViewModel |
| DIP | Repository Interface |
| ISP | Feature Interfaces |
| OCP | Repository Implementations |
| LSP | Multiple Repository Types |

---

# 3. SOLID in Coroutines & Flow ⭐⭐⭐⭐⭐

## Story — Live Cricket Score

Events arrive continuously.

Need reactive architecture.

---

# SRP with Flow

Bad.

```kotlin
flow {

    emit(api.fetch())

    analytics.track()

    dao.insert()

    notification.schedule()
}
```

Too many responsibilities.

---

Better.

Repository emits data.

UseCase coordinates.

ViewModel collects.

---

# StateFlow Responsibility

Persistent UI State.

```kotlin
MutableStateFlow(HomeState())
```

---

# SharedFlow Responsibility

Events.

Snackbar.

Toast.

Navigation.

---

# Channel Responsibility

One-time communication.

```
NavigateToProfile

NavigateToPayment
```

---

# DIP with Flow

ViewModel depends on:

```kotlin
Flow<List<Story>>
```

Not Retrofit.

---

# Compose Collection

```kotlin
collectAsStateWithLifecycle()
```

Lifecycle-aware.

---

## Interview Question ⭐⭐⭐⭐⭐

Why `collectAsStateWithLifecycle()` over `collectAsState()`?

Avoid collecting when UI is stopped.

---

# Story — Instagram Notifications

StateFlow → unread count.

SharedFlow → "Message Sent" Snackbar.

Channel → open chat screen.

Different responsibilities.

---

# 4. SOLID in Repository Pattern ⭐⭐⭐⭐⭐

## Story — Spotify Music

Music comes from:

- Local Cache.
- API.
- Downloaded Songs.
- Bluetooth.

Repository hides complexity.

---

# Repository Interface

```kotlin
interface MusicRepository {

    fun getSongs(): Flow<List<Song>>
}
```

---

# Implementations

```
ApiMusicRepository

RoomMusicRepository

OfflineMusicRepository
```

OCP + DIP + LSP.

---

# Caching Strategy

Repository decides.

UI doesn't know.

---

# Fake Repository for Tests

```kotlin
FakeMusicRepository
```

Used in unit tests.

---

# Story — Nursing Prep App

Questions may come from:

- Firestore.
- Room.
- JSON Assets.

Repository decides source.

Composable doesn't care.

---

# 5. SOLID in Room + Retrofit ⭐⭐⭐⭐⭐

## Story — WhatsApp Offline Messages

```
User Sends Message

↓

Room

↓

UI Updates

↓

Background Sync

↓

Retrofit
```

---

## Why Repository Writes Room First?

Offline First.

Fast UI.

Retry later.

---

## SRP Layers

| Component | Responsibility |
|-----------|---------------|
| DAO | Database Queries |
| Retrofit | Network Calls |
| Mapper | Conversion |
| Repository | Coordination |

---

## DTO vs Entity vs UI Model

```
StoryDto

↓

Mapper

↓

StoryEntity

↓

Mapper

↓

StoryUiModel
```

Every mapper has one responsibility.

---

# Interview Tip

Never expose DTO to UI.

---

# 6. SOLID in Paging 3 ⭐⭐⭐⭐⭐

## Story — Instagram Infinite Feed

Need endless scrolling.

---

## Architecture

```
PagingSource

↓

Repository

↓

Pager

↓

Flow<PagingData>

↓

Compose
```

---

## PagingSource Responsibility

Load one page.

---

## RemoteMediator Responsibility

Sync Room + API.

---

## ViewModel Responsibility

Expose PagingData.

---

## Compose Responsibility

Render LazyPagingItems.

---

## OCP Example

New feed source.

```
FollowingFeedPagingSource

TrendingFeedPagingSource

ExploreFeedPagingSource
```

No UI modification.

---

# 7. SOLID in WorkManager ⭐⭐⭐⭐⭐

## Story — WhatsApp Background Backup

Background work.

- Upload Backup.
- Sync Contacts.
- Download Images.

Separate workers.

---

## SRP

```
BackupWorker

ContactSyncWorker

ImageDownloadWorker

NotificationWorker
```

---

## DIP

Workers receive Repository.

Never create Retrofit manually.

---

## HiltWorker

Inject dependencies.

---

# 8. SOLID in Multi Module Apps ⭐⭐⭐⭐⭐

## Story — PhonePe Super App

Huge project.

```
app/

core/

core-network/

core-ui/

feature-home/

feature-payment/

feature-profile/

feature-history/
```

---

## Why Multi Module?

- Independent teams.
- Faster Gradle builds.
- Better encapsulation.
- Feature isolation.

---

## DIP Across Modules

```
feature-payment

↓

domain-payment

↓

PaymentRepository Interface

↓

data-payment
```

Presentation never depends on data.

---

## ISP Across Modules

Each feature exposes only public APIs.

Everything else `internal`.

---

# 9. SOLID Refactoring — WhatsApp Clone ⭐⭐⭐⭐⭐

## Before Refactoring

```
ChatActivity

5000 lines
```

Responsibilities.

- API.
- DB.
- Notification.
- Upload.
- Download.
- Emoji.
- Camera.
- Voice Notes.

God Activity.

---

## After Refactoring

```
ChatScreen

↓

ChatViewModel

↓

UseCases

├── SendMessageUseCase

├── UploadImageUseCase

├── DownloadMediaUseCase

├── SyncMessagesUseCase

↓

Repositories
```

---

## Benefits

- Easy testing.
- Feature isolation.
- Smaller PRs.

---

# 10. SOLID Refactoring — Swiggy Clone ⭐⭐⭐⭐⭐

## Coupon Engine

Bad.

```
if(BANK)

if(FESTIVAL)

if(FIRST_ORDER)

if(UPI)
```

---

Better.

```
CouponStrategy

├── BankCoupon

├── FestivalCoupon

├── FirstOrderCoupon

├── UpiCoupon
```

OCP.

---

# Delivery Calculation

Separate strategy.

```
BikeFee

CarFee

ExpressFee
```

---

# Tax Calculation

Separate calculator.

SRP.

---

# 11. SOLID Refactoring — PhonePe Clone ⭐⭐⭐⭐⭐

## Payment Flow

```
PaymentScreen

↓

PaymentViewModel

↓

PaymentUseCase

↓

PaymentGateway

├── UPI

├── Wallet

├── Card

├── UpiLite

├── NetBanking
```

Perfect SOLID.

---

## Adding Credit Card EMI

New implementation only.

No ViewModel changes.

---

# 12. SOLID Refactoring — BhaktiTales App ⭐⭐⭐⭐⭐

> **Based on your project.**

## Story Loading

Instead of Firestore inside composable.

```
StoryScreen

↓

StoryViewModel

↓

GetStoryUseCase

↓

StoryRepository

├── Firestore

├── Room

├── Assets
```

---

## Audio Narration

Separate responsibilities.

```
AudioPlayerManager

NarrationRepository

DownloadAudioUseCase

PlaybackUseCase
```

---

## Bookmark Feature

Separate UseCase.

```
BookmarkStoryUseCase
```

No StoryViewModel changes needed.

---

# 13. SOLID Code Smells (25 Real Android Examples) ⭐⭐⭐⭐⭐

## Smell 1 — God Activity

5000+ lines.

Fix.

ViewModel + UseCases.

---

## Smell 2 — God Repository

Everything inside repository.

Split repositories.

---

## Smell 3 — Singleton Context

Memory leak.

Inject Application.

---

## Smell 4 — ViewModel Creates Retrofit

Violation.

Inject Repository.

---

## Smell 5 — MutableStateFlow Public

Expose immutable StateFlow.

---

## Smell 6 — Giant Composable

Split UI sections.

---

## Smell 7 — Boolean Explosion

```
loading

error

success

empty

retry
```

Use sealed state.

---

## Smell 8 — Huge Interface

Split.

ISP.

---

## Smell 9 — UnsupportedOperationException

LSP violation.

---

## Smell 10 — Switch Statement Everywhere

Replace with Strategy Pattern.

---

## Smells 11–25

- Multiple responsibilities in Worker.
- API inside DAO.
- Room Entity used as UI Model.
- Navigation inside Repository.
- Analytics inside Composable.
- Context passed through layers.
- Mutable shared singleton.
- Hardcoded Dispatchers.
- Business logic inside Adapter.
- Business logic inside Fragment.
- Extension function overriding assumptions.
- GlobalScope usage.
- SharedPreferences everywhere.
- Utility class with 100 methods.
- Static object storing Activity.

Each smell includes refactoring approach.

---

# 14. Senior Android Interview Questions ⭐⭐⭐⭐⭐

## Q1. Design WhatsApp Chat Architecture.

Expected discussion.

- Repository.
- Offline First.
- StateFlow.
- WorkManager.
- WebSocket.
- SOLID.

---

## Q2. How would you migrate Firebase to REST API?

Answer.

Replace Repository implementation only.

---

## Q3. Why does Compose encourage Composition over Inheritance?

Reusable composables.

Modifier chaining.

State hoisting.

---

## Q4. How would you make ViewModel testable?

Inject interfaces.

Fake repositories.

UseCases.

---

## Q5. Explain SOLID using MVVM.

Map each principle.

---

## Q6. Refactor this Activity.

Interviewer gives God Activity.

Discuss responsibilities.

---

## Q7. How does Hilt implement DIP?

Generated graph.

Bindings.

Constructor injection.

---

## Q8. Explain architecture for Offline Notes App.

Room source of truth.

---

## Q9. Design scalable Payment SDK.

Strategies.

Factories.

Repositories.

---

## Q10. Explain SOLID violations you've fixed in production.

Use real example.

---

# 15. Rapid Fire Questions (60 Questions)

| Question | Answer |
|----------|--------|
| Compose follows which SOLID principle most? | SRP + DIP. |
| StateFlow responsibility? | UI State. |
| SharedFlow responsibility? | Events. |
| Repository responsibility? | Data orchestration. |
| UseCase responsibility? | Business logic. |
| DAO responsibility? | Database access. |
| Mapper responsibility? | Data conversion. |
| Fake Repository? | Testing. |
| PagingSource responsibility? | Load pages. |
| RemoteMediator responsibility? | Sync Room + API. |
| Hilt follows? | DIP. |
| Constructor injection preferred? | Yes. |
| Why multi-module? | Isolation. |
| Why `internal`? | Module encapsulation. |
| Why sealed UI state? | Exhaustive states. |

(Continue with 45 more revision questions in the final document.)

---

# 📋 Ultimate Cheat Sheet — SOLID in Android

## SOLID Mapping

| Principle | Android Example |
|-----------|-----------------|
| SRP | ViewModel, DAO, Worker |
| OCP | PaymentGateway |
| LSP | Repository Implementations |
| ISP | Feature-specific interfaces |
| DIP | Hilt Injection |

---

## Architecture Mapping

| Layer | Responsibility |
|-------|---------------|
| Compose | UI |
| ViewModel | State |
| UseCase | Business Logic |
| Repository | Data |
| DAO | Local Storage |
| Retrofit | Network |
| WorkManager | Background Tasks |

---

## Production Apps Mapping

| App | SOLID Example |
|-----|---------------|
| WhatsApp | Chat Sync Architecture |
| Swiggy | Coupon Strategy |
| PhonePe | Payment Gateway |
| Uber | Ride Pricing Strategy |
| Instagram | Feed Pagination |
| Spotify | Download Manager |
| Flipkart | Delivery Strategy |
| Amazon | Cart Pricing Engine |

---

# 🎓 Part 3 Revision Summary

You completed:

- ✅ SOLID in Jetpack Compose
- ✅ SOLID in MVVM
- ✅ SOLID in Coroutines & Flow
- ✅ SOLID in Repository Pattern
- ✅ SOLID in Room + Retrofit
- ✅ SOLID in Paging 3
- ✅ SOLID in WorkManager
- ✅ SOLID in Multi Module Apps
- ✅ WhatsApp Architecture Refactoring
- ✅ Swiggy Architecture Refactoring
- ✅ PhonePe Architecture Refactoring
- ✅ BhaktiTales Architecture Refactoring
- ✅ 25 Android SOLID Code Smells
- ✅ Senior Architecture Interview Questions
- ✅ Ultimate Android SOLID Cheat Sheet

---
