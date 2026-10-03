### Scope 
  The professor stated **"first 47 lecture slides only."**
  *   **INCLUDED:** Arithmetic, Logical operations, Branching (`if`, `loop`), Instruction Formats, Memory access (`LDUR`/`STUR`).
  *   **EXCLUDED:** Slide 48 starts "Procedure Calling." This means **The Stack (`SP`), Push/Pop, Recursion, and the `fact` function** are technically **NOT** on the exam based on the slide count, even though they were on the homework.
  *   *My Advice:* Focus 95% of your energy on the topics below. Put the recursion logic in a small corner of your crib sheet just in case, but prioritize the list below.
  
  ---
### **Topic 1: Performance (Chapter 1)**
  *This will likely be the math portion of the exam.*
  
  **1. The "Iron Law" of Performance**
  *   **Formula:** $\text{CPU Time} = \text{Instruction Count} \times \text{CPI} \times \text{Clock Cycle Time}$
  *   **Alternate:** $\text{CPU Time} = \frac{\text{Instruction Count} \times \text{CPI}}{\text{Clock Frequency}}$
  *   **Conversions:** Know that $1\text{GHz} = 10^9\text{ Hz}$ and $1\text{ns} = 10^{-9}\text{ seconds}$.
  
  **2. Performance Ratio (Speedup)**
  *   **The Formula:** $\frac{\text{Performance}_A}{\text{Performance}_B} = \frac{\text{Execution Time}_B}{\text{Execution Time}_A} = n$
  *   **The Rule:** Put the **SLOWER** (Old/Larger) time on top to get a number $> 1$.
  *   **Example:** "Machine A is 1.25 times faster than Machine B."
  
  **3. Amdahl’s Law**
  *   **Formula:** $\text{Speedup} = \frac{1}{(1 - \text{Fraction}_{enhanced}) + \frac{\text{Fraction}_{enhanced}}{\text{Speedup}_{enhanced}}}$
  *   **Concept:** You can only improve the part of the program that is actually being enhanced. If a feature is used 50% of the time, the max speedup is 2x, even if the feature becomes infinite speed.
  
  **4. The "Eight Great Ideas"**
  *   Be able to match the idea to a description (e.g., "Pipelining" = Laundry analogy, "Abstraction" = Hiding details).
  
  ---
### **Topic 2: Assembly Basics (Chapter 2)**
  *You must be able to read/write LEGv8 code.*
  
  **1. Registers**
  *   **X0 - X30:** 64-bit registers.
  *   **XZR (X31):** Zero register. Always reads 0. Writes are ignored.
  *   **Data Size:** "Doubleword" = 64 bits (8 bytes). "Word" = 32 bits (4 bytes).
  
  **2. Arithmetic Operations**
  *   `ADD`, `SUB`, `ADDI`, `SUBI`.
  *   **Syntax:** `OP Dest, Source1, Source2` (e.g., `ADD X1, X2, X3` means $X1 = X2 + X3$).
  *   **Immediate:** `ADDI X1, X2, #10`. No `SUBI` opcode exists in machine code (it uses `ADDI` with negative), but you can use `SUBI` in assembly.
  
  **3. Memory Access (Data Transfer)**
  *   **Loads:** `LDUR` (Load Unscaled Register). Moves Memory $\to$ Register.
  *   **Stores:** `STUR` (Store Unscaled Register). Moves Register $\to$ Memory.
  *   **Offset Math:** `LDUR X9, [X10, #16]`
    *   This loads from address $(X10 + 16)$.
    *   **Byte Addressing:** Memory is addressed by bytes. To access the next 64-bit doubleword in an array, you must add **8**, not 1.
  
  **4. Logical Operations**
  *   `AND`, `ORR`, `EOR` (Exclusive OR).
  *   `LSL` (Logical Shift Left): Multiplies by $2^n$.
  *   `LSR` (Logical Shift Right): Divides by $2^n$.
  
  ---
### **Topic 3: Logic & Branching (Slides 37-47)**
  *How to translate C `if` and `while` loops.*
  
  **1. Conditional Branches**
  *   `CBZ register, Label`: Branch if Zero.
  *   `CBNZ register, Label`: Branch if Not Zero.
  *   **Inverting Logic:** To implement `if (a == b) do X;`, assembly usually does `SUB X9, a, b; CBNZ X9, Exit;` (If $a-b \neq 0$, skip X).
  
  **2. Loops**
  *   Know the structure:
    1.  Label at the top.
    2.  Check condition (`CBZ` to exit).
    3.  Do body.
    4.  Increment counters.
    5.  `B Label` (Unconditional branch back to top).
  
  ---
### **Topic 4: Machine Code (The Binary)**
  *You will need your Green Card for this.*
  
  **1. Instruction Formats**
  You must know which format corresponds to which instruction (Check the Green Card columns!).
  *   **R-Format:** `ADD`, `SUB`, `AND`, `ORR`. (Reg, Reg, Reg).
  *   **D-Format:** `LDUR`, `STUR`. (Reg, Address).
  *   **I-Format:** `ADDI`, `SUBI`. (Reg, Reg, Immediate).
  *   **B-Format:** `B`. (Branch).
  *   **CB-Format:** `CBZ`, `CBNZ`. (Conditional Branch).
  
  **2. Decoding (Hex $\to$ Assembly)**
  *   Convert Hex to 32-bit Binary.
  *   Look at the first 11 bits (Opcode) and find it on the Green Card.
  *   Slice the remaining bits according to the format (R, D, I, etc.).
  *   Convert binary fields to Decimal (for registers/immediates).
  
  **3. Encoding (Assembly $\to$ Binary)**
  *   **Registers:** Convert X-number to 5-bit binary (e.g., X9 = `01001`).
  *   **Immediates:** Convert number to binary.
  *   **D-Format Address:** Just the number (e.g., #16 $\to$ `000010000`).
  *   **Branch Address (CRITICAL):**
    *   The machine code stores the **Instruction Count**, not bytes.
    *   If you jump down 3 lines: Stored value is `3`.
    *   If you jump up to a `Loop` 4 lines above: Stored value is `-4`.
    *   **Don't forget:** 2's complement for negative jumps!
  
  ---
### **What goes on the Crib Sheet?**
  Since you have the Green Card, you don't need to write down opcodes. Put these on your sheet:
  
  1.  **Formulas:**
    *   $CPU Time = \frac{IC \times CPI}{Freq}$
    *   $Speedup = \frac{Time_{OLD}}{Time_{NEW}}$
    *   Amdahl's Law equation.
  2.  **Powers of 2 table:** ($2^5=32$, $2^{10}=1024$, etc.) for quick binary conversion.
  3.  **Hex to Binary table:** ($A=1010, B=1011...$)
  4.  **Branch Offset Rule:**
    *   Assembly Offset = Number of **Instructions** (Lines) to jump.
    *   Byte Offset = Instructions $\times$ 4.
    *   Machine Code = Instructions (Not bytes).
  5.  **2's Complement Steps:** (Invert + 1) for negative numbers.
  6.  **Instruction Format "Slices":**
    *   Draw the boxes for R, D, I, B, and CB formats so you don't have to decipher the tiny text on the Green Card under stress.
    *   *Example:* D-Format = `Op(11) | Address(9) | Op2(2) | Rn(5) | Rt(5)`