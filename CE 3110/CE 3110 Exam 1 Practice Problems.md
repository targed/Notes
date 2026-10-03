- Here is a comprehensive set of **50 practice problems** based exactly on the boundaries of your Exam 1 (Chapter 1 and Chapter 2 up to slide 47). I have excluded anything related to the stack, procedures, or the recursive `fact` function, as those fall outside the allowed slides. 
  
  I have grouped these into **5 Major Topics**, with 10 questions each. Grab your Green Card, a pencil, and some scratch paper!
  
  ---
### **Topic 1: Clocking, CPU Time, and Relative Performance**
  Formulas: $f = 1/T_c$ | $\text{CPU Time} = (IC \times CPI) / f$ | $\text{Speedup} = Time_{old} / Time_{new}$
  
  **Q1:** A processor has a clock cycle time of 250ps. What is its clock frequency in GHz?
  *   **Solution:** $f = 1 / T_c = 1 / (250 \times 10^{-12}\text{ s}) = 4 \times 10^9\text{ Hz} = \mathbf{4\text{ GHz}}$.
  
  **Q2:** A CPU runs at 3.2 GHz. What is the duration of one clock cycle in picoseconds (ps)?
  *   **Solution:** $T_c = 1 / f = 1 / (3.2 \times 10^9) = 0.3125 \times 10^{-9}\text{ s} = \mathbf{312.5\text{ ps}}$.
  
  **Q3:** Machine A executes a program in 15 seconds. Machine B is 2.5 times faster than Machine A. How long does Machine B take?
  *   **Solution:** $\text{Speedup} = Time_A / Time_B \rightarrow 2.5 = 15 / Time_B \rightarrow Time_B = 15 / 2.5 = \mathbf{6\text{ seconds}}$.
  
  **Q4:** A program has $2 \times 10^6$ instructions. The CPU has a clock frequency of 2 GHz and an average CPI of 1.5. What is the CPU execution time?
  *   **Solution:** $\text{Time} = (IC \times CPI) / f = (2 \times 10^6 \times 1.5) / (2 \times 10^9) = 3 \times 10^6 / 2 \times 10^9 = 1.5 \times 10^{-3}\text{ s} = \mathbf{1.5\text{ ms}}$.
  
  **Q5:** You overclock a 2.0 GHz processor to 3.0 GHz. If the program originally took 12 seconds, what is the new execution time? (Assume IC and CPI remain constant).
  *   **Solution:** Time is inversely proportional to frequency. $\text{Time}_{new} = \text{Time}_{old} \times (f_{old} / f_{new}) = 12 \times (2.0 / 3.0) = \mathbf{8\text{ seconds}}$.
  
  **Q6:** CPU X has a clock rate of 4 GHz and a CPI of 2.0. CPU Y has a clock rate of 3 GHz and a CPI of 1.2. Which is faster, and by how much? (Assume same ISA and IC).
  *   **Solution:** $\text{Time}_X = (IC \times 2.0) / 4\text{G} = 0.5 \times IC$ ns. $\text{Time}_Y = (IC \times 1.2) / 3\text{G} = 0.4 \times IC$ ns. CPU Y takes less time, so it is faster. $\text{Ratio} = 0.5 / 0.4 = \mathbf{1.25\text{ times faster}}$.
  
  **Q7:** A program takes 10 billion clock cycles to execute on a 2.5 GHz processor. What is the CPU time?
  *   **Solution:** $\text{Time} = \text{Cycles} / f = 10 \times 10^9 / 2.5 \times 10^9 = \mathbf{4\text{ seconds}}$.
  
  **Q8:** A compiler optimizes a program, reducing the instruction count from 10 million to 8 million. However, the average CPI increases from 1.5 to 2.0. The clock rate is 2 GHz. Did performance improve?
  *   **Solution:** $\text{Time}_{old} = (10\text{M} \times 1.5) / 2\text{G} = 7.5\text{ ms}$. $\text{Time}_{new} = (8\text{M} \times 2.0) / 2\text{G} = 8.0\text{ ms}$. **No, it slowed down** (from 7.5ms to 8.0ms).
  
  **Q9:** Machine A runs a program in 20s. Machine B runs the same program in 16s. What is the relative performance of B compared to A?
  *   **Solution:** $\text{Speedup} = Time_{slower} / Time_{faster} = Time_A / Time_B = 20 / 16 = \mathbf{1.25}$.
  
  **Q10:** If you want to cut a program's execution time in half (Speedup = 2), but you can only improve the CPI from 2.4 to 1.6, what must you do to the clock frequency?
  *   **Solution:** $T_{new} = 0.5 \times T_{old}$. $(IC \times 1.6)/f_{new} = 0.5 \times (IC \times 2.4)/f_{old}$. $1.6 / f_{new} = 1.2 / f_{old}$. $f_{new} = (1.6 / 1.2) \times f_{old}$. You must increase the clock frequency by **1.33x (or 33%)**.
  
  ---
### **Topic 2: CPI, Weighted CPI, and MIPS**
  Formulas: $\text{Global CPI} = \sum (CPI_i \times \text{Frequency}_i)$ | $MIPS = (IC) / (\text{Time} \times 10^6)$
  
  **Q1:** A program has 3 instruction classes: ALU (40%, CPI=1), Load/Store (40%, CPI=3), and Branch (20%, CPI=2). What is the weighted average CPI?
  *   **Solution:** $\text{CPI} = (0.4 \times 1) + (0.4 \times 3) + (0.2 \times 2) = 0.4 + 1.2 + 0.4 = \mathbf{2.0}$.
  
  **Q2:** A program has 500 ALU instructions (1 cycle), 300 Memory instructions (4 cycles), and 200 Branch instructions (2 cycles). Calculate total clock cycles.
  *   **Solution:** $\text{Cycles} = (500 \times 1) + (300 \times 4) + (200 \times 2) = 500 + 1200 + 400 = \mathbf{2100\text{ cycles}}$.
  
  **Q3:** Using the data from Q2, calculate the average CPI.
  *   **Solution:** Total IC = 1000. $\text{Average CPI} = \text{Total Cycles} / \text{Total IC} = 2100 / 1000 = \mathbf{2.1}$.
  
  **Q4:** A processor runs at 2 GHz and executes a program of 4 million instructions with a CPI of 2.5. What is the MIPS rating?
  *   **Solution:** $MIPS = f / (CPI \times 10^6) = (2 \times 10^9) / (2.5 \times 10^6) = \mathbf{800\text{ MIPS}}$.
  
  **Q5:** Computer A has a MIPS rating of 500 and takes 2 seconds to run a program. How many instructions were in the program?
  *   **Solution:** $MIPS = IC / (\text{Time} \times 10^6) \rightarrow 500 = IC / (2 \times 10^6) \rightarrow IC = 500 \times 2 \times 10^6 = \mathbf{1\text{ billion instructions}}$.
  
  **Q6:** Why is MIPS a misleading performance metric when comparing two different ISAs (like x86 vs ARM)?
  *   **Solution:** MIPS only measures the *rate* of instructions, not the *work done* per instruction. A complex ISA might execute fewer instructions total (lower MIPS) but finish the task faster. 
  
  **Q7:** Sequence 1 has 5 instructions and takes 10 cycles. Sequence 2 has 6 instructions and takes 9 cycles. Which has the better CPI, and which is faster?
  *   **Solution:** Sequence 1 $\text{CPI} = 10/5 = 2.0$. Sequence 2 $\text{CPI} = 9/6 = 1.5$. **Sequence 2 is both faster (9 cycles vs 10) and has the better CPI.**
  
  **Q8:** A compiler replaces 100 memory instructions (CPI=5) with 200 ALU instructions (CPI=1). Does total execution time increase or decrease?
  *   **Solution:** Old cycles = $100 \times 5 = 500$. New cycles = $200 \times 1 = 200$. **Time decreases (performance improves)** because total cycles dropped by 300.
  
  **Q9:** True or False: Average CPI is entirely determined by the hardware designer.
  *   **Solution:** **False**. While hardware determines the CPI of *individual* instructions, the *average* CPI depends on the instruction mix generated by the compiler/programmer.
  
  **Q10:** A 3 GHz processor has a MIPS of 1500. What is its average CPI?
  *   **Solution:** $MIPS = (f / 10^6) / CPI \rightarrow 1500 = 3000 / CPI \rightarrow CPI = 3000 / 1500 = \mathbf{2.0}$.
  
  ---
### **Topic 3: Amdahl’s Law**
  *Formula: $T_{new} = \frac{T_{affected}}{S} + T_{unaffected}$  |  $Speedup = \frac{1}{(1-f) + f/S}$*
  
  **Q1:** A program takes 100 seconds to execute. 40 seconds of that time is spent doing floating-point math. You upgrade the floating-point hardware to be 4 times faster. What is the new execution time?
  *   **Solution:** $T_{new} = (40 / 4) + (100 - 40) = 10 + 60 = \mathbf{70\text{ seconds}}$.
  
  **Q2:** Using the data from Q1, what is the overall speedup?
  *   **Solution:** $\text{Speedup} = T_{old} / T_{new} = 100 / 70 \approx \mathbf{1.43\text{x}}$.
  
  **Q3:** An enhancement speeds up database queries by 10x. Database queries account for 30% of the program's runtime. What is the overall speedup?
  *   **Solution:** $f = 0.3, S = 10$. $\text{Speedup} = 1 / ((1 - 0.3) + (0.3 / 10)) = 1 / (0.7 + 0.03) = 1 / 0.73 \approx \mathbf{1.37\text{x}}$.
  
  **Q4:** You want to achieve an overall speedup of 2.0x on a program. The code you are optimizing accounts for 80% of the runtime. How much faster ($S$) must you make that specific code?
  *   **Solution:** $2.0 = 1 / (0.2 + (0.8 / S)) \rightarrow 0.5 = 0.2 + (0.8 / S) \rightarrow 0.3 = 0.8 / S \rightarrow S = 0.8 / 0.3 = \mathbf{2.67\text{x}}$.
  
  **Q5:** You want to achieve an overall speedup of 5x. The code you are optimizing accounts for 60% of the runtime. Is this possible?
  *   **Solution:** The maximum possible speedup if $S = \infty$ is $1 / (1 - 0.6) = 1 / 0.4 = \mathbf{2.5\text{x}}$. Since $5x > 2.5x$, it is **impossible**.
  
  **Q6:** A web server spends 70% of its time on network I/O and 30% on CPU calculation. You upgrade the CPU to be infinitely fast. What is the overall speedup?
  *   **Solution:** $\text{Speedup} = 1 / ((1 - 0.3) + (0.3 / \infty)) = 1 / 0.7 \approx \mathbf{1.43\text{x}}$.
  
  **Q7:** Program execution is 60s. Memory access takes 20s. You install a cache that makes memory access 5 times faster. What is the new execution time?
  *   **Solution:** $T_{new} = (20 / 5) + 40 = 4 + 40 = \mathbf{44\text{ seconds}}$.
  
  **Q8:** Which is a better investment: Making a function that takes 20% of the time 100x faster, or making a function that takes 50% of the time 2x faster?
  *   **Solution:** Opt A: $1 / (0.8 + 0.2/100) = 1 / 0.802 = 1.24x$. Opt B: $1 / (0.5 + 0.5/2) = 1 / 0.75 = 1.33x$. **Option B is better.**
  
  **Q9:** An optimization gives an overall speedup of 1.25x. The optimization speeds up a specific function by 2x. What fraction of the original time was spent in that function?
  *   **Solution:** $1.25 = 1 / ((1-f) + f/2) \rightarrow 0.8 = 1 - f + 0.5f \rightarrow 0.8 = 1 - 0.5f \rightarrow 0.5f = 0.2 \rightarrow f = \mathbf{0.4 \text{ (or 40\%)}}$.
  
  **Q10:** The corollary to Amdahl's Law is "Make the ________ case fast."
  *   **Solution:** Make the **common** case fast.
  
  ---
### **Topic 4: Binary, Memory Offsets, and Logic**
  *Remember: Byte offsets = Doubleword Index $\times$ 8.*
  
  **Q1:** Convert the decimal number $-14$ to 8-bit 2's complement binary.
  *   **Solution:** +14 = `0000 1110`. Invert = `1111 0001`. Add 1 = **`1111 0010`**.
  
  **Q2:** You have the 8-bit binary number `1111 1100`. What is its decimal value if interpreted as Unsigned? What if interpreted as Signed?
  *   **Solution:** Unsigned: $128+64+32+16+8+4 = \mathbf{252}$. Signed: MSB is 1, so it's negative. Invert (`0000 0011`), add 1 (`0000 0100` = 4), so it is **$-4$**.
  
  **Q3:** The base address of a 64-bit integer array `A` is in `X20`. What is the LEGv8 instruction to load `A[5]` into `X10`?
  *   **Solution:** Offset = $5 \times 8 = 40$. Instruction: **`LDUR X10, [X20, #40]`**.
  
  **Q4:** If `X1 = 0000...0011` (3 in decimal), what is the value of `X2` after `LSL X2, X1, #4`?
  *   **Solution:** LSL by 4 multiplies the number by $2^4 = 16$. $3 \times 16 = \mathbf{48}$. (Binary: `0011 0000`).
  
  **Q5:** `X5` contains `0000 0000 ... 1100 1100`. `X6` contains `0000 0000 ... 1010 1010`. What is `X7` after `AND X7, X5, X6`? (Show last 8 bits).
  *   **Solution:** `1100 1100` AND `1010 1010` = **`1000 1000`**.
  
  **Q6:** Using the same values from Q5, what is `X7` after `ORR X7, X5, X6`?
  *   **Solution:** `1100 1100` ORR `1010 1010` = **`1110 1110`**.
  
  **Q7:** Using the same values from Q5, what is `X7` after `EOR X7, X5, X6`?
  *   **Solution:** `1100 1100` EOR `1010 1010` = **`0110 0110`**.
  
  **Q8:** Write an instruction sequence to invert all the bits of `X9`. (Hint: Use `EORI`).
  *   **Solution:** Since EORing with 1 flips a bit, XOR with a string of 1s (which is -1 in decimal). **`EORI X9, X9, #-1`** (Wait, I-formats take unsigned 12-bit immediates, so we would use `EOR X9, X9, X10` where X10 is set to all 1s). *Alternatively*, the question might just look for the logic concept: **XOR with all 1s**.
  
  **Q9:** In comparing two numbers `X1` and `X2` for equality, the hardware does `SUBS XZR, X1, X2`. If they are equal, which condition flag is set to 1?
  *   **Solution:** The **Z (Zero) flag** is set to 1 because $X1 - X2 = 0$.
  
  **Q10:** Which instruction extends a signed 8-bit number loaded from memory into a 64
- ---
### **Topic 1: Clock Cycles, Frequency, and Relative Performance**
  
  **Problem 1: Basic Clocking and Frequency**
  A processor has a clock cycle time of $400\text{ ps}$ (picoseconds). 
  A) What is the clock frequency in GHz?
  B) If the hardware engineers optimize the processor to run at a $3.2\text{ GHz}$ frequency, what is the new clock cycle time in picoseconds?
  
  **Solution 1:**
  *Part A:*
  1.  Recall formula: $f = \frac{1}{T}$
  2.  Convert $400\text{ ps}$ to seconds: $400 \times 10^{-12}\text{ s}$
  3.  Calculate: $f = \frac{1}{400 \times 10^{-12}} = 2.5 \times 10^9\text{ Hz}$
  4.  Convert to GHz: $2.5\text{ GHz}$
  
  *Part B:*
  1.  Recall formula: $T = \frac{1}{f}$
  2.  Convert $3.2\text{ GHz}$ to Hz: $3.2 \times 10^9\text{ Hz}$
  3.  Calculate: $T = \frac{1}{3.2 \times 10^9} = 0.3125 \times 10^{-9}\text{ seconds}$
  4.  Convert to picoseconds: $0.3125\text{ ns} = \mathbf{312.5\text{ ps}}$
  
  ***
- **Problem 2: Relative Performance**
  You are testing a video rendering program. On Machine A, the program takes $12\text{ seconds}$ to run. On Machine B, the program takes $15\text{ seconds}$. 
  Calculate the performance ratio. How many times faster is Machine A than Machine B?
  
  **Solution 2:**
  1.  Identify the formula for Relative Performance: 
    $\text{Performance Ratio} = \frac{\text{Execution Time}_{\text{Slow (B)}}}{\text{Execution Time}_{\text{Fast (A)}}}$
  2.  Substitute values: $\frac{15\text{ seconds}}{12\text{ seconds}}$
  3.  Calculate: $1.25$
  4.  *Answer:* Machine A is **1.25 times faster** than Machine B.
  
  ***
- **Problem 3: The Iron Law of Performance**
  A program executes $2 \times 10^6$ instructions. The CPU has an average CPI of $1.5$ and a clock frequency of $3\text{ GHz}$. Calculate the CPU execution time in milliseconds.
  
  **Solution 3:**
  1.  Identify the formula: $\text{CPU Time} = \frac{IC \times CPI}{\text{Frequency}}$
  2.  Substitute values:
    $$IC = 2 \times 10^6$$
    $$CPI = 1.5$$
    $$f = 3 \times 10^9\text{ Hz}$$
  3.  Calculate total cycles: $(2 \times 10^6) \times 1.5 = 3 \times 10^6\text{ cycles}$
  4.  Divide by frequency: $\frac{3 \times 10^6}{3 \times 10^9} = 1 \times 10^{-3}\text{ seconds}$
  5.  Convert to milliseconds: $1.0\text{ ms}$
  
  ***
- **Problem 4: Finding Unknown Frequency (Like HW1 Q4)**
  Computer X and Computer Y implement the same ISA. Computer X has a CPI of $2.5$ and a clock frequency of $4\text{ GHz}$. Computer Y has a CPI of $1.5$. If Computer Y is $1.2$ times faster than Computer X, what is the clock frequency of Computer Y?
  
  **Solution 4:**
  1.  Set up the relationship: $T_X = 1.2 \times T_Y$ (Since Y is faster, X takes 1.2 times longer).
  2.  Substitute the CPU Time formula ($T = \frac{I \times CPI}{f}$):
    $$\frac{I \times 2.5}{4\text{ GHz}} = 1.2 \times \left( \frac{I \times 1.5}{f_Y} \right)$$
  3.  Cancel out $I$ (Instruction Count is the same for the same ISA):
    $$\frac{2.5}{4} = 1.2 \times \frac{1.5}{f_Y}$$
  4.  Simplify:
    $$0.625 = \frac{1.8}{f_Y}$$
  5.  Solve for $f_Y$:
    $$f_Y = \frac{1.8}{0.625} = \mathbf{2.88\text{ GHz}}$$
  
  ---
### **Topic 2: Instruction Count and Weighted CPI**
  
  **Problem 5: Calculating Average CPI**
  A compiler outputs a program with the following instruction mix:
  *   ALU operations: 50% (takes 1 cycle)
  *   Loads/Stores: 30% (takes 3 cycles)
  *   Branches: 20% (takes 2 cycles)
  What is the overall average CPI for this program?
  
  **Solution 5:**
  1.  Identify the formula for Weighted CPI: $\text{CPI} = \sum (\text{CPI}_i \times \text{Frequency}_i)$
  2.  Calculate per class:
    *   ALU: $1 \times 0.50 = 0.5$
    *   Load/Store: $3 \times 0.30 = 0.9$
    *   Branches: $2 \times 0.20 = 0.4$
  3.  Sum them up: $0.5 + 0.9 + 0.4 = \mathbf{1.8\text{ CPI}}$
  
  ***
  
  **Problem 6: Comparing Code Sequences**
  A programmer writes two different assembly sequences to perform the same task. 
  *   **Sequence 1:** 4 ALU instructions, 1 Load instruction, 1 Branch instruction.
  *   **Sequence 2:** 2 ALU instructions, 2 Load instructions, 1 Branch instruction.
  Assume ALU instructions take 1 cycle, Loads take 4 cycles, and Branches take 2 cycles. Which sequence is faster, and how many total clock cycles does it take?
  
  **Solution 6:**
  1.  Calculate total cycles for Sequence 1:
    *   ALU: $4 \times 1 = 4$
    *   Load: $1 \times 4 = 4$
    *   Branch: $1 \times 2 = 2$
    *   *Total Seq 1:* $4 + 4 + 2 = \mathbf{10\text{ cycles}}$
  2.  Calculate total cycles for Sequence 2:
    *   ALU: $2 \times 1 = 2$
    *   Load: $2 \times 4 = 8$
    *   Branch: $1 \times 2 = 2$
    *   *Total Seq 2:* $2 + 8 + 2 = \mathbf{12\text{ cycles}}$
  3.  *Answer:* **Sequence 1 is faster** (takes 10 cycles compared to 12).
  
  ---
### **Topic 3: Amdahl’s Law**
  
  **Problem 7: Basic Amdahl's Law**
  You are upgrading a web server. Database queries currently account for 60% of the execution time. You buy a new SSD array that speeds up database queries by a factor of 4. What is the overall speedup of the system?
  
  **Solution 7:**
  1.  Identify variables: $f$ (affected fraction) $= 0.60$. $S$ (speedup) $= 4$.
  2.  Formula: $\text{Speedup} = \frac{1}{(1 - f) + \frac{f}{S}}$
  3.  Substitute: $\text{Speedup} = \frac{1}{(1 - 0.60) + \frac{0.60}{4}}$
  4.  Simplify denominator: $0.40 + 0.15 = 0.55$
  5.  Solve: $\frac{1}{0.55} \approx \mathbf{1.82}$ (Overall speedup is 1.82x).
  
  ***
  
  **Problem 8: The "Can't Be Done" Scenario**
  A program takes 100 seconds to run. Matrix multiplication accounts for 70 seconds of this time. You want the program to run 4 times faster overall (Total time = 25 seconds). How much faster must you make the matrix multiplication unit to achieve this?
  
  **Solution 8:**
  1.  Use the execution time version of Amdahl's Law: $T_{new} = T_{unaffected} + \frac{T_{affected}}{S}$
  2.  Identify values:
    *   $T_{new} = 25\text{ s}$
    *   $T_{affected} = 70\text{ s}$
    *   $T_{unaffected} = 100 - 70 = 30\text{ s}$
  3.  Substitute:
    $$25 = 30 + \frac{70}{S}$$
  4.  Solve:
    $$25 - 30 = \frac{70}{S}$$
    $$-5 = \frac{70}{S}$$
  5.  *Answer:* This is physically **impossible** ("Can't be done!"). The unaffected portion alone takes 30 seconds. You can never reduce the total time to 25 seconds, even if the matrix multiplication took 0 seconds.
  
  ---
### **Topic 4: MIPS & Throughput**
  
  **Problem 9: MIPS Calculation**
  Computer A runs at $2\text{ GHz}$ and executes a program with $5 \times 10^6$ instructions. The average CPI is 2.0. Calculate the MIPS rating for this computer running this program.
  
  **Solution 9:**
  1.  Calculate Execution Time first: $T = \frac{IC \times CPI}{f} = \frac{5 \times 10^6 \times 2.0}{2 \times 10^9} = 0.005\text{ seconds}$.
  2.  Apply MIPS formula: $\text{MIPS} = \frac{\text{Instruction Count}}{\text{Execution Time} \times 10^6}$
  3.  Substitute: $\text{MIPS} = \frac{5 \times 10^6}{0.005 \times 10^6}$
  4.  Solve: $\frac{5}{0.005} = \mathbf{1000\text{ MIPS}}$
    (Alternative Formula check: $\text{MIPS} = \frac{\text{Clock Rate}}{\text{CPI} \times 10^6} = \frac{2 \times 10^9}{2.0 \times 10^6} = 1000$)
  
  ***
  
  **Problem 10: Response Time vs Throughput**
  You upgrade a server by adding 3 more identical processors (going from 1 core to 4 cores). Assuming tasks are completely independent and there is no overhead:
  A) What happens to the *Throughput*?
  B) What happens to the *Response Time* of a single task?
  
  **Solution 10:**
  A) **Throughput increases by 4x.** The server can now finish 4 times as many tasks per hour because it processes them in parallel.
  B) **Response time stays the same.** Each individual task is still being run on a processor of the exact same speed, so the latency (time to finish one specific task) does not improve.
  
  ---
  
  ---
### **Topic 1: Binary Integers & Sign Extension**
  
  **Problem 1: 2's Complement Representation**
  What is the 8-bit binary representation of the decimal number **-14**? 
  
  **Solution 1:**
  1. Find the positive binary value of 14 in 8 bits: `0000 1110`
  2. **Invert** all the bits (1's complement): `1111 0001`
  3. **Add 1** to the result (2's complement): 
   ```text
     1111 0001
   +         1
   -----------
     1111 0010
   ```
  4. *Answer:* **`1111 0010`**
  
  ***
  
  **Problem 2: Sign Extension**
  You have the 8-bit signed binary number `1111 0101` (which is -11 in decimal). You need to load this into a 32-bit register using a sign-extending instruction (`LDURSB`). What will the 32-bit binary value be?
  
  **Solution 2:**
  1. Identify the Sign Bit (the Most Significant Bit / leftmost bit). Here, it is **1**.
  2. To preserve the negative value when moving to a larger space, you must "extend" that sign bit into all the new empty spaces to the left.
  3. Pad the left with twenty-four `1`s.
  4. *Answer:* **`1111 1111 1111 1111 1111 1111 1111 0101`**
  
  ---
### **Topic 2: Memory & Array Offsets**
  
  **Problem 3: Calculating Memory Addresses**
  You are working with an array of 64-bit integers (Doublewords) called `Array`. The base address of `Array` is stored in register `X20`, and it holds the value `1000` (decimal).
  A) What is the exact memory address of `Array[6]`?
  B) Write the LEGv8 assembly instruction to load `Array[6]` into register `X9`.
  
  **Solution 3:**
  1. *Part A:* Memory is byte-addressed. A 64-bit doubleword takes up **8 bytes**.
  2. The offset to the 6th element is $6 \times 8 = 48\text{ bytes}$.
  3. Target Address = $1000 + 48 = \mathbf{1048}$.
  4. *Part B:* The `LDUR` instruction applies the offset directly.
  5. *Answer:* **`LDUR X9, [X20, #48]`**
  
  ---
### **Topic 3: Logical Operations**
  
  **Problem 4: Bitwise Logic**
  Assume `X1` contains the binary value `...0000 1010` (Decimal 10) and `X2` contains `...0000 1100` (Decimal 12). 
  What is the binary result stored in `X3` after executing: `AND X3, X1, X2`?
  
  **Solution 4:**
  1. Align the bits vertically.
  2. Apply the `AND` rule: The output is 1 *only* if both top and bottom bits are 1.
   ```text
     0000 1010  (X1)
   & 0000 1100  (X2)
   -----------
     0000 1000
   ```
  3. *Answer:* **`...0000 1000`** (which is decimal 8).
  
  ***
  
  **Problem 5: Emulating NOT**
  LEGv8 does not have a dedicated `NOT` instruction. If you want to invert every single bit in register `X5` and store the result in `X6`, what logical instruction would you use, and what would the second operand be?
  
  **Solution 5:**
  1. To flip a bit, you use the **EOR** (Exclusive OR) operation.
  2. Rule of EOR: `A EOR 1 = ~A` (Flipped). `A EOR 0 = A` (Unchanged).
  3. Therefore, you must EOR `X5` with a mask of all `1`s (which is `-1` in 2's complement).
  4. *Answer:* **`EORI X6, X5, #-1`** *(Note: The assembler handles the immediate conversion).*
  
  ---
### **Topic 4: Encoding (Assembly to Machine Code)**
  
  *Have your Green Card ready for these!*
  
  **Problem 6: R-Format Encoding**
  Convert the following instruction into 32-bit binary: `ADD X5, X10, X15`
  
  **Solution 6:**
  1. **Identify Format:** `ADD` uses **R-Format**.
  2. **Find Opcode:** Green Card says `ADD` is `10001011000` (11 bits).
  3. **Parse Fields:** `Opcode` | `Rm` | `shamt` | `Rn` | `Rd`
   * `Rm` (2nd Source) = X15 $\to 01111$
   * `shamt` (Shift Amount) = 0 $\to 000000$
   * `Rn` (1st Source) = X10 $\to 01010$
   * `Rd` (Destination) = X5 $\to 00101$
  4. **Combine:** `10001011000 01111 000000 01010 00101`
  5. *Answer:* **`1000 1011 0000 1111 0000 0001 0100 0101`**
  
  ***
  
  **Problem 7: D-Format Encoding (The Trap!)**
  Convert the following instruction into 32-bit binary: `STUR X0, [X2, #8]`
  
  **Solution 7:**
  1. **Identify Format:** `STUR` uses **D-Format**.
  2. **Find Opcode:** Green Card says `STUR` is `11111000000` (11 bits).
  3. **Parse Fields:** `Opcode` | `Address` | `op2` | `Rn` | `Rt`
   * **`Address`:** The offset is **decimal 8**. In 9-bit binary, this is `000001000`. *(Do not mistake this for hex 0x08!)*
   * `op2` = standard `00`
   * `Rn` (Base) = X2 $\to 00010$
   * `Rt` (Source) = X0 $\to 00000$
  4. **Combine:** `11111000000 000001000 00 00010 00000`
  5. *Answer:* **`1111 1000 0000 0000 1000 0000 0100 0000`**
  
  ***
  
  **Problem 8: Branch Offset Encoding**
  Consider the following code snippet:
  ```assembly
  1. Loop:  ADD X1, X2, X3
  2.        SUB X4, X5, X6
  3.        AND X7, X8, X9
  4.        CBZ X4, Loop
  ```
  Convert the `CBZ X4, Loop` instruction into 32-bit binary.
  
  **Solution 8:**
  1. **Identify Format:** `CBZ` uses **CB-Format**.
  2. **Find Opcode:** Green Card says `CBZ` is `10110100` (8 bits).
  3. **Calculate Offset:** 
   * `CBZ` is at line 4. `Loop` is at line 1.
   * Target - Current = $1 - 4 = \mathbf{-3 \text{ instructions}}$.
   * *Trap Check:* The machine code stores the number of **instructions** to jump, not bytes!
   * Convert -3 to 19-bit 2's Complement: 
     * +3 = `000...00011`
     * Invert = `111...11100`
     * Add 1 = `111...11101` (nineteen 1s and 0s: `1111111111111111101`)
  4. **Parse Fields:** `Opcode(8)` | `Address(19)` | `Rt(5)`
   * `Rt` (Register to test) = X4 $\to 00100$
  5. **Combine:** `10110100 1111111111111111101 00100`
  6. *Answer:* **`1011 0100 1111 1111 1111 1111 1101 0100`**
  
  ---
### **Topic 5: Decoding (Hex to Assembly)**
  
  **Problem 9: Decoding Data Transfer**
  What is the LEGv8 assembly statement for the machine instruction `0xF85F8040`?
  
  **Solution 9:**
  1. **Convert Hex to Binary:**
   `F85F8040` $\to$ `1111 1000 0101 1111 1000 0000 0100 0000`
  2. **Isolate Opcode:** The first 11 bits are `1111 1000 010`.
  3. **Match on Green Card:** Looking at the Green Card, `11111000010` matches **`LDUR`**.
  4. **Apply D-Format Slices:**
   * `Opcode` (11): `11111000010`
   * `Address` (9): `011111000`
   * `op2` (2): `00`
   * `Rn` (5): `00010` (Decimal 2 $\to$ X2)
   * `Rt` (5): `00000` (Decimal 0 $\to$ X0)
  5. **Calculate the Offset:** The 9-bit address is `011111000`.
   * *Check Sign:* It starts with `0`, so it is positive.
   * *Decimal value:* $128 + 64 + 32 + 16 + 8 = 248$. (Or just convert Hex to Dec: `0xF8` = 248).
  6. **Assemble the syntax:** `LDUR Rt, [Rn, #Offset]`
  7. *Answer:* **`LDUR X0, [X2, #248]`**
  
  ***
  
  **Problem 10: Decoding Immediate Math**
  What is the LEGv8 assembly statement for the machine instruction `0xD1003169`?
  
  **Solution 10:**
  1. **Convert Hex to Binary:**
   `D1003169` $\to$ `1101 0001 0000 0000 0011 0001 0110 1001`
  2. **Isolate Opcode:** The first 10 bits (for I-format) or 11 bits. Let's look at 11: `1101 0001 000`.
  3. **Match on Green Card:** `11010001000` falls in the range of **`SUBI`** (which is `1101000100` followed by a don't care bit). Since it's `SUBI`, we use **I-Format** (10-bit opcode).
  4. **Apply I-Format Slices:**
   * `Opcode` (10): `1101000100` (`SUBI`)
   * `Immediate` (12): `000000001100` (Decimal 12)
   * `Rn` (5): `01011` (Decimal 11 $\to$ X11)
   * `Rd` (5): `01001` (Decimal 9 $\to$ X9)
  5. **Assemble the syntax:** `SUBI Rd, Rn, #Immediate`
  6. *Answer:* **`SUBI X9, X11, #12`**
  
  ---
- ---
- ---
  ---
### **Topic 1: Assembly to Machine Code (Binary / Hex)**
  
  **Problem 1: I-Format with a Large Immediate**
  Convert the following instruction into 32-bit binary and then into Hexadecimal.
  `ADDI X4, X5, #2047`
  
  **Step-by-Step Solution:**
  1.  **Identify Format:** `ADDI` uses **I-Format**.
  2.  **Find Opcode:** On the Green Card, `ADDI` is `1001000100` (10 bits).
  3.  **Calculate Immediate:** 
    *   The immediate is `2047`. 
    *   *Shortcut:* We know $2^{11} = 2048$. Therefore, $2047$ is eleven `1`s.
    *   I-Format requires a 12-bit immediate: `011111111111`.
  4.  **Identify Registers:**
    *   `Rn` (Source) = X5 $\rightarrow$ `00101`
    *   `Rd` (Destination) = X4 $\rightarrow$ `00100`
  5.  **Build the 32-bit string (Opcode | Imm | Rn | Rd):**
    `1001000100 011111111111 00101 00100`
  6.  **Convert to Hex:** (Regroup into chunks of 4 from left to right)
    `1001 0001 0001 1111 1111 1110 0101 0100`
    `   9    1    1    F    F    E    5    4`
  *Answer:* **Binary: `10010001000111111111111001010100` | Hex: `0x911FFE54`**
  
  ***
  
  **Problem 2: D-Format with a Negative Offset**
  Convert the following instruction into 32-bit binary.
  `LDUR X10, [X11, #-8]`
  
  **Step-by-Step Solution:**
  1.  **Identify Format:** `LDUR` uses **D-Format**.
  2.  **Find Opcode:** `LDUR` is `11111000010` (11 bits).
  3.  **Calculate Offset (The Trap!):**
    *   The offset is `-8`. It must be a **9-bit 2's complement** number.
    *   Step A (Positive 8): `000001000`
    *   Step B (Invert bits): `111110111`
    *   Step C (Add 1): `111111000`
  4.  **Identify remaining fields:**
    *   `op2`: Always `00` for standard loads.
    *   `Rn` (Base) = X11 $\rightarrow$ `01011`
    *   `Rt` (Destination) = X10 $\rightarrow$ `01010`
  5.  **Build the 32-bit string (Opcode | Addr | op2 | Rn | Rt):**
  *Answer:* **`11111000010 111111000 00 01011 01010`**
  
  ***
  
  **Problem 3: B-Format (Unconditional Branch)**
  Assume the following instruction is located at memory address `100`. The label `Exit` is located at memory address `116`. Convert the instruction into binary.
  `B Exit`
  
  **Step-by-Step Solution:**
  1.  **Identify Format:** `B` uses **B-Format**.
  2.  **Find Opcode:** `B` is `000101` (6 bits).
  3.  **Calculate Offset:**
    *   *Rule:* The machine code stores the number of **instructions** to jump, not bytes.
    *   Byte difference: $116 - 100 = +16\text{ bytes}$.
    *   Instruction difference: $16 / 4 = \mathbf{+4\text{ instructions}}$.
    *   Convert +4 to a 26-bit binary string: `00000000000000000000000100`.
  4.  **Build the 32-bit string (Opcode | Address):**
  *Answer:* **`000101 00000000000000000000000100`**
  
  ---
### **Topic 2: Machine Code to Assembly (Decoding)**
  
  **Problem 4: Decoding a logic operation**
  Translate the following machine instruction from Hex into LEGv8 Assembly.
  `0xEA02001F`
  
  **Step-by-Step Solution:**
  1.  **Convert Hex to Binary:**
    `E    A    0    2    0    0    1    F`
    `1110 1010 0000 0010 0000 0000 0001 1111`
  2.  **Isolate Opcode:** Look at the first 11 bits: `1110 1010 000`.
  3.  **Match on Green Card:** `11101010000` matches **`ANDS`** (AND & Set Flags).
    *   Because it's an arithmetic/logical instruction, it uses **R-Format**.
  4.  **Slice the remaining bits (R-Format):**
    *   `Opcode` (11): `11101010000` (`ANDS`)
    *   `Rm` (5): `00001` (X1)
    *   `shamt` (6): `000000` (No shift)
    *   `Rn` (5): `00000` (X0)
    *   `Rd` (5): `11111` (Decimal 31 = `XZR`)
  5.  **Assemble syntax:** `ANDS Rd, Rn, Rm`
  *Answer:* **`ANDS XZR, X0, X1`** 
  *(Note: This is how the CPU performs a logical "Test" to see if bits match without saving the result).*
  
  ***
  
  **Problem 5: Decoding a conditional branch**
  Translate the following binary instruction into LEGv8 Assembly.
  `1011 0101 1111 1111 1111 1111 1110 0101`
  
  **Step-by-Step Solution:**
  1.  **Isolate Opcode:** Check the first 8 bits (since 11 doesn't match a standard R/D format easily). 
    *   `10110101` matches **`CBNZ`**.
    *   This is **CB-Format**.
  2.  **Slice the bits (Opcode(8) | Offset(19) | Rt(5)):**
    *   `Opcode`: `10110101`
    *   `Offset`: `1111111111111111110`
    *   `Rt`: `0101` $\rightarrow$ Wait, I missed a bit. Let's count properly:
    *   `10110101` (8) | `1111111111111111110` (19) | `00101` (5).
  3.  **Evaluate the Offset:**
    *   It starts with `1`, so it is a **negative jump** (backwards loop).
    *   Apply 2's complement in reverse to find the magnitude:
        *   Invert: `000...0000001`
        *   Add 1: `000...0000010` (which is 2).
        *   Therefore, the offset is **-2**.
  4.  **Evaluate Rt:** `00101` is **X5**.
  *Answer:* **`CBNZ X5, -2`** *(Jumps back 2 instructions if X5 is not zero).*
  
  ---
### **Topic 3: Condition Flags (NZVC)**
  
  *Context from Slide 41:* 
  *   **N (Negative):** Result had 1 in MSB.
  *   **Z (Zero):** Result was 0.
  *   **V (Overflow):** Result overflowed the bounds of the register.
  *   **C (Carry):** Result had carryout from MSB.
  
  **Problem 6: Setting Z and N Flags**
  Assume register `X1` holds the value `50` and `X2` holds the value `50`. 
  The CPU executes: `SUBS X9, X1, X2`.
  What are the values of the **N** and **Z** flags after execution?
  
  **Solution 6:**
  1.  **Perform the math:** $X9 = 50 - 50 = 0$.
  2.  **Check Z (Zero):** Is the result exactly zero? Yes. **Z = 1**.
  3.  **Check N (Negative):** Is the result less than zero? No (MSB is 0). **N = 0**.
  
  ***
  
  **Problem 7: Overflow in Signed Arithmetic (Like Quiz 02/16)**
  Assume a simplified **4-bit** CPU architecture using 2's Complement. 
  Register `A` holds `0110` (Decimal 6). Register `B` holds `0011` (Decimal 3).
  The CPU executes an Add & Set Flags instruction (`ADDS`). 
  What is the binary result, and what is the value of the **V (Overflow)** flag?
  
  **Solution 7:**
  1.  **Perform binary addition:**
    ```text
      0110  (6)
    + 0011  (3)
    -------
      1001
    ```
  2.  **Analyze the Result:** 
    *   The result is `1001`. 
    *   In 4-bit unsigned, this is 9. 
    *   However, in 4-bit **signed 2's complement**, the maximum positive number is `0111` (7). 
    *   Because the MSB of the result is `1`, the computer interprets `1001` as **-7**.
  3.  **Check Overflow (V):** We added two positive numbers (MSB=0) and got a negative result (MSB=1). This is the definition of an overflow. **V = 1**.
  
  ***
  
  **Problem 8: Branching Logic based on Flags**
  Write a LEGv8 assembly sequence to implement the following C code logic. Assume `a` is in `X19` and `b` is in `X20`. 
  ```c
  if (a >= b) {
    a = a + 1;
  }
  ```
  
  **Solution 8:**
  *   *Thought Process:* We need to check if $a \ge b$. To do this, we subtract $b$ from $a$ and set the flags (`SUBS XZR, a, b`). 
  *   Because assembly jumps *skip* blocks of code, we want to branch to the end if the condition is **FALSE** ($a < b$). 
  *   Therefore, we use the `B.LT` (Branch Less Than) instruction.
  
  *Assembly Code:*
  ```assembly
    SUBS XZR, X19, X20    // Subtract 'b' from 'a', set flags, discard result
    B.LT Skip             // If a < b (Negative flag set), skip the addition
    ADDI X19, X19, #1     // a = a + 1
  Skip:
    // Program continues here
  ```
  
  ---