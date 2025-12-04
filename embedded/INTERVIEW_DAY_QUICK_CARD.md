# 📋 EMBEDDED INTERVIEW DAY - QUICK REFERENCE CARD

## Wednesday, December 4, 2024 | Print this!

---

## ⚡ SAY THIS IN FIRST 5 MINUTES

> "I have an **active Secret clearance** that's currently in-scope, and I'm ready to pursue SAP access immediately. I'm also currently employed at Northrop Grumman as an Associate Software Engineer."

---

## 🎯 THE BIG 3 MESSAGES

1. **I have clearance + insider status** → Massive advantage
2. **I'm a proven fast learner** → Android ramp-up story, 40% bug reduction
3. **I understand RTOS concepts** → RMS, priority inversion, determinism

---

## 🖥️ RTOS QUICK FACTS

| Concept | Key Point |
|---------|-----------|
| **Hard real-time** | Deadline miss = system failure |
| **Soft real-time** | Deadline miss = quality degradation |
| **RMS** | Shorter period = higher priority |
| **69% rule** | Utilization ≤ 69% = schedulable |
| **Priority inversion** | High blocked by low, medium preempts |
| **Solution** | Priority inheritance (Mars Pathfinder fix) |

---

## 🔧 VXWORKS vs LINUX

| VxWorks | Linux |
|---------|-------|
| Guaranteed timing | Best-effort |
| Microsecond latency | Millisecond latency |
| Small footprint (KB) | Large footprint (MB) |
| DO-178C certified | Complex to certify |

---

## 💻 EMBEDDED C++ - AVOID

| ❌ Avoid | Why |
|----------|-----|
| Exceptions | Non-deterministic timing |
| Dynamic memory | Fragmentation, unpredictable |
| RTTI | Code size overhead |

**Use instead:** Error codes, static allocation, RAII, templates

---

## 📜 DO-178C LEVELS

| Level | Failure | Example |
|-------|---------|---------|
| **DAL A** | Catastrophic | Flight controls |
| **DAL B** | Hazardous | Engine controls |
| **DAL C** | Major | Navigation display |
| **DAL D** | Minor | Passenger WiFi |
| **DAL E** | No effect | IFE system |

---

## 🌟 YOUR STRONGEST STORIES

1. **AI Collective Intelligence** → Real-time scheduling, 90% efficiency
2. **Android Debugging** → 40% bug reduction, systematic approach
3. **Learning Agility** → Android ramp-up in first quarter
4. **HIL Testing** → Cross-functional, 30% reliability improvement
5. **SonarQube** → Static analysis, 30% defect reduction

---

## 🔄 GAP ACKNOWLEDGMENT PATTERNS

**For VxWorks:**
> "Haven't used professionally, but studied RTOS principles. Similar concepts in my distributed systems work. Confident I can ramp up quickly."

**For Rhapsody:**
> "Haven't used specifically, but familiar with UML. Fast learner with tools - picked up Jenkins, SonarQube, Android Studio rapidly."

**For DO-178C:**
> "Haven't been through certification, but researched the standard. My work involved similar quality rigor. Excited to learn formal process."

---

## ❓ QUESTIONS TO ASK THEM

1. "What RTOS does your team primarily use?"
2. "What DO-178C DAL level are you targeting?"
3. "What does onboarding look like for someone new to embedded?"
4. "What are the biggest technical challenges your team faces?"
5. "How does the team approach mentorship?"

---

## 💬 IF YOU DON'T KNOW SOMETHING

> "I haven't worked with [X] specifically, but here's how I'd approach learning it..."

> "That's outside my direct experience, but based on my understanding of [related concept]..."

---

## 🧘 CONFIDENCE REMINDERS

- ✅ Active Secret clearance (in-scope)
- ✅ Current Northrop employee (insider)
- ✅ Proven fast learner
- ✅ Mission-critical systems mindset
- ✅ Strong OOP foundation (Java → C++)
- ✅ Georgia Tech MSCS student

---

## ⏰ INTERVIEW FORMAT

| Section | Weight | Approach |
|---------|--------|----------|
| **Behavioral (STAR)** | 70% | Use your 7 stories |
| **Conceptual Technical** | 25% | Explain principles |
| **Light Technical** | 5% | Architecture discussion |

---

## ✅ MORNING CHECKLIST

- [ ] Light concept review (don't cram)
- [ ] Professional attire
- [ ] Resume printed (3 copies)
- [ ] Questions for interviewer ready
- [ ] Arrive 15 minutes early
- [ ] Deep breaths, confident posture

---

## 🏆 CLOSING STRONG

> "I'm genuinely excited about this opportunity. My combination of **active clearance**, **Northrop Grumman experience**, and **strong software engineering foundation** makes me confident I can contribute meaningfully. I'm particularly excited to apply my distributed systems experience to embedded aerospace applications."

---

**YOU'VE GOT THIS! 🎯**

Your clearance + Northrop insider status + learning agility = strong candidate. Be honest about gaps, show enthusiasm for the domain, demonstrate your ability to learn.

