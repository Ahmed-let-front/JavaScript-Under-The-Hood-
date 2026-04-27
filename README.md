# javascript under the hood

## ليه اسمها JavaScript؟

زمان كانت صفحات الويب **static** خالص — مفيش حركة ولا تفاعل. Netscape قررت تعمل لغة برمجة للمتصفح تخلي الويب **Interactive**.

كلفوا موظف عندهم اسمه **Brendan Eich** يعملها، وعملها في **10 أيام بس!** كانت بسيطة جداً — ومكانوش متوقعين إنها هتبقى اللي بنعمل بيها web apps وكمان backend.

> اللغة اتسمت الأول **Mocha** ← بعدين **LiveScript** ← وأخيراً **JavaScript** — مش علشان ليها علاقة بـ Java، ده كان مجرد حركة **تسويق** علشان يجذبوا المبرمجين.
> 

المهم مش عايز أكمل التفاصيل دي لأني هتظهر **Internet Explorer** بـ **JScript** وبعدها تيجي **ES** علشان تصلح المشكلة — بس دي مش مهمتنا دلوقتي.

---

## 1. High-level

لغات البرمجة بتتقسم قسمين:

### Low-level

- قريبة من الـ Hardware
- الكود معظمه ترجمة نصية للـ machine code (زي Assembly: `MOV`, `ADD`)
- إنت بتدير الـ **memory management** بنفسك
- بتشتغل على معالج معين بس — لأن:
    - كل معالج ليه **Instruction Set Architecture (ISA)** خاص بيه
    - كل معالج ليه **ترتيب transistors** مختلف
    - كل معالج ليه **سجلات** مختلفة
    - كل معالج ليه **Memory Map** مختلفة
- **سريعة جداً** لأنها قريبة من الـ machine code

### High-level

- الكود قريب من **كلامنا البشري**
- بتحتاج ترجمة → يعني أبطأ من الـ low-level
- الـ **memory management** بتتعملها أوتوماتيك
- **بتشتغل على أي حاجة** — لأنها مش بتتعامل مع الـ hardware مباشرة

JavaScript = **High-level** ✅

---

## 2. Prototype-based Object-Oriented

### رحلة البرمجة من الأول

**Imperative (زي Assembly/Basic)**

- لغة أوامر بس: افتح، خزّن، اقفل، امسح
- الـ loop كانت `GOTO 100`
- كل الداتا داخلة في بعض → الكود بيبقى كارثة

**Functional Programming (زي C)**

- ظهر مفهوم الـ **Scope**
- الكود بقى functions بدل ما يبقى أوامر متفرقة
- الـ function بتستقبل arguments → الكود قابل للتوسع
- **المشكلة:** البيانات الأساسية للبرنامج لازم تتحط في الـ Global Scope → أي function تقدر تعدل فيها 😬

**Object-Oriented Programming**

- الفكرة: نربط الداتا والأوامر في مكان واحد
- الـ **Object** = كيان ليه **properties** + **methods** + variables خاصة بيه

### الـ 4 أعمدة الأساسية للـ OOP

| العمود | المعنى |
| --- | --- |
| Encapsulation | تغليف الداتا والأوامر مع بعض |
| Abstraction | إخفاء التفاصيل وإظهار بس اللي مهم |
| Inheritance | الوراثة من parent object |
| Polymorphism | نفس الأمر بيتصرف بشكل مختلف حسب الـ object |

### Inheritance: Classes vs Prototype

**Classes:**

- الـ object بياخد الخصائص **copy-paste** حرفياً
- لو مسحت الـ parent → اللي عندك بيفضل معاك

**Prototype-based (اللي بتستخدمه JS):**

- في الـ object property مخفية اسمها `[[Prototype]]`
- لو عملت object تاني من نفس النوع → بيكون عنده **وصلة** للـ prototype ده
- لو حاجة في الـ prototype اتغيرت → **الكل اتغير**
- أكثر كفاءة في الـ memory

> ليه اسمها **Object-Oriented**؟ زمان كنت بفكر "أنا هدي مين الأوامر دي؟" — دلوقتي بفكر "أنا هعمل الأوامر دي **على** الـ object ده."
> 

---

## 3. Multi-paradigm

الـ **Paradigm** = الطريقة اللي بتكتب بيها الـ logic بتاعك.

JS بتدعم الـ 3 بالراحة:

- ✅ **Imperative**
- ✅ **Functional Programming**
- ✅ **OOP**

---

## 4. Interpreted or Just-In-Time Compiled

### قبل الترجمة: الـ AST

الكود بيمر بتحليل وبيتحول لـ **Abstract Syntax Tree (AST)** — ده اللي بيحدد:

- أسماء الـ variables
- اتجاه الكود
- أولوية الـ operators
- الـ Data types

### الـ JIT Compiled

1. الـ **Interpreter** يترجم الكود كله ويحوله لـ **Bytecode** (لغة قريبة من الـ machine code)
2. ده بيخلي الصفحة تفتح للـ user على طول ✅ (مفيد للـ Lighthouse Performance)
3. بعدها في **مراقب** — لو لقى كود **hot** (= متكرر زي الـ for loop):
    - بيرجع يترجمه تاني كـ **machine code** على طول ويبعته للـ CPU طلقة 🚀
    - ده اسمه **Optimization**
4. لو المراقب لقى إن الـ function اللي كانت بتاخد `string` بقت بتاخد `number`:
    - بيلغي الـ optimization → **Deoptimization**
    - بيرجع الكود للـ interpreter

---

## 5. Dynamic

يعني **Loosely Typed Language** — مش لازم تحدد نوع الـ variable وإنت بتكتبه.

وده اللي بيخليها هي اللي بتدير الـ **memory management** بدالك.

---

## 6. Single-Threaded

الـ JS Engine بينفذ **سطر واحد بس** في نفس الوقت.

> "طب مش هيحصل وقفة لما يلاقي async code؟" → مش لازم، وده بسبب الـ **Event Loop** 👇
> 

---

## 7. Garbage-Collected

الـ **Garbage Collector (GC)** هو اللي بيمسح الحاجات اللي ملهاش reference في الـ heap علشان يوفر مساحة.

### Memory Life Cycle

1. **Allocate** — الحجز في الـ memory (بيحصل في مرحلة الـ Creation)
2. **Usage** — الاستخدام أثناء الـ running
3. **Release** — التحرير عن طريق الـ GC أو الـ stack

---

## 8. JS Engine — Stack & Heap

### الـ Stack

- بيخزن الـ **primitives** وينفذ الكود
- اسمه **Call Stack** لما بينفذ
- بيمسح الداتا بعد ما يخلص

### الـ Heap

- بيخزن الـ **objects**
- كل object ليه **hash** وليه **reference (pointer)** في الـ stack
- الـ GC هو اللي بيمسح منه لو ملقيش pointer

> كل حاجة في JS ما عدا الـ primitives هي **object** — حتى الـ Array والـ Function! الـ Array هي object بس بتستخدم indexes، والـ Function هي object بس ليها حته الـ calling وبتنفذ كود.
> 

---

## 9. Execution Context (EC)

الـ EC هو **سياق التنفيذ** — بيتخزن فيه الـ primitives والـ object references.

### مرحلتين:

**أولاً: Creation Phase** (بناءً على الـ AST)

- بيتم فيها الـ **Hoisting**
- حجز الـ data في الـ memory
- مثال: `const Ahmed = 10` → بيحجز اسم Ahmed وبيخلي قيمته فاضية الأول

**ثانياً: Execution Phase**

- بيساوي القيم الفعلية

> ليه كده؟ علشان لو حجز وبعدين نفذ → اللغة بتبقى بطيئة جداً. الـ separation ده بيساعد في الأداء.
> 

### الـ EC بيتكون من 3 حاجات:

1. **Variable Environment Object (VEO)**
2. **Scope Chain**
3. **this**

---

## 10. Variable Environment Object (VEO)

هو object كبير جواه:

- الـ variables
- الـ arguments object (للـ regular functions)
- الـ `this` للـ execution context
- الـ `if` / `for` لو regular function

**قبل ES6:** كل حاجة كانت في **Object Environment Record** → يعني كل variable بيتسجل كـ property في الـ object → لو global مش هيتمسح → مشاكل كبيرة.

**بعد ES6:** ظهر **Declarative Record** منفصل:

- شايل الـ `let` و `const`
- مش ظاهر للـ window object
- مش بيعمل hoisting بنفس طريقة الـ `var`

---

## 11. Scope & Scope Chain

### الـ Scope

الـ scope هو اللي بيخلي الداتا تتخزن في VE خاصة بيها وتتحبس جواها.

**أنواع الـ Scope:**

- **Global Scope** — بره كل حاجة
- **Function Scope** — جوه function
- **Block Scope (ES6+)** — جوه `if` / `for` وكده

### Lexical Scoping

الـ scope بيتحدد **وإنت بتكتب** — مش وقت التنفيذ. الـ AST هو اللي بيمسك ده.

### الـ Keywords

| Keyword | Block Scope | Redeclare | Window Object |
| --- | --- | --- | --- |
| `var` | ❌ (function scope بس) | ✅ | ✅ |
| `let` | ✅ | ❌ | ❌ |
| `const` | ✅ | ❌ | ❌ |

> `var` اتبنت قبل الـ block scope وملقيوش حل فيها علشان مبدأ **"Don't Break the Web"**.
> 

### الـ Scope Chain

- موجودة في الـ **Heap**
- شايلة كل الـ VEs
- لو variable مش موجود في الـ scope الحالي → بيطلع للـ parent → لحد الـ global → ملقاش → **ReferenceError**
- بيدور للـ أعلى بس (مش لتحت) — لأن الكود بيتنفذ من فوق لتحت

---

## 12. Hoisting & TDZ

### Hoisting

الحجز في الـ memory قبل التنفيذ (= مرحلة الـ Creation).

- **Function declaration** → بتتحجز كاملة في الـ heap ← اقدر اعملها call قبل ما أوصلها ✅
- **`var`** → بيتحجز الاسم بس، القيمة = `undefined` ⚠️
- **`let` / `const`** → بيتحجزوا بس من غير قيمة خالص (مش حتى `undefined`)

### Temporal Dead Zone (TDZ)

المنطقة من **بداية الـ scope** لحد **سطر التعريف** — لو لمست الـ variable فيها:

```
ReferenceError: Cannot access before initialization
```

---

## 13. this Keyword

- **Special variable** بتشاور على الـ object اللي بتشتغل فيه
- لكل EC ليها `this` خاصة

| النوع | الـ this |
| --- | --- |
| Regular Function (method call `obj.fun()`) | بيشاور على الـ object |
| Regular Function (simple call `fun()`) | بيشاور على الـ window / undefined |
| Arrow Function | بياخد الـ `this` من الـ parent (lexical this) |

> Arrow function مش عندها `this` خاص بيها ولا **arguments object**.
> 

### الـ Arguments Object

- object شايل كل الـ arguments في array-like structure
- اقدر أعد الـ `length` بس مش اقدر أعمل `.map()`
- الـ Arrow function ملهاش ❌

---

## 14. Memory Management

### Memory Life Cycle

1. **Allocate** — الحجز
2. **Usage** — الاستخدام
3. **Release** — التحرير

### الـ Stack vs Heap

- **Stack:** بيمسح الـ primitives والـ references أوتوماتيك لما الـ EC يخلص
- **Heap:** الـ GC هو اللي بيمسح لما يشوف object ملهوش pointer

### الـ Global Scope والـ Window

أي حاجة في الـ global scope (`var` قديم) مش بتتمسح لحد ما الـ tab يتقفل — لأن الـ window object شايلها.

---

## 15. First-Class Functions

يعني الـ function بتتعامل كـ **مواطن درجة أولى**:

- ✅ اقدر أخزنها جوه variable
- ✅ اقدر أحطها كـ argument
- ✅ اقدر أرجع function من function

**ليه بتتعامل كده؟**

علشان زي ما شرحنا في القسم 8 — في JS كل حاجة ما عدا الـ primitives هي **object**. والـ function مش استثناء، هي object بس من نوع خاص بيه حتة الـ calling وبينفذ كود.

وعلشان هي object، بتتخزن في الـ **Heap** زي أي object تاني، ومعاها **reference (pointer)** في الـ stack. ده بالظبط اللي بيخلي JS تقدر تعامل الـ function كـ value عادية — تحطها في variable، تبعتها كـ argument، أو ترجعها من function تانية — لأن في الآخر هي مجرد **pointer لـ object في الـ heap**.

> ده السبب اللي بيخلي مفاهيم زي الـ callbacks والـ higher-order functions شغالة في JS بشكل طبيعي جداً.
> 

---

## 16. Non-blocking Event Loop (Concurrency Model)

### مكونات الـ Runtime

```
┌─────────────────┐    ┌──────────────┐
│   JS Engine     │    │   Web APIs   │
│  ┌───────────┐  │    │              │
│  │Call Stack │  │───>│ setTimeout   │
│  └───────────┘  │    │ DOM events   │
│  ┌───────────┐  │    │ fetch        │
│  │   Heap    │  │    └──────┬───────┘
│  └───────────┘  │           │
└─────────────────┘           ▼
                    ┌──────────────────┐
                    │  Callback Queue  │
                    └────────┬─────────┘
                             │
                    ┌────────▼─────────┐
                    │   Event Loop     │
                    │ (لو Stack فاضي  │
                    │  بيطلع من Queue │
                    │  وبيحطه في Stack)│
                    └──────────────────┘
```

### إزاي بيشتغل؟

1. الـ JS Engine لو لقى حاجة زي DOM أو setTimeout → بيبعتها للـ **Web APIs**
2. لما تخلص → بيتحطها في الـ **Callback Queue**
3. الـ **Event Loop** بيراقب — لو الـ Call Stack فاضي → بيطلع الـ callback ويحطه في الـ stack
4. بالتالي الـ UI مش بيقف ✅ → **Non-blocking**

---

## ملخص: شرح الجملة كلها

> *JavaScript is a high-level, prototype-based object-oriented, multi-paradigm, interpreted or just-in-time compiled, dynamic, single-threaded, garbage-collected programming language with first-class functions and a non-blocking event loop concurrency model.*
> 

| **المصطلح** | **المعنى ببساطة** |
| --- | --- |
| High-level | قريبة من البشر، بعيدة عن الـ hardware، بتدير الـ memory بنفسها |
| Prototype-based OOP | بتدعم الـ objects عن طريق الـ prototype chain مش الـ classes الكلاسيكية |
| Multi-paradigm | تقدر تكتب Imperative أو Functional أو OOP |
| JIT Compiled | بتترجم لـ bytecode أول، وبعدين لـ machine code للكود الـ hot |
| Dynamic | Loosely typed — مش لازم تحدد نوع الـ variable |
| Single-threaded | بتنفذ سطر واحد في نفس الوقت |
| Garbage-collected | بتمسح الـ objects اللي ملهاش reference أوتوماتيك |
| First-class functions | الـ function مواطن درجة أولى — تخزن، تبعت، ترجع |
| Non-blocking Event Loop | الـ async code مش بيوقف الـ UI عن طريق الـ Web APIs والـ Callback Queue |
