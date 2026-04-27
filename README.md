# JavaScript Under The Hood 🙌

## Why the name "JavaScript"?

Initially, web pages were entirely **static**—no movement, no interaction. Netscape decided to create a programming language for the browser to make the web **interactive**.

They assigned an employee named **Brendan Eich** to build it, and he did it in just **10 days!** It was very simple—no one expected it to become the language we use today for massive web apps and even backend development.

> The language was first named **Mocha** → then **LiveScript** → and finally **JavaScript**. This wasn't because it was related to Java; it was purely a **marketing** move to attract developers at the time.

---

## 1. High-level

Programming languages are divided into two main categories:

### Low-level
- Close to the **Hardware**.
- The code is mostly a text translation of **machine code** (e.g., Assembly: `MOV`, `ADD`).
- You manage **memory management** yourself.
- It runs on a specific processor only—because:
    - Each processor has its own **Instruction Set Architecture (ISA)**.
    - Each processor has a different **transistor arrangement**.
    - Each processor has different **registers**.
    - Each processor has a different **Memory Map**.
- **Extremely fast** because it is close to the machine code.

### High-level
- The code is close to **human language**.
- Needs translation → making it slower than low-level languages.
- **Memory management** is handled automatically.
- **Portable (Runs on anything)** — because it doesn't interact with the hardware directly.

JavaScript = **High-level** ✅

---

## 2. Prototype-based Object-Oriented

### The Evolution of Programming

**Imperative (e.g., Assembly/Basic)**
- Pure commands: Open, Store, Close, Delete.
- Loops used `GOTO 100`.
- Data was all mixed together → the code was a mess.

**Functional Programming (e.g., C)**
- The concept of **Scope** appeared.
- Code became structured into **functions** instead of scattered commands.
- Functions receive **arguments** → making code scalable.
- **The Problem:** Core program data had to stay in the **Global Scope** → any function could accidentally modify it 😬.

**Object-Oriented Programming (OOP)**
- The Idea: Bind data and logic together in one place.
- The **Object** = An entity with its own **properties** + **methods** + variables.

### The 4 Pillars of OOP

| Pillar | Meaning |
| :--- | :--- |
| **Encapsulation** | Binding data and methods together. |
| **Abstraction** | Hiding details and showing only what is important. |
| **Inheritance** | Inheriting from a parent object. |
| **Polymorphism** | The same command behaves differently depending on the object. |

### Inheritance: Classes vs. Prototype

**Classes:**
- The object takes properties via **copy-paste** (blueprints).
- If you delete the parent → your instance keeps what it already has.

**Prototype-based (Used by JS):**
- Objects have a hidden property called `[[Prototype]]`.
- If you create another object of the same type → it has a **link** to that prototype.
- If something in the prototype changes → **it changes for everyone**.
- More **memory efficient**.

> Why is it called **Object-Oriented**? In the past, the thought was "To whom do I give these commands?"—now it is "I will perform these commands **on** this object."

---

## 3. Multi-paradigm

A **Paradigm** = The style or approach you use to write your logic.

JS supports 3 seamlessly:
- ✅ **Imperative**
- ✅ **Functional Programming**
- ✅ **OOP**

---

## 4. Interpreted or Just-In-Time Compiled

### Before Translation: The AST
The code undergoes analysis and transforms into an **Abstract Syntax Tree (AST)**—this determines:
- Variable names.
- Code flow/direction.
- Operator precedence.
- Data types.

### The JIT Compiled
1. The **Interpreter** translates the code into **Bytecode** (a language close to machine code).
2. This allows the page to open for the user immediately ✅ (Great for **Lighthouse Performance**).
3. Then there is a **Monitor**—if it finds **hot** code (= repetitive, like a `for loop`):
    - It re-translates it directly into **machine code** and sends it to the CPU for maximum speed 🚀.
    - This is called **Optimization**.
4. If the monitor finds that a function that used to take a `string` suddenly receives a `number`:
    - It reverts the optimization → **Deoptimization**.
    - It sends the code back to the interpreter.

---

## 5. Dynamic
Meaning it is a **Loosely Typed Language**—you don't have to define the variable type while writing. This is what allows the engine to handle **memory management** for you.

---

## 6. Single-Threaded
The JS Engine executes **one line at a time**.

---

## 7. Garbage-Collected
The **Garbage Collector (GC)** is what removes things that no longer have a reference in the **Heap** to free up space.

### Memory Life Cycle
1. **Allocate** — Memory allocation (happens during the **Creation Phase**).
2. **Usage** — Usage during execution.
3. **Release** — Deallocation via the GC or the stack.

---

## 8. JS Engine — Stack & Heap

### The Stack
- Stores **primitives** and executes code.
- Called the **Call Stack** during execution.
- Clears data once execution is finished.

### The Heap
- Stores **objects**.
- Each object has a **hash** and a **reference (pointer)** in the stack.
- The **GC** wipes it only if it finds no pointer leading to it.

---

## 9. Execution Context (EC)
The EC is the **Execution Context**—it stores primitives and object references.

### Two Phases:

**First: Creation Phase** (Based on the AST)
- **Hoisting** occurs.
- Data is allocated in memory.

**Second: Execution Phase**
- It assigns the actual values.

### The EC consists of 3 things:
1. **Variable Environment Object (VEO)**
2. **Scope Chain**
3. **this**

---

## 10. Variable Environment Object (VEO)
A large object containing variables, the arguments object, and `this`.

**After ES6:** A separate **Declarative Record** appeared to hold `let` and `const`, keeping them safe from the global `window` object.

---

## 11. Scope & Scope Chain

### The Scope
Scope determines where data is accessible.

**Types of Scope:**
- **Global Scope**
- **Function Scope**
- **Block Scope (ES6+)**

### Lexical Scoping
Scope is determined **while you are writing** the code—not at runtime.

### The Keywords

| Keyword | Block Scope | Redeclare | Window Object |
| :--- | :---: | :---: | :---: |
| `var` | ❌ | ✅ | ✅ |
| `let` | ✅ | ❌ | ❌ |
| `const` | ✅ | ❌ | ❌ |

---

## 12. Hoisting & TDZ

### Hoisting
Memory allocation before execution.
- **Function declaration** → Fully allocated.
- **`var`** → Allocated as `undefined`.
- **`let` / `const`** → Reserved but uninitialized.

### Temporal Dead Zone (TDZ)
The period between the start of the scope and the actual declaration. Accessing variables here causes a `ReferenceError`.

---

## 13. this Keyword
A special variable pointing to the object currently executing the code.

| Type | What `this` points to |
| :--- | :--- |
| **Regular Function (Method)** | The calling object. |
| **Regular Function (Simple)** | `window` / `undefined`. |
| **Arrow Function** | The parent's `this` (**Lexical this**). |

---

## 14. Memory Management
- **Stack:** Auto-clears when the EC ends.
- **Heap:** Cleared by the **Garbage Collector** when no pointers remain.

---

## 15. First-Class Functions
Functions are **first-class citizens**:
- ✅ Store in variables.
- ✅ Pass as arguments.
- ✅ Return from other functions.

---

## 16. Non-blocking Event Loop

1. **JS Engine** sends async tasks (DOM, fetch) to **Web APIs**.
2. When finished, tasks wait in the **Callback Queue**.
3. The **Event Loop** pushes them to the **Stack** only when it is empty.

---

## Summary

| **Term** | **Simplified Meaning** |
| :--- | :--- |
| **High-level** | Manages memory and abstracts hardware. |
| **Prototype-based** | Uses prototype chains for inheritance. |
| **Multi-paradigm** | Supports OOP, Functional, and Imperative. |
| **JIT Compiled** | Optimizes hot code at runtime. |
| **Dynamic** | Loosely typed variables. |
| **Single-threaded** | One task at a time. |
| **Garbage-collected** | Auto-memory cleanup. |
| **First-class functions** | Functions as values. |
| **Non-blocking** | Async code via the Event Loop. |

---
