---
marp: true
theme: default
paginate: true
title: Lecture 3
author: Trevor Douglas
---

## C/Assembly Interface

To successfully call an assembly language function from C, or vice versa, requires understanding the Arm Architecture Procedure Call Standard (AAPCS).  <br>

See the file ![AAPCS](res/aapcs-2012.pdf)

I will cover the bare essentials of the AAPCS below. For deeper understanding, here is the recommended reading list from AAPCS (note: some good, some irrelevant):

---
## Arm Architecture Procedure Call Standard

> 📘 **Recommended Sections**  
> 1.2, 1.3, 2, 3, 4.1 (skip 4.1.1, 4.1.2),  
> 4.2, 4.3.x (skip 4.3.5),  
> 5.1, 5.1.1 (some names don’t apply to Cortex-M3),  
> 5.1.1.1, skip 5.1.2,  
> 5.2, 5.3 (skip 5.3.1.1), 5.4,  
> skip 5.5, 5.6,  
> skip all of 6,  
> read all of 7 except 7.1.7.x (bitfields)

---

The AAPCS defines a contract between the **caller** and **callee**, such as which registers each is responsible for saving.

- Registers **not required to be preserved**:  
  `r0`, `r1`, `r2`, `r3`, `r12`  
  → These are **caller-saved**. Caller must preserve if needed.

- Registers **automatically saved/restored by callee**:  
  `r4` through `r11`  
  → These are **callee-saved**.

---

## C/Assembly Interface

Further AAPCS rules:

- For simple cases, **parameters** are passed in:
  - `r0` → parameter 1  
  - `r1` → parameter 2  
  - `r2` → parameter 3  
  - `r3` → parameter 4

- Return value (simple cases):  
  - Returned in `r0`

---

## C/Assembly Interface

![Yiu AAPCS Diagram](images/yiu-fig8-1.png)

---