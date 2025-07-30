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