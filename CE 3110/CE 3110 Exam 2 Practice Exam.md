# **CpE 3110 - Exam 2 (Practice)**
  **Allowed Materials:** Pencil, Eraser, 1-page Crib Sheet, LEGv8 Green Card. No calculators.
### **Question 1 (10 pts)**
  Show the content of the `X9` register in **64-bit binary** and **decimal** after executing the following LEGv8 instruction sequence. Assume that the type of value stored in `X9` is a signed integer:
  ```assembly
  SUBI X9, XZR, #4
  ADDI X10, XZR, #3
  ORR X9, X9, X10
  LSL X9, X9, #3
  ```
### **Question 2 (10 pts)**
  For the LEGv8 assembly instruction sequence given below, what is the **final value in register X19 in 64-bit binary and decimal?** Clearly show your work by tracing the value changes in the registers.
  ```assembly
        ADDI X9, XZR, #4
        ADD X19, XZR, XZR
  LOOP:   CBZ X9, DONE
        ADDI X19, X19, #3
        SUBI X9, X9, #1
        B LOOP
  DONE:   LSL X19, X19, #1
  ```
### **Question 3 (15 pts)**
  Find the IEEE 754 **64-bit double-precision** floating-point representation in binary for the decimal value **-13.375**. Show your normalization and biased exponent math.
### **Question 4 (15 pts)**
  Multiply the following IEEE 754 **single-precision** floating-point numbers and show each and every step (Align, Add, Normalize). Clearly show the final result in IEEE 754 single-precision format in binary.
  ```text
  0 1000 0011 1010 0000 0000 0000 0000 000
  0 1000 0001 1100 0000 0000 0000 0000 000
  ```
### **Question 5 (15 pts)**
  Using the **optimized** integer multiplication datapath, show every step and register value in calculating **$1101 \times 1010$** in binary. Note: 4-bit unsigned integer input operands are given for simplicity. Construct a trace table showing the Iteration, Step description, Multiplicand, and the 8-bit Product register.
### **Question 6 (15 pts)**
  A LEGv8 assembly instruction sequence is given below. Convert both the **`CBNZ`** and **`B`** instructions used in this sequence into **binary machine instructions**. Clearly justify your offset calculations.
  ```assembly
  LOOP: LSL X10, X1, #2
      ADD X11, X2, X10
      LDUR X12, [X11, #0]
      CBNZ X12, SKIP
      ADDI X1, X1, #1
      B LOOP
  SKIP: STUR X12, [X11, #8]
  ```
### **Question 7 (20 pts)**
  For the single-cycle LEGv8 processor datapath, consider the instruction **`STUR X5,[X6, #-16]`**. 
  1. State the required binary value (0, 1, or X) for the following three control signals: **`MemtoReg`**, **`ALUSrc`**, and **`RegWrite`**.
  2. Clearly justify *why* the hardware requires these specific values for this instruction to execute successfully.
  
  ***
  ***
  *(Stop here! Don't scroll down until you have finished the practice exam).*
  ***
  ***
  
  <br><br><br>
# **Practice Exam Answer Key \& Explanations**
### **Question 1 Solution**
  1. `SUBI X9, XZR, #4` $\rightarrow$ $X9 = 0 - 4 = -4$. 
   Binary: +4 is `0100`. Invert: `1011`. Add 1: `1100`. (So `...1111 1100`).
  2. `ADDI X10, XZR, #3` $\rightarrow$ $X10 = 0 + 3 = 3$. 
   Binary: `...0000 0011`.
  3. `ORR X9, X9, X10` $\rightarrow$ Bitwise OR of `-4` and `3`.
   ```text
     ...1111 1100 (-4)
   | ...0000 0011 ( 3)
   ------------------
     ...1111 1111 (-1)
   ```
   $X9$ is now `-1`.
  4. `LSL X9, X9, #3` $\rightarrow$ Shift left by 3 (Mathematically multiplies by $2^3 = 8$).
   `-1 * 8 = -8`.
   Shift `...1111 1111` left by 3: `...1111 1000`.
  
  **Final Answers:** 
  *   **Decimal:** `-8`
  *   **64-bit Binary:** `11111111 11111111 11111111 11111111 11111111 11111111 11111111 11111000`
### **Question 2 Solution**
  *Trace the loop:*
  *   **Init:** X9 = 4, X19 = 0.
  *   **Iter 1:** X9 is not 0. X19 = 0 + 3 = 3. X9 = 4 - 1 = 3.
  *   **Iter 2:** X9 is not 0. X19 = 3 + 3 = 6. X9 = 3 - 1 = 2.
  *   **Iter 3:** X9 is not 0. X19 = 6 + 3 = 9. X9 = 2 - 1 = 1.
  *   **Iter 4:** X9 is not 0. X19 = 9 + 3 = 12. X9 = 1 - 1 = 0.
  *   **Iter 5:** X9 is 0. `CBZ` triggers and jumps to `DONE`.
  *   **DONE:** `LSL X19, X19, #1` $\rightarrow$ Shifts X19 left by 1 (multiplies by 2). $12 \times 2 = 24$.
  
  **Final Answers:**
  *   **Decimal:** `24`
  *   **64-bit Binary:** `00000000 00000000 00000000 00000000 00000000 00000000 00000000 00011000`
### **Question 3 Solution**
  1.  **Sign:** Negative $\rightarrow S = 1$.
  2.  **Binary Magnitude:** $13 = 8+4+1 = 1101_2$. $0.375 = 3/8 = 1/4 + 1/8 = .011_2$. Combined = $1101.011_2$.
  3.  **Normalize:** Shift left 3 spaces $\rightarrow 1.101011 \times 2^3$. 
  4.  **Fraction:** Drop the hidden 1 $\rightarrow$ `1010 1100...0`.
  5.  **Exponent:** Bias for double precision is 1023. 
    Stored Exp = $3 + 1023 = 1026$. 
    $1026$ in 11-bit binary = $1024 + 2 = \mathbf{10000000010_2}$.
  6.  **Final 64-bit String:**
    `1 10000000010 1010110000000000000000000000000000000000000000000000`
### **Question 4 Solution**
  *Analysis:* 
  *   Num A: Exp = 131 $\to$ Actual Exp = 4. Significand = $1.101_2$.
  *   Num B: Exp = 129 $\to$ Actual Exp = 2. Significand = $1.110_2$.
  
  1.  **Align:** Shift Num B right by 2 spots (since $4-2=2$). 
    Num B becomes $0.01110_2 \times 2^4$.
  2.  **Add:** 
    ```text
      1.10100
    + 0.01110
    ---------
     10.00010
    ```
  3.  **Normalize:** $10.00010_2 \times 2^4 \rightarrow 1.00001_2 \times 2^5$.
    *   New Actual Exp = 5. 
    *   New Biased Exp = $5 + 127 = 132 = \mathbf{1000 0100_2}$.
    *   New Fraction = `0000 1000...0`.
  4.  **Final Result:**
    `0 10000100 00001000000000000000000`
### **Question 5 Solution**
  *   Multiplicand = `1101`
  *   Multiplier = `1010` (Starts in the lower half of the 8-bit Product register).
  *   Product initialized to `[0] 0000 1010`
  
  | Iteration | Step | Multiplicand | Product `[C] Upper Lower` |
  | :--- | :--- | :--- | :--- |
  | **0** | Initial values | `1101` | `[0] 0000 1010` |
  | **1** | LSB=0, No Add | `1101` | `[0] 0000 1010` |
  | | Shift Right | `1101` | `[0] 0000 0101` |
  | **2** | LSB=1, Add Mcand (*0000+1101*) | `1101` | `[0] 1101 0101` |
  | | Shift Right | `1101` | `[0] 0110 1010` |
  | **3** | LSB=0, No Add | `1101` | `[0] 0110 1010` |
  | | Shift Right | `1101` | `[0] 0011 0101` |
  | **4** | LSB=1, Add Mcand (*0011+1101=10000, C=1*) | `1101` | `[1] 0000 0101` |
  | | Shift Right | `1101` | **`[0] 1000 0010`** |
  
  *Check:* $13 \times 10 = 130$. Binary `10000010` is $128 + 2 = 130$.
### **Question 6 Solution**
  **1. `CBNZ X12, SKIP`**
  *   **Format:** CB-Format (`Opcode` | `Offset(19)` | `Rt`)
  *   **Opcode:** `10110101`
  *   **Offset:** `SKIP` is 3 lines *down*. Target - Current = $+3$. 
    19-bit binary: `0000000000000000011`.
  *   **Rt:** X12 = `01100`
  *   **Binary:** `10110101 0000000000000000011 01100`
  
  **2. `B LOOP`**
  *   **Format:** B-Format (`Opcode` | `Offset(26)`)
  *   **Opcode:** `000101`
  *   **Offset:** `LOOP` is 5 lines *up*. Target - Current = $-5$.
    Find 2's Complement of 5:
    +5: `...00101`. Invert: `...11010`. Add 1: `...11011`.
    26-bit binary: `11111111111111111111111011`.
  *   **Binary:** `000101 11111111111111111111111011`
### **Question 7 Solution**
  1.  **Signal Values:**
    *   **`MemtoReg`:** `X` (Don't Care)
    *   **`ALUSrc`:** `1`
    *   **`RegWrite`:** `0`
  2.  **Justifications:**
    *   **`MemtoReg` is X:** A store instruction (`STUR`) writes data into memory, it does NOT load data into a register. Because `RegWrite` is 0, the Register File completely ignores incoming data. Therefore, the hardware "doesn't care" whether the `MemtoReg` multiplexor routes ALU data or Memory data to the register file's write port.
    *   **`ALUSrc` is 1:** The ALU must calculate the physical memory address. To do this, it needs to add the Base Register (`X6`) to the immediate offset (`-16`). Setting `ALUSrc` to 1 forces the multiplexor to route the 64-bit Sign-Extended output of the offset into the ALU rather than a second register value.
    *   **`RegWrite` is 0:** `STUR` writes to Data Memory, not the Register File. If `RegWrite` were 1, the CPU would accidentally overwrite a valid register with garbage data during the execution cycle.
- ---
- ---
- ---
# **CpE 3110 - Exam 2 (Practice B)**
  **Allowed Materials:** Pencil, Eraser, 1-page Crib Sheet, LEGv8 Green Card. No calculators.
### **Question 1: Integer Addition & Overflow (15 pts)**
  Consider the two 4-bit binary numbers: **`1010`** and **`1011`**.
  1. Add these two numbers together in binary. What is the 4-bit sum and the carry-out bit?
  2. If these numbers are **unsigned**, did an overflow occur? Justify your answer.
  3. If these numbers are **signed (2's complement)**, did an overflow occur? Justify your answer.
### **Question 2: Division Hardware (10 pts)**
  In the Restoring Division hardware algorithm, the ALU subtracts the Divisor from the Remainder register during each iteration. 
  1. If the result of this subtraction is **negative**, what are the next two specific actions the hardware must take before the next iteration begins?
  2. If the result of this subtraction is **positive or zero**, what action does the hardware take?
### **Question 3: IEEE 754 Hex to Decimal (15 pts)**
  A single-precision IEEE 754 floating-point number is stored in memory as **`0xC1280000`**. 
  Convert this machine code back into its **decimal** representation. Clearly show the extraction of the Sign, Exponent, and Fraction fields.
### **Question 4: Floating-Point Multiplication (10 pts)**
  You are multiplying two single-precision IEEE 754 floating-point numbers. 
  *   Number A has a stored biased exponent of `10000101` (Decimal 133).
  *   Number B has a stored biased exponent of `10000011` (Decimal 131).
  
  Assuming the multiplication of their significands does *not* require any normalization shifts, what will be the **new stored biased exponent** in the final product? Show your work and write the final exponent in both decimal and 8-bit binary.
### **Question 5: Single-Cycle Performance Timing (15 pts)**
  Assume the hardware components in the LEGv8 single-cycle datapath have the following operating delays:
  *   Memory (Instruction or Data): $200\text{ ps}$
  *   ALU: $100\text{ ps}$
  *   Register File (Read or Write): $50\text{ ps}$
  
  1. What is the total execution time (in ps) for an **`ADD`** instruction? 
  2. What is the total execution time (in ps) for an **`LDUR`** instruction?
  3. What is the minimum clock cycle time required for this processor? Justify your answer.
### **Question 6: Branch Target \& PC Logic (15 pts)**
  Consider the instruction: **`CBZ X5, #-8`**
  1. What is the exact **64-bit binary output** of the "Shift left 2" unit for this instruction?
  2. Write the Boolean logic equation that controls the PC Multiplexor (which chooses between `PC + 4` and the `Branch Target Address`) using the control signals **`Uncondbranch`**, **`Branch`**, and the ALU's **`Zero`** flag.
### **Question 7: Control Signals (20 pts)**
  For the single-cycle LEGv8 processor datapath, find the appropriate binary values (0, 1, or X for Don't Care) for the control signals to execute the instruction **`LDUR X9,[X10, #32]`**. 
  
  Fill in the values for the following 5 signals, and briefly justify *why* the hardware requires these values:
  1. `Reg2Loc`
  2. `ALUSrc`
  3. `MemtoReg`
  4. `MemRead/MemWrite` (2-bit signal: 00=Hold, 10=Read, 01=Write)
  5. `RegWrite`
  
  ***
  ***
  *(Stop here! Don't scroll down until you have finished the practice exam).*
  ***
  ***
  
  <br><br><br>
# **Practice Exam B Answer Key \& Explanations**
### **Question 1 Solution**
  1. **Addition:**
   ```text
     Carry: 1 0 1 0 
            1 0 1 0
          + 1 0 1 1
          ---------
     Sum: 1 0 1 0 1
   ```
   **4-bit Sum:** `0101`. **Carry-out bit:** `1`.
  2. **Unsigned Overflow:** **YES.** In unsigned math, an overflow happens if and only if the Carry-Out of the MSB is `1`. ($10 + 11 = 21$, which exceeds the max unsigned 4-bit capacity of 15).
  3. **Signed Overflow:** **YES.** In signed 2's complement math, an overflow happens if adding two numbers of the same sign results in the opposite sign. Here, we added a negative (`1010` / -6) and a negative (`1011` / -5), but the 4-bit sum resulted in a positive number (`0101` / +5).
### **Question 2 Solution**
  1. **If result is negative:** The hardware "subtracted too much." It must (1) shift a **`0`** into the Quotient register, and (2) **Restore** the original value by adding the Divisor back to the Remainder register.
  2. **If result is positive/zero:** The subtraction was successful. It must shift a **`1`** into the Quotient register. (No restore is needed).
### **Question 3 Solution**
  1. **Hex to Binary:** `C` = `1100`, `1` = `0001`, `2` = `0010`, `8` = `1000`.
   Binary: `1100 0001 0010 1000 0000 0000 0000 0000`
  2. **Slice:**
   *   **Sign:** `1` $\rightarrow$ Negative.
   *   **Exponent:** `1000 0010` $\rightarrow$ Decimal $128 + 2 = 130$.
   *   **Fraction:** `010 1000...000`
  3. **Calculate Exponent:** Actual Exp = Stored Exp - Bias = $130 - 127 = \mathbf{3}$.
  4. **Calculate Significand:** Add hidden `1.`. $\rightarrow 1.0101_2$.
  5. **Combine:** $-1.0101_2 \times 2^3$. Shift decimal right 3 places: $-1010.1_2$.
  6. **Convert to Decimal:** $1010_2 = 10$. $.1_2 = 0.5$. 
   **Final Answer: -10.5**
### **Question 4 Solution**
  *The Exponent Trap:* If you just add the two stored exponents, you add the bias of 127 twice!
  *   **Formula:** $\text{New Stored Exp} = (\text{Stored}_1 + \text{Stored}_2) - \text{Bias}$
  *   **Calculation:** $(133 + 131) - 127 = 264 - 127 = \mathbf{137}$.
  *   **Binary:** $137 = 128 + 8 + 1 = $ **`1000 1001`**.
  *(Sanity check: Actual exps are $133-127=6$ and $131-127=4$. $6+4 = 10$. $10 + 127 = 137$. Math checks out!).*
### **Question 5 Solution**
  1. **`ADD` Execution Time:** Fetch(200) + RegRead(50) + ALU(100) + RegWrite(50) = **$400\text{ ps}$**. *(Data memory is skipped).*
  2. **`LDUR` Execution Time:** Fetch(200) + RegRead(50) + ALU(100) + DataMem(200) + RegWrite(50) = **$600\text{ ps}$**.
  3. **Minimum Clock Cycle Time:** **$600\text{ ps}$**. 
   *Justification:* In a single-cycle processor, the clock cycle must be long enough to accommodate the *longest* possible instruction (the critical path), which is always the Load instruction. If the clock were faster (e.g., $400\text{ps}$), the `LDUR` instruction would not finish reading from memory before the next cycle began.
### **Question 6 Solution**
  1. **Shift Left 2 Math:**
   *   Instruction Offset = `-8`.
   *   Binary `+8` = `000...01000`. Invert = `111...10111`. Add 1 = `111...11000`.
   *   Sign-extended 64-bit: `1111...1111 11000`.
   *   Shift Left 2 (append `00`): **`1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1111 1110 0000`** 
   *(Decimal check: $-8 \text{ instr} \times 4 \text{ bytes} = -32$. `1110 0000` is 2's complement for -32).*
  2. **Boolean Logic Equation:**
   *   **`Mux Control = Uncondbranch OR (Branch AND Zero)`**
   *(The PC jumps if the instruction is an unconditional branch (B), OR if it is a conditional branch (CBZ) and the condition was met (Zero=1)).*
### **Question 7 Solution**
  1. **`Reg2Loc` = X (Don't Care).** `LDUR` only reads one register (`Rn` base address). It doesn't read a second register, so it doesn't matter what bits this mux sends to "Read Register 2".
  2. **`ALUSrc` = 1.** The ALU must calculate a memory address by adding the base register to the immediate offset. Setting this to 1 routes the 64-bit Sign-Extended offset into the ALU.
  3. **`MemtoReg` = 1.** We are pulling data from Data Memory. This mux must route the output of the Data Memory back to the Register File.
  4. **`MemRead/MemWrite` = 10.** We are reading data from the Data Memory.
  5. **`RegWrite` = 1.** `LDUR` places the fetched data into a destination register (`X9`), so we must assert the write enable signal on the Register File.