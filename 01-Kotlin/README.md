# 💜 Kotlin Interview Guide (2026 Edition)

> Master Kotlin from fundamentals to Staff Android Engineer level for Android interviews.

This section contains **70+ Kotlin interview notes**, production-ready code examples, JVM internals, compiler behavior, performance discussions, and company-specific interview questions.

---

# 🎯 Why Learn Kotlin Deeply?

Kotlin is no longer just an Android language—it's the foundation of modern Android development, Kotlin Multiplatform, Compose, backend development with Ktor, and many product company interviews.

This guide is designed for:

- Android Developers (0–10+ years)
- Kotlin Multiplatform Developers
- Senior Android Engineers
- Staff Android Engineer Interviews

---

# 🗺️ Kotlin Learning Roadmap

```text
Kotlin Basics
      │
      ▼
Functions & OOP
      │
      ▼
Collections
      │
      ▼
Generics
      │
      ▼
Delegation
      │
      ▼
Advanced Kotlin
      │
      ▼
Reflection & DSL
      │
      ▼
Compiler & JVM Internals
      │
      ▼
Performance & Interview Questions
```

---

# 📚 Kotlin Curriculum

## 🟢 Module 1 — Kotlin Fundamentals (10 Notes)

📂 `01-Basics`

| # | Topic |
|--:|-------|
| 01 | Variables (`val` vs `var`) |
| 02 | Data Types |
| 03 | Type Inference |
| 04 | Null Safety |
| 05 | Operators |
| 06 | If, When Expressions |
| 07 | Loops |
| 08 | Ranges |
| 09 | String Templates |
| 10 | Type Casting |

---

## 🔵 Module 2 — Object-Oriented Kotlin (8 Notes)

📂 `02-OOP`

| # | Topic |
|--:|-------|
| 11 | Classes & Objects |
| 12 | Constructors |
| 13 | Inheritance |
| 14 | Abstract Classes |
| 15 | Interfaces |
| 16 | Data Classes |
| 17 | Enum Classes |
| 18 | Sealed Classes |

---

## 🟣 Module 3 — Functions & Lambdas (10 Notes)

📂 `03-Functions`

| # | Topic |
|--:|-------|
| 19 | Functions |
| 20 | Default Arguments |
| 21 | Named Arguments |
| 22 | Extension Functions |
| 23 | Higher Order Functions |
| 24 | Lambda Expressions |
| 25 | Inline Functions |
| 26 | noinline |
| 27 | crossinline |
| 28 | Tail Recursion |

---

## 🟠 Module 4 — Collections & Sequences (10 Notes)

📂 `04-Collections`

| # | Topic |
|--:|-------|
| 29 | List |
| 30 | MutableList |
| 31 | Set |
| 32 | Map |
| 33 | ArrayList vs LinkedList |
| 34 | HashMap vs LinkedHashMap |
| 35 | Collection Operators |
| 36 | Grouping |
| 37 | Sequences |
| 38 | Performance of Collections |

---

## 🟡 Module 5 — Advanced Kotlin (15 Notes)

📂 `05-Advanced-Kotlin`

| # | Topic |
|--:|-------|
| 39 | Object Declaration |
| 40 | Companion Object |
| 41 | Singleton |
| 42 | Delegation |
| 43 | Lazy Delegation |
| 44 | Observable Delegates |
| 45 | Vetoable Delegates |
| 46 | Generics |
| 47 | Variance (`in` / `out`) |
| 48 | Star Projection |
| 49 | Reified Types |
| 50 | Type Erasure |
| 51 | Value Classes |
| 52 | Context Receivers |
| 53 | Contracts API |

---

## 🔴 Module 6 — JVM & Compiler Internals (10 Notes)

📂 `06-Kotlin-Internals`

| # | Topic |
|--:|-------|
| 54 | Kotlin Bytecode |
| 55 | JVM Memory Model |
| 56 | Stack vs Heap |
| 57 | Garbage Collection |
| 58 | Reflection |
| 59 | KClass vs Java Class |
| 60 | Annotation Processing |
| 61 | Kotlin Compiler Pipeline |
| 62 | IR Compiler |
| 63 | Kotlin Performance Tips |

---

## 🟤 Module 7 — Interview Questions (8 Notes)

📂 `07-Interview-Questions`

| # | Topic |
|--:|-------|
| 64 | Top 100 Kotlin Questions |
| 65 | Google Kotlin Questions |
| 66 | PhonePe Kotlin Questions |
| 67 | Uber Kotlin Questions |
| 68 | Amazon Kotlin Questions |
| 69 | Tricky Kotlin Questions |
| 70 | Kotlin Coding Questions |
| 71 | Kotlin Cheat Sheet |

---

# ⭐ Interview Importance Matrix

| Topic | Interview Frequency |
|-------|----------------------|
| Null Safety | ⭐⭐⭐⭐⭐ |
| Scope Functions | ⭐⭐⭐⭐⭐ |
| Higher Order Functions | ⭐⭐⭐⭐⭐ |
| Coroutines + Suspend | ⭐⭐⭐⭐⭐ |
| Collections | ⭐⭐⭐⭐⭐ |
| Generics | ⭐⭐⭐⭐ |
| Variance | ⭐⭐⭐⭐ |
| Delegation | ⭐⭐⭐ |
| Reflection | ⭐⭐⭐ |
| DSL | ⭐⭐ |
| Compiler Internals | ⭐⭐⭐⭐ (Senior) |
| JVM Memory | ⭐⭐⭐⭐ |

---

# 🏢 Company Focus

| Company | Most Asked Kotlin Topics |
|----------|--------------------------|
| Google | Generics, Inline Functions, Bytecode, Memory |
| PhonePe | Collections, Coroutines, Variance |
| Uber | Lambdas, DSL, Performance |
| Razorpay | Kotlin Internals, Extension Functions |
| Microsoft | OOP, Collections, Null Safety |
| Amazon | OOP, Collections, Generics |

---

# 📊 Kotlin Progress Tracker

## Fundamentals

- [ ] Variables
- [ ] Data Types
- [ ] Null Safety
- [ ] Operators
- [ ] When Expression
- [ ] Loops

## OOP

- [ ] Classes
- [ ] Constructors
- [ ] Inheritance
- [ ] Interfaces
- [ ] Data Classes
- [ ] Sealed Classes

## Functions

- [ ] Extension Functions
- [ ] Higher Order Functions
- [ ] Lambdas
- [ ] Inline Functions

## Collections

- [ ] List
- [ ] Set
- [ ] Map
- [ ] Sequence

## Advanced

- [ ] Delegation
- [ ] Generics
- [ ] Variance
- [ ] Reflection
- [ ] Contracts
- [ ] Context Receivers

## Internals

- [ ] JVM Memory
- [ ] Bytecode
- [ ] Compiler
- [ ] Performance

---

# 📖 How Every Kotlin Note Is Structured

Every topic follows the same documentation format.

```markdown
# Topic Name

## Definition

## Why It Matters

## Internal Working

## JVM Behavior

## Memory Diagram

## Code Example

## Production Example

## Best Practices

## Common Mistakes

## Comparison Table

## Interview Questions

## Cheat Sheet
```

---

# 🎯 Kotlin Revision Checklist

### Beginner

- [ ] Completed all basics.
- [ ] Solved 30 Kotlin questions.

### Intermediate

- [ ] Collections mastered.
- [ ] Lambdas mastered.
- [ ] Scope Functions mastered.

### Advanced

- [ ] Generics mastered.
- [ ] Variance mastered.
- [ ] Delegation mastered.

### Senior

- [ ] Reflection understood.
- [ ] JVM Bytecode understood.
- [ ] Kotlin Compiler basics understood.

---

# 🚀 Next Step

Start with **Module 1 → Variables (`01-Basics/01-Variables.md`)** and follow the modules sequentially.