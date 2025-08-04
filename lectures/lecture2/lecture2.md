---
marp: true
theme: default
paginate: true
title: Lecture 2
author: Trevor Douglas
---

## A Bug in the Boeing 787

Here’s an illustrative problem, ripped from the headlines  
(April 2015).

---

## The Situation

In simulator tests of their 787 aircraft, Boeing discovered that the  
**4 redundant power control units** would **simultaneously shut down**  
after approximately **248 days** of continuous operation.

---

## The Fix

> Reboot the aircraft after at most 120 days.

😲 Really?  
The solution is **“reboot your Boeing 787”?**

---

## The Question

What is **likely the bug** here?

> Hint: it’s probably something to do with **time-keeping**.

🧮 A calculator might come in handy here.

---

## Second Hint

Most embedded systems keep track of time by incrementing a counter at a fairly short period:

- Usually **10 ms**, occasionally **1 ms**
- The counter value starts at zero on reboot
- It is stored in a **machine word**

---

## 🧠 Class Discussion
<!-- _note:


- What is the **maximum time** a 32-bit counter (in ms units) can run before overflow?
- How would you **safely handle time overflow** in long-running embedded systems?
- Why is relying on uptime counters risky in **safety-critical systems**?

-->

## Assignments & Lab Submissions

Your submissions must include all files needed to build and run your code. These may include:

- Source code files
- Test data files
- Test scripts
- Makefiles or other build tools
- Any required external libraries (ask first)
- Any documentation or helper files

---

## Submission Requirements (cont'd)

All source files **must** contain a header comment:
- File name
- Course: `enel452`
- Date
- Programmer
- Brief description

---

## 📌 Example Header!

```c
/**
 * FILENAME:
 *
 * DESCRIPTION:
 * This file is an example of how header information should be
 * displayed. This example is for C or C++ but gives an idea of what
 * information should be included in your file headers as well.
 *
 * AUTHOR: Trevor Douglas SID
 * DATE: 
 */
```
---

## Use Git Version Control (GitHub)

- All work must be version-controlled using **git**
- In fact this course is under GitHub Classroom.
- All assignments will be a link to create a private student repository.
- This will automatically allow access by me and our TA.
- No binary or derived files; only text (ASCII) files.

<!-- _note:

A GitHub optional tutorial will be the first lab session

-->
---

## Don't Submit Binaries

Do **not** include:
- Executables
- Object files
- Build artifacts

✅ Allowed:
- Static images (e.g., `.png`, `.jpg`) if essential and small

---
<!-- _note:

We do not want object files or derived files.  Typically something you are writing with some exceptions.

-->

## Basic Style Guidelines

- Consistent brace placement:
```c
int function() {
   // ...
}
// or
int function()
{
   // ...
}
```
- Constants should be:
```c
#define MAX_SIZE 128
const int MAX_SIZE = 128;
```
- UPPER_CASE for constants

<!-- _note:

Everyplace I worked at had their own coding style.  Some of the people in my software group complained about the style and my manager said:  "If you dont like the style, start your own company"

Quesion: what style do you thin had the biggest bone of contention:   Tabs vs Spaces.
-->

---

## Naming Conventions

Use **one consistent style**:
- `camelCaseStyle`
- `underscore_separated_style`

Pick one per project.

---

## Naming Guidelines (cont'd)

- Class/struct names: `UppercaseStart`
- Other variables: `lowercaseStart`
- Consistent style throughout

Example:
```c
int x = 27;
class Complex {}
```

---

## Document Class Fields

Every field in a struct/class **must** be documented:
```c
struct ReactorState {
  float temperature; // in K, > 300.0f
  Vec3 position;     // GPS coordinates
  float pH;          // range: 6.7 to 6.9
  float density;     // g/mL, 5.3 - 5.6
};
```

---

## Self-Documenting Code

- Use **descriptive names** for variables and functions
- Comments should describe the **why**, not the **how**

If the code is readable, comments may be minimal.

---

## Required Comments

- **File header**: Project, course, date, author, description
- **Class header**: Purpose, invariants, fields
- **Function header**: Purpose, inputs, outputs, side effects

Use block comments for complex logic.

---

## Use Whitespace Wisely

- Choose a consistent indentation style
- Keep lines < 80 characters
- Prefer spaces over tabs

```c
/**
 * Note: 1 TAB = 4 spaces
 */
```

---

## Variable Naming Scope

- Small scope = short names (e.g., `i`)
- Large/global scope = descriptive names
- Prefix global vars (e.g., `global_resource_table`)

---

## Avoid Magic Numbers

Replace magic numbers with constants:
```c
#define TABLE_ROWS 20
for (int i = 0; i < TABLE_ROWS; ++i)
  printf("%d", i);
```

Only leave literals if they’re fundamental to the algorithm.

---

## Clean Up Your Code

- Remove dead/commented-out code before submission
- Use git history if you need to revisit deleted code

---

## Debugging and Test Code

- Use macros (e.g., `assert`) for debugging output
- Separate large test code into a dedicated file/directory

```
proj/
  code/
  test/
  docs/
```

---

## Modular Code

- One function = one job
- Name functions clearly
- Avoid long functions and duplicate code
- Use helper functions and clean interfaces

---

## Keep `main` Small

`main.c` should:
- Only coordinate the high-level flow
- Delegate all low-level tasks to other modules

---

## Think About Failure Modes

- What if input is huge?
- What if `malloc()` fails?
- What if the connection drops?

In C++, use **RAII** to manage resources safely.

---

## Resource Cleanup

- Free all memory, close files/sockets
- Use tools like **valgrind** to check for leaks

C++ RAII pattern:
1. Acquire in constructor
2. Use
3. Release in destructor

---

## Embedded Systems - Confronting the Challenges

Embedded systems are difficult to design, test, and maintain:

- Apply **software engineering principles**:
  - High cohesion, loose coupling
  - Modularity: separate functionality, avoid abstraction mixing
  - Encapsulation: define robust interfaces
- Think with an **object-oriented mindset**

---

## System Decomposition

How do we break a system into parts?

- Identify **physical objects**: sensors, communication channels
- Use **OO design techniques** from CS courses
- Objects exist at many levels:
  - Subsystems
  - The full system as an object interacting with others

---

## Real-Time Definitions

**Response time** = Time between input presentation and all outputs appearing

---

## Hard vs Soft Real-Time Systems

- **Hard real-time**: missed deadlines have dire consequences
- **Soft real-time**: missed deadlines are tolerable but inconvenient
- Hard response time → high criticality

---

## Firm Real-Time Systems

- Allow a low probability of missing a deadline
- Example: telephone switches, online transaction systems
- Similar to soft RT systems but formalized with probabilities

---

## Fast vs Slow Systems

- Rule of thumb: divide at 1-second response time
- <1s response often needs more architectural planning

---

## Four Categories of RT Systems

- **Hard-Fast**: airbag, anti-skid, avionics
- **Hard-Slow**: missile defense, often AI-powered
- **Soft-Fast**: user interfaces, tolerant to missed inputs
- **Soft-Slow**: monitoring and analysis

---

## Informal Real-Time Systems

- Any computer system may be *informally* considered soft real-time
- True RT systems have **explicit timeliness engineering**

---

## Impact of Criticality on Architecture

- High-criticality systems may use **triple redundancy**
- Low-criticality (soft) systems may not need it

---

## Event-Driven Behavior

- Most RT systems are **reactive**
- Events may be:
  - Periodic
  - Aperiodic
  - Sporadic
- Also possible: internal events (e.g. timers)

**Deadline** = absolute time by which system must respond

---

## Where Do Deadlines Come From?

- Deadlines are driven by **real-world constraints** (laws of physics)
- Key challenge: know why and where constraints come from
- Try to **relax constraints** if safely possible

---

## Phases of Response Time

1. Time for sensor to detect event
2. Time for hardware to respond
3. Time to recognize/process event
4. Time to dispatch task
5. **Execution time** (often the largest phase)

---

## CPU Utilization

- % of time CPU does useful work
- **Utilization (U)** quantifies this

---

## Utilization Formula

Let:
- `n` = number of tasks
- `Cᵢ` = worst case execution time (WCET) of task *i*
- `Pᵢ` = period of task *i*

Then:

```math
U = \sum_{i=1}^n \frac{C_i}{P_i}
```

---

## Utilization Guidelines

- Over 100% → **time overloaded**
- Design target: **50%–68%** CPU loading
- Other factors influence the design beyond this number

---

## Related Disciplines

RT system design draws from:

- Control theory
- Programming languages
- Scheduling theory (operations research)
- Algorithms
- Queueing theory
- Operating systems
- Software engineering
- Computer architecture
- Data structures

---
## I/O addressing

- I/O addressing is one subject in which beginner students often have a noticeable lack of knowledge.
- when entering the job market. Some particular areas that need emphasis:
  - understanding the distinction between I/O-mapped devices, memory mapped devices, and protocl based devices.
  - when to use volatile and when to use const (and when to use both)

---

## I/O Addressing

Microcontrollers access I/O devices in several ways:

- **I/O mapped**: Devices have a separate address space (e.g., Intel `in`/`out`)
- **Memory mapped**: Devices appear as memory (used by most micros)
- **Protocol-based**: I2C, SPI, etc. → ultimately map to device registers

➡️ Our microcontroller uses **memory-mapped I/O**

---

## Direct Register Access in Assembly

Set bit 5 of memory word at `0xb0000000` using R1:

```asm
setBit5:
  mov r1, =0xb0000000
  ldr r2, [r1]
  orr r2, r2, #32
  str r2, [r1]
  bx lr
```

---

## Equivalent C Code

Use `volatile` to prevent compiler optimization:

```c
#include <stdint.h>

uint32_t volatile * const REG = (uint32_t volatile *) 0xb0000000;

void setBit5(void) {
    *REG |= (1 << 5);
}
```
---

## Volatile Example 

```c
#include <stdint.h>

uint32_t volatile * const REG = (uint32_t volatile *) 0xb0000000;
```

➡️ `volatile` ensures correct behavior even under optimization.

<!-- _note
Great question — here’s an illustrative example showing how not using volatile can lead to the compiler optimizing away hardware register access (like *REG |= (1 << 5)), especially when the result is not used again.

⸻

🔍 Optimized-Away Example (Without volatile)

#include <stdint.h>

uint32_t * const REG = (uint32_t * const) 0xb0000000;

void setBit5(void) {
    *REG |= (1 << 5);
}
❌ What might happen here?

If the compiler sees that:
	•	REG points to a fixed address
	•	The value stored has no observable side effects
	•	The value at *REG is not used elsewhere

Then during optimization, it may remove the load/store entirely — because from a pure C perspective, this code just modifies memory and has no external impact (unlike writing to I/O).

What it means:
From the perspective of a regular C program, writing to memory (e.g., *REG = value) is only meaningful if that memory is used somewhere else later in the program. Otherwise, it’s like changing a variable and then never using it — the compiler thinks it’s a waste of time and can safely remove it.

Why it matters for embedded systems:
In hardware, writing to *REG = value might trigger something important, like:
	•	Turning on an LED
	•	Starting a timer
	•	Writing to a UART or GPIO pin
-->

---

## Using `#define` Instead

```c
#define REG ((uint32_t volatile *) 0xb0000000)

void setBit5(void) {
    *REG |= (1 << 5);
}
```

- Access is the same (`*REG`), but `#define` style is more flexible
- Dereferencing still required

<!--
In our case we are using Code generation and header files that will map our I/O.... However I may ask you about it !! So be ready

-->
---

## Understanding `volatile` and `const`

Examples:
```c
volatile uint32_t * TIMER;
uint32_t volatile * TIMER;
uint32_t * volatile TIMER;  // ⚠️ incorrect usage
```

- First two: TIMER is a pointer to `volatile uint32_t`
- Third: TIMER is a `volatile pointer` to non-volatile data

---

## Why `volatile` Is Needed

- We read/write special function registers frequently
- Compiler should **not optimize** these accesses
- Address itself (`TIMER`) can be in a register, but the data must be volatile

---

## Const Pointer Variants

```c
const uint32_t * RAM_START;
uint32_t const * RAM_START;
uint32_t * const RAM_START;
```

- First two: pointer to const data (RAM shouldn't be const!)
- Third: constant pointer to non-const data → ✅ correct

---

## Combined Const and Volatile

```c
volatile uint32_t * const TIMER;
uint32_t volatile * const TIMER;

// For read-only timers:
volatile const uint32_t * const TIMER;
uint32_t volatile const * const TIMER;
```

- Leftmost `volatile`: data is volatile
- Leftmost `const`: data is read-only
- Rightmost `const`: pointer can't change

---

## Optimization and `volatile`

- `volatile` tells compiler: **don’t remove or reorder** memory accesses
- Crucial for hardware registers

