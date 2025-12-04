
# Mastering embedded systems and RTOS

The embedded systems community has reached clear consensus on learning resources: **Shawn Hymel's Digi-Key series** and **Miro Samek's Quantum Leaps course** stand out as the definitive free resources, with **FastBit Embedded Brain Academy** leading paid options. These creators excel at Feynman-style teaching—explaining complex RTOS concepts through clear analogies, practical hardware demonstrations, and progressive skill-building challenges.

## Comprehensive RTOS courses cover all fundamentals

Two standout series dominate Reddit recommendations and community forums for their depth and pedagogical quality.

**Digi-Key: Introduction to RTOS** by Shawn Hymel

- **Channel:** Digi-Key Electronics
- **URL:** [https://www.youtube.com/playlist?list=PLEBQazB0HUyQ4hAPU1cJED6t3DU0h34bz](https://www.youtube.com/playlist?list=PLEBQazB0HUyQ4hAPU1cJED6t3DU0h34bz)
- **Why it's excellent:** Uses ESP32 with FreeRTOS, includes hands-on challenges after each video, PowerPoint slides available under Creative Commons license [GitHub](https://github.com/ShawnHymel/introduction-to-rtos)
- **Topics:** RTOS fundamentals (Part 1), task scheduling (Part 3), queues (Part 5), mutex (Part 6), semaphores (Part 7), deadlock (Part 10), **priority inversion** (Part 11), multicore systems (Part 12)
- **Duration:** 12 parts, approximately **15-20 minutes each**
- **GitHub:** [https://github.com/ShawnHymel/introduction-to-rtos](https://github.com/ShawnHymel/introduction-to-rtos)

The series brilliantly explains that "real-time" doesn't mean "fast"—it means **deterministic execution** where developers can calculate ahead of time whether tasks will meet deadlines. [digikey](https://www.digikey.com/en/maker/projects/introduction-to-rtos-solution-to-part-2-freertos/b3f84c9c9455439ca2dcb8ccfce9dec5)[DigiKey](https://www.digikey.com/en/maker/projects/introduction-to-rtos-solution-to-part-2-freertos/b3f84c9c9455439ca2dcb8ccfce9dec5)

**Quantum Leaps: Modern Embedded Systems Programming** by Miro Samek

- **Channel:** Quantum Leaps (StateMachineCOM)
- **URL:** [https://www.youtube.com/playlist?list=PLPW8O6W-1chwyTzI3BHwBLbGQoPFxPAPM](https://www.youtube.com/playlist?list=PLPW8O6W-1chwyTzI3BHwBLbGQoPFxPAPM)
- **Companion site:** [https://www.state-machine.com/video-course](https://www.state-machine.com/video-course)
- **Why it's excellent:** University-level depth with hands-on approach; you actually **build your own RTOS (MiROS)** from scratch, seeing exactly what happens inside ARM Cortex-M at the machine level
- **Hardware:** TivaC LaunchPad (EK-TM4C123GXL) ~$13 or STM32 NUCLEO-C031C6 [state-machine](https://www.state-machine.com/video-course)
- **Topics:** Lessons 22-28 cover RTOS specifically—context switching, round-robin scheduling, preemptive priority-based scheduling, **Rate Monotonic Scheduling**, blocking/semaphores, mutual exclusion, **priority inversion with priority ceiling and inheritance protocols**
- **Duration:** 45+ lessons total, RTOS section spans **7 comprehensive lessons**

Miro Samek holds a PhD in Nuclear Physics [Embedded Related](https://www.embeddedrelated.com/showarticle/951.php)[Embedded Related](https://www.embeddedrelated.com/showarticle/1223.php) and progresses historically through computing paradigms: superloop → timesharing → preemptive RTOS → event-driven programming. [State Machine](https://www.state-machine.com/embedded-programming-video-course-teaches-rtos)

## Embedded C++ and systems programming fundamentals

**Jacob Sorber** (Professor at Clemson University)

- **Channel:** [https://www.youtube.com/@JacobSorber](https://www.youtube.com/@JacobSorber)
- **Topics:** Introduction to C (24 episodes), debugging (23 episodes), [Iraspa](https://iraspa.org/links/c/) memory management, pointers, threads, processes, embedded fundamentals
- **Subscribers:** ~270K
- **Teaching style:** Explains "what happens under the hood"—how C statements translate to machine instructions and how fast processors execute them

**Phil's Lab**

- **Channel:** [https://www.youtube.com/@PhilsLab](https://www.youtube.com/@PhilsLab)
- **Website:** [https://www.phils-lab.net/videos](https://www.phils-lab.net/videos)
- **Best videos:** STM32 Programming Tutorial for Custom Hardware (#13, **379K+ views**), [FEDEVEL](https://fedevel.com/blog/stm32-programming-tutorial-for-custom-hardware-swd-pwm-usb-spi-phils-lab-13) Digital Audio Processing series, FreeRTOS on STM32 introduction, SDRAM Hardware & Firmware (#80)
- **Topics:** STM32 firmware development, real-time DSP (FIR/IIR filters), DMA programming, I2C/SPI/USB peripherals, PCB design integration [Phils-lab](https://www.phils-lab.net/videos)
- **Why it's valuable:** Combines firmware programming with practical hardware design for complete embedded systems understanding

**Ben Eater**

- **Channel:** [https://www.youtube.com/@BenEater](https://www.youtube.com/@BenEater)
- **8-bit computer playlist:** [https://www.youtube.com/playlist?list=PLowKtXNTBypGqImE405J2565dvjafglHU](https://www.youtube.com/playlist?list=PLowKtXNTBypGqImE405J2565dvjafglHU)
- **Why it's foundational:** Legendary for building computers from scratch on breadboards—provides deep understanding of CPU architecture and instruction execution essential for embedded programming

**CppCon Embedded Track**

- **Channel:** [https://www.youtube.com/@CppCon](https://www.youtube.com/@CppCon)
- **Notable talks:** "Using C++14 in an Embedded 'SuperLoop' Firmware" by Erik Rainey (1 hour 31 minutes), "C++ Sender Patterns to Wrangle Concurrency in Embedded Devices" by Michael Caisse (CppCon 2024)
- **Topics:** Modern C++ for embedded, memory management, real-time constraints, MISRA/AUTOSAR compliance

## Rate Monotonic Scheduling resources remain academic-focused

RMS video content is less abundant than other RTOS topics, with the best explanations embedded within larger courses.

**Quantum Leaps Lesson 26** provides the most practical RMS coverage, demonstrating real-time scheduling with logic analyzers and mathematical proofs of schedulability. Key concepts include the **69% (ln(2)) utilization bound** derived from Liu and Layland's seminal 1973 paper, and CPU utilization calculations for deadline compliance.

**IIT Kharagpur NPTEL Course** by Prof. Rajib Mall

- **URL:** [https://www.classcentral.com/course/youtube-computer-science-real-time-systems-47638](https://www.classcentral.com/course/youtube-computer-science-real-time-systems-47638)
- **RMS content:** Lectures 7-9 cover Rate Monotonic Analysis in depth, Lecture 10 covers Deadline Monotonic Scheduling
- **Topics:** Real-time task scheduling basics, **"shorter period = higher priority" rule**, schedulability tests, multiprocessor scheduling
- **Duration:** Full semester course

**Carnegie Mellon SEI RMA Tutorial**

- **Reference:** [https://www.cs.fsu.edu/~baker/realtime/restricted/notes/s6-r.pdf](https://www.cs.fsu.edu/~baker/realtime/restricted/notes/s6-r.pdf)
- **Topics:** Priority assignment algorithm, utilization bound theorem, schedulability analysis with examples, synchronization and priority inversion, aperiodic servers
- **Format:** Tutorial with group exercises (PDF slides, can supplement video learning)

## Priority inversion gets thorough treatment with Mars Pathfinder context

**Digi-Key Part 11: Priority Inversion**

- **Solution page:** [https://www.digikey.com/en/maker/projects/introduction-to-rtos-solution-to-part-11-priority-inversion/abf4b8f7cd4a4c70bece35678d178321](https://www.digikey.com/en/maker/projects/introduction-to-rtos-solution-to-part-11-priority-inversion/abf4b8f7cd4a4c70bece35678d178321)
- **Topics:** Bounded vs unbounded priority inversion, [DigiKey](https://www.digikey.com/en/maker/projects/introduction-to-rtos-solution-to-part-11-priority-inversion/abf4b8f7cd4a4c70bece35678d178321) the classic scenario (high-priority task blocked by low-priority while medium-priority runs), priority inheritance mechanism, critical section guards using portENTER_CRITICAL/portEXIT_CRITICAL [DigiKey](https://www.digikey.com/en/maker/projects/introduction-to-rtos-solution-to-part-11-priority-inversion/abf4b8f7cd4a4c70bece35678d178321)
- **Duration:** ~15-20 minutes

**ControllersTech FreeRTOS Tutorial 7.1**

- **Website:** [https://controllerstech.com/freertos-tutorial-7-using-mutex/](https://controllerstech.com/freertos-tutorial-7-using-mutex/)
- **Topics:** Mutex vs binary semaphore differences, **priority inversion with UART output demonstration**, priority inheritance in FreeRTOS on STM32 [ControllersTech®](https://controllerstech.com/freertos-tutorial-7-using-mutex/)
- **Teaching approach:** Step-by-step code walkthrough showing the problem and solution

**Mars Pathfinder 1997 Incident Documentation:** The famous bug is referenced across multiple video courses but comprehensive coverage exists primarily in written form:

- **Cornell CS document:** [http://www.cs.cornell.edu/courses/cs614/1999sp/papers/pathfinder.html](http://www.cs.cornell.edu/courses/cs614/1999sp/papers/pathfinder.html) (based on David Wilner's IEEE Real-Time Systems Symposium keynote)
- **Chalmers University technical report:** [https://www.cse.chalmers.se/~risat/Report_MarsPathFinder.pdf](https://www.cse.chalmers.se/~risat/Report_MarsPathFinder.pdf)
- **Glenn Reeves' account (JPL Software Team Leader):** [https://www.cs.unc.edu/~anderson/teach/comp790/papers/mars_pathfinder_long_version.html](https://www.cs.unc.edu/~anderson/teach/comp790/papers/mars_pathfinder_long_version.html)

The fix involved enabling a single mutex flag from false to true—uploaded from Earth to Mars—implementing the **Priority Inheritance Protocol** developed by Sha, Rajkumar, and Lehoczky at CMU.

## Synchronization primitives explained through memorable analogies

The community has developed consistent analogies that appear across top tutorials:

|Primitive|Best Analogy|Video Source|
|---|---|---|
|**Mutex**|Coffee shop bathroom key—one key, one bathroom, only the holder can unlock|Barr Group Tech Talk|
|**Binary Semaphore**|Signaling flag—any task can raise or lower it|Digi-Key Part 7|
|**Counting Semaphore**|Parking garage counter—tracks available spaces until zero|Digi-Key Part 7|
|**Message Queue**|Pipeline/mailbox—FIFO, thread-safe, decouples sender/receiver|Digi-Key Part 5|

**Digi-Key Parts 5-7** provide the most cohesive primitive coverage:

- Part 5 (Queues): [https://youtu.be/pHJ3lxOoWeI](https://youtu.be/pHJ3lxOoWeI) — xQueueSend(), xQueueReceive(), passing structs
- Part 6 (Mutex): [https://youtu.be/I55auRpbiTs](https://youtu.be/I55auRpbiTs) — xSemaphoreCreateMutex(), critical sections, race condition prevention
- Part 7 (Semaphores): [https://youtu.be/5JcMtbA9QEE](https://youtu.be/5JcMtbA9QEE) — binary vs counting semaphores, producer-consumer patterns, ISR-safe operations

**Barr Group Tech Talk: Mutexes and Semaphores Demystified**

- **URL:** [https://barrgroup.com/tech-talks/rtos-mutex-semaphore](https://barrgroup.com/tech-talks/rtos-mutex-semaphore)
- **Presenters:** Andrew Girson and Salomon Singer
- **Key distinction:** Mutex has **ownership** (only locker can unlock, supports priority inheritance); semaphore is for **signaling** (any task can give/take, no ownership concept) [Barr Group](https://barrgroup.com/tech-talks/rtos-mutex-semaphore)[TutorialsPoint](https://www.tutorialspoint.com/mutex-vs-semaphore)

**CircuitDigest Arduino FreeRTOS Tutorial 3**

- **URL:** [https://circuitdigest.com/microcontroller-projects/arduino-freertos-tutorial-using-semaphore-and-mutex-in-freertos-with-arduino](https://circuitdigest.com/microcontroller-projects/arduino-freertos-tutorial-using-semaphore-and-mutex-in-freertos-with-arduino)
- **Topics:** Binary semaphore, counting semaphore, mutex—all demonstrated with Arduino for the gentlest learning curve
- **Practical example:** Temperature and LDR tasks sharing LCD display protected by mutex [CircuitDigest](https://circuitdigest.com/microcontroller-projects/arduino-freertos-tutorial-using-semaphore-and-mutex-in-freertos-with-arduino)

## VxWorks resources are scarce but official training exists

VxWorks content is limited due to its proprietary nature, but Wind River provides structured learning paths.

**Wind River VxWorks Tour Series**

- **URL:** [https://www.windriver.com/resource/vxworks-tour-part-1-introduction-to-vxworks](https://www.windriver.com/resource/vxworks-tour-part-1-introduction-to-vxworks)
- **Format:** 6-part video series covering VxWorks introduction, architecture, development workflow
- **Topics:** VxWorks basics, Workbench IDE, real-time application development

**Wind River Learning Portal**

- **URL:** [https://learning.windriver.com/](https://learning.windriver.com/)
- **Available courses:** VxWorks 7 and Workbench Essentials (4-day expert-led), VxWorks Cert Edition Essentials, VxWorks SMP programming, VxBus driver development
- **Learning path:** [https://learning.windriver.com/path/vxworks7-essentials-workbench-and-tools](https://learning.windriver.com/path/vxworks7-essentials-workbench-and-tools)

**Critical VxWorks priority system facts** (opposite of Linux):

- Priority range: **0-255** where **0 is HIGHEST** priority (255 is lowest)
- Scheduling: Preemptive priority-based with optional round-robin time-slicing
- Key APIs: taskPrioritySet(), kernelTimeSlice() [Torontomu](https://www.ecb.torontomu.ca/~courses/ee8205/lectures/RTOS-VxWorks-RTX.pdf)
- Priority inheritance available via mutex boolean parameter

**VxWorks aerospace deployments** demonstrate why it's the industry standard:

- **Mars rovers:** Pathfinder (1997), Spirit, Opportunity, Curiosity—first commercial RTOS on Mars
- **Boeing 787 Dreamliner:** VxWorks 653 orchestrates **70+ applications from 15+ suppliers** in the Integrated Modular Avionics system
- **F-35 Lightning II:** Mission-critical avionics rely on VxWorks deterministic performance

**Toronto Metropolitan University Graduate Lecture**

- **URL:** [https://www.ecb.torontomu.ca/~courses/ee8205/lectures/RTOS-VxWorks-RTX.pdf](https://www.ecb.torontomu.ca/~courses/ee8205/lectures/RTOS-VxWorks-RTX.pdf)
- **Topics:** VxWorks task states, scheduling, routines (taskPrioritySet, kernelTimeSlice)
- **Format:** Graduate-level PDF lecture slides

## Conference talks provide expert-level depth

**Embedded Online Conference: "Understanding RTOSs in 45 minutes"** by Jean Labrosse

- **URL:** [https://embeddedonlineconference.com/session/Understanding_RTOSs_in_45_minutes](https://embeddedonlineconference.com/session/Understanding_RTOSs_in_45_minutes)
- **Creator:** Founder of Micrium, author of µC/OS
- **Topics:** Task management, scheduling, inter-process communication, RTOS internals
- **Duration:** 45 minutes

**Embedded Online Conference: "Modern Embedded Software Goes Beyond the RTOS"** by Miro Samek

- **URL:** [https://embeddedonlineconference.com/session/Modern_Embedded_Software_Goes_Beyond_the_RTOS](https://embeddedonlineconference.com/session/Modern_Embedded_Software_Goes_Beyond_the_RTOS)
- **Topics:** Active Object design pattern, hierarchical state machines, [Embeddedonlineconference](https://embeddedonlineconference.com/speaker/Miro_Samek) event-driven programming [Embedded Related](https://www.embeddedrelated.com/showarticle/951.php)
- **Duration:** ~1 hour 8 minutes

**FOSDEM Embedded, Mobile and Automotive Devroom** (Free annual conference)

- **2024:** [https://fosdem.org/2024/schedule/track/embedded-mobile-and-automotive/](https://fosdem.org/2024/schedule/track/embedded-mobile-and-automotive/)
- **Topics:** Linux, Zephyr, NuttX, FreeRTOS comparisons; RISC-V implementations; Sound Open Firmware
- **Format:** All videos available free on FOSDEM website

## Recommended learning path from the community

Reddit's embedded community recommends this progression based on FastBit Embedded Brain Academy's structure:

1. **Embedded C programming fundamentals** (Jacob Sorber's Introduction to C)
2. **ARM Cortex-M architecture** (Ben Eater for fundamentals, Phil's Lab for STM32)
3. **RTOS basics** (Digi-Key/Shawn Hymel's 12-part series)
4. **Deep RTOS understanding** (Quantum Leaps/Miro Samek—build your own RTOS)
5. **Real-time scheduling theory** (NPTEL course for RMS mathematics)
6. **Synchronization primitives** (ControllersTech for STM32 specifics)
7. **Advanced concepts** (CppCon talks, Embedded Online Conference sessions)

The total time investment for comprehensive RTOS mastery: approximately **40-60 hours** of video content plus hands-on practice. Starting with Shawn Hymel's Digi-Key series (3-4 hours) provides the fastest path to practical FreeRTOS competency, while Miro Samek's course (15+ hours for RTOS sections) delivers the deepest understanding of how real-time systems actually work under the hood.