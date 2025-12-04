
**Embedded C++ vs regular differences**
--------------------------------------------------------------
- **only 64KB of RAM, every microsecond counts** (Unlike traditional with GB of RAM, fast CPU)

## 🎯 INTERVIEW TALKING POINTS (Memorize These)

### On Embedded C++:

> "I understand embedded C++ has unique constraints. You avoid exceptions because they're non-deterministic - you can't predict timing. Dynamic allocation is dangerous due to fragmentation and unpredictable allocation time. Instead, you use static allocation, RAII for resource management, and templates for zero-cost abstraction."

### On MISRA:

> "MISRA C++ is a coding standard for safety-critical systems. It's designed to avoid undefined behavior and ensure code is analyzable. It's enforced by static analysis tools like Coverity and is required for DO-178C certification."

### On DO-178C:

> "DO-178C defines development rigor based on failure criticality. DAL A is for catastrophic failures like flight controls - requiring complete requirements traceability and MC/DC coverage. Lower DAL levels have proportionally less stringent requirements."

### On Your C++ Experience:

> "My recent work has been Python and Java, but I have C++ foundation from university and my Northrop internship where I built an ANSYS automation tool. My strong Java OOP background - inheritance, polymorphism, design patterns - transfers directly to C++. I've been studying embedded-specific constraints like MISRA guidelines and deterministic memory management."

### On Deterministic vs Non-Deterministic

| Deterministic                          | Non-Deterministic                |
| -------------------------------------- | -------------------------------- |
| Same input → Same timing → Same output | Timing can vary unpredictably    |
| Bounded loops (for i < 100)            | Unbounded loops (while (!ready)) |
| Static allocation                      | Dynamic allocation (new)         |
| Iteration                              | Recursion (stack depth varies)   |
| Error codes                            | Exceptions                       |


> "Deterministic code has predictable, analyzable timing - same input always produces same timing and output. Non-deterministic code has variable timing that can't be predicted at design time. Examples of non-deterministic: unbounded loops, recursion, dynamic allocation, exception handling."

#### **FORBIDDEN C++ FEATURES**
- **1) Exceptions (try/catch)** ❌
```
// ❌ NEVER DO THIS IN EMBEDDED
try {
    riskyFunction();
} catch (const std::exception& e) {
    // handle error
}
```
- *Non deterministic timing, you cant predict how long exception handling takes*
- *code bloat (exception tables are big code size), MISRA violations BANNED*
- *Use error codes instead*
- **Error Codes ✅**
```
enum class Status { OK, ERROR_TIMEOUT, ERROR_INVALID };

Status safeFunction() {
    if (problem) return Status::ERROR_TIMEOUT;
    return Status::OK;
}

// Caller checks the return
if (safeFunction() != Status::OK) {
    handleError();
}
```
- ***Exceptions are non-deterministic - you can't predict when they'll occur or how long stack unwinding takes. In hard real-time systems where you need to guarantee response within microseconds, that's unacceptable. We use error codes and explicit status returns instead.***

-  **2) Dynamic Memory** ❌
```
// ❌ NEVER DO THIS IN EMBEDDED
int* ptr = new int[1000];  // Heap allocation
delete[] ptr;
```
- *Heap fragmentation - running for long time is hard on the memory*
- *Non deterministic timing - allocation time varies too much*
- *Memory exhaustion - no garbage collector so can run out*
- *No way to recover - if **<font color="#ffff00">new</font>** fails in flight, then what?*
```
// Option 1 ✅: Static arrays (compile-time, predictable)
static int buffer[1000];

// Option 2 ✅: Memory pools (pre-allocated chunks)
class MemoryPool {
    static uint8_t pool[POOL_SIZE];
    // All memory allocated at startup, never freed
};

// Option 3 ✅: Placement new (construct in existing memory)
alignas(MyClass) uint8_t storage[sizeof(MyClass)];
MyClass* obj = new (storage) MyClass();  // No heap!
```
- **dynamic allocation causes fragmentation over long runtimes and has unpredictable timing, we use static allocation, everythings allocated at compile time or startup, if system runs for 10 years memory is still predictable**

- **3) RTTI (Run Time Type Information) ❌**
```
// ❌ AVOID IN EMBEDDED
if (dynamic_cast<DerivedClass*>(basePtr)) {
    // ...
}
typeid(obj).name();  // Also RTTI
```
- Code size overhead - type tables for every class
- Runtime cost - Looking up type info
-✅ What to use instead: Compile-time polymorphism
```
// Templates - resolved at compile time, zero runtime cost
template<typename T>
void process(T& obj) {
    obj.doSomething();  // Duck typing - if it has the method, it works
}
```
- RTTI adds code size overhead and runtime cost, if runtime types need checking it means design is not optimized, use templates for compile time polymorphism for zero overhead

### ✅ RAII (Resource Acquisition Is Initialization)

This is GOLD for embedded. Resources are tied to object lifetime.

```
class MutexGuard {
    Mutex& mutex;
public:
    MutexGuard(Mutex& m) : mutex(m) {
        mutex.lock();      // Acquire in constructor
    }
    ~MutexGuard() {
        mutex.unlock();    // Release in destructor - GUARANTEED!
    }
};

void safeFunction() {
    MutexGuard guard(myMutex);  // Lock acquired
    // ... do work ...
    
    if (error) return;  // Early return? Destructor STILL runs!
    
}  // Destructor called automatically - unlock guaranteed
```

Why this is brilliant:

- Resource cleanup guaranteed even on early return

- Impossible to forget to unlock

- Compiler optimizes away the overhead

Interview soundbite:

> "RAII ties resource lifetime to object lifetime. When the object goes out of scope, the destructor runs automatically - even on early returns. It's impossible to forget to release a resource. This is perfect for embedded where forgetting to unlock a mutex could cause deadlock."


**Why Embedded C++ instead of C**
--------------------------------------------------------------
- Object Oriented - can organize complex systems
- Better Type safety
- RAII - aka resources automatically clean up (huge for safety)
- Templates available + industry standard

### Example MISRA Rules:

```
// Rule 0-1-1: No unreachable code
if (true) { /* ok */ }
else { /* VIOLATION - can never execute */ }

// Rule 5-0-15: Array bounds
int arr[10];
arr[10] = 5;  // VIOLATION - out of bounds (0-9 valid)

// Rule 6-4-2: switch must have default
switch (x) {
    case 1: break;
    // VIOLATION - missing default
}

// Rule 15-0-2: No goto
goto label;  // VIOLATION

// Rule 18-0-5: Use bounded string functions
strcpy(dest, src);   // VIOLATION - could overflow
strncpy(dest, src, sizeof(dest));  // OK - bounded
```

Interview soundbite:

> "MISRA is a coding standard for safety-critical C++. It's designed to eliminate undefined behavior and make code analyzable. Every switch needs a default case, no goto, no unbounded string functions. It's enforced by static analysis tools and required for DO-178C certification."

### DO-178C?

The FAA/EASA standard for avionics software certification.
Full name: "Software Considerations in Airborne Systems and Equipment Certification"
If your software flies on an aircraft, it needs DO-178C certification.

### DAL Levels (Design Assurance Levels)

DAL = How catastrophic is failure?

|Level|Failure Effect|Example System|Rigor Level|
|---|---|---|---|
|DAL A|Catastrophic (loss of aircraft)|Flight controls, autopilot|Extreme|
|DAL B|Hazardous (serious injury)|Engine controls|Very High|
|DAL C|Major (workload impact)|Navigation display|High|
|DAL D|Minor (inconvenience)|Passenger WiFi management|Moderate|
|DAL E|No safety effect|In-flight entertainment|Basic|

### What Higher DAL Means:

DAL A Requirements
- 100% requirements traceability (requirement → design → code → test)
- MC/DC coverage (Modified Condition/Decision Coverage)
- Independent verification (different team reviews)
- Extensive documentation
- Every single line of code scrutinized
DAL E: Basic requirements, standard testing.

## 6️⃣ MEMORY MANAGEMENT FOR EMBEDDED

### Stack vs Heap
| Stack ✅          | Heap ❌                          |                    |
| ---------------- | ------------------------------- | ------------------ |
| Timing           | Deterministic                   | Non-deterministic  |
| Fragmentation    | None                            | Risk over time     |
| Size             | Limited (watch overflow!)       | Flexible           |
| Use case         | Local variables, function calls | Dynamic allocation |
| Embedded verdict | ✅ Preferred                     | ❌ Avoid            |
## 7️⃣ DETERMINISM PRINCIPLES

### What is Deterministic Behavior?

Deterministic = Same input → Same timing → Same output
Every. Single. Time.
In hard real-time systems, you must guarantee response time. If the system needs to respond in 10ms, it must respond in 10ms - not 10ms sometimes and 50ms other times.
### Determinism Checklist:

| ❌ Non-Deterministic              | ✅ Deterministic                   |
| -------------------------------- | --------------------------------- |
| Unbounded loops                  | Bounded loops with max iterations |
| Recursion (stack depth varies)   | Iteration (fixed stack)           |
| Dynamic allocation               | Static allocation                 |
| Exception handling               | Error codes                       |
| Virtual functions in tight loops | CRTP or direct calls              |
Interview soundbite:

> "Determinism means same input gives same timing and output every time. We avoid unbounded loops, recursion, and dynamic allocation because they have unpredictable timing. Everything must be analyzable at design time - you need to prove worst-case execution time."

# 1️⃣ RTOS Fundamentals

- What is RTOS vs regular OS
- Hard vs Soft real-time (with examples)

# 2️⃣ Rate Monotonic Scheduling (RMS)

- SHORTER PERIOD = HIGHER PRIORITY
- 69% utilization bound

# 3️⃣ Priority Inversion (THEY WILL ASK THIS)

- The problem: High waits for Low while Medium runs
- The Mars Pathfinder story (1997)
- The solution: Priority Inheritance

# 4️⃣ Synchronization Primitives

- Mutex = protect shared data
- Semaphore = signaling events
- Message Queue = passing data

# 5️⃣ VxWorks Basics

- Wind River, most popular aerospace RTOS
- Used in Mars rovers, F-35, Boeing 787
- Priority 0-255 (0 = highest - opposite of Linux!)