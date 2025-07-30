---
marp: true
theme: default
paginate: true
title: Course Outline
author: Trevor Douglas
---

## Intro to Embedded and Real-Time Systems

Why is taking an embedded real-time development course a desirable thing?

- You develop a skillset that straddles hardware and software — this can make you an extremely valuable employee.
- Embedded development is hard, and the skills it exercises can make you a more effective programmer in general.
- There are many billions of deployed microprocessor-based embedded systems.  
  Many of these have at least one real-time deadline.  
  You might just end up working in this area, but even if not, the *mindset* is justified and appropriate.

---

## Why Take This Course?

- Designing small embedded projects is fun!
- It's *really* inexpensive to get into — there are tons of cheap micros, free development environments, and a large amount of online information.
- Having a deep understanding from taking a class can give you an **order of magnitude boost**.

---

## What will we learn?

- Good software design
- Control of real-world things  
- Real Time Operating System
- Good use of Revision Control  
- Better developer skills
- Other skills to help with the 4th year project

---

## What will we do? (cont)

- Labs are focused on a project  
  - Lots of engineering involved  
  - Minimal hardware  
  - Will use [FreeRTOS](https://www.freertos.org) (widely used in industry)

- Assignments  
  - More general, not all focused on the project  
  - We'll try to touch on fundamental skills, or tool use

---

## Quick, but useful definitions:

- **Embedded system**:  
  A purpose-built system that interacts with the real world and includes a computer as a component.

- **Real-time system**:  
  A (generally embedded) system that must explicitly satisfy timeliness constraints or suffer serious consequences (i.e. failure).  
  *Example:* On a fly-by-wire aircraft, when the control stick is moved, the control surfaces must respond within a bounded time.

<!-- _note:
Various applications need different requirements.  Relate Nortel phone development vs Opuswave needing RAM to execute code from.-->
---

## Other notable features of embedded systems

- They tend to be *single-purpose* systems, specialized for a given application.

- They tend to be resource-constrained in several ways:  
  memory, processing capability, word-size, physical size, power, weight,  
  time-to-market, cost, ...

- They are inconspicuously running *the world*.

---

## Other notable features of embedded systems

- They often use very complex processing algorithms.

- They may include sophisticated *user interfaces*.

- They often have *real-time* deadlines  
  - Hard real-time  
  - Soft real-time
  
<!-- _note: 
Old systems (e.g. SCSI) required sustained data rates from the host. Hard real-time
Modern interfaces (e.g. SAS, Fibre Channel) + buffering can absorb delays. Soft real-time
-->

---

## Other notable features of embedded systems

- They usually deal with periodic things, but they frequently  
  must deal with phenomena that are *multirate* (multiple periods), or even *sporadic*.

- They must often continue to run despite the occurrence of one or more HW or SW faults.

- They are often in control of safety-critical systems.

- They often have to run for decades without failing, sometimes with minimal or no maintenance.

- They may need to operate in harsh environments.

- They may have unique security requirements.

<!-- _note:
One place I worked at was tape storage backup designed by HW Engineers.  Entirely state machine and if they got into an unknown state it bricke... bad design -->

---

## Five broad categories of embedded systems

- Those that interact primarily with humans and not the physical world  
  *(e.g., games, ATMs, mobile phones)*

- Control systems  
  *(e.g., automotive, robotics, flight control)*

- Signal processing  
  *(e.g., music, video, radar, sonar)*

- Networking and communications  
  *(e.g., mobile phones, network switches, routers)*

- Monitoring and logging

---

## In engineering one can usefully model most anything as a

- **system**.  A succinct definition: A system is a mapping of a
  set of inputs into a set of outputs.

- Any computing system can be usefully modelled this way.

  <img src="./images/rtsystem.png" width="400" />

  ---

---

## The computing system has a set of one or more inputs entering a  
black box (which performs the system function), and a set of one or  
more outputs exiting the black box.

- Most frequently, the system receives its inputs from  
  *sensors* and *input devices* and delivers its outputs  
  to *actuators* and *displays*.

---

## Typical architecture of microcontroller

<img src="images/typical-architecture.png" width="700" />

---

## The presence of the FPGA/ASIC implies the possibility of custom  
hardware (either in the field or at fabrication time), which in turn  
means there is another engineering tradeoff.

- do everything in software?  
- more flexible, can change by changing firmware.
- do everything in custom hardware?  
- lower power, lower cost (but probably higher NRE cost), better performance.
- It turns out that the hardware/software partitioning of an embedded  
system is a tricky business.
- In our case in this class: we have a fixed hardware platform with no  
FPGA. Our focus is *mostly* on software.

<!-- _note:
one place I work had software integrated with custom asics.
One of the HW design engineers explained it to me like in a plumbing situation.  The software adjusts the valves and pipes and the h/w is the water flowing through -->

---

## Example embedded system

- Embedded systems must deal with concurrency: events arrive in unpredictable patterns, and the system must cope.
- The decisions we make as we design a system to handle concurrency give rise to a number of *concurrency architectures*.

---

Let's take an evocative and familiar example: a temperature controller—the thermostat/furnace system in a house.

> The choice of concurrency architecture used to solve this problem results in very different systems.

---

## But first we need to identify some requirements!

Here’s a first pass view of the temperature controller requirements:

1. Monitor temperature of the house and turn the furnace on/off to keep it constant  
2. At bedtime turn down the temperature to save fuel, in the morning turn it back up to the normal setting  
3. Allow user to increase/decrease the temperature with a keypad

---

## Three solutions using classical architectural styles:

1. One big loop (BDL): polls sensors and system time, stores state in variables and controls the temperature accordingly. 
2. Finite State Machine (FSM): reacts to events from the outside world, moves between states, uses global variables for communication.
3. Independent tasks (OS/RTOS): tasks run concurrently and communicate via interprocess communication.

---

## These three designs map to real-time embedded system strategies

- **#1:** Simple, no concurrency, no [*race conditions*](https://en.wikipedia.org/wiki/Race_condition)  
  - Easy to break, hard to maintain, not flexible
- **#2 and #3:** More sophisticated, concurrent, potential for race conditions  
  - More modular, maintainable, but riskier

---

## **Example - implementation of design 3: independent tasks**

<!-- _note:
**Pseudocode for controlling the temperature

`
function monitor_temperature() {
  while (true) {
    temp = measure_temp()
    if (temp < setting)
      furnace_start()
    else if (temp > setting + hysteresis)
      furnace_stop()
  }
}
`

- hysteresis is some allowable delta from setpoint


`
function monitor_time_of_day() {
  while (true) {
    time = get_time()
    if (time == bedtime)
      setting = NIGHTTIME_TEMP
    else if (time == morning)
      setting = DAYTIME_TEMP
  }
}

function monitor_user_keypad() {
  while (true) {
    key = wait_for_keypress()
    if (key == UP)
      setting++
    else if (key == DOWN)
      setting--
  }
}
`
-->

---

## Why Three Tasks?

Notice that the problem was divided into *three* tasks (why three?).

Some communication is needed between tasks — this is accomplished via global (shared) variables, for example `setting`.  
> This is **evil**.

Apart from the communication problem (which is a **killer!**), this design has some good features:

- Each feature maps to a separate task, and each task is implemented as a forever loop — it’s a modular design  
- Each task responds to asynchronous or synchronous external events  
- As a result, the programmer can think of each task as an independent little project

---

## Concurrency and Race Conditions

What sorts of problems can occur in a system with concurrency, and  
*actual* race conditions?

---

## Therac-25: Computer-controlled radiotherapy

- Canadian company, deployed 11 systems in the 1980s  
- Single programmer, little formal background  
- Software undocumented, written in assembly language  
- Lots of asynchronous tasks handling user input/output, system monitoring, and control  
- Communication via global variables shared between tasks — no proper handling of concurrent access  
- ⚠️ At least **5 people died** due to radiation overdoses 100× the specified amount

---

## Therac-25: More Issues

- The company believed software was immune to failure:  
  > "*Software does not wear out like hardware*"
- Investigations found the software development process was deeply flawed

---

## Toyota Unintended Acceleration

- [Talk by Phil Koopman (2014-09-18)](https://betterembsw.blogspot.ca/2014/09/a-case-study-of-toyota-unintended.html)  
  *(Slides and streaming video available)*

  - Several deaths directly linked to software development process flaws
<!-- _note:
**Give example about 4th year project using an accelerometer and that their primary use was in airbags.  Engineering can really impact society.

-->
---

## Time is Hard in Software

- [Post by Phil Koopman](https://betterembsw.blogspot.ca/2016/02/a-nice-rant-about-representing-computer.html)  
  Links to a Computerphile video rant about time zones

- Includes a “rogues gallery” of software time-handling blunders

