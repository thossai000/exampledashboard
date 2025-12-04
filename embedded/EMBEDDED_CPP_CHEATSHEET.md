# 💻 EMBEDDED C++ & SAFETY-CRITICAL CHEAT SHEET

## Key constraints that make embedded C++ different from desktop C++

---

## WHY C++ FOR EMBEDDED?

```
ADVANTAGES:
├── Object-oriented design (abstraction, encapsulation)
├── Better type safety than C
├── RAII for automatic resource management
├── Templates for zero-cost abstraction
├── Industry adoption (aerospace, automotive)
└── Existing codebases and expertise

BUT... need to avoid certain features
```

---

## ❌ WHAT TO AVOID (AND WHY)

### 1. EXCEPTIONS
```cpp
// ❌ AVOID IN EMBEDDED
try {
    riskyFunction();
} catch (const std::exception& e) {
    // handle error
}

// WHY AVOID?
// - Non-deterministic timing (can't predict when/how long)
// - Code size bloat (exception tables)
// - Many embedded compilers have poor support
// - Violates MISRA guidelines

// ✅ USE INSTEAD: Error codes, status returns
enum class Status { OK, ERROR_TIMEOUT, ERROR_INVALID };

Status safeFunction() {
    if (problem) return Status::ERROR_TIMEOUT;
    return Status::OK;
}

// Check return value
if (safeFunction() != Status::OK) {
    handleError();
}
```

### 2. DYNAMIC MEMORY (new/delete, malloc/free)
```cpp
// ❌ AVOID IN EMBEDDED
void riskyFunction() {
    int* ptr = new int[1000];  // Heap allocation
    // ... use ptr ...
    delete[] ptr;
}

// WHY AVOID?
// - Heap FRAGMENTATION over long runtime
// - Non-deterministic allocation time
// - Memory exhaustion risk
// - No garbage collection

// ✅ USE INSTEAD: Static allocation
static int buffer[1000];  // Compile-time, predictable

// ✅ USE INSTEAD: Memory pools
class MemoryPool {
    static uint8_t pool[POOL_SIZE];
    // Pre-allocated, deterministic
};

// ✅ USE INSTEAD: Placement new (if needed)
alignas(MyClass) uint8_t storage[sizeof(MyClass)];
MyClass* obj = new (storage) MyClass();  // No heap allocation
```

### 3. RTTI (Run-Time Type Information)
```cpp
// ❌ AVOID IN EMBEDDED
if (dynamic_cast<DerivedClass*>(basePtr)) {
    // ...
}

typeid(obj).name();  // Also RTTI

// WHY AVOID?
// - Code size overhead (type tables)
// - Runtime cost
// - Usually unnecessary with good design

// ✅ USE INSTEAD: Compile-time polymorphism
template<typename T>
void process(T& obj) {
    obj.doSomething();  // Duck typing, no RTTI
}

// ✅ USE INSTEAD: Virtual functions (if needed)
class Base {
    virtual void doSomething() = 0;
};
```

### 4. VIRTUAL FUNCTIONS (Use With Care)
```cpp
// ⚠️ CAREFUL IN EMBEDDED
class Base {
    virtual void update();  // vtable lookup overhead
};

// CONSIDERATIONS:
// - Small overhead per call (vtable indirection)
// - Memory for vtable pointer in each object
// - Generally acceptable for most embedded
// - Avoid in tight loops or interrupt handlers

// ✅ ALTERNATIVE: CRTP (Curiously Recurring Template Pattern)
template<typename Derived>
class Base {
    void interface() {
        static_cast<Derived*>(this)->implementation();
    }
};

class Derived : public Base<Derived> {
    void implementation() { /* ... */ }
};
// Zero overhead - resolved at compile time
```

---

## ✅ WHAT TO USE

### RAII (Resource Acquisition Is Initialization)
```cpp
// ✅ EXCELLENT FOR EMBEDDED
class MutexGuard {
    Mutex& mutex;
public:
    MutexGuard(Mutex& m) : mutex(m) {
        mutex.lock();      // Acquire in constructor
    }
    ~MutexGuard() {
        mutex.unlock();    // Release in destructor
    }
};

void safeFunction() {
    MutexGuard guard(myMutex);  // Lock acquired
    // ... do work ...
}  // Destructor called automatically - unlock guaranteed!

// WHY THIS IS GREAT:
// - Resource cleanup GUARANTEED even on early return
// - No way to forget to unlock
// - Exception-safe (if you use exceptions)
// - Compiler optimizes away overhead
```

### SMART POINTERS (Carefully)
```cpp
// ✅ unique_ptr: Zero overhead, safe ownership
std::unique_ptr<Sensor> sensor = std::make_unique<Sensor>();

// ⚠️ shared_ptr: Use carefully
std::shared_ptr<Data> data;  // Reference counting overhead

// FOR EMBEDDED: Often better to use static allocation
// But unique_ptr is fine for owned resources
```

### TEMPLATES (Zero Runtime Cost)
```cpp
// ✅ GREAT FOR EMBEDDED
template<size_t SIZE>
class StaticBuffer {
    uint8_t data[SIZE];  // Compile-time size
public:
    uint8_t& operator[](size_t i) { return data[i]; }
};

StaticBuffer<256> smallBuffer;
StaticBuffer<4096> largeBuffer;
// Different sizes, same code, no runtime overhead
```

### CONSTEXPR (Compile-Time Computation)
```cpp
// ✅ EXCELLENT - Zero runtime cost
constexpr int BUFFER_SIZE = 1024;
constexpr int calculateChecksum(int data) {
    return data ^ 0xFF;  // Computed at compile time
}

static_assert(BUFFER_SIZE > 0, "Buffer must be positive");
// Compile-time error if false
```

---

## MISRA C++ GUIDELINES

### What is MISRA?
```
MISRA = Motor Industry Software Reliability Association

PURPOSE:
├── Coding standard for safety-critical C/C++
├── Avoid undefined behavior
├── Avoid implementation-defined behavior
├── Required for DO-178C, ISO 26262
└── Enforced by static analysis tools
```

### Key MISRA Principles:
```
1. NO UNDEFINED BEHAVIOR
   - Uninitialized variables
   - Buffer overflows
   - Null pointer dereference
   
2. NO IMPLEMENTATION-DEFINED RELIANCE
   - Integer sizes (use stdint.h: int32_t, uint16_t)
   - Signedness of char
   
3. EXPLICIT OVER IMPLICIT
   - Always initialize variables
   - Explicit casts, not implicit
   
4. AVOID DANGEROUS CONSTRUCTS
   - No goto
   - Single return point (sometimes)
   - No fall-through in switch
```

### Example MISRA Rules:
```cpp
// Rule 0-1-1: No unreachable code
if (true) { /* ok */ }
else { /* VIOLATION - unreachable */ }

// Rule 5-0-15: Array indexing must be in bounds
int arr[10];
arr[10] = 5;  // VIOLATION - out of bounds

// Rule 6-4-2: switch must have default
switch (x) {
    case 1: break;
    // VIOLATION - no default
}

// Rule 15-0-2: No goto
goto label;  // VIOLATION

// Rule 18-0-5: Use bounded string functions
strcpy(dest, src);   // VIOLATION - unbounded
strncpy(dest, src, sizeof(dest));  // OK - bounded
```

---

## DO-178C OVERVIEW

### What is DO-178C?
```
"Software Considerations in Airborne Systems and Equipment Certification"

├── FAA/EASA standard for avionics software
├── Defines development rigor by criticality
├── Required for aircraft certification
└── Updated from DO-178B in 2012
```

### DAL Levels (Design Assurance Levels):
```
┌───────┬─────────────────────────────────┬─────────────────────┐
│ Level │ Failure Effect                  │ Example             │
├───────┼─────────────────────────────────┼─────────────────────┤
│ DAL A │ Catastrophic (loss of aircraft) │ Flight controls     │
│ DAL B │ Hazardous (serious injury)      │ Engine controls     │
│ DAL C │ Major (workload/capability)     │ Navigation display  │
│ DAL D │ Minor (inconvenience)           │ Passenger WiFi mgmt │
│ DAL E │ No effect on safety             │ IFE system          │
└───────┴─────────────────────────────────┴─────────────────────┘
```

### What DAL Level Drives:
```
DAL A (Most Rigorous):
├── 100% requirements traceability
├── MC/DC coverage (Modified Condition/Decision Coverage)
├── Independent verification
├── Extensive documentation
├── Very expensive, very slow
└── Every line of code scrutinized

DAL E (Least Rigorous):
├── Basic requirements
├── Standard testing
├── Minimal documentation
└── Faster development
```

### Requirements Traceability:
```
Requirement → Design → Code → Test
     ↑          ↑        ↑       ↑
     └──────────┴────────┴───────┘
        Bidirectional tracing

Change requirement? Must trace to:
- Which design elements affected
- Which code implements it
- Which tests verify it

This is WHY model-based development (Rhapsody) helps:
- Automatic trace generation
- Consistency checking
- Change impact analysis
```

---

## MEMORY MANAGEMENT FOR EMBEDDED

### Stack vs Heap:
```
STACK (✅ Preferred for embedded):
├── Automatic allocation/deallocation
├── Deterministic timing
├── Limited size (watch for overflow!)
└── Local variables, function calls

HEAP (❌ Avoid in embedded):
├── Dynamic allocation
├── Non-deterministic timing
├── Fragmentation risk
└── Memory leaks possible
```

### Static Allocation Patterns:
```cpp
// 1. Global/static arrays
static uint8_t rxBuffer[RX_BUFFER_SIZE];

// 2. Compile-time sized containers
std::array<int, 100> fixedArray;  // No heap

// 3. Object pools
template<typename T, size_t N>
class ObjectPool {
    T objects[N];
    bool used[N] = {false};
public:
    T* allocate() { /* find unused slot */ }
    void deallocate(T* ptr) { /* mark slot unused */ }
};

// 4. Ring buffers (circular buffers)
template<typename T, size_t N>
class RingBuffer {
    T buffer[N];
    size_t head = 0, tail = 0;
public:
    bool push(T item);
    bool pop(T& item);
};
```

---

## DETERMINISM PRINCIPLES

### What is Deterministic Behavior?
```
DETERMINISTIC = Same input → Same timing → Same output

In embedded real-time:
├── Execution time must be PREDICTABLE
├── No unbounded loops
├── No unbounded recursion
├── No operations with variable timing
└── Must be analyzable at design time
```

### Ensuring Determinism:
```cpp
// ❌ NON-DETERMINISTIC: Unbounded loop
while (!dataReady) {
    // Could run forever!
}

// ✅ DETERMINISTIC: Bounded loop with timeout
for (int i = 0; i < MAX_ITERATIONS; i++) {
    if (dataReady) break;
    delay_us(10);
}

// ❌ NON-DETERMINISTIC: Dynamic allocation
std::vector<int> data;  // May allocate

// ✅ DETERMINISTIC: Static allocation
std::array<int, 100> data;  // Fixed size

// ❌ NON-DETERMINISTIC: Recursion (stack depth varies)
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n-1);
}

// ✅ DETERMINISTIC: Iteration
int factorial(int n) {
    int result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

---

## INTERVIEW TALKING POINTS

### On Embedded C++:
> "I understand embedded C++ has unique constraints. You avoid exceptions because they're non-deterministic - you can't predict timing. Dynamic allocation is dangerous due to fragmentation and unpredictable allocation time. Instead, you use static allocation, RAII for resource management, and templates for zero-cost abstraction."

### On MISRA:
> "MISRA C++ is a coding standard for safety-critical systems. It's designed to avoid undefined behavior and ensure code is analyzable. It's enforced by static analysis tools like Coverity and is required for DO-178C certification."

### On DO-178C:
> "DO-178C defines development rigor based on failure criticality. DAL A is for catastrophic failures like flight controls - requiring complete requirements traceability and MC/DC coverage. Lower DAL levels have proportionally less stringent requirements."

### On Your C++ Experience:
> "My recent work has been Python and Java, but I have C++ foundation from university and my Northrop internship where I built an ANSYS automation tool. My strong Java OOP background - inheritance, polymorphism, design patterns - transfers directly to C++. I've been studying embedded-specific constraints like MISRA guidelines and deterministic memory management."

---

**Know these concepts! They show you understand embedded constraints. 🎯**

