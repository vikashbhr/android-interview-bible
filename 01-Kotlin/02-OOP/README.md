# 💜 Kotlin Object-Oriented Programming (OOP) — Android Interview Bible (2026 Edition)

> Master Kotlin Object-Oriented Programming from fundamentals to advanced JVM internals, Android architecture patterns, Jetpack Compose usage, and Staff Engineer interview questions.

**Module:** 02 — Kotlin OOP

**Difficulty:** Beginner → Staff Engineer

**Interview Frequency:** ⭐⭐⭐⭐⭐

**Companies Covered:** Google • Uber • PhonePe • Amazon • Microsoft • Razorpay • Flipkart • CRED • Meesho

---

# 🎯 Module Goal

By the end of this module, you'll understand:

- Kotlin classes and objects from JVM internals.
- Constructors and initialization order.
- Inheritance and polymorphism.
- Interfaces vs Abstract classes.
- Data classes and immutability.
- Sealed classes for MVI and Compose.
- Value classes (Inline Classes) and performance.
- Delegation pattern in Kotlin.
- Object declarations, companion objects, singleton design.
- Nested vs Inner classes.
- Visibility modifiers.
- Extension functions vs member functions.
- Android architecture examples.
- JVM bytecode and memory diagrams.
- Google/Uber/PhonePe interview questions.

---

# 📚 Module Roadmap

| # | Topic | Difficulty |
|---|-------|------------|
| 01 | Classes & Objects | ⭐ |
| 02 | Constructors & Init Blocks | ⭐ |
| 03 | Inheritance | ⭐⭐ |
| 04 | Interfaces | ⭐⭐ |
| 05 | Abstract Classes | ⭐⭐ |
| 06 | Visibility Modifiers | ⭐⭐ |
| 07 | Data Classes | ⭐⭐⭐ |
| 08 | Enum Classes | ⭐⭐ |
| 09 | Sealed Classes & Interfaces | ⭐⭐⭐⭐ |
| 10 | Object Declarations & Singleton | ⭐⭐⭐ |
| 11 | Companion Objects | ⭐⭐⭐ |
| 12 | Nested & Inner Classes | ⭐⭐⭐ |
| 13 | Object Expressions | ⭐⭐⭐ |
| 14 | Delegation (`by`) | ⭐⭐⭐⭐ |
| 15 | Value Classes (Inline Classes) | ⭐⭐⭐⭐ |
| 16 | Extension Functions | ⭐⭐⭐ |
| 17 | Generics in OOP | ⭐⭐⭐⭐ |
| 18 | Composition vs Inheritance | ⭐⭐⭐⭐ |
| 19 | Design Patterns in Kotlin | ⭐⭐⭐⭐⭐ |
| 20 | OOP Interview Questions | ⭐⭐⭐⭐⭐ |

**Total Chapters:** 20

---

# 🧠 Interview Coverage Matrix

| Company | Topics Asked |
|---------|--------------|
| Google | Sealed Classes, Delegation, Value Classes, Interfaces, Companion Objects |
| Uber | Data Classes, Inheritance, Generics, Delegation |
| PhonePe | Sealed Classes + MVI, Object Singleton, Interfaces |
| Amazon | Abstract vs Interface, Nested Classes, Visibility |
| Microsoft | Data Classes, Extension Functions, Composition |

---

# 🏗️ Android Architecture Mapping

| Kotlin OOP Feature | Android Usage |
|--------------------|---------------|
| Data Class | UI State (`UiState`) |
| Sealed Class | API Result / Navigation State |
| Interface | Repository Contract |
| Abstract Class | Base ViewModel |
| Object Declaration | Singleton (Retrofit, Room, DI) |
| Companion Object | Factory Methods |
| Value Class | Strongly Typed IDs |
| Delegation | Compose State / Repository Delegates |

---

# 📱 Jetpack Compose Mapping

| Compose Concept | Kotlin OOP Feature |
|-----------------|--------------------|
| UiState | Data Class |
| ScreenState | Sealed Class |
| Navigation Event | Sealed Interface |
| remember | Delegation |
| MutableState | Property Delegation (`by`) |
| Stable Models | Immutable Data Classes |

---

# 💼 Machine Coding Examples Included

Every chapter contains real Android examples.

### Examples You'll Build

- User Profile Model
- Shopping Cart
- Authentication State
- Payment State
- Chat Message State
- API Result Wrapper
- Navigation State Machine
- Theme Manager Singleton
- Analytics Singleton
- Repository Pattern
- Factory Pattern
- Compose UI State Models

---

# 🧬 JVM Deep Dive Topics

Each chapter includes internals like:

- Object creation in JVM.
- Heap vs Stack diagrams.
- Constructor bytecode.
- Companion Object bytecode.
- Data class generated methods.
- Sealed class metadata.
- Inline value class optimization.
- Interface default implementations.
- Delegation bytecode generation.

---

# 📦 Folder Structure

```text
02-OOP/
│
├── README.md
│
├── 01-Classes-And-Objects.md
├── 02-Constructors-And-Init.md
├── 03-Inheritance.md
├── 04-Interfaces.md
├── 05-Abstract-Classes.md
├── 06-Visibility-Modifiers.md
├── 07-Data-Classes.md
├── 08-Enum-Classes.md
├── 09-Sealed-Classes.md
├── 10-Object-Declarations.md
├── 11-Companion-Objects.md
├── 12-Nested-And-Inner-Classes.md
├── 13-Object-Expressions.md
├── 14-Delegation.md
├── 15-Value-Classes.md
├── 16-Extension-Functions.md
├── 17-Generics.md
├── 18-Composition-vs-Inheritance.md
├── 19-Design-Patterns.md
├── 20-OOP-Interview-Questions.md
│
└── Cheat-Sheet.md
```

---

# 🎓 Learning Pattern (Same for Every Chapter)

Every `.md` file follows this structure:

1. Definition
2. Syntax
3. JVM Internals
4. Memory Diagram
5. Android Example
6. Jetpack Compose Example
7. Performance Notes
8. Best Practices
9. Common Mistakes
10. Interview Questions (Basic → Advanced)
11. 2-Minute Interview Answer
12. Cheat Sheet
13. Revision Summary

---

# 📖 Recommended Reading Order

## Foundation

- Classes & Objects
- Constructors
- Inheritance
- Interfaces
- Abstract Classes
- Visibility Modifiers

## Android Essentials

- Data Classes
- Enum Classes
- Sealed Classes
- Object Declaration
- Companion Objects

## Senior Android Topics

- Delegation
- Value Classes
- Extension Functions
- Generics
- Composition vs Inheritance

## Staff Engineer Topics

- Kotlin Design Patterns
- Complete OOP Interview Questions

---

# 🎯 Expected Outcome

After this module, you'll be able to confidently answer questions like:

- Why are data classes ideal for Compose UI state?
- Sealed Class vs Enum vs Abstract Class?
- Interface vs Abstract Class in Kotlin?
- Object Declaration vs Companion Object?
- How does Kotlin Delegation work internally?
- What bytecode does a data class generate?
- How are value classes optimized on the JVM?
- Composition vs Inheritance in Android architecture?
- Why do modern Android apps prefer sealed interfaces?

---

# 🚀 Module Completion Checklist

- [ ] Classes & Objects
- [ ] Constructors & Init
- [ ] Inheritance
- [ ] Interfaces
- [ ] Abstract Classes
- [ ] Visibility Modifiers
- [ ] Data Classes
- [ ] Enum Classes
- [ ] Sealed Classes
- [ ] Object Declaration
- [ ] Companion Objects
- [ ] Nested & Inner Classes
- [ ] Object Expressions
- [ ] Delegation
- [ ] Value Classes
- [ ] Extension Functions
- [ ] Generics
- [ ] Composition vs Inheritance
- [ ] Kotlin Design Patterns
- [ ] OOP Cheat Sheet & Interview Questions

**Estimated Size:** 400–600 pages of interview-quality notes.