#### **1. Introduction to ISA**
  The **Instruction Set Architecture (ISA)** is the vocabulary of the computer.
  *   **ARMv8 vs. LEGv8:** The slides mention **LEGv8**. This is a specific subset of the commercial ARMv8 architecture designed for teaching. It removes the weird, complex edge-cases of the real world so you can learn the logic.
  *   *Note:* If you Google "ARMv8 assembly," 95% of it applies to LEGv8, but LEGv8 simplifies some syntax.
  *   **RISC:** LEGv8 is a **RISC** (Reduced Instruction Set Computer) architecture. This means it has a small set of simple instructions, rather than complex ones that do ten things at once.
#### **2. Arithmetic Operations**
  In LEGv8, arithmetic is rigid. You cannot just say `Add a + b + c + d`. You have to break it down.
  
  *   **Syntax:** `OPERATION Destination, Source1, Source2`
  *   **Example:** `ADD a, b, c` means $a = b + c$.
  *   **Design Principle 1: Simplicity favors Regularity.**
    *   *The Rule:* Every arithmetic instruction has exactly **three** operands.
    *   *The Why:* Hardware is physical silicon. If every instruction looks the same (3 operands), the hardware engineers only have to build one type of circuit to handle them. If you had some instructions with 1 operand and some with 5, the hardware becomes complex and slow.
  
  **Translation Example:**
  C Code: `f = (g + h) - (i + j);`
  Assembly:
  ```assembly
  ADD t0, g, h    // t0 is a temporary register holding (g+h)
  ADD t1, i, j    // t1 is a temporary register holding (i+j)
  SUB f, t0, t1   // f = t0 - t1
  ```
  *(Note: You must calculate the sub-parts first, then combine them.)*
#### **3. Register Operands**
  This is the most fundamental concept in the chapter.
  *   **The "Bricks":** The CPU does not work directly on data in RAM (Memory). It works on **Registers**.
  *   **The Register File:** LEGv8 has **32 registers**, each **64-bits** wide.
    *   **Notation:** `X0` to `X30`.
    *   **Doubleword:** A 64-bit chunk of data is called a doubleword.
  *   **Design Principle 2: Smaller is faster.**
    *   *The Why:* Why not 1,000 registers? Because electricity has to travel physically. A bank of 32 registers is tiny and lightning fast. A bank of 1,000 registers would be physically larger, increasing the time it takes to find and retrieve data (clock cycle time).
  
  **The Register Roles (Memorize Slide 8 - Critical):**
  You cannot just use any register for anything. By convention, they have jobs:
  *   **X0 – X7:** Arguments for functions and Return values.
  *   **X9 – X15:** Temporary variables (scratchpad).
  *   **X19 – X27:** Saved variables (variables meant to survive a function call).
  *   **X28 (SP):** Stack Pointer (points to top of memory stack).
  *   **X29 (FP):** Frame Pointer.
  *   **X30 (LR):** Link Register (remembers where to go back to after a function finishes).
  *   **XZR (Register 31):** **The Zero Register.** It always contains the value `0`. If you try to write to it, the write is ignored.
#### **4. Memory Operands**
  Since we only have 32 registers, we can't store a whole database or image in them. We store bulk data in **Memory (RAM)** and copy it to registers only when we need to do math on it.
  
  *   **The Analogy:**
    *   **Memory:** The Public Library (Huge, holds everything, slow to find a book).
    *   **Registers:** The Desk in front of you (Small, only holds 32 books, instant access).
  *   **Data Transfer Instructions:**
    *   **Load (`LDUR`):** Copies data from Memory $\rightarrow$ Register.
    *   **Store (`STUR`):** Copies data from Register $\rightarrow$ Memory.
  
  **Calculating Addresses (The "Offset" Math):**
  Memory is a giant 1-dimensional array of **Bytes** (8 bits).
  However, registers hold **64 bits** (8 bytes).
  *   **Byte Addressing:** Every index in memory is 1 byte.
  *   **Alignment:** To get the next 64-bit number (doubleword) in an array, you must move **8 bytes** forward.
  
  **Example:**
  C Code: `A[12] = h + A[8];`
  *Assume array A starts at address in register `X22`.*
  *Assume `h` is in `X21`.*
  
  1.  **Get A[8]:** We need the 8th item. Each item is 8 bytes wide.
    *   Offset = $8 \times 8 = 64$.
    *   Instruction: `LDUR X9, [X22, #64]` (Load value at Base X22 + 64 bytes into temp X9).
  2.  **Add h:**
    *   Instruction: `ADD X9, X21, X9` (X9 = h + A[8]).
  3.  **Store in A[12]:** We need the 12th item location.
    *   Offset = $12 \times 8 = 96$.
    *   Instruction: `STUR X9, [X22, #96]` (Store result X9 into memory at Base X22 + 96 bytes).
#### **5. Immediate Operands**
  Sometimes you just want to add the number "4". You don't want to put "4" in memory, calculate its address, load it, and then add it.
  *   **Immediate Instructions:** Instructions that contain the constant number *inside* the instruction itself.
  *   **Syntax:** `ADDI X22, X22, #4` (Add Immediate).
  *   **Design Principle 3: Make the common case fast.**
    *   *The Why:* Adding small constants (like `i++` or `x + 1`) happens constantly in programming. Avoiding a Load from memory makes the CPU significantly faster.
  
  ---
### **Student "Check Your Understanding"**
  *Try to solve these based on Session 1 notes:*
  
  1.  **Translation:** Convert this C code to LEGv8 Assembly. Assume `a` is in X19, `b` is in X20, `c` is in X21. Use X9 for temporary data.
    `a = b + (c - 5);`
    *(Hint: You need `SUBI` for the subtraction).*
  2.  **Memory Math:** You have an array of 64-bit integers (doublewords) starting at memory address 1000. What is the memory address (in bytes) of `Array[3]`?
  3.  **Concept:** Why does `LDUR X0, [X1, #8]` load the *second* element of a 64-bit array, not the eighth element?