# 🖥️ RTOS CONCEPTS CHEAT SHEET

## Print this and review before the interview!

---

## WHAT IS AN RTOS?

```
RTOS = Real-Time Operating System

PURPOSE: Guarantee timing constraints are met

KEY DIFFERENCE FROM WINDOWS/LINUX:
├── Regular OS: "Best effort" - usually fast enough
├── RTOS: "Guaranteed" - ALWAYS meets deadlines
└── Why? Safety-critical systems can't afford "usually"
```

---

## HARD vs SOFT REAL-TIME

| Type | Deadline Miss = | Example | Consequence |
|------|-----------------|---------|-------------|
| **Hard** | System FAILURE | Airbag, flight controls | Catastrophic |
| **Soft** | Quality DEGRADATION | Video streaming | Annoying |
| **Firm** | Result WORTHLESS | Stock trading | Missed opportunity |

**INTERVIEW EXAMPLE:**
> "Airbag deployment is hard real-time - you have milliseconds to respond. Missing that deadline could be fatal. Video streaming is soft real-time - a missed frame causes stuttering but nobody dies."

---

## RATE MONOTONIC SCHEDULING (RMS)

### THE GOLDEN RULE (MEMORIZE THIS):
```
┌─────────────────────────────────────────────────────┐
│      SHORTER PERIOD = HIGHER PRIORITY               │
│                                                     │
│  Task running every 10ms gets higher priority       │
│  than task running every 50ms                       │
└─────────────────────────────────────────────────────┘
```

### Utilization Bound:
```
Formula: U = Σ(Ci/Ti)

Where:
  Ci = Execution time of task i
  Ti = Period of task i

RULE: If U ≤ 69%, tasks are GUARANTEED schedulable

Example:
  Task A: 10ms period, 2ms execution → 2/10 = 0.2
  Task B: 25ms period, 5ms execution → 5/25 = 0.2
  Task C: 50ms period, 10ms execution → 10/50 = 0.2
  
  Total: 0.2 + 0.2 + 0.2 = 0.6 = 60%
  60% < 69% → SCHEDULABLE ✓
```

### Why RMS Works:
- Static priorities (set once, never change)
- Mathematically provable
- Industry standard for aerospace

---

## PRIORITY INVERSION

### The Problem:
```
WHAT SHOULD HAPPEN:
  HIGH runs before MEDIUM runs before LOW

PRIORITY INVERSION:
  1. LOW acquires mutex (lock)
  2. HIGH needs mutex → BLOCKED (waiting for LOW)
  3. MEDIUM preempts LOW (doesn't need mutex)
  4. HIGH waits for MEDIUM to finish
  
RESULT: HIGH effectively runs at LOW's priority!
```

### Visual:
```
HIGH  ──blocked──────────────────────┐
                                     │ waiting for mutex
MED   ─────────────running───────────┤
       ↑ preempts LOW                │
LOW   ────┴─────blocked──────────────┘
          holding mutex
```

### Mars Pathfinder (1997):
- Bus management (LOW) held mutex
- Weather task (MEDIUM) kept preempting
- Critical task (HIGH) was starved
- Watchdog timer reset system
- **FIX:** Enable priority inheritance in VxWorks

### Solutions:

**Priority Inheritance:**
```
When HIGH is blocked by LOW:
  → LOW temporarily gets HIGH's priority
  → MEDIUM can't preempt LOW anymore
  → LOW finishes quickly, releases mutex
  → LOW returns to normal priority
```

**Priority Ceiling:**
```
Each mutex has "ceiling" = highest priority of users
Task holding mutex runs at ceiling priority
More efficient, prevents deadlock too
```

---

## VXWORKS KEY FACTS

```
VXWORKS:
├── Made by Wind River
├── Most popular RTOS for aerospace/defense
├── Used in: Mars rovers, Boeing 787, F-35
├── POSIX-compliant APIs
├── Supports priority inheritance
└── DO-178C certified versions available

TASK PRIORITIES:
├── 0 = HIGHEST priority
├── 255 = LOWEST priority
└── Opposite of Linux!

TASK STATES:
├── READY - waiting to run
├── RUNNING - currently executing
├── SUSPENDED - explicitly stopped
├── DELAYED - waiting for timeout
└── PENDED - waiting for resource
```

### VxWorks vs Linux:
| Feature | VxWorks | Linux |
|---------|---------|-------|
| Determinism | Guaranteed | Best-effort |
| Interrupt latency | Microseconds | Milliseconds |
| Memory footprint | Small (KB) | Large (MB) |
| Certification | DO-178C ready | Complex |

---

## SYNCHRONIZATION PRIMITIVES

### Quick Reference:
| Primitive | Use For | Priority Inheritance? |
|-----------|---------|----------------------|
| **Mutex** | Protecting shared data | YES |
| **Binary Semaphore** | Signaling events | NO |
| **Counting Semaphore** | Resource pools | NO |
| **Message Queue** | Passing data | Depends |

### Mutex:
```
- Binary lock (locked/unlocked)
- OWNER can unlock (ownership concept)
- Use for: One task at a time accessing shared memory
- Supports priority inheritance
```

### Semaphore:
```
- Signal between tasks
- ANY task can give/take
- No ownership
- Binary: 0 or 1 (signaling)
- Counting: 0 to N (resource pool)
```

### Message Queue:
```
- FIFO data transfer
- Producer-consumer pattern
- Sends actual data, not just signals
- Example: Sensor → Processing task
```

### Rule of Thumb:
```
Protecting data?     → MUTEX
Signaling event?     → BINARY SEMAPHORE
Counting resources?  → COUNTING SEMAPHORE
Passing data?        → MESSAGE QUEUE
```

---

## CONTEXT SWITCHING

```
WHAT IS IT?
├── Saving current task state (registers, stack pointer)
├── Loading next task state
├── Switching execution
└── Takes time (overhead!)

WHY IT MATTERS:
├── Every switch has overhead (microseconds)
├── Too much switching = wasted CPU
├── Must account for in schedulability analysis
└── RTOS minimizes this overhead

INTERRUPT HANDLING:
├── Interrupt = external event (hardware signal)
├── ISR (Interrupt Service Routine) runs immediately
├── ISR should be SHORT
├── ISR signals task to do heavy work
└── ISR → Semaphore → Task pattern
```

---

## KEY INTERVIEW TALKING POINTS

### On RTOS Experience:
> "I haven't used VxWorks in production, but I've studied RTOS architecture and understand rate monotonic scheduling, priority inversion, and task synchronization. My distributed systems work involved similar challenges - priority-based scheduling, deterministic timing, resource contention."

### On Hard Real-Time:
> "Hard real-time means deadlines are absolute - missing one is system failure. Flight controls are a perfect example. That's why RTOS with guaranteed scheduling is essential for aerospace systems."

### On Priority Inversion:
> "Priority inversion is when a high-priority task effectively runs at low priority because it's waiting for a resource held by a low-priority task. Mars Pathfinder experienced this in 1997 - the fix was enabling priority inheritance in VxWorks."

### On RMS:
> "Rate Monotonic Scheduling assigns priorities by period - shorter period means higher priority. It's mathematically elegant: if total CPU utilization stays under about 69%, all deadlines are guaranteed. That predictability is crucial for certification."

---

## QUICK FORMULAS

```
RMS Utilization Bound:
  U ≤ n(2^(1/n) - 1)
  For large n: U ≤ 0.693 (69%)

Schedulability Test:
  If Σ(Ci/Ti) ≤ 0.69, system is schedulable

Response Time:
  Response = Execution + Blocking + Interference
```

---

## COMMON INTERVIEW QUESTIONS

1. **"What's the difference between hard and soft real-time?"**
   → Deadline miss consequences: failure vs degradation

2. **"Explain Rate Monotonic Scheduling"**
   → Shorter period = higher priority, 69% utilization bound

3. **"What is priority inversion? How do you prevent it?"**
   → High blocked by low, medium preempts; Priority inheritance

4. **"What are the differences between VxWorks and Linux?"**
   → Determinism, interrupt latency, certification, footprint

5. **"When would you use a mutex vs a semaphore?"**
   → Mutex for data protection, semaphore for signaling

---

**Know these concepts cold! 🎯**

