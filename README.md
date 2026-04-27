# JavaScript Under The Hood 🙌

## Why the name "JavaScript"?

Initially, web pages were entirely **static**—no movement, no interaction. Netscape decided to create a programming language for the browser to make the web **interactive**.

They assigned an employee named **Brendan Eich** to build it, and he did it in just **10 days!** It was very simple—no one expected it to become the language we use today for massive web apps and even backend development.

> The language was first named **Mocha** → then **LiveScript** → and finally **JavaScript**. This wasn't because it was related to Java; it was purely a **marketing** move to attract developers at the time.

I won't go into more history, like how **Internet Explorer** appeared with **JScript** and how **ES** (ECMAScript) came to solve the fragmentation—that’s a story for another time.

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
- The object takes properties via **copy-paste** literally (blueprints).
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

> "Wait, won't it freeze when it hits async code?" → Not necessarily, and that is thanks to the **Event Loop** 👇.

---

## 7. Garbage-Collected
The **Garbage Collector (GC)** is what removes things that no longer have a reference in the **Heap** to free up space.

### Memory Life Cycle
1. **Allocate** — Memory allocation (happens during the **Creation Phase**).
2. **Usage** — Usage during execution (**Running**).
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

> Everything in JS except primitives is an **object**—even Arrays and Functions! An Array is an object using indexes; a Function is an object with a "calling" capability that executes code.

---

## 9. Execution Context (EC)
The EC is the **Execution Context**—it stores primitives and object references.

### Two Phases:

**First: Creation Phase** (Based on the AST)
- **Hoisting** occurs.
- Data is allocated in memory.
- Example: `const Ahmed = 10` → it reserves the name "Ahmed" and initializes it as empty first.

**Second: Execution Phase**
- It assigns the actual values.

> Why? Because if it allocated and executed line-by-line → the language would be very slow. This separation boosts performance.

### The EC consists of 3 things:
1. **Variable Environment Object (VEO)**
2. **Scope Chain**
3. **this**

---

## 10. Variable Environment Object (VEO)
A large object containing:
- Variables.
- The **arguments object** (for regular functions).
- `this` for the current execution context.
- The `if` / `for` logic if inside a regular function.

**Before ES6:** Everything was in the **Object Environment Record** → meaning every variable was registered as a property on the global object → if global, it was never cleared → huge problems.

**After ES6:** A separate **Declarative Record** appeared:
- Holds `let` and `const`.
- Not visible to the `window` object.
- Does not hoist the same way as `var`.

---

## 11. Scope & Scope Chain

### The Scope
Scope is what allows data to be stored in its own specific VE and keeps it "locked" inside.

**Types of Scope:**
- **Global Scope** — Outside everything.
- **Function Scope** — Inside a function.
- **Block Scope (ES6+)** — Inside `{}` blocks like `if` / `for`.

### Lexical Scoping
Scope is determined **while you are writing** the code—not at runtime. The AST handles this.

### The Keywords

| Keyword | Block Scope | Redeclare | Window Object |
| :--- | :---: | :---: | :---: |
| `var` | ❌ (Function scope only) | ✅ | ✅ |
| `let` | ✅ | ❌ | ❌ |
| `const` | ✅ | ❌ | ❌ |

> `var` was built before block scope, and they couldn't "fix" it later due to the **"Don't Break the Web"** principle.

### The Scope Chain
- Located in the **Heap**.
- Contains all **VEs**.
- If a variable is not in the current scope → it looks at the parent → then the global → if not found → **ReferenceError**.
- It only looks **Up** (not down)—because code executes top-down.

---

## 12. Hoisting & TDZ

### Hoisting
Memory allocation before execution (during the **Creation Phase**).
- **Function declaration** → Fully allocated in the heap ← Can be called before its definition ✅.
- **`var`** → Only the name is reserved, value = `undefined` ⚠️.
- **`let` / `const`** → Reserved but without any value (not even `undefined`).

### Temporal Dead Zone (TDZ)
The area from the **start of the scope** until the **line of declaration**—if you touch the variable here:
`ReferenceError: Cannot access before initialization`

---

## 13. this Keyword
- A **Special variable** that points to the object currently being executed.
- Each EC has its own `this`.

| Type | What `this` points to |
| :--- | :--- |
| **Regular Function (Method call `obj.fun()`)** | The object calling the method. |
| **Regular Function (Simple call `fun()`)** | `window` / `undefined` (in strict mode). |
| **Arrow Function** | The parent's `this` (**Lexical this**). |

> Arrow functions do NOT have their own `this` or **arguments object**.

### The Arguments Object
- An object containing all arguments in an array-like structure.
- You can check `.length` but you cannot use `.map()`.
- Arrow functions do not have it ❌.

---

## 14. Memory Management

### Memory Life Cycle
1. **Allocate** — Reservation.
2. **Usage** — Utilization.
3. **Release** — Freeing up.

### Stack vs. Heap
- **Stack:** Automatically clears primitives and references when the EC ends.
- **Heap:** The GC clears it when it sees an object with no pointer.

### Global Scope and the Window
Anything in the global scope (old `var`) is not cleared until the tab is closed—because the `window` object is holding onto it.

---

## 15. First-Class Functions
This means the function is treated as a **first-class citizen**:
- ✅ Can be stored in a variable.
- ✅ Can be passed as an argument.
- ✅ Can be returned from a function.

**Why is it treated this way?**
As explained in Section 8—in JS, everything except primitives is an **object**. Functions are no exception; they are objects of a special type that can be "called" and execute code.

Since they are objects, they are stored in the **Heap** just like any other object, with a **reference (pointer)** in the stack. This is exactly what allows JS to treat a function as a regular value—to put it in a variable, send it as an argument, or return it from another function—because in the end, it is just a **pointer to an object in the heap**.

> This is why concepts like **callbacks** and **higher-order functions** work naturally in JS.

---

## 16. Non-blocking Event Loop (Concurrency Model)

### Runtime Components


### How it works:
1. If the JS Engine finds something like the DOM or `setTimeout` → it sends it to the **Web APIs**.
2. Once finished → it is placed in the **Callback Queue**.
3. The **Event Loop** monitors—if the **Call Stack** is empty → it takes the callback and pushes it onto the stack.
4. Consequently, the UI does not freeze ✅ → **Non-blocking**.

---

## Summary: Explaining the Full Definition

> *JavaScript is a high-level, prototype-based object-oriented, multi-paradigm, interpreted or just-in-time compiled, dynamic, single-threaded, garbage-collected programming language with first-class functions and a non-blocking event loop concurrency model.*

| **Term** | **Simplified Meaning** |
| :--- | :--- |
| **High-level** | Human-friendly, abstracts hardware, manages memory. |
| **Prototype-based OOP** | Uses prototype chains for objects instead of classical classes. |
| **Multi-paradigm** | Supports Imperative, Functional, and OOP styles. |
| **JIT Compiled** | Translates to bytecode first, then machine code for "hot" segments. |
| **Dynamic** | Loosely typed—no need to declare types manually. |
| **Single-threaded** | Executes one line at a time. |
| **Garbage-collected** | Automatically clears objects with no references. |
| **First-class functions** | Functions can be stored, passed, and returned. |
| **Non-blocking Event Loop** | Async code doesn't freeze the UI via Web APIs/Queues. |
