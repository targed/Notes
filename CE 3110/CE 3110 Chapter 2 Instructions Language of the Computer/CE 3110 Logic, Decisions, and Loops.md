#### **1. The Stored Program Concept**
  Before moving to code, understand this concept.
  *   **The Idea:** Instructions (code) are just numbers (binary), exactly like data (integers/characters).
  *   **The Consequence:** Because code is data, we can write programs that modify other programs (Compilers, Linkers, Loaders). Both your `.exe` file and your `.docx` file are just sequences of binary sitting in the same memory.
#### **2. Logical Operations**
  Sometimes we don't want to do math; we want to manipulate the specific bits inside a register.
  
  *   **Shifts (`LSL`, `LSR`):** Moving bits left or right.
    *   **LSL (Logical Shift Left):** Moves bits left, fills with 0s.
        *   *Math equivalent:* Multiplying by $2^i$. (Shifting left by 3 is like multiplying by $2^3 = 8$).
    *   **LSR (Logical Shift Right):** Moves bits right, fills with 0s.
        *   *Math equivalent:* Dividing by $2^i$ (Integer division).
  *   **AND (`AND`):**
    *   *Function:* Output is 1 only if *both* inputs are 1.
    *   *Use Case:* **Masking.** If you want to isolate specific bits (e.g., force the first 60 bits to 0 and keep the last 4), you `AND` the register with a "Mask" of `...00001111`.
  *   **OR (`ORR`):**
    *   *Function:* Output is 1 if *either* input is 1.
    *   *Use Case:* **Setting bits.** If you want to force specific bits to become 1 (without changing the others), you `OR` them with 1s.
  *   **EOR (`EOR` - Exclusive OR):**
    *   *Function:* Output is 1 if inputs are *different*.
    *   *Use Case:* **Inverting (NOT).** If you EOR a bit with `1`, it flips (0$\to$1, 1$\to$0). If you EOR with `0`, it stays the same.
#### **3. Decisions: Conditional Branching**
  This is how we implement `if` statements and loops. The CPU "branches" (jumps) to a different line of code based on a value.
  
  **Basic Branches:**
  *   `CBZ register, Label`: Compare and Branch if **Zero**. (If register == 0, go to Label).
  *   `CBNZ register, Label`: Compare and Branch if **Not Zero**. (If register != 0, go to Label).
  *   `B Label`: **Unconditional** Branch. (Go to Label no matter what. Used for `else` or infinite loops).
  
  **Compiling Logic (The "Inverted" Strategy):**
  When translating C code to Assembly, we often invert the logic.
  *   *C Code:* `if (i == j) { f = g + h; }`
  *   *Assembly Strategy:* We don't say "If equal, do the math." We usually say "If **NOT** equal, **skip** the math."
  
  *   *Example:*
    ```assembly
    SUB  X9, X22, X23      // Calculate difference (i - j)
    CBNZ X9, Else          // If difference is NOT zero (i != j), jump to Else
    ADD  X19, X20, X21     // (This is the 'Then' block: f = g + h)
    B    Exit              // Skip the Else block
    Else: SUB X19, X20, X21 // (The Else block)
    Exit:
    ```
  
  **Loops:**
  A loop is just a label at the top and a conditional branch at the bottom that jumps back up.
#### **4. Basic Blocks**
  *   **Definition:** A sequence of instructions with **one entry point** (at the top) and **one exit point** (at the bottom).
  *   *Why it matters:* Compilers look for these blocks to optimize code. If there are no branches inside a block, the compiler knows that if the first instruction runs, *all* of them will run, allowing for aggressive optimization.
#### **5. Advanced Conditionals (Flags)**
  `CBZ`/`CBNZ` are limited (only checks vs Zero). What if we want to check `if (a > b)`?
  We use **Condition Codes (Flags)**.
  
  *   **The Flags (NZVC):** 4 bits in the CPU that record the status of the *last* arithmetic operation.
    *   **N (Negative):** Result was negative.
    *   **Z (Zero):** Result was zero.
    *   **V (Overflow):** Signed overflow occurred.
    *   **C (Carry):** Unsigned carry occurred.
  *   **Setting the Flags:**
    *   Standard instructions (`ADD`, `SUB`) **do not** set flags.
    *   **Flag-setting instructions** end in **S** (`ADDS`, `SUBS`).
  *   **The Workflow:**
    1.  Perform a subtraction to compare two numbers (`SUBS X9, A, B`).
    2.  Use a **Conditional Branch** instruction that looks at the flags:
        *   `B.EQ`: Branch if Equal (Z=1).
        *   `B.NE`: Branch if Not Equal (Z=0).
        *   `B.LT`: Branch if Less Than (signed).
        *   `B.GT`: Branch if Greater Than (signed).
#### **6. Branch Formats & Addressing**
  How does the machine know *where* to jump?
  
  *   **PC-Relative Addressing:**
    *   The "Label" in assembly is converted into a **Number** (Offset).
    *   The CPU calculates: `Target Address = Current PC + Offset`.
    *   *Note:* Since all instructions are 32-bits (4 bytes), the offset in the machine code usually counts **instructions**, not bytes. The CPU automatically multiplies the offset by 4.
  *   **Formats:**
    *   **B-Format:** Used for unconditional `B`. Has a massive 26-bit offset (can jump very far).
    *   **CB-Format:** Used for `CBZ`/`CBNZ`. Has a 19-bit offset (can jump moderately far).
  
  ---
### **Student "Check Your Understanding"**
  *Try to solve these based on Session 3 notes:*
  
  1.  **Logical Math:** If you have the number `5` in a register. You perform `LSL` (Left Shift) by 3. What is the decimal value of the result?
  2.  **Coding Logic:** You want to implement `if (X19 < X20) GoTo Label;`. Write the LEGv8 instructions to do this. (Hint: Use `SUBS` and a specific Branch instruction).
  3.  **Branch Offsets:** The instruction `B Loop` is at memory address 1000. The label `Loop` is at memory address 900.
    *   Is the offset positive or negative?
    *   Does the branch use B-Format or CB-Format?