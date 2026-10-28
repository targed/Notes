#### **1. Clock Cycles & Frequency **
  Computers run on a "heartbeat" called the **Clock**.
  *   **Clock Cycle:** A single "tick" of the clock. It is the basic unit of time for the processor.
  *   **Clock Period:** The length of time one cycle takes (e.g., $250\text{ ps}$ or $0.25\text{ ns}$).
  *   **Clock Rate (Frequency):** How many cycles happen per second (e.g., $4.0\text{ GHz}$).
  
  **The Relationship:**
  $$ \text{Clock Rate} = \frac{1}{\text{Clock Period}} $$
  *(Example: If Period = $0.25\text{ ns}$, Rate = $1 / (0.25 \times 10^{-9}) = 4\text{ GHz}$)*
#### **2. The "Iron Law" of Performance — ** *Memorize This* ** **
  This is the Classic CPU Performance Equation. It breaks execution time down into three factors:
  
  $$ \text{CPU Time} = \text{Instruction Count} \times \text{CPI} \times \text{Clock Cycle Time} $$
  
  Alternatively, using Clock Rate:
  $$ \text{CPU Time} = \frac{\text{Instruction Count} \times \text{CPI}}{\text{Clock Rate}} $$
  
  **The Three Factors:**
  1.  **Instruction Count (IC):** The total number of instructions (lines of machine code) the program runs.
    *   *Determined by:* The source code, the Compiler, and the ISA.
  2.  **CPI (Cycles Per Instruction):** The average number of clock cycles it takes to execute one instruction.
    *   *Determined by:* The Hardware organization (architecture).
    *   *Note:* Some instructions are fast (1 cycle), some are slow (memory access might be 100 cycles). CPI is the **average**.
  3.  **Clock Cycle Time:** The speed of the hardware heartbeat.
    *   *Determined by:* The Physical Technology (Transistor physics).
  
  **Why is this called the "Iron Law"?**
  It shows the trade-offs. If you increase the Clock Rate (good), you might accidentally increase the CPI (bad) because memory looks slower by comparison. You cannot optimize one without checking the others.
#### **3. Calculating Weighted Average CPI**
  Since different instructions take different amounts of time, we calculate the "Global CPI" using a weighted average.
  
  **Formula:**
  $$ \text{Global CPI} = \sum (\text{CPI}_i \times \text{Frequency}_i) $$
  Where $\text{Frequency}_i$ is the percentage of the total code that instruction type represents.
  
  **Example (from Slide 37):**
  *   **Type A:** 1 cycle (Count: 2)
  *   **Type B:** 2 cycles (Count: 1)
  *   **Type C:** 3 cycles (Count: 2)
  *   **Total Instructions:** $2 + 1 + 2 = 5$
  
  $$ \text{Total Cycles} = (2\times1) + (1\times2) + (2\times3) = 10 \text{ cycles} $$
  $$ \text{Average CPI} = \frac{10 \text{ cycles}}{5 \text{ instructions}} = 2.0 $$
#### **4. The "Power Wall" & Multiprocessors**
  *   **The Trend (Slide 39):** From 1980 to 2005, we made computers faster by just increasing the Clock Rate (Frequency scaling).
  *   **The Wall:** Around 2005, the graph flattens. We hit the **Power Wall**. Increasing frequency further generated too much heat; the chips would literally melt.
  *   **The Solution:** **Multiprocessors (Multicore).**
    *   Instead of one fast processor (4 GHz), we put two slower ones (2 GHz each) on the same chip.
    *   *Challenge:* This forces programmers to write **Parallel code**. If your program is single-threaded, it only uses one core, so it doesn't run any faster on a new computer than an old one.
#### **5. Pitfall: Amdahl’s Law**
  This law calculates the limit of how much you can speed up a computer by improving just *one* part of it.
  
  **Formula:**
  $$ T_{\text{improved}} = \frac{T_{\text{affected}}}{\text{Improvement Factor}} + T_{\text{unaffected}} $$
  
  **The Slide Example Explained:**
  *   **Task:** A program takes 100 seconds.
  *   **Affected Part:** Multiplication operations take 80 seconds of that time.
  *   **Improvement:** You want to make multiplication 5x faster.
  *   **Unaffected Part:** The other 20 seconds (I/O, etc.) remain the same.
  
  $$ T_{\text{new}} = \frac{80\text{ seconds}}{5} + 20\text{ seconds} $$
  $$ T_{\text{new}} = 16 + 20 = 36 \text{ seconds} $$
  
  *   **The "Can't be done" Corollary:** The slide asks: "How much must I improve multiply to get a **5x overall speedup**?"
    *   Target time = $100 / 5 = 20\text{ seconds}$.
    *   Since the *unaffected* part is already 20 seconds, the affected part would have to take **0 seconds**. This is physically impossible. You cannot achieve 5x speedup even if multiplication becomes instant.
    *   *Lesson:* **Make the Common Case Fast.** If you optimize something that rarely happens, you won't see a big benefit.
#### **6. Pitfall: MIPS**
  *   **MIPS:** Million Instructions Per Second.
  *   *Why it is bad:* It looks at *Quantity*, not *Quality*.
    *   Computer A might do a task in 1 million simple instructions (High MIPS).
    *   Computer B might do the *same* task in 100 complex instructions (Low MIPS).
    *   Computer B is actually faster (less work), but has a lower MIPS score. **Never use MIPS to compare different architectures.**
  
  ---
### **Student "Check Your Understanding"**
  *This concludes the introduction chapter. Try these final practice problems:*
  
  1.  **The Iron Law:** Program A has 1,000 instructions. It has a CPI of 2.0. The CPU runs at 1 GHz ($10^9$ cycles/sec). How long does it take to run?
  2.  **Amdahl's Law:** You have a program that runs in 50 seconds. 10 seconds of that is "Database Access." You buy a new hard drive that is **10x faster**. What is the new execution time? (Hint: Separate affected vs. unaffected).
  3.  **Concept:** Why did CPU clock speeds stop increasing around 2005? What did architects do instead to increase performance?