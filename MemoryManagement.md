# 📘 Java Memory Management & Garbage Collection – Complete Structured Notes

---

## ✅ 1. What is Memory Management in Java?

### 🔹 **Introduction (Detailed)**

Memory management in Java refers to how the **JVM allocates**, **uses**, and **reclaims** memory automatically to run Java applications efficiently and safely.

Unlike languages like C/C++, Java abstracts away manual memory deallocation (`free()`), thanks to:

* **Automatic garbage collection (GC)**
* **Object reachability tracking**
* **Efficient heap and stack separation**

Java memory management aims to:

* Minimize memory leaks
* Maximize throughput and responsiveness
* Avoid crashes due to `OutOfMemoryError`

It manages:

* **Object allocation**
* **Lifetime tracking**
* **Automatic cleanup**

➡️ Java uses a combination of stack memory (for execution) and heap memory (for object storage), orchestrated via the **JVM Memory Model**.

---

## ✅ 2. JVM Memory Areas

### 🔹 **Categorized and Detailed**

The JVM splits memory into logical sections. These are created when the JVM starts and can be tuned via JVM options.

---

### 🧠 **Heap Memory** (GC Managed)

| Section                      | Description                                                       |
| ---------------------------- | ----------------------------------------------------------------- |
| **Young Generation**         | Stores **newly created objects**                                  |
| - Eden Space                 | Objects are allocated here first                                  |
| - Survivor Spaces (S0/S1)    | Objects that survive GC are moved here                            |
| **Old Generation (Tenured)** | Stores **long-lived objects** promoted from Young Gen             |
| **Metaspace** *(Java 8+)*    | Stores class metadata. Grows with class loading. Replaces PermGen |

🔸 JVM Option Examples:

* `-Xms512m` → Initial heap size
* `-Xmx1024m` → Max heap size

---

### 📚 **Stack Memory** (Thread-local)

* Each thread gets a **separate stack**.
* Stores:

  * Method frames (activation records)
  * Local variables
  * Primitive data
  * Object references (but not the object itself)

📝 Stack is **automatically cleaned** as methods return.

---

### 🖥 **Other Memory Areas**

| Area                    | Purpose                                                      |
| ----------------------- | ------------------------------------------------------------ |
| **PC Register**         | Keeps track of current instruction being executed per thread |
| **Native Method Stack** | Handles native (non-Java) method calls (e.g., via JNI)       |

---

## ✅ 3. Java Object Lifecycle

### 🔹 **Detailed Lifecycle Flow**

```
Object created → Eden Space → Survivor Spaces → Old Generation → Garbage Collected
```

### 🔄 Object Creation and Promotion:

| Phase         | Description                                   |
| ------------- | --------------------------------------------- |
| **Creation**  | `new` keyword allocates object in Eden        |
| **Survival**  | Survives minor GC → moved to Survivor space   |
| **Promotion** | If survives multiple GCs, promoted to Old Gen |
| **Death**     | Object becomes unreachable → collected by GC  |

✅ GC uses **reference chains** from **GC roots** to determine reachability.

---

## ✅ 4. Memory Leaks in Java

### 🔹 **What is a Memory Leak?**

A **memory leak** in Java occurs when **unused objects are not garbage collected** because they are **still referenced** — even though they are no longer needed.

---

### 🔍 **Common Causes of Memory Leaks**

| Cause                 | Description                                      |
| --------------------- | ------------------------------------------------ |
| ❗ Static References   | Static fields hold references longer than needed |
| ❗ Large Collections   | Objects added to a collection and never removed  |
| ❗ Event Listeners     | Registered listeners not unregistered            |
| ❗ ThreadLocal Misuse  | Failing to remove ThreadLocal values             |
| ❗ Cache Mismanagement | No eviction policy, growing unbounded            |

---

### 🛠️ **Solutions**

* Always **remove event listeners**
* Use **WeakHashMap**, `WeakReference` for caching
* Set object to `null` if not needed
* Use **try-with-resources** for I/O and DB connections
* Profile heap regularly using:

  * VisualVM
  * Java Flight Recorder
  * Eclipse MAT

---

## 🗑️ GC SECTION – GARBAGE COLLECTION

---

## ✅ 5. Introduction to Garbage Collection (GC)

Garbage Collection in Java is an **automatic memory management** process that:

* Identifies objects **no longer in use**
* **Reclaims memory** to avoid leaks and `OutOfMemoryError`
* Helps ensure high availability and performance

> GC relieves the developer from manual memory deallocation.

GC works in the **heap area** and is highly tunable.

---

## ✅ 6. GC Eligibility – When is an Object Ready for Collection?

### 🔹 **Rules for GC Eligibility**

An object becomes eligible for GC if it is **no longer reachable** from any **GC Root**.

#### 🔹 GC Roots include:

* Local variables in the stack
* Static variables
* Active thread references
* JNI references

---

### ✅ **Examples of GC Eligibility**

```java
Object obj = new Object();
obj = null; // Eligible
```

```java
void method() {
    Object temp = new Object(); // Eligible after method ends
}
```

---

## ✅ 7. Stages of Garbage Collection

| Stage       | Description                                                |
| ----------- | ---------------------------------------------------------- |
| **Mark**    | Find all reachable objects (GC Roots → reachability graph) |
| **Sweep**   | Remove all unreachable objects from memory                 |
| **Compact** | Defragment heap by moving objects together (optional)      |
| **Promote** | Move surviving objects to Old Generation                   |

⚠️ Not all collectors do compaction.

---

## ✅ 8. Types of Garbage Collectors

### 🔹 1. **Serial GC**

* Single-threaded collection
* Best for small apps (e.g., CLI tools)
* `-XX:+UseSerialGC`

---

### 🔹 2. **Parallel GC (Throughput Collector)**

* Multi-threaded GC for Young and Old Gen
* Focus on throughput, longer pause times
* `-XX:+UseParallelGC` (Java 8 default)

---

### 🔹 3. **CMS (Concurrent Mark-Sweep)** *(Deprecated in Java 14)*

* Concurrently collects Old Gen
* Low pause time, but fragmentation issues
* `-XX:+UseConcMarkSweepGC`

---

### 🔹 4. **G1 GC (Garbage First)** 🧠 (Java 9+ Default)

* Heap divided into small **regions**
* Mixed collections (Young + Old Gen)
* Predictable pause times
* `-XX:+UseG1GC`

---

### 🔹 5. **ZGC (Java 11+)**

* Pause time < 10ms, even on large heaps
* Collects heap concurrently
* `-XX:+UseZGC`

---

### 🔹 6. **Shenandoah (Java 12+)**

* Low-pause GC developed by RedHat
* Concurrent compaction
* `-XX:+UseShenandoahGC`

---

## 🔧 Bonus: Useful JVM GC Options

```bash
-Xms512m                     # Initial heap size
-Xmx1024m                    # Max heap size
-XX:+UseG1GC                 # Use G1 collector
-XX:+PrintGCDetails          # Print GC logs
-XX:+HeapDumpOnOutOfMemoryError
-XX:SurvivorRatio=8          # Eden:Survivor space ratio
```

---

## ✅ Summary: Memory Management + GC Flow

```
[Object Created] → Eden → Survivor → Old Gen → GC Mark → Sweep → Compact → Free Memory
```

* GC checks **reachability**
* GC reclaims **heap memory**
* GC types optimized for:

  * **Throughput** (Parallel)
  * **Responsiveness** (CMS, G1)
  * **Low latency** (ZGC, Shenandoah)

---
