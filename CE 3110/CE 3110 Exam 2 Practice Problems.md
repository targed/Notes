### **Topic: Addition, Subtraction & Overflow**
  
  **Problem 1: Unsigned Addition and Overflow**
  Add the two 4-bit **unsigned** binary integers `1110` and `0111`. What is the 4-bit sum and the carry-out bit? Does it cause an overflow? Justify your answer.
  
  **Solution 1:**
  1.  **Perform Addition:**
  ```text
  Carry: 1 1 1 0
       1 1 1 0
     + 0 1 1 1
     ---------
  Sum: 1 0 1 0 1
  ```
  2.  **Identify Bits:**
  *   **4-bit Sum:** `0101`
  *   **Carry-out bit:** `1`
  3.  **Overflow Check \& Justification:**
  *   **Yes, an overflow occurs.**
  *   *Justification:* For unsigned integers, an overflow is defined strictly by the carry-out of the Most Significant Bit (MSB). Because the carry-out is `1`, the true result ($14 + 7 = 21$) exceeds the maximum capacity of a 4-bit unsigned integer ($2^4 - 1 = 15$).
  
  ***
  
  **Problem 2: Signed Addition and Overflow**
  Add the two 4-bit **signed** (2's complement) binary integers `1110` and `0111`. What is the 4-bit sum and the carry-out bit? Does it cause an overflow? Justify your answer.
  
  **Solution 2:**
  1.  **Perform Addition:** (The binary math is identical to Problem 1).
  *   **4-bit Sum:** `0101`
  *   **Carry-out bit:** `1`
  2.  **Overflow Check \& Justification:**
  *   **No, an overflow does NOT occur.**
  *   *Justification:* For signed integers, an overflow can *only* happen if you add two numbers of the same sign and get a result of the opposite sign. Here, we are adding a negative number (`1110`, which is -2) and a positive number (`0111`, which is 7). Adding a positive and a negative number can **never** cause an overflow. The result `0101` is +5, which fits perfectly in the 4-bit signed range[-8 to +7].
  
  ***
  
  **Problem 3: Signed Subtraction and Overflow**
  Subtract the 4-bit signed integer `1011` from `0101` (i.e., `0101 - 1011`). What is the 4-bit result? Does it cause an overflow? Justify your answer.
  
  **Solution 3:**
  1.  **Convert Subtraction to Addition:** $A - B = A + (\text{2's complement of } B)$.
  2.  **Find 2's Complement of `1011`:**
  *   Invert bits: `0100`
  *   Add 1: `0101`
  3.  **Perform Addition (`0101 + 0101`):**
  ```text
  Carry: 0 1 0 1
       0 1 0 1
     + 0 1 0 1
     ---------
  Sum: 0 1 0 1 0
  ```
  4.  **Identify Bits:**
  *   **4-bit Result:** `1010`
  5.  **Overflow Check \& Justification:**
  *   **Yes, an overflow occurs.**
  *   *Justification:* We added two positive numbers (`0101` and `0101`, which are both +5). The result (`1010`) has a `1` in the MSB, making it a negative number (-6). Adding two positive numbers and getting a negative result signifies a signed overflow. (Decimal check: $5 - (-5) = 10$, which exceeds the max 4-bit signed value of +7).
  
  ***
  
  **Problem 4: The V-Flag (Hardware Overflow Detection)**
  In the CPU hardware, the Overflow flag ($V$) is calculated using the Carry-In ($C_{in}$) to the MSB and the Carry-Out ($C_{out}$) from the MSB using an XOR gate: $V = C_{in} \oplus C_{out}$. 
  Using the binary addition of `1000 + 1001`, calculate $C_{in}$, $C_{out}$, and $V$ to prove whether a signed overflow occurred.
  
  **Solution 4:**
  1.  **Perform Addition and track carries closely:**
  ```text
  Carry: 1 0 0 0  <-- (The carry into the MSB is 0)
     1 0 0 0  (-8)
   + 1 0 0 1  (-7)
   ---------
   1 0 0 0 1
   ^
   (Carry-out is 1)
  ```
  2.  **Identify the variables:**
  *   $C_{in}$ (to MSB) = `0`
  *   $C_{out}$ (from MSB) = `1`
  3.  **Calculate V:**
  *   $V = 0 \oplus 1 = \mathbf{1}$.
  4.  *Conclusion:* Since $V = 1$, a **signed overflow occurred**. (We added -8 and -7 and got +1, which is incorrect).
  
  ---
### **Topic: Multiplication Hardware**
  
  **Problem 5: Optimized Multiplication Tracing (Like Past Exam)**
  Use the **optimized** multiplication hardware to multiply the 4-bit unsigned integers `1011` (Multiplicand) and `1101` (Multiplier). Construct a trace table showing the 1) Iteration, 2) Step Description, 3) Multiplicand, and 4) 8-bit Product register.
  
  **Solution 5:**
  *Initial Setup:* 
  *   Multiplicand = `1011`
  *   Product Register (8 bits) = `[Carry] Upper | Lower`. Initially: `[0] 0000 1101` (Multiplier goes in the lower half).
  
  | Iteration | Step Description | Multiplicand | Product `[C] Upper Lower` |
  | :--- | :--- | :--- | :--- |
  | **0** | Initial values | `1011` | `[0] 0000 1101` |
  | **1** | LSB=1. Add Mcand to Upper | `1011` | `[0] 1011 1101` |
  | | Shift Product Right | `1011` | `[0] 0101 1110` |
  | **2** | LSB=0. No Add. | `1011` | `[0] 0101 1110` |
  | | Shift Product Right | `1011` | `[0] 0010 1111` |
  | **3** | LSB=1. Add Mcand to Upper<br>*(0010 + 1011 = 1101)* | `1011` | `[0] 1101 1111` |
  | | Shift Product Right | `1011` | `[0] 0110 1111` |
  | **4** | LSB=1. Add Mcand to Upper<br>*(0110 + 1011 = 10001 $\to$ C=1)* | `1011` | **`[1] 0001 1111`** |
  | | Shift Product Right | `1011` | `[0] 1000 1111` |
  
  *Final Result:* `10001111_2` ($143_{10}$). Check: $11 \times 13 = 143$. Correct.
  
  ***
  
  **Problem 6: Unoptimized vs Optimized Multiplication**
  If you are multiplying two 32-bit integers, compare the hardware requirements of the **unoptimized** multiplier versus the **optimized** multiplier. Specifically, list the bit-width size required for the ALU, the Multiplicand register, and the Product register in both designs.
  
  **Solution 6:**
  *   **Unoptimized Multiplier:**
    *   ALU Size: **64-bit**
    *   Multiplicand Register: **64-bit** (shifted left during operation)
    *   Product Register: **64-bit**
  *   **Optimized Multiplier:**
    *   ALU Size: **32-bit** (cuts ALU hardware cost in half!)
    *   Multiplicand Register: **32-bit** (stays stationary)
    *   Product Register: **64-bit** (shifts right, initially holds multiplier in lower half)
  
  ***
  
  **Problem 7: LEGv8 Multiplication Instructions**
  You are writing a LEGv8 assembly program. You need to multiply two 64-bit signed integers stored in `X1` and `X2`. Because the numbers are large, the result will require 128 bits. Write the **two** LEGv8 instructions needed to capture the full 128-bit product, storing the lower 64 bits in `X3` and the upper 64 bits in `X4`.
  
  **Solution 7:**
  LEGv8 cannot store 128 bits in one register, so the ISA provides separate instructions for the upper and lower halves.
  1.  `MUL X3, X1, X2`    *(Calculates the lower 64 bits of the product)*
  2.  `SMULH X4, X1, X2`  *(Calculates the upper 64 bits of the product for **S**igned integers)*
  
  ---
### **Topic: Division & Advanced Concepts**
  
  **Problem 8: Restoring Division Algorithm**
  In the restoring division algorithm, the hardware subtracts the Divisor from the Remainder register. 
  A) If the result of this subtraction is **negative**, what two actions must the hardware take?
  B) If the result of this subtraction is **positive or zero**, what action does the hardware take?
  
  **Solution 8:**
  *   **Part A (Negative Result):** The hardware realizes it "subtracted too much." It must:
    1.  Shift a **`0`** into the LSB of the Quotient register.
    2.  **Restore** the original value by adding the Divisor back to the Remainder register.
  *   **Part B (Positive/Zero Result):** The subtraction was successful. It must:
    1.  Shift a **`1`** into the LSB of the Quotient register. (No restore is needed; the subtracted value is kept).
  
  ***
  
  **Problem 9: Subword Parallelism (SIMD / NEON)**
  The ARMv8 architecture includes 128-bit NEON registers (V0-V31) for SIMD operations. If a graphics application needs to add two arrays of 16-bit RGB values together, how many 16-bit additions can a single NEON `ADD` instruction perform simultaneously? 
  
  **Solution 9:**
  *   A single NEON register is **128 bits** wide.
  *   The data elements are **16 bits** wide.
  *   $128 \div 16 = \mathbf{8}$.
  *   *Answer:* The hardware can perform **8** parallel 16-bit additions in a single clock cycle using one instruction.
  
  ***
  
  **Problem 10: 2's Complement Edge Case (The Asymmetry Trap)**
  Consider a 4-bit signed (2's complement) architecture. 
  A) What is the most negative number that can be represented? (Provide binary and decimal).
  B) If you attempt to negate this number using the standard 2's complement algorithm (Invert bits + 1), what binary number do you get? Explain why this happens.
  
  **Solution 10:**
  *   **Part A:** The most negative 4-bit signed number is **`1000`**, which is **-8** in decimal.
  *   **Part B:** 
    *   Step 1: Invert `1000` $\rightarrow$ `0111`.
    *   Step 2: Add 1 $\rightarrow$ `0111 + 1 = 1000`.
    *   *Result:* You get **`1000`** (-8) back!
    *   *Explanation:* 2's complement range is asymmetrical. A 4-bit system can hold -8 to +7. There is no positive +8 to represent the negation of -8. Therefore, trying to negate the most negative number causes a signed overflow, wrapping the number completely around the number line back to itself.
- ---
- ---
- ---
### **Topic: Decimal to IEEE 754 Conversion**
  
  **Problem 1: Single-Precision Conversion**
  Convert the decimal number **-13.5** into IEEE 754 Single-Precision format. Show your work and provide the final 32-bit binary string.
  
  **Solution 1:**
  1.  **Determine Sign:** The number is negative, so **$S = 1$**.
  2.  **Convert to Binary:**
    *   Integer part: $13 = 8 + 4 + 1 = 1101_2$
    *   Fractional part: $0.5 = 1/2 = .1_2$
    *   Combined: $1101.1_2$
  3.  **Normalize:** Shift the decimal point left 3 spaces to get $1.\text{XXXX}$.
    *   $1.1011_2 \times 2^3$
  4.  **Calculate Biased Exponent:**
    *   Actual Exponent = $3$.
    *   Bias for Single Precision = $127$.
    *   Stored Exponent = $3 + 127 = 130$.
    *   $130$ in binary = $128 + 2 =$ **`1000 0010`**.
  5.  **Determine Fraction:** Drop the hidden leading "1.".
    *   Fraction = `1011` (Pad with zeros to 23 bits: **`1011 0000 0000 0000 0000 000`**).
  6.  *Final Answer:* **`1 10000010 10110000000000000000000`**
  
  ***
  
  **Problem 2: Double-Precision Conversion**
  Convert the decimal number **6.25** into IEEE 754 Double-Precision format.
  
  **Solution 2:**
  1.  **Determine Sign:** The number is positive, so **$S = 0$**.
  2.  **Convert to Binary:**
    *   Integer part: $6 = 4 + 2 = 110_2$
    *   Fractional part: $0.25 = 1/4 = .01_2$
    *   Combined: $110.01_2$
  3.  **Normalize:**
    *   $1.1001_2 \times 2^2$
  4.  **Calculate Biased Exponent:**
    *   Actual Exponent = $2$.
    *   Bias for Double Precision = **$1023$**.
    *   Stored Exponent = $2 + 1023 = 1025$.
    *   $1025$ in 11-bit binary = $1024 + 1 =$ **`10000000001`**.
  5.  **Determine Fraction:** Drop the hidden "1.".
    *   Fraction = `1001` (Pad with zeros to 52 bits: **`1001 0000...000`**).
  6.  *Final Answer:* **`0 10000000001 1001000000000000000000000000000000000000000000000000`**
  
  ---
### **Topic: Decoding IEEE 754 to Decimal**
  
  **Problem 3: Hexadecimal Decoding**
  A single-precision floating-point number is stored in memory as `0xC0E00000`. What decimal value does this represent?
  
  **Solution 3:**
  1.  **Convert Hex to Binary:**
    *   `C` = `1100`, `0` = `0000`, `E` = `1110`...
    *   Binary: `1100 0000 1110 0000 0000 0000 0000 0000`
  2.  **Slice into Fields:**
    *   Sign: `1` (Negative)
    *   Exponent (8 bits): `1000 0001`
    *   Fraction (23 bits): `110 0000...000`
  3.  **Evaluate Exponent:**
    *   Stored Exponent = $128 + 1 = 129$.
    *   Actual Exponent = $129 - 127 = \mathbf{2}$.
  4.  **Evaluate Significand:**
    *   Add the hidden `1.` back to the front of the fraction.
    *   Significand = $1.11_2$.
  5.  **Calculate Value:**
    *   Value = $-1.11_2 \times 2^2$.
    *   Shift decimal right by 2: $-111.0_2$.
    *   Convert to decimal: $-(4 + 2 + 1) = \mathbf{-7.0}$.
  6.  *Final Answer:* **-7.0**
  
  ---
### **Topic: Floating-Point Addition Algorithm**
  
  **Problem 4: Step-by-Step FP Addition (Like Past Exam)**
  Add the following two single-precision IEEE 754 numbers. Show all 4 steps: Align, Add, Normalize, Round.
  *   Number A: `0 10000010 10000000000000000000000` (which is $1.1_2 \times 2^3$)
  *   Number B: `0 10000001 11000000000000000000000` (which is $1.11_2 \times 2^2$)
  
  **Solution 4:**
  1.  **Step 1: Align Binary Points**
    *   Num A Exponent = 3. Num B Exponent = 2.
    *   We must shift the significand of the *smaller* number (Num B) to the right by the difference ($3 - 2 = 1$ shift).
    *   Original Num B Significand: $1.11_2$
    *   Aligned Num B Significand: **$0.111_2 \times 2^3$**
  2.  **Step 2: Add Significands**
    ```text
      1.100  (Num A, padded with a zero)
    + 0.111  (Num B aligned)
    -------
     10.011
    ```
    *   Result: $10.011_2 \times 2^3$
  3.  **Step 3: Normalize**
    *   The result $10.011$ is not in $1.\text{X}$ format. We must shift the decimal point **left by 1**.
    *   Because we shifted left by 1, we must **add 1** to the exponent.
    *   Normalized Result: $1.0011_2 \times 2^4$.
    *   New Stored Exponent = $4 + 127 = 131 = \mathbf{1000 0011_2}$.
  4.  **Step 4: Round**
    *   Fraction is `001100...00`. It fits entirely in 23 bits, so no rounding is needed.
  5.  *Final Binary:* **`0 10000011 00110000000000000000000`**
  
  ***
  
  **Problem 5: Loss of Precision in Addition**
  You perform the floating-point addition $1.0_2 \times 2^{25} + 1.0_2 \times 2^0$ using a Single-Precision processor (which has 23 fraction bits). What is the exact result stored in the computer? Explain why.
  
  **Solution 5:**
  *   **The Result:** The result stored is exactly **$1.0_2 \times 2^{25}$** (the smaller number is effectively ignored).
  *   **Explanation:** During Step 1 of the addition algorithm (Alignment), the CPU takes the smaller number ($1.0 \times 2^0$) and shifts its significand right by the difference in exponents ($25 - 0 = 25$ shifts). 
  *   Because the Single-Precision format only has **23 bits of fraction**, shifting a number right by 25 positions pushes the `1` completely off the edge of the hardware registers. It is truncated/rounded to $0.0$. Thus, $X + \text{tiny} = X$.
  
  ---
### **Topic: Floating-Point Multiplication**
  
  **Problem 6: The Exponent Bias Trap**
  In a single-precision floating-point multiplication, Number A has a stored biased exponent of `10000010` (Decimal 130) and Number B has a stored biased exponent of `10000100` (Decimal 132). If you simply add these two stored binary fields together, the new exponent is wrong. 
  A) What mathematical step must you take to fix the sum?
  B) What is the correct new stored exponent (in decimal)?
  
  **Solution 6:**
  *   **Part A:** If you add two biased exponents together $(E_1 + 127) + (E_2 + 127)$, you have added the bias **twice** $(E_1 + E_2 + 254)$. To fix this, you must **subtract the bias (127)** from the sum.
  *   **Part B:** 
    *   Sum: $130 + 132 = 262$.
    *   Correction: $262 - 127 = \mathbf{135}$. 
    *   *(Sanity check: Actual exponents are $130-127 = 3$ and $132-127 = 5$. $3+5 = 8$. $8 + 127 = 135$. Correct!)*
  
  ***
  
  **Problem 7: FP Multiplication Algorithm**
  Multiply the two binary floating-point numbers $1.1_2 \times 2^2$ and $1.01_2 \times 2^3$. Show the 4 steps of the algorithm and the final normalized answer in scientific notation. Both are positive.
  
  **Solution 7:**
  1.  **Add Exponents:** Actual exponents are $2$ and $3$. $2 + 3 = \mathbf{5}$.
  2.  **Multiply Significands:**
    ```text
        1.10
      x 1.01
      ------
         110
        0000
      +11000
      ------
       1.1110
    ```
    *(Result: $1.111_2 \times 2^5$)*
  3.  **Normalize:** The result is $1.111_2$. This is already in $1.\text{XXXX}$ format, so no shift is required. Exponent remains $5$.
  4.  **Determine Sign:** Positive $\times$ Positive = Positive.
  5.  *Final Answer:* **$+1.111_2 \times 2^5$**
  
  ---
### **Topic: Special Values \& Edge Cases**
  
  **Problem 8: Recognizing Special Patterns**
  Identify the mathematical meaning of the following three IEEE 754 Single-Precision bit patterns (e.g., Infinity, NaN, Zero, Denormalized).
  A) `0 11111111 00000000000000000000000`
  B) `1 00000000 00000000000000000000000`
  C) `0 11111111 10000000000000000000000`
  
  **Solution 8:**
  The hardware looks at the Exponent to identify special cases:
  *   **A) +Infinity:** Exponent is all 1s (255), Fraction is exactly 0. Sign is 0 (Positive).
  *   **B) -Zero:** Exponent is all 0s, Fraction is exactly 0. Sign is 1 (Negative).
  *   **C) NaN (Not a Number):** Exponent is all 1s (255), Fraction is NON-zero.
  
  ***
  
  **Problem 9: Denormalized Numbers**
  A floating-point number has an Exponent field of `0000 0000` and a Fraction field of `1000...000`. 
  A) What type of number is this? 
  B) What is the value of the "hidden bit" for this number?
  C) What purpose do these numbers serve in computer architecture?
  
  **Solution 9:**
  *   **Part A:** This is a **Denormalized Number** (or Denormal).
  *   **Part B:** For denormalized numbers, the hidden bit is explicitly assumed to be **0** (i.e., $0.\text{Fraction}$).
  *   **Part C:** They allow for **gradual underflow**. They allow the computer to represent numbers that are closer to zero than the smallest possible *normalized* number, preventing a sudden hard crash to 0.0 when dealing with extremely tiny values.
  
  ---
### **Topic: Architecture (LEGv8 FP)**
  
  **Problem 10: LEGv8 Floating-Point Instructions**
  Write a LEGv8 assembly sequence to perform the following C operation:
  `result = a + b;`
  Assume `a` is a single-precision float located at the memory address in `X10`, `b` is a single-precision float located at the memory address in `X11`, and `result` should be stored at the memory address in `X12`.
  
  **Solution 10:**
  *Note: Because they are single-precision, we use the `S` registers and the `S` suffix on our instructions.*
  ```assembly
  LDURS S0,[X10, #0]    // Load 'a' into FP register S0
  LDURS S1, [X11, #0]    // Load 'b' into FP register S1
  FADDS S2, S0, S1       // Add them: S2 = S0 + S1
  STURS S2, [X12, #0]    // Store the result back to memory at X12
  ```
- ---
- ---
- ---
### **Topic: Logic Components & Instruction Tracing**
  
  **Problem 1: Combinational vs. Sequential Logic**
  In the LEGv8 single-cycle datapath, identify whether the following components are **Combinational** or **Sequential** logic elements. Explain the primary difference between the two types of logic.
  A) The ALU
  B) The Program Counter (PC)
  C) The Sign-Extend Unit
  D) The Data Memory
  
  **Solution 1:**
  *   **A) The ALU:** Combinational.
  *   **B) The Program Counter (PC):** Sequential.
  *   **C) The Sign-Extend Unit:** Combinational.
  *   **D) The Data Memory:** Sequential.
  *   *Explanation:* **Combinational** logic outputs depend *only* on the current inputs and have no memory (if you change the input, the output changes instantly). **Sequential** logic contains state (memory) and only updates its stored value when triggered by a clock edge.
  
  ***
  
  **Problem 2: Tracing an R-Type Instruction**
  Trace the instruction `SUB X1, X2, X3` through the datapath. 
  A) What two inputs go into the ALU?
  B) What value does the `MemtoReg` multiplexor pass to the Register File's "Write data" port?
  
  **Solution 2:**
  *   **Part A:** The ALU takes **Read Data 1** (the value of register X2) and **Read Data 2** (the value of register X3). The `ALUSrc` multiplexor is set to `0` to allow Read Data 2 into the ALU.
  *   **Part B:** The `MemtoReg` multiplexor is set to `0`, meaning it bypasses the Data Memory and passes the **ALU Result** directly back to the Register File to be written into X1.
  
  ***
  
  **Problem 3: Tracing a Load Instruction**
  Trace the instruction `LDUR X9,[X10, #32]` through the datapath. 
  A) What does the ALU calculate during this instruction?
  B) Does this instruction read from the Data Memory, write to the Data Memory, or neither?
  
  **Solution 3:**
  *   **Part A:** The ALU calculates the **Physical Memory Address**. It takes the base address from X10 and adds it to the sign-extended immediate offset (`32`).
  *   **Part B:** It **reads** from the Data Memory. The calculated address from the ALU goes into the Data Memory's "Address" port, the `MemRead` control signal is asserted (`1`), and the memory outputs the requested data.
  
  ---
### **Topic: Multiplexors \& Control Signals**
  
  **Problem 4: The ALUSrc Multiplexor**
  The `ALUSrc` multiplexor sits directly in front of the second input to the ALU. It chooses between two data sources. 
  A) What are the two data sources?
  B) For the instruction `ADD X5, X6, X7`, what is the value of the `ALUSrc` control signal (0 or 1)?
  C) For the instruction `STUR X5, [X6, #16]`, what is the value of the `ALUSrc` control signal (0 or 1)?
  
  **Solution 4:**
  *   **Part A:** Source `0` is **Read Data 2** from the Register File. Source `1` is the 64-bit output of the **Sign-Extend Unit**.
  *   **Part B:** `0`. An `ADD` instruction needs to do math on a second register (X7), so it selects Read Data 2.
  *   **Part C:** `1`. A `STUR` instruction needs to calculate an address by adding the offset (`#16`), so it selects the Sign-Extend output.
  
  ***
  
  **Problem 5: Generating Control Signals (Like HW4)**
  Determine the values (0, 1, or X for Don't Care) of the following control signals for the instruction `STUR X0,[X1, #-8]`.
  *   `RegWrite`
  *   `ALUSrc`
  *   `MemWrite`
  *   `MemtoReg`
  
  **Solution 5:**
  *   **`RegWrite` = 0**: A store instruction writes to memory, *not* to a register.
  *   **`ALUSrc` = 1**: The ALU needs to add the offset (`-8`) to the base register, so we select the sign-extended immediate.
  *   **`MemWrite` = 1**: We are writing data into the Data Memory.
  *   **`MemtoReg` = X (Don't Care)**: Because `RegWrite` is 0, the Register File is ignoring incoming data. It does not matter what value the `MemtoReg` multiplexor outputs.
  
  ***
  
  **Problem 6: The Reg2Loc Multiplexor Trap**
  The LEGv8 datapath features a `Reg2Loc` multiplexor right before the "Read register 2" input of the Register File. 
  A) Why does this multiplexor exist? (Hint: Look at the difference between R-Format and D-Format instructions).
  B) For the instruction `CBZ X9, Loop`, what field of the machine code (`Rm` or `Rt`) should the multiplexor select?
  
  **Solution 6:**
  *   **Part A:** In R-format instructions (like `ADD`), the second source register is `Rm` (bits 20-16). However, in D-Format (`STUR`) and CB-format (`CBZ`), the register being read is actually in the `Rt` position (bits 4-0). The `Reg2Loc` mux ensures the Register File reads the correct bits depending on the instruction format.
  *   **Part B:** It should select **`Rt`** (bits 4-0), because `CBZ` uses the `Rt` field to specify which register to test against zero.
  
  ---
### **Topic: Branching \& Address Calculation**
  
  **Problem 7: The "Shift Left 2" Unit**
  A `CBZ` instruction is located at memory address 100. It needs to jump **backwards by 6 instructions** to a label.
  A) What is the exact 64-bit output of the Sign-Extend unit?
  B) What is the exact 64-bit output of the Shift Left 2 unit? Show your work.
  
  **Solution 7:**
  *   **Part A (Sign-Extend):** 
    *   The offset is `-6`. 
    *   Find +6: `...00110`
    *   Invert bits: `...11001`
    *   Add 1: `...11010`
    *   Sign-extended 64-bit output: **`1111 1111...1111 1010`**
  *   **Part B (Shift Left 2):**
    *   Shift the bits left by 2 positions (append `00` to the right).
    *   Output: **`1111 1111...1110 1000`**
    *   *(Decimal verification: $-6 \text{ instructions} \times 4 \text{ bytes/instruction} = -24 \text{ byte offset}$. `1110 1000` in 2's complement is indeed -24).*
  
  ***
  
  **Problem 8: Conditional Branch Datapath (AND Gate)**
  Look at the top right of the single-cycle datapath diagram. There is an **AND** gate that helps control the PC Multiplexor. 
  A) What two signals go into this AND gate?
  B) Under what specific instruction and data condition will this AND gate output a `1`?
  
  **Solution 8:**
  *   **Part A:** The two signals are the **`Branch` control signal** (from the Main Control unit) and the **`Zero` flag** (from the ALU).
  *   **Part B:** The AND gate outputs a `1` if and only if the instruction is a **`CBZ`** (which sets the `Branch` signal to 1) **AND** the register being tested is actually zero (which causes the ALU to output a `Zero` flag of 1). If both are true, the PC mux switches to the branch target address.
  
  ***
  
  **Problem 9: The Unconditional Branch (B)**
  The `B` instruction uses the `Uncondbranch` control signal. When `Uncondbranch` is `1`, it forces the PC multiplexor to accept the Branch Target Address, bypassing the AND gate entirely.
  During a `B` instruction, what are the values of the `RegWrite` and `MemWrite` control signals? Why?
  
  **Solution 9:**
  *   **`RegWrite` = 0**
  *   **`MemWrite` = 0**
  *   *Justification:* An unconditional branch *only* updates the Program Counter (PC). It does not perform math to save in a register, nor does it store data into RAM. If either of these signals were `1`, the CPU would accidentally overwrite valid data with garbage while jumping.
  
  ---
### **Topic: Single-Cycle Performance Limits**
  
  **Problem 10: Critical Path and Clock Period**
  Assume the hardware components in a datapath have the following latencies (delays):
  *   Memory (Instruction or Data): $250\text{ ps}$
  *   ALU: $150\text{ ps}$
  *   Register File (Read or Write): $100\text{ ps}$
  *   *Ignore all mux and wire delays.*
  
  A) Which instruction class (`ADD`, `LDUR`, `STUR`, or `CBZ`) takes the longest physical time to execute? This is called the "critical path."
  B) What is the minimum clock cycle time required for this single-cycle datapath to function correctly?
  
  **Solution 10:**
  *   **Part A:** **`LDUR` (Load)** is the critical path. It must access Instruction Memory, read the Registers, use the ALU to calculate the address, access Data Memory, and finally write back to the Registers.
  *   **Part B:** The clock cycle time must accommodate the longest instruction. We sum the delays for `LDUR`:
    *   Instruction Fetch (Memory): $250\text{ ps}$
    *   Register Read: $100\text{ ps}$
    *   ALU (Address Calculation): $150\text{ ps}$
    *   Data Memory Read: $250\text{ ps}$
    *   Register Write: $100\text{ ps}$
    *   **Total Time:** $250 + 100 + 150 + 250 + 100 = \mathbf{850\text{ ps}}$.
    *   The minimum clock cycle time must be **$850\text{ ps}$**. (If it were faster, the `LDUR` instruction would not have time to finish writing to the register before the next clock tick).
- ---
- ---
- ---
### **New Practice Problems: Datapath, Control \& Tracing**
  
  **Problem 1: Control Signal Generation**
  Fill out the 9 Main Control Signals (`Reg2Loc`, `Uncondbranch`, `Branch`, `MemRead/Write`, `MemtoReg`, `ALUOp`, `ALUSrc`, `RegWrite`) for the instruction **`AND X1, X2, X3`**. 
  *(Note: Use 00 for Mem Hold, 10 for Mem Read, 01 for Mem Write).*
  
  **Solution 1:**
  *   `Reg2Loc`: **0** (We need to read `Rm`, which is `X3`, at bits 20-16).
  *   `Uncondbranch`: **0**
  *   `Branch`: **0**
  *   `MemR/W`: **00** (Hold, we are not accessing data memory).
  *   `MemtoReg`: **0** (We want the ALU result to go back to the register).
  *   `ALUOp`: **10** (R-type format, tells ALU control to look at the opcode).
  *   `ALUSrc`: **0** (We want to feed Register 2 into the ALU, not an immediate).
  *   `RegWrite`: **1** (We are saving the answer into `X1`).
  
  ***
  
  **Problem 2: Reverse Engineering Control Signals**
  You freeze the processor and read the following signals: 
  `Reg2Loc = 1`, `MemtoReg = X`, `MemR/W = 01`, `ALUSrc = 1`, `RegWrite = 0`.
  What specific instruction is currently executing? How do you know?
  
  **Solution 2:**
  *   **Instruction:** `STUR`
  *   **Justification:** `MemR/W = 01` tells us the Data Memory is writing. `RegWrite = 0` tells us we aren't saving anything to the registers. `ALUSrc = 1` means we are calculating an address using an offset. This perfectly describes a Store instruction.
  
  ***
  
  **Problem 3: Tracing the `STUR` Datapath**
  During a `STUR X9, [X10, #32]` instruction, data flows through the Register File.
  A) What register number goes into "Read register 1"?
  B) What register number goes into "Read register 2"?
  C) Where does the output of "Read data 2" physically travel to next in the datapath?
  
  **Solution 3:**
  *   **A)** `X10` (The base address).
  *   **B)** `X9` (The data to be stored, routed by setting `Reg2Loc` to 1 to read bits 4-0).
  *   **C)** It bypasses the ALU entirely and travels straight into the **"Write data" port of the Data Memory**, where it waits for the `MemWrite` signal to commit it to RAM.
  
  ***
  
  **Problem 4: The Sign-Extend Unit**
  The instruction `LDUR X5, [X6, #-12]` is fetched from memory. 
  A) What specific bits (e.g., 31-0, 20-12, etc.) are routed into the Sign-Extend unit?
  B) What is the exact 64-bit binary output of the Sign-Extend unit for this instruction?
  
  **Solution 4:**
  *   **A)** Bits **[20-12]** (The 9-bit address offset field of the D-Format).
  *   **B)** The offset is `-12`. 
    *   Positive 12: `000001100`
    *   Invert: `111110011`
    *   Add 1: `111110100`
    *   Sign-Extend to 64 bits: **`1111 1111 ... 1111 0100`**
  
  ***
  
  **Problem 5: PC Mux Logic (Boolean Algebra)**
  The multiplexor that decides the next Program Counter value (top right of the schematic) relies on an OR gate and an AND gate. 
  If the current instruction is `CBZ X1, Loop` and the value inside `X1` is `5`, determine the binary values (0 or 1) of:
  1. The `Uncondbranch` wire.
  2. The `Branch` wire.
  3. The `Zero` wire coming out of the ALU.
  4. The final selection bit for the PC Multiplexor.
  
  **Solution 5:**
  1.  `Uncondbranch` = **0** (It is a conditional branch, not a `B`).
  2.  `Branch` = **1** (It is a `CBZ` instruction).
  3.  `Zero` = **0** (The ALU is testing the value 5. Since $5 \neq 0$, the Zero flag is 0).
  4.  PC Mux selection = `Uncondbranch OR (Branch AND Zero)` $\to 0 \text{ OR } (1 \text{ AND } 0) =$ **`0`**. The PC will advance to `PC + 4`.
  
  ***
  
  **Problem 6: The "Shift Left 2" Math**
  A `B` instruction needs to jump **forward by 14 instructions**. 
  What is the exact 64-bit binary output of the **Shift Left 2** unit that gets sent to the Branch Adder?
  
  **Solution 6:**
  1.  The offset is `+14`. 
  2.  In binary, 14 is `...0000 1110`.
  3.  Sign-extended to 64 bits: `0000 0000 ... 0000 1110`.
  4.  The "Shift Left 2" unit shifts this left by 2 (appends `00` to the right).
  5.  Output: **`0000 0000 ... 0011 1000`**
    *(Sanity check: Jumping forward 14 instructions = $14 \times 4 = 56$ bytes. `111000` in binary is $32 + 16 + 8 = 56$. Correct!)*
  
  ***
  
  **Problem 7: ALU Control Delegation**
  The Main Control unit sets `ALUOp = 10` for an instruction. 
  A) What type of instruction format is currently running?
  B) Who generates the final 4-bit command for the ALU?
  C) If the 11-bit opcode of the current instruction is `11001011000`, what 4-bit signal is sent to the ALU?
  
  **Solution 7:**
  *   **A)** An **R-Format** instruction (Arithmetic/Logical).
  *   **B)** The **ALU Control Unit** generates the final 4-bit command.
  *   **C)** Looking at the Green Card, opcode `11001011000` is **`SUB`**. The ALU control table maps `SUB` to the 4-bit ALU control signal **`0110`**.
  
  ***
  
  **Problem 8: Datapath Timing (Critical Path)**
  Assume the following component delays:
  *   Instruction Memory: $200\text{ ps}$
  *   Register File (Read): $100\text{ ps}$
  *   ALU: $150\text{ ps}$
  *   Data Memory: $250\text{ ps}$
  *   Register File (Write): $100\text{ ps}$
  
  Calculate the execution time for the following two instructions:
  1. `ADD X1, X2, X3`
  2. `LDUR X4, [X5, #0]`
  
  **Solution 8:**
  1.  **`ADD`:** Instr Mem (200) + Reg Read (100) + ALU (150) + Reg Write (100) = **$550\text{ ps}$**. (Bypasses Data Memory).
  2.  **`LDUR`:** Instr Mem (200) + Reg Read (100) + ALU (150) + Data Mem (250) + Reg Write (100) = **$800\text{ ps}$**.
  
  ***
  
  **Problem 9: Stuck-At Faults (Debugging)**
  Due to a defect, the **`MemtoReg`** multiplexor is permanently stuck at `1` (always routes Data Memory output to the Register File).
  If the CPU executes `ADD X1, X2, X3`, what will be stored in `X1`?
  
  **Solution 9:**
  *   *Intended behavior:* `MemtoReg` should be `0` to route the ALU's math result ($X2+X3$) into `X1`.
  *   *Fault behavior:* Because it is stuck at `1`, the CPU will route whatever garbage happens to be coming out of the Data Memory's "Read Data" port into `X1`. 
  *   *Conclusion:* The math happens correctly in the ALU, but the answer is thrown away. `X1` gets loaded with **garbage memory data**.
  
  ***
  
  **Problem 10: "Don't Cares" Logic**
  In the Main Control Table, why is `Uncondbranch` set to `0` for a `CBZ` instruction, even though `CBZ` is technically a branch?
  
  **Solution 10:**
  `CBZ` is a *conditional* branch. The decision to jump is made by the **AND gate** (which combines the `Branch` signal with the ALU's `Zero` flag). 
  If `Uncondbranch` were set to `1` during a `CBZ`, the **OR gate** at the very top right of the datapath would instantly output a `1`, bypassing the AND gate entirely. This would cause the `CBZ` to jump *every single time*, regardless of whether the register was zero or not!
- ---
- ---
- ---
### **Practice Problems**
  
  **Problem 1: Unoptimized Multiplication**
  Use the unoptimized hardware to multiply the 4-bit numbers `0110` and `0111`. Create a trace table showing Iteration, Step, Multiplier, Multiplicand, and Product.
  
  **Problem 2: Optimized Multiplication**
  Use the optimized hardware to multiply the 4-bit numbers `1110` and `0101`. Create a trace table showing Iteration, Step, Multiplicand, and Product `[C] Upper Lower`.
  
  **Problem 3: Restoring Division**
  Use the restoring division algorithm to divide the 4-bit number `1101` (13) by `0100` (4). Create a trace table showing Iteration, Step, Quotient, Divisor, and Remainder.
- ---
### **Solutions to Arithmetic Problems**
  
  **Solution 1: Unoptimized Multiplication ($0110 \times 0111$)**
  *   **Multiplicand (Mcand):** `0110` (6). Initialized to 8 bits: `0000 0110`.
  *   **Multiplier (Mplier):** `0111` (7).
  *   **Product (Prod):** `0000 0000`.
  
  | Iter | Step | Mplier | Mcand | Prod |
  | :--- | :--- | :--- | :--- | :--- |
  | **0** | Init | `0111` | `0000 0110` | `0000 0000` |
  | **1** | LSB=1 $\to$ Add | `0111` | `0000 0110` | `0000 0110` |
  | | Shift Mcand L, Mplier R | `0011` | `0000 1100` | `0000 0110` |
  | **2** | LSB=1 $\to$ Add | `0011` | `0000 1100` | `0001 0010` |
  | | Shift Mcand L, Mplier R | `0001` | `0001 1000` | `0001 0010` |
  | **3** | LSB=1 $\to$ Add | `0001` | `0001 1000` | `0010 1010` |
  | | Shift Mcand L, Mplier R | `0000` | `0011 0000` | `0010 1010` |
  | **4** | LSB=0 $\to$ No Add | `0000` | `0011 0000` | `0010 1010` |
  | | Shift Mcand L, Mplier R | `0000` | `0110 0000` | `0010 1010` |
  *Final Product:* `0010 1010` ($32 + 8 + 2 = 42$). $6 \times 7 = 42$. Correct!
  
  **Solution 2: Optimized Multiplication ($1110 \times 0101$)**
  *   **Multiplicand (Mcand):** `1110` (14). Stays fixed!
  *   **Product (Prod):** Initialized to `[0] 0000 0101`.
  
  | Iter | Step | Mcand | Product `[C] Upper Lower` |
  | :--- | :--- | :--- | :--- |
  | **0** | Init | `1110` | `[0] 0000 0101` |
  | **1** | LSB=1 $\to$ Add to Upper | `1110` | `[0] 1110 0101` |
  | | Shift Prod Right | `1110` | `[0] 0111 0010` |
  | **2** | LSB=0 $\to$ No Add | `1110` | `[0] 0111 0010` |
  | | Shift Prod Right | `1110` | `[0] 0011 1001` |
  | **3** | LSB=1 $\to$ Add to Upper *(0011+1110)* | `1110` | **`[1] 0001 1001`** |
  | | Shift Prod Right | `1110` | `[0] 1000 1100` |
  | **4** | LSB=0 $\to$ No Add | `1110` | `[0] 1000 1100` |
  | | Shift Prod Right | `1110` | `[0] 0100 0110` |
  *Final Product:* `0100 0110` ($64 + 4 + 2 = 70$). $14 \times 5 = 70$. Correct!
  
  **Solution 3: Restoring Division ($1101 \div 0100$)**
  *   **Divisor (Div):** `0100` (4). Init: `0100 0000`.
  *   **Remainder (Rem):** `1101` (13). Init: `0000 1101`.
  *   **Quotient (Quot):** `0000`.
  
  | Iter | Step | Quot | Divisor | Remainder |
  | :--- | :--- | :--- | :--- | :--- |
  | **0** | Init | `0000` | `0100 0000` | `0000 1101` |
  | **1** | Sub Div from Rem *(0000 - 0100)* | `0000` | `0100 0000` | `1100 1101` *(Neg!)* |
  | | Restore! Quot<<0, Div>>1 | `0000` | `0010 0000` | `0000 1101` |
  | **2** | Sub Div from Rem *(0000 - 0010)* | `0000` | `0010 0000` | `1110 1101` *(Neg!)* |
  | | Restore! Quot<<0, Div>>1 | `0000` | `0001 0000` | `0000 1101` |
  | **3** | Sub Div from Rem *(0000 - 0001)* | `0000` | `0001 0000` | `1111 1101` *(Neg!)* |
  | | Restore! Quot<<0, Div>>1 | `0000` | `0000 1000` | `0000 1101` |
  | **4** | Sub Div from Rem *(1101 - 1000)* | `0000` | `0000 1000` | `0000 0101` *(Pos!)* |
  | | Keep! Quot<<1, Div>>1 | **`0001`** | `0000 0100` | `0000 0101` |
  | **5** | Sub Div from Rem *(0101 - 0100)* | `0001` | `0000 0100` | `0000 0001` *(Pos!)* |
  | | Keep! Quot<<1, Div>>1 | **`0011`** | `0000 0010` | `0000 0001` |
  *Final Check:* Quot = `0011` (3). Rem = `0000 0001` (1). $13 \div 4 = 3 \text{ R } 1$. Correct!
  
  ***
  <br>
### **New Practice Problems: Datapath, Control \& Tracing**
  
  **Problem 1: Control Signal Generation**
  Fill out the 9 Main Control Signals (`Reg2Loc`, `Uncondbranch`, `Branch`, `MemRead/Write`, `MemtoReg`, `ALUOp`, `ALUSrc`, `RegWrite`) for the instruction **`AND X1, X2, X3`**. 
  *(Note: Use 00 for Mem Hold, 10 for Mem Read, 01 for Mem Write).*
  
  **Solution 1:**
  *   `Reg2Loc`: **0** (We need to read `Rm`, which is `X3`, at bits 20-16).
  *   `Uncondbranch`: **0**
  *   `Branch`: **0**
  *   `MemR/W`: **00** (Hold, we are not accessing data memory).
  *   `MemtoReg`: **0** (We want the ALU result to go back to the register).
  *   `ALUOp`: **10** (R-type format, tells ALU control to look at the opcode).
  *   `ALUSrc`: **0** (We want to feed Register 2 into the ALU, not an immediate).
  *   `RegWrite`: **1** (We are saving the answer into `X1`).
  
  ***
  
  **Problem 2: Reverse Engineering Control Signals**
  You freeze the processor and read the following signals: 
  `Reg2Loc = 1`, `MemtoReg = X`, `MemR/W = 01`, `ALUSrc = 1`, `RegWrite = 0`.
  What specific instruction is currently executing? How do you know?
  
  **Solution 2:**
  *   **Instruction:** `STUR`
  *   **Justification:** `MemR/W = 01` tells us the Data Memory is writing. `RegWrite = 0` tells us we aren't saving anything to the registers. `ALUSrc = 1` means we are calculating an address using an offset. This perfectly describes a Store instruction.
  
  ***
  
  **Problem 3: Tracing the `STUR` Datapath**
  During a `STUR X9, [X10, #32]` instruction, data flows through the Register File.
  A) What register number goes into "Read register 1"?
  B) What register number goes into "Read register 2"?
  C) Where does the output of "Read data 2" physically travel to next in the datapath?
  
  **Solution 3:**
  *   **A)** `X10` (The base address).
  *   **B)** `X9` (The data to be stored, routed by setting `Reg2Loc` to 1 to read bits 4-0).
  *   **C)** It bypasses the ALU entirely and travels straight into the **"Write data" port of the Data Memory**, where it waits for the `MemWrite` signal to commit it to RAM.
  
  ***
  
  **Problem 4: The Sign-Extend Unit**
  The instruction `LDUR X5, [X6, #-12]` is fetched from memory. 
  A) What specific bits (e.g., 31-0, 20-12, etc.) are routed into the Sign-Extend unit?
  B) What is the exact 64-bit binary output of the Sign-Extend unit for this instruction?
  
  **Solution 4:**
  *   **A)** Bits **[20-12]** (The 9-bit address offset field of the D-Format).
  *   **B)** The offset is `-12`. 
    *   Positive 12: `000001100`
    *   Invert: `111110011`
    *   Add 1: `111110100`
    *   Sign-Extend to 64 bits: **`1111 1111 ... 1111 0100`**
  
  ***
  
  **Problem 5: PC Mux Logic (Boolean Algebra)**
  The multiplexor that decides the next Program Counter value (top right of the schematic) relies on an OR gate and an AND gate. 
  If the current instruction is `CBZ X1, Loop` and the value inside `X1` is `5`, determine the binary values (0 or 1) of:
  1. The `Uncondbranch` wire.
  2. The `Branch` wire.
  3. The `Zero` wire coming out of the ALU.
  4. The final selection bit for the PC Multiplexor.
  
  **Solution 5:**
  1.  `Uncondbranch` = **0** (It is a conditional branch, not a `B`).
  2.  `Branch` = **1** (It is a `CBZ` instruction).
  3.  `Zero` = **0** (The ALU is testing the value 5. Since $5 \neq 0$, the Zero flag is 0).
  4.  PC Mux selection = `Uncondbranch OR (Branch AND Zero)` $\to 0 \text{ OR } (1 \text{ AND } 0) =$ **`0`**. The PC will advance to `PC + 4`.
  
  ***
  
  **Problem 6: The "Shift Left 2" Math**
  A `B` instruction needs to jump **forward by 14 instructions**. 
  What is the exact 64-bit binary output of the **Shift Left 2** unit that gets sent to the Branch Adder?
  
  **Solution 6:**
  1.  The offset is `+14`. 
  2.  In binary, 14 is `...0000 1110`.
  3.  Sign-extended to 64 bits: `0000 0000 ... 0000 1110`.
  4.  The "Shift Left 2" unit shifts this left by 2 (appends `00` to the right).
  5.  Output: **`0000 0000 ... 0011 1000`**
    *(Sanity check: Jumping forward 14 instructions = $14 \times 4 = 56$ bytes. `111000` in binary is $32 + 16 + 8 = 56$. Correct!)*
  
  ***
  
  **Problem 7: ALU Control Delegation**
  The Main Control unit sets `ALUOp = 10` for an instruction. 
  A) What type of instruction format is currently running?
  B) Who generates the final 4-bit command for the ALU?
  C) If the 11-bit opcode of the current instruction is `11001011000`, what 4-bit signal is sent to the ALU?
  
  **Solution 7:**
  *   **A)** An **R-Format** instruction (Arithmetic/Logical).
  *   **B)** The **ALU Control Unit** generates the final 4-bit command.
  *   **C)** Looking at the Green Card, opcode `11001011000` is **`SUB`**. The ALU control table maps `SUB` to the 4-bit ALU control signal **`0110`**.
  
  ***
  
  **Problem 8: Datapath Timing (Critical Path)**
  Assume the following component delays:
  *   Instruction Memory: $200\text{ ps}$
  *   Register File (Read): $100\text{ ps}$
  *   ALU: $150\text{ ps}$
  *   Data Memory: $250\text{ ps}$
  *   Register File (Write): $100\text{ ps}$
  
  Calculate the execution time for the following two instructions:
  1. `ADD X1, X2, X3`
  2. `LDUR X4, [X5, #0]`
  
  **Solution 8:**
  1.  **`ADD`:** Instr Mem (200) + Reg Read (100) + ALU (150) + Reg Write (100) = **$550\text{ ps}$**. (Bypasses Data Memory).
  2.  **`LDUR`:** Instr Mem (200) + Reg Read (100) + ALU (150) + Data Mem (250) + Reg Write (100) = **$800\text{ ps}$**.
  
  ***
  
  **Problem 9: Stuck-At Faults (Debugging)**
  Due to a defect, the **`MemtoReg`** multiplexor is permanently stuck at `1` (always routes Data Memory output to the Register File).
  If the CPU executes `ADD X1, X2, X3`, what will be stored in `X1`?
  
  **Solution 9:**
  *   *Intended behavior:* `MemtoReg` should be `0` to route the ALU's math result ($X2+X3$) into `X1`.
  *   *Fault behavior:* Because it is stuck at `1`, the CPU will route whatever garbage happens to be coming out of the Data Memory's "Read Data" port into `X1`. 
  *   *Conclusion:* The math happens correctly in the ALU, but the answer is thrown away. `X1` gets loaded with **garbage memory data**.
  
  ***
  
  **Problem 10: "Don't Cares" Logic**
  In the Main Control Table, why is `Uncondbranch` set to `0` for a `CBZ` instruction, even though `CBZ` is technically a branch?
  
  **Solution 10:**
  `CBZ` is a *conditional* branch. The decision to jump is made by the **AND gate** (which combines the `Branch` signal with the ALU's `Zero` flag). 
  If `Uncondbranch` were set to `1` during a `CBZ`, the **OR gate** at the very top right of the datapath would instantly output a `1`, bypassing the AND gate entirely. This would cause the `CBZ` to jump *every single time*, regardless of whether the register was zero or not!
- ---
- ---
- ---
### **Practice Problems**
  
  **Problem 1: Hardware Overflow Detection**
  Add the 4-bit 2's complement signed integers `0111` (7) and `0010` (2). 
  1. Write out the binary addition, clearly showing the "Carry Row" at the top.
  2. Identify the value of $C_{in}$ (the carry into the MSB) and $C_{out}$ (the carry out of the MSB).
  3. Use the XOR logic gate formula to prove whether an overflow occurred.
  
  **Problem 2: Hardware Overflow Detection (Negative)**
  Add the 4-bit 2's complement signed integers `1101` (-3) and `0100` (4).
  1. Write out the binary addition, clearly showing the "Carry Row" at the top.
  2. Identify the value of $C_{in}$ and $C_{out}$.
  3. Prove whether an overflow occurred.
  
  **Problem 3: Optimized Division Trace**
  Use the **Optimized Division** hardware algorithm to divide **`1010` (10) by `0011` (3)**. 
  Construct a trace table showing the Iteration, Step, Divisor, and the 8-bit Remainder register (`[Upper] [Lower]`). 
  *(Hint: 4 iterations. You should end with Upper=0001 and Lower=0011).*
  
  **Problem 4: Unoptimized Division Trace**
  Use the **Unoptimized Division** hardware algorithm to divide **`1111` (15) by `0101` (5)**.
  Construct a trace table showing the Iteration, Step, Quotient, Divisor, and Remainder. 
  *(Hint: 5 iterations. Divisor starts at `0101 0000`. You should end with Quotient=0011 and Remainder=0000 0000).*