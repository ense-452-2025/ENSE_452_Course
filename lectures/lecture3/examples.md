## Lecture 3 Examples

### Register preservation between C and called Assembly programs

+------------------------+------------------------+
| Register               | Responsibility         |
+------------------------+------------------------+
| r0, r1, r2, r3, r12    | Caller-saved           |
| r4, r5, r6, r7,        | Callee-saved           |
| r8, r9, r10, r11       |                        |
| r13 (sp), r14 (lr),    | Special purpose        |
| r15 (pc)               |                        |
+------------------------+------------------------+

```C
// Caller function
void mainFunction() {
    int x = 5;          // x may be stored in r0
    int y = 10;         // y may be in r1

    someFunction();     // call may overwrite r0 and r1!

    // If x and y were needed after this call, and caller
    // didn’t save them, they could be lost.
}
```

```armasm

; Callee function (someFunction)
someFunction:
    push {r4, r5}        ; callee must save r4, r5 if it uses them

    ; Do work here — may freely use r0–r3, r12 (no need to restore them)

    pop {r4, r5}         ; restore callee-saved regs before returning
    bx lr                ; return to caller
```
