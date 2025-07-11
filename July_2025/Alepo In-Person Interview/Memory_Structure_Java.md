
# 🧠 Java Memory Structure (JVM Memory Model)

 ✅ **Short Answer (Interview-Friendly):**

Java memory is mainly divided into **five parts** inside the **JVM (Java Virtual Machine)**:

1. **Heap Area**
2. **Stack Area**
3. **Method Area (a.k.a. MetaSpace in Java 8+)**
4. **Program Counter (PC) Register**
5. **Native Method Stack**

<img width="717" height="238" alt="JVM_Basic_Diagram_of_Memory" src="https://github.com/user-attachments/assets/b210b734-1d14-4cda-8823-a17d9486e040" />


 ✅ **Detailed Breakdown:**

 🟩 1. **Heap Memory (Runtime data area)**

* 📍 **Stores:** Objects and their instance variables (`new` objects go here)
* 💡 Shared across all threads
* 🧹 Managed by the **Garbage Collector**
* 🎯 Used for:

  * `new Student("Tom",101)`
  * `new String("abc")` (outside String pool)

#### 🔸 String Pool:

* A **special part inside heap** where **string literals** are stored.

```java
String s = "hello"; // goes to string pool
```

---

### 🟦 2. **Stack Memory**

* 📍 **Stores:** Method calls, local variables, and function parameters
* 🧵 Each **thread has its own stack**
* 📌 Grows/shrinks as methods are called/returned
* ⚠️ Objects are **not** stored here — only references to objects

```java
int x = 10;          // stored in stack
Student s = new Student();  // 's' reference in stack, actual object in heap
```

---

### 🟨 3. **Method Area (or MetaSpace)**

* 📍 Stores:

  * Class metadata (like class name, method names, variable names)
  * Static variables
  * Constant pool (e.g. `final int x = 10`)
* 📦 Shared across threads
* 🔄 From Java 8 onward, **MetaSpace replaced PermGen** area and it grows outside the heap (in native memory)

---

### 🟥 4. **Program Counter Register (PC Register)**

* 📍 Holds the address of the **current instruction** being executed in the method.
* 🧵 Each thread has its own PC register.

---

### 🟫 5. **Native Method Stack**

* 📍 Used for **native methods** (written in C/C++ and called via JNI - Java Native Interface)
* Rarely used in typical Java apps unless you're using native libraries.

---

### 🧠 Summary Table:

| Memory Area                 | Stores                         | Shared/Per Thread |
| --------------------------- | ------------------------------ | ----------------- |
| **Heap**                    | Objects (via `new`)            | Shared            |
| **Stack**                   | Local vars, method calls, refs | Per thread        |
| **Method Area / MetaSpace** | Class-level info, static vars  | Shared            |
| **PC Register**             | Instruction address            | Per thread        |
| **Native Method Stack**     | Native method data             | Per thread        |

---

### ✅ Example Code with Memory Mapping:

```java
public class Main {
    public static void main(String[] args) {
        int x = 10;                        // in Stack
        String s = "hello";               // "hello" in String pool (Heap)
        Student st = new Student();       // 'st' in Stack, object in Heap
    }
}
```

---

### Interviewer Follow-Ups to Prepare:

1. ✅ Where is `static` data stored?
   → In **Method Area**

2. ✅ Where are `final` constants stored?
   → In **Method Area (Constant Pool)**

3. ✅ What’s the difference between Heap and Stack?
   → Heap stores objects (shared), Stack stores methods/local variables (per-thread)

4. ✅ What happens when memory is full in heap?
   → **OutOfMemoryError** and GC might try to clean memory before that

---

### 🔚 Final Answer (Summary):

> Java memory is divided into five parts: **Heap, Stack, Method Area (MetaSpace), PC Register, and Native Method Stack.**
>
> * Heap stores objects
> * Stack stores method calls and local variables
> * Method Area stores class metadata and static members
> * PC register tracks current instruction
> * Native Method Stack is used by native code
>   Together, they form the **JVM memory model**.

---

# My Doubts and answer of it :
Let’s break this down step by step, covering each of your questions 🔍:

---

## 1. Heap memory allocated at runtime?

✅ **Yes** — The JVM allocates heap space **dynamically at runtime** when `new` is called or certain data structures are initialized.

## 2. Is stack memory assigned at compile time?

☑️ **Partially true** — The layout (e.g. which methods, local variables) is fixed at **compile time**, but **actual allocation** of frames happens **at runtime** when methods are called.

---

## 3. Is the String Pool part of Heap?

✅ **Yes** — It’s a specialized **area within the heap** for storing string literals and interned strings. So you can treat it as a subset of the heap in memory diagrams.

---

## 4. Difference between String Pool and Constant Pool

| Pool Type         | Location                | Contents                                                  |
| ----------------- | ----------------------- | --------------------------------------------------------- |
| **String Pool**   | Heap (String Pool area) | String literals (`"hello"`) and interned strings          |
| **Constant Pool** | Method Area             | Constants from classes: literal strings, final primitives |

---

## 5. Metaspace, PermGen, NewGen vs OldGen

| Java Version | Class Metadata Storage    | GC Generations in Heap                                |
| ------------ | ------------------------- | ----------------------------------------------------- |
| Java ≤ 7     | PermGen (in JVM memory)   | Young Generation (Eden + Survivor), Old Generation    |
| Java ≥ 8     | Metaspace (native memory) | Same Heap structure: NewGen (Eden → Survivor), OldGen |

* **PermGen** was removed in Java 8 and replaced by **Metaspace**.
* **New Gen (Young)**: where most new objects are allocated and GC’ed quickly.
* **Old Gen**: long-lived objects moved here after surviving multiple GCs.

---

## 6. Where are static blocks, variables, methods, and class structures stored?

All of the following live in the **Method Area / Metaspace**:

* Class metadata (structure, methods, fields)
* `static` variables
* `final static` constants (in the Constant Pool)
* Bytecode for **`static`, `public`, `final`** or any method

---

## 7. Code Examples with Memory Mapping

### a) Variables

```java
public class VarTypes {
    static int staticVar = 100;              // Method Area
    static final int CONST_VAR = 200;        // Method Area > Constant Pool
    final int instanceFinal = 300;           // Heap, inside each object

    public void method() {
        int localVar = 10;                   // Stack (in method frame)
        final int localFinalVar = 20;        // Stack too
    }
}
```

### b) Methods

```java
public class MethodTypes {
    public void instanceMethod() {}           // Bytecode in Method Area
    public static void staticMethod() {}     // Bytecode in Method Area
    public final void finalMethod() {}       // Bytecode in Method Area
}
```

### c) Nested Classes

```java
public class ClassTypes {
    static class StaticNested {}            // Bytecode in Method Area/Metaspace
    final class Inner { }                   // Bytecode in Method Area
}
```

### d) Threads

```java
public class ThreadExample {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {});  // `t1` reference on Stack, new Thread object in Heap
        Thread t2 = new Thread(() -> {});
        t1.start();
        t2.start();
    }
}
```

* **Heap**: `Thread` objects, Runnable lambdas
* **Stack (per thread)**: `t1`, `t2` references in `main()` — each OS thread will also have its own call stack
* **Method Area/Metaspace**: Bytecode and class info for `ThreadExample`, `Thread`, Runnable

---

### 🧠 Summary: Where Everything Lives

| Component                       | Memory Location                         |
| ------------------------------- | --------------------------------------- |
| `new` object                    | Heap                                    |
| String literal (`"hi"`)         | String Pool (Heap)                      |
| Primitive variable              | Stack (inside method frame)             |
| Local object reference          | Stack                                   |
| `static` / `final static`       | Method Area / Metaspace / Constant Pool |
| Class structure (methods, etc.) | Method Area / Metaspace                 |
| Thread objects, lambdas         | Heap                                    |
| Object references (`t1`, etc.)  | Stack                                   |

---

### ✅ TL;DR

1. Heap: dynamic objects + String pool
2. Stack: method frames, local vars & references
3. Method Area/Metaspace: class structures + static/final variables + bytecode
4. PC Registers & Native stacks: per thread JVM bookkeeping
5. NewGen vs OldGen: memory regions within Heap for GC

---

# Additional 

Great follow-up question! Let’s break down the difference between **`StackOverflowError`** and **`OutOfMemoryError`**, as they are **common JVM runtime errors** — and interviewers love to ask this. 🔥

---

## 🔴 1. **`StackOverflowError`**

### 🧠 What it is:

Occurs when the **stack memory** is **exhausted** — typically due to **deep or infinite recursion**, or too many nested method calls.

### 🛠️ Example:

```java
public class StackOverflowExample {
    public static void recursiveCall() {
        recursiveCall(); // infinite recursion
    }

    public static void main(String[] args) {
        recursiveCall(); // 💥 StackOverflowError
    }
}
```

### 💡 Why this happens:

* Each method call adds a **new frame** to the **thread’s call stack**.
* Stack has limited size (e.g. 512KB to 1MB by default per thread).
* Too many calls cause the stack to **overflow**.

### 🧩 Error Message:

```
Exception in thread "main" java.lang.StackOverflowError
```

---

## 🔴 2. **`OutOfMemoryError`**

### 🧠 What it is:

Occurs when the **Java heap space** or **Metaspace** or **other memory regions** are exhausted, and **GC cannot reclaim enough memory**.

### 🛠️ Example 1: Heap space full

```java
import java.util.*;

public class OOMExample {
    public static void main(String[] args) {
        List<int[]> list = new ArrayList<>();
        while (true) {
            list.add(new int[1_000_000]); // keeps filling heap
        }
    }
}
```

### 🛠️ Example 2: Metaspace full (Java 8+)

```java
// Frameworks like Spring with excessive class loading can exhaust Metaspace
```

### 💡 Why this happens:

* Heap is full → GC tries → still can't reclaim → throws `OutOfMemoryError`.
* Can happen in:

  * Heap (`java.lang.OutOfMemoryError: Java heap space`)
  * Metaspace (`java.lang.OutOfMemoryError: Metaspace`)
  * Direct memory (e.g., NIO buffers)

---

## 🔍 Comparison Table

| Feature       | `StackOverflowError`                                    | `OutOfMemoryError`                                        |
| ------------- | ------------------------------------------------------- | --------------------------------------------------------- |
| Memory Type   | **Stack memory** (per thread)                           | **Heap**, **Metaspace**, or **native memory**             |
| Common Cause  | Infinite recursion, too many method calls               | Too many objects, excessive memory usage                  |
| Recovery      | Hard to recover — JVM halts current thread              | Can sometimes recover, depending on GC                    |
| Trigger Point | Stack frame exceeds thread limit                        | Memory allocation fails                                   |
| Fix           | Optimize recursion, tail recursion, increase stack size | Reduce memory usage, fix memory leaks, increase heap size |

---

## 🔧 JVM Options (for tuning)

| Option                      | Description                   |
| --------------------------- | ----------------------------- |
| `-Xss512k`                  | Set stack size per thread     |
| `-Xms512m -Xmx1024m`        | Set initial and max heap size |
| `-XX:MaxMetaspaceSize=256m` | Limit Metaspace (Java 8+)     |

---

## ✅ Summary (Interview Answer Style):

> A **`StackOverflowError`** occurs when the **call stack exceeds its size limit**, typically due to **infinite or deep recursion**.
> An **`OutOfMemoryError`** happens when **JVM memory (heap or metaspace)** is exhausted and GC can't reclaim space.
> One is stack-related, the other is heap/meta-related.

---

Let me know if you'd like:

* A **visual memory diagram** of both errors 🔍
* A **code debugging walkthrough**
* Or to move to the next interview topic ✅

