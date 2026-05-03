# Part 5: Real Code Execution Trace (Slides 108–133)

This section traces the execution of a C program where `main()` calls `myfun(1, 2)`.

**The Code:**
```c
int myfun(int a, int b) {
  int c = a + b;
  int d = a - b;
  return c + d;
}
```
- ## I. Main gains control (Slides 109–115)
  1.  **Prologue:** `main` sets up its own stack frame (pushing EBP, moving ESP).
  2.  **Alignment (Slide 111):** `and $0xfffffff0, %esp`.
    *   This weird instruction forces the stack pointer to be a multiple of 16 (ends in 0). This is an optimization for modern CPU cache lines.
  3.  **Argument Setup (Slides 112–114):**
    *   The code allocates space (`sub $0x20, %esp`).
    *   It moves the value `2` (Argument B) into `0x4(%esp)` (Top of stack + 4).
    *   It moves the value `1` (Argument A) into `(%esp)` (Top of stack).
    *   *Note:* It used `mov` instead of `push` here, but the result is identical: the arguments are sitting at the top of the stack.
  4.  **The Call (Slide 115):** `call 804841d <myfun>`.
    *   **Action:** Pushes the **Return Address** (the address of the *next* instruction in `main`) onto the stack.
    *   **Jump:** Moves execution to `myfun`.
- ## II. Myfun takes over (Slides 116–130)
  1.  **The Prologue (Slide 116):**
    *   `push %ebp` (Save main's EBP).
    *   `mov %esp, %ebp` (EBP now points to the new frame base).
  2.  **Stack Geography Check (Slide 118):**
    *   `%ebp` points to the Saved EBP.
    *   `%ebp + 4` is the Return Address.
    *   `%ebp + 8` is the first argument (`a` = 1).
    *   `%ebp + 12` is the second argument (`b` = 2).
  3.  **Local Variables:** `sub $0x10, %esp`. Space is made for `c` and `d`.
  4.  **The Logic (Slides 119–130):**
    *   The CPU moves `a` and `b` from the stack into registers (EAX, EDX).
    *   It performs the `add` and `sub` instructions.
    *   It stores the results back into the local variable space on the stack (`c` at `-0x4(%ebp)`, `d` at `-0x8(%ebp)`).
- ## III. The Epilogue & Return (Slides 131–133)
  1.  **`leave` Instruction (Slide 131):**
    *   This is a shortcut instruction that does two things:
        1.  `mov %ebp, %esp` (Restores the stack pointer, effectively deleting local variables `c` and `d`).
        2.  `pop %ebp` (Restores `main`'s base pointer).
  2.  **`ret` Instruction (Slide 132):**
    *   Pops the **Return Address** off the stack.
    *   Jumps EIP back to that address.
  3.  **Back in Main (Slide 133):**
    *   `main` resumes exactly where it left off.
    *   The return value of `myfun` is waiting in `%eax`.
  
  ---
- ### Key Technical Insight
  Notice on **Slide 122** how the code accesses arguments:
  `mov 0x8(%ebp), %edx` (Get argument `a`).
  It uses a **positive offset** from EBP to get arguments (because they were pushed *before* the function was called, so they are "higher" in memory).
  It uses a **negative offset** (like `-0x4(%ebp)`) to access local variables (because they are "lower" in memory, in the current frame).
  
  **This is the Buffer Overflow "Map":**
  *   If you write to a local variable (`-0x4`), and you write too much...
  *   You walk UP memory addresses:
    *   Smash Saved EBP (`0x0`).
    *   Smash **Return Address** (`+0x4`).
    *   *Game Over.*
  
  ---
- ### Study Questions for Part 5
  1.  **Offset Logic:** If `0x8(%ebp)` is the first argument, and `0x12(%ebp)` is the second argument, how many bytes "wide" is the first argument?
  2.  **The `leave` instruction:** Why is `mov %ebp, %esp` considered a "deallocation" of memory? Does it actually erase the data in the local variables? (Hint: No, it just moves the pointer. The data stays there as "garbage" until overwritten).
  3.  **Trace:** In Slide 115, the `call` instruction pushes the Return Address. Where does that address physically come from? (It is the value of EIP *before* the jump).
  
  ---