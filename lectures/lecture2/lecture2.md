---
marp: true
theme: default
paginate: true
title: Lecture
author: ENEL452 — Fall 2024
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
