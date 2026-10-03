### **Topic: Pipeline Performance & Timing Math**
  
  **Problem 1: The HW5 Clone (Execution Time & Speedup)**
  You are given two processors, P1 and P2. P1 is a single-cycle processor with a clock cycle time of $1000\text{ ps}$. P2 is a **5-stage** pipelined processor with a clock cycle time of $200\text{ ps}$. 
  You need to execute a program with **100 instructions** (assume absolutely no pipeline hazards). 
  1. Calculate the total execution time for P1.
  2. Calculate the total execution time for P2.
  3. Calculate the overall speedup of P2 over P1.
  
  **Problem 2: Determining Clock Cycle Time**
  Assume the hardware stages in a processor have the following latencies:
  *   Instruction Fetch (IF): $250\text{ ps}$
  *   Instruction Decode (ID): $150\text{ ps}$
  *   Execute (EX): $200\text{ ps}$
  *   Memory Access (MEM): $300\text{ ps}$
  *   Write-Back (WB): $100\text{ ps}$
  
  1. If you build a **single-cycle** processor using these components, what is the minimum clock cycle time?
  2. If you build a **pipelined** processor using these components, what is the minimum clock cycle time?
  3. What is the theoretical steady-state speedup of the pipelined processor over the single-cycle processor?
  
  **Problem 3: Latency vs. Throughput**
  Using the pipelined processor from Problem 2 (where the clock cycle time is set to $300\text{ ps}$ and there are 5 stages):
  1. How long does it take for exactly **one** instruction to travel from start to finish? (This is the latency).
  2. Is this latency faster, slower, or the same as the single-cycle processor from Problem 2? 
  3. If the pipelined processor is slower for a single instruction, why do we use it?
  
  ---
### **Topic: Pipeline Stages & ISA Design**
  id:: 6a020423-4e38-4ba1-84e0-c317c2f33693
  
  **Problem 4: Instruction Stage Usage**
  In the standard 5-stage LEGv8 pipeline (IF, ID, EX, MEM, WB), identify which specific stages are actively doing useful work versus which stages are effectively idle (passing time) for the following instructions:
  1. `ADD X1, X2, X3`
  2. `LDUR X4, [X5, #0]`
  3. `CBZ X6, Loop`
  
  **Problem 5: Structural Hazards**
  If you look at the Single-Cycle Datapath, there is only one "Memory" block, but it is split logically into "Instruction Memory" and "Data Memory". 
  If you tried to build a pipelined processor using only **ONE** combined Memory module for both instructions and data, what specific pipeline hazard would occur? Describe which two stages would collide.
  
  **Problem 6: Pipelining and ISA Design**
  The LEGv8 Instruction Set Architecture (ISA) was intentionally designed to make pipelining easy. State **two** specific features of the LEGv8 ISA that simplify pipeline design and explain why they help. *(Hint: Look at Chapter 4, Slide 37).*
  
  ---
### **Topic: Overlapping Execution (Trace Diagrams)**
  
  **Problem 7: Tracing Overlapped Execution**
  Consider a program with 3 independent instructions:
  1. `ADD X1, X2, X3`
  2. `SUB X4, X5, X6`
  3. `AND X7, X8, X9`
  
  Assume a 5-stage pipeline with no hazards. 
  1. During Clock Cycle 3, which stage (IF, ID, EX, MEM, WB) is each of the three instructions currently in?
  2. During which Clock Cycle will the `AND` instruction write its result back to the register file?
  
  **Problem 8: Pipeline Fill and Drain**
  In a 5-stage pipeline executing 8 independent instructions, how many clock cycles are spent where the pipeline is **not entirely full**? 
  *(Hint: Think about how many cycles it takes to fill the pipe at the start, and drain the pipe at the end).*
  
  ---
### **Topic: Advanced Performance (ILP)**
  
  **Problem 9: Multiple-Issue (Superscalar) Math**
  A modern processor is designed with a **4-way multiple-issue** pipeline, meaning it has replicated hardware that can fetch, decode, and execute up to 4 instructions perfectly in parallel during every clock cycle. 
  If the processor has a clock rate of **$3\text{ GHz}$**:
  1. What is the peak Instructions Per Cycle (IPC)?
  2. What is the peak CPI?
  3. What is the maximum possible throughput in BIPS (Billion Instructions Per Second)?
  
  **Problem 10: The Unbalanced Pipeline**
  Your team designs a 5-stage pipeline. Four of the stages take exactly $100\text{ ps}$ to complete, but the Memory (MEM) stage takes $400\text{ ps}$ because you used cheap, slow RAM.
  1. What must the clock cycle time be for this processor?
  2. If you swap to a Single-Cycle design using the exact same components, what is the clock cycle time?
  3. What is the speedup of the pipelined design over the single-cycle design? Was pipelining worth the effort here?
  
  ***
  ***
  ***
  ***
# **Practice Answer Key \& Explanations**
### **Question 1 Solution**
  1.  **P1 (Single-Cycle):**
    *   Time = Total Instructions $\times$ Clock Cycle Time
    *   Time = $100 \times 1000\text{ ps} =$ **$100,000\text{ ps}$**
  2.  **P2 (Pipelined):**
    *   Total Cycles = Stages + Instructions - 1 = $5 + 100 - 1 = 104\text{ cycles}$.
    *   Time = $104 \text{ cycles} \times 200\text{ ps/cycle} =$ **$20,800\text{ ps}$**
  3.  **Speedup:**
    *   Speedup = Time(P1) / Time(P2) = $100,000 / 20,800$.
    *   *(Mental math: $1000 / 208 \approx 1000 / 200 = 5$)*. 
    *   Exact speedup = **$\approx 4.81$**
### **Question 2 Solution**
  1.  **Single-cycle clock:** Must accommodate the sum of all stages.
    *   $250 + 150 + 200 + 300 + 100 =$ **$1000\text{ ps}$**.
  2.  **Pipelined clock:** Must accommodate the *longest single stage* (critical path).
    *   Max(250, 150, 200, 300, 100) = **$300\text{ ps}$**.
  3.  **Steady-state speedup:** 
    *   Time(Single) / Time(Pipelined) = $1000 / 300 =$ **$3.33\text{x}$ speedup**.
    *   *(Note: It is not a perfect 5x speedup because the stages are unbalanced!)*
### **Question 3 Solution**
  1.  **Latency:** 5 stages $\times 300\text{ ps}$ per stage = **$1500\text{ ps}$**.
  2.  **Comparison:** $1500\text{ ps}$ is **slower** than the single-cycle processor (which only took $1000\text{ ps}$ for one instruction).
  3.  **Why use it?** Pipelining does not improve the *latency* of individual instructions; it actually makes it slightly worse due to clock padding. We use it because it vastly improves **throughput**. We finish an instruction every $300\text{ ps}$ instead of every $1000\text{ ps}$.
### **Question 4 Solution**
  1.  **`ADD`:** Active in IF, ID, EX, WB. (Idle in MEM, because R-types do not access data memory).
  2.  **`LDUR`:** Active in IF, ID, EX, MEM, WB. (Uses every stage).
  3.  **`CBZ`:** Active in IF, ID, EX. (Idle in MEM and WB. Branches do not write to memory or registers).
### **Question 5 Solution**
  *   **Hazard:** A **Structural Hazard**.
  *   **Collision:** The **IF** stage (fetching the next instruction from memory) and the **MEM** stage (a Load/Store instruction accessing data in memory) would collide. They would fight for access to the single memory module at the exact same time.
### **Question 6 Solution**
  *(Any two of these three work):*
  1.  **All instructions are 32 bits:** This allows the IF and ID stages to easily fetch and decode in one cycle, because the processor always knows exactly where an instruction begins and ends.
  2.  **Few and regular formats:** The source registers (`Rn`, `Rm`) are always in the same physical location in the 32-bit string, allowing the ID stage to read the registers at the exact same time it decodes the opcode.
  3.  **Memory Alignment:** Data memory operands must be aligned, allowing all data memory accesses to complete in exactly one clock cycle (the MEM stage).
### **Question 7 Solution**
  1.  **Clock Cycle 3 Snapshot:**
    *   Instr 1 (`ADD`): In the **EX** stage.
    *   Instr 2 (`SUB`): In the **ID** stage.
    *   Instr 3 (`AND`): In the **IF** stage.
  2.  **`AND` Write-back:**
    *   CC3: IF
    *   CC4: ID
    *   CC5: EX
    *   CC6: MEM
    *   CC7: **WB**. (It writes back on Clock Cycle 7).
### **Question 8 Solution**
  *   **Filling:** It takes 4 cycles to fill the pipeline. (On cycle 5, the pipe is 100% full, with Instr 1 in WB and Instr 5 in IF).
  *   **Draining:** It takes 4 cycles to drain the pipeline after the last instruction enters IF. 
  *   **Total cycles not entirely full:** $4 \text{ (fill)} + 4 \text{ (drain)} =$ **8 cycles**.
### **Question 9 Solution**
  1.  **Peak IPC:** Since it can issue 4 instructions per cycle, the peak IPC is **4**.
  2.  **Peak CPI:** $\text{CPI} = 1 / \text{IPC}$. $1 / 4 =$ **$0.25$**.
  3.  **Max Throughput:** $3 \text{ GHz} \times 4 \text{ IPC} =$ **12 BIPS** (Billion Instructions Per Second).
### **Question 10 Solution**
  1.  **Pipelined Clock:** Dictated by the slowest stage (MEM). **$400\text{ ps}$**.
  2.  **Single-Cycle Clock:** Sum of all stages ($100+100+100+400+100$). **$800\text{ ps}$**.
  3.  **Speedup:** $800 / 400 =$ **$2\text{x}$ speedup**. 
    *   *Was it worth it?* Barely. You added all the complex pipeline registers and hazard hardware, but because the memory stage was a massive bottleneck, you only got a 2x speedup instead of an ideal 5x speedup.
- ---
- ---
- ---
- ---
- ---
- ---
### **Topic: Identifying Hazards & Forwarding (HW5 Style)**
  
  **Problem 1: The Standard Data Hazard**
  Consider the following instruction sequence:
  1. `SUB X1, X2, X3`
  2. `AND X4, X1, X5`
  3. `ORR X6, X1, X7`
  
  A) Identify the two data hazards in this sequence.
  B) For each hazard, specify which forwarding path (EXE-forwarding or MEM-forwarding) the hardware will use to resolve it without stalling.
  
  **Problem 2: The Load-Use Data Hazard**
  Consider the following instruction sequence:
  1. `LDUR X9,[X10, #0]`
  2. `ADD X11, X9, X12`
  
  A) Identify the data hazard in this sequence. 
  B) Can this hazard be resolved purely by forwarding, or is a stall required? Justify your answer.
  C) If a stall is required, which forwarding path is used *after* the stall cycle?
  
  **Problem 3: The Double Data Hazard**
  Consider the following instruction sequence:
  1. `ADD X1, X2, X3`
  2. `SUB X1, X1, X4`
  3. `AND X5, X1, X6`
  
  The `AND` instruction needs the value of `X1`. However, both the `ADD` and `SUB` instructions write to `X1`. 
  Which instruction's value does the Forwarding Unit pass to the `AND` instruction, and which forwarding path is used? Justify why the hardware makes this choice.
  
  ---
### **Topic: Hardware Logic (How the CPU detects hazards)**
  
  **Problem 4: Hazard Detection Unit Logic**
  The Hazard Detection Unit sits in the **ID** stage and is responsible for detecting Load-Use hazards. It checks the control signals of the instruction currently in the **EX** stage. 
  What specific control signal from the EX stage does the Hazard Detection Unit look at to determine if the instruction ahead of it is a Load? (Hint: It is a 2-bit signal).
  
  **Problem 5: Forwarding Unit Logic**
  The Forwarding Unit sits in the **EX** stage. It wants to perform **MEM-forwarding** (a 2-cycle gap forwarding). 
  Write the Boolean logic condition the Forwarding Unit checks to decide if it should forward from the `MEM/WB` pipeline register to the ALU. (Assume the ALU needs to read register `Rn`).
  
  ---
### **Topic: Code Scheduling & Performance**
  
  **Problem 6: Code Scheduling to Avoid Stalls**
  A compiler translates the C code `A = B + C` and `D = E + F` into the following unoptimized assembly sequence. Assume `B, C, E, F` are in memory, and `X0` holds the base address.
  
  1. `LDUR X1,[X0, #0]`   *(Load B)*
  2. `LDUR X2,[X0, #8]`   *(Load C)*
  3. `ADD  X3, X1, X2`     *(A = B + C)*
  4. `STUR X3, [X0, #16]`  *(Store A)*
  5. `LDUR X4, [X0, #24]`  *(Load E)*
  6. `LDUR X5, [X0, #32]`  *(Load F)*
  7. `ADD  X6, X4, X5`     *(D = E + F)*
  8. `STUR X6,[X0, #40]`  *(Store D)*
  
  This sequence contains **two** Load-Use hazards that will cause stalls. Reorder the instructions so that the program computes the exact same math, but **zero stalls** occur. 
  
  **Problem 7: Structural Hazards**
  Suppose you are designing a cheap CPU and decide to use only **one** unified memory module for both Instructions and Data. 
  If Instruction 1 is `LDUR X1, [X2, #0]`, and Instruction 4 is `ADD X3, X4, X5`, what happens during Clock Cycle 4? Explain the structural hazard that occurs.
  
  ---
### **Topic: Control Hazards & Branching**
  
  **Problem 8: The Branch Flush Penalty**
  In the original 5-stage pipeline, the branch target and condition were calculated in the **EX** stage. To improve performance, LEGv8 engineers moved the branch adder and register comparator to the **ID** stage.
  A) If the branch decision is made in the EX stage, how many instructions must be flushed if a branch is taken?
  B) If the branch decision is made in the ID stage, how many instructions must be flushed if a branch is taken?
  
  **Problem 9: Static Branch Prediction Trace**
  Assume a LEGv8 processor uses a static **"Always Predict Not Taken"** strategy. 
  It fetches a `CBZ` instruction, followed by an `ADD`, followed by a `SUB`. 
  If the `CBZ` condition turns out to be **TRUE** (the branch should be taken), what specifically happens to the `ADD` instruction currently sitting in the IF/ID register?
  
  **Problem 10: Comprehensive HW5 Clone**
  Consider the following sequence running on a 5-stage pipeline with full forwarding and hazard detection:
  1. `LDUR X1, [X2, #0]`
  2. `ADD  X3, X1, X4`
  3. `SUB  X5, X3, X6`
  
  A) Identify all data hazards.
  B) For each hazard, state whether a stall is needed.
  C) For each hazard, state which forwarding path is used (EXE-forwarding or MEM-forwarding).
  
  ***
  ***
  ***
  ***
# **Practice Answer Key \& Explanations**
### **Question 1 Solution**
  A) **Hazard 1:** Between Inst 1 (`SUB`) and Inst 2 (`AND`) on register `X1`. 
  **Hazard 2:** Between Inst 1 (`SUB`) and Inst 3 (`ORR`) on register `X1`.
  B) **Hazard 1:** Resolved via **EXE-forwarding** (1-cycle gap. Data is forwarded from the EX/MEM pipeline register to the ALU).
  **Hazard 2:** Resolved via **MEM-forwarding** (2-cycle gap. Data is forwarded from the MEM/WB pipeline register to the ALU).
### **Question 2 Solution**
  A) **Hazard:** Between `LDUR` and `ADD` on register `X9`. This is a Read-After-Write hazard, specifically a Load-Use hazard.
  B) **Stall is REQUIRED (1 cycle).** *Justification:* `LDUR` gets its data from Data Memory at the *end* of the MEM stage. The `ADD` instruction needs that data at the *beginning* of the EX stage. Because the EX stage of `ADD` overlaps with the MEM stage of `LDUR`, the data hasn't been fetched yet. We cannot forward backward in time.
  C) After the 1-cycle stall, the data is pushed forward from the MEM/WB pipeline register into the ALU. This is **MEM-forwarding**.
### **Question 3 Solution**
  The Forwarding Unit will pass the value from the **`SUB`** instruction (Inst 2) to the `AND` instruction. 
  *Path used:* **EXE-forwarding**.
  *Justification:* This is a double data hazard. The hardware is programmed to always take the most recent version of the data. Because `SUB` is immediately in front of `AND` (a 1-cycle gap), the EX hazard takes priority over the MEM hazard from the `ADD` instruction.
### **Question 4 Solution**
  The Hazard Detection Unit looks at the **`MemRead`** control signal (which is part of the `MemR/W` 2-bit signal, specifically `10`). If `MemRead == 1` in the ID/EX register, the unit knows the instruction ahead of it is a Load, and it begins checking if the destination register matches the current source registers.
### **Question 5 Solution**
  `if (MEM/WB.RegWrite == 1  AND  MEM/WB.Rd == ID/EX.Rn)`
  *(Translation: If the instruction currently in the Write-Back stage is actually writing to a register, AND the register it is writing to is the exact same register the current instruction in the ALU needs to read, then forward from MEM/WB).*
### **Question 6 Solution**
  To avoid stalls, we must separate the `LDUR` instructions from the `ADD` instructions that use their results. We can do this by moving the loads for the *second* equation up!
  **Optimized Sequence:**
  1. `LDUR X1, [X0, #0]`   *(Load B)*
  2. `LDUR X2,[X0, #8]`   *(Load C)*
  3. `LDUR X4, [X0, #24]`  *(Load E)*  <-- *Moved up! Fills the delay slot for ADD X3*
  4. `ADD  X3, X1, X2`     *(A = B + C)*
  5. `LDUR X5,[X0, #32]`  *(Load F)*  <-- *Moved up!*
  6. `STUR X3, [X0, #16]`  *(Store A)* <-- *Fills the delay slot for ADD X6*
  7. `ADD  X6, X4, X5`     *(D = E + F)*
  8. `STUR X6, [X0, #40]`  *(Store D)*
  *(Now, no ADD immediately follows the LDUR that provides its data! Zero stalls).*
### **Question 7 Solution**
  During Clock Cycle 4, the `LDUR` instruction is in the **MEM** stage, attempting to read data from the unified memory. At the exact same time, Instruction 4 (`ADD`) is in the **IF** stage, attempting to fetch its instruction code from the unified memory. 
  Because a memory module can only be accessed once per clock cycle, the two instructions collide. The hardware must stall the IF stage to allow the MEM stage to finish.
### **Question 8 Solution**
  A) If decided in **EX**, the CPU has already fetched instructions during the ID, IF, and the next IF stages. Therefore, **3 instructions** must be flushed (turned into NOPs).
  B) If decided in **ID**, the CPU has only fetched one extra instruction during the IF stage. Therefore, only **1 instruction** must be flushed.
### **Question 9 Solution**
  Because the static prediction was "Not Taken", the CPU fetched the `ADD` instruction into the IF/ID register. When the CPU realizes in the ID stage that the branch *should* have been taken, it flushes the pipeline. 
  The `ADD` instruction in the IF/ID register has its control signals forced to **`0`**, turning it into a **Bubble (NOP)**, and the PC is updated to the correct branch target to fetch the right instruction on the next cycle.
### **Question 10 Solution**
  A) **Hazard 1:** Load-Use Hazard between `LDUR` (Inst 1) and `ADD` (Inst 2) on `X1`. 
  **Hazard 2:** RAW Hazard between `ADD` (Inst 2) and `SUB` (Inst 3) on `X3`.
  B) **Hazard 1:** Stall is **REQUIRED** (1 cycle).
  **Hazard 2:** Stall is **NOT REQUIRED**.
  C) **Hazard 1:** After the 1-cycle stall, it uses **MEM-forwarding** (from MEM/WB).
  **Hazard 2:** It uses **EXE-forwarding** (from EX/MEM) to pass `X3` directly into the ALU. *(Note: Because of the stall inserted for Hazard 1, the `ADD` and `SUB` instructions are pushed back, but they remain right next to each other, so Hazard 2 acts exactly like a normal 1-cycle gap RAW hazard).*
- ---
- ---
- ---
### **Topic 1: 1-Bit Dynamic Predictors**
  
  **Problem 1: 1-Bit Predictor Trace (Like HW5 Q3)**
  Suppose you have a 5-stage pipelined processor with a built-in **1-bit dynamic predictor**. 
  Calculate the branch prediction accuracy (Correct / Total) for the following branch outcome sequence. Assume the initial state is **“Predict Not Taken”**.
  Sequence: `N, N, T, T, N, T`
  
  **Solution 1:**
  *Rule: A 1-bit predictor always predicts that the branch will do exactly what it did the previous time.*
  
  | Step | Current Prediction State | Actual Branch Outcome | Prediction Result | Next Prediction State |
  | :---: | :---: | :---: | :---: | :---: |
  | 1 | Predict **N** | **N** | ✅ Correct | Predict N |
  | 2 | Predict **N** | **N** | ✅ Correct | Predict N |
  | 3 | Predict **N** | **T** | ❌ Incorrect | Predict T |
  | 4 | Predict **T** | **T** | ✅ Correct | Predict T |
  | 5 | Predict **T** | **N** | ❌ Incorrect | Predict N |
  | 6 | Predict **N** | **T** | ❌ Incorrect | Predict T |
  
  *   **Total Predictions:** 6
  *   **Correct Predictions:** 3
  *   **Accuracy:** $3 / 6 = \mathbf{50\%}$
  
  ***
  
  **Problem 2: The Alternating Pattern Flaw**
  Using a **1-bit dynamic predictor**, trace the following alternating sequence: `T, N, T, N, T, N`. 
  Assume the initial state is **"Predict Taken"**. Calculate the accuracy and briefly explain why the 1-bit predictor struggles here.
  
  **Solution 2:**
  
  | Step | Current Prediction | Actual | Result | Next Prediction |
  | :---: | :---: | :---: | :---: | :---: |
  | 1 | Predict **T** | **T** | ✅ Correct | Predict T |
  | 2 | Predict **T** | **N** | ❌ Incorrect | Predict N |
  | 3 | Predict **N** | **T** | ❌ Incorrect | Predict T |
  | 4 | Predict **T** | **N** | ❌ Incorrect | Predict N |
  | 5 | Predict **N** | **T** | ❌ Incorrect | Predict T |
  | 6 | Predict **T** | **N** | ❌ Incorrect | Predict N |
  
  *   **Accuracy:** $1 / 6 = \mathbf{16.67\%}$
  *   *Explanation:* A 1-bit predictor always assumes the next branch will match the last branch. When a pattern alternates, the predictor is constantly one step behind, causing it to mispredict almost 100% of the time.
  
  ***
  
  **Problem 3: The Nested Loop Shortcoming**
  In Chapter 4, the slides mention that a 1-bit predictor fails **twice** on inner loop branches. Explain specifically *when* and *why* these two mispredictions occur during a standard `for` or `while` loop.
  
  **Solution 3:**
  1.  **Misprediction 1 (At the exit of the loop):** The loop has been jumping back to the top over and over, so the predictor is firmly set to "Predict Taken". When the loop finally finishes, the branch falls through (Not Taken). The predictor guesses "Taken" and is **incorrect**. It flips its state to "Predict Not Taken".
  2.  **Misprediction 2 (At the start of the NEXT run):** The next time the program reaches this same loop, the predictor remembers that it exited last time, so it guesses "Not Taken". But the loop starts up again (Taken). The predictor is **incorrect** and flips its state back to "Predict Taken".
  
  ---
### **Topic 2: Static Predictors**
  
  **Problem 4: Static Prediction Calculation (Like HW5 Q4)**
  For the branch outcome sequence given below, calculate the branch prediction accuracy of the static branch prediction strategies: “Always predict taken” and “Always predict not taken”. Which strategy is better for this sequence?
  Sequence: `T, T, T, T, N, T, N, T`
  
  **Solution 4:**
  *   **Total Branches:** 8
  *   **Total 'T' Outcomes:** 6
  *   **Total 'N' Outcomes:** 2
  
  *   **"Always Predict Taken" Accuracy:** $6 / 8 = \mathbf{75\%}$
  *   **"Always Predict Not Taken" Accuracy:** $2 / 8 = \mathbf{25\%}$
  *   *Conclusion:* **"Always predict taken"** is the significantly better strategy for this sequence because 'T' naturally occurs far more frequently.
  
  ***
  
  **Problem 5: Heuristics for Static Prediction**
  Compilers and hardware can use "rules of thumb" to implement smarter static predictions. If a processor encounters a branch that jumps **backward** in the code, what should it statically predict? If it encounters a branch that jumps **forward**, what should it predict? Justify your answers based on typical programming behavior.
  
  **Solution 5:**
  *   **Backward Branches $\to$ Predict TAKEN:** A backward jump almost always represents a **loop** (e.g., jumping back to the top of a `while` loop). Loops iterate many times, so guessing "Taken" is statistically highly accurate.
  *   **Forward Branches $\to$ Predict NOT TAKEN:** A forward jump almost always represents an **if-statement** skipping over a block of code. Code is generally meant to be executed sequentially, so guessing "Not Taken" (falling through) is the safer statistical bet.
  
  ***
  
  **Problem 6: The Pipeline Penalty**
  In the LEGv8 pipeline, the hardware assumes a static **"Predict Not-Taken"** approach by default. It just keeps fetching `PC + 4`. 
  If the instruction in the ID stage is evaluated and the branch *should* have been **Taken**, how many stall cycles (bubbles) does the CPU suffer? What happens to the instruction currently in the IF stage?
  
  **Solution 6:**
  *   The CPU suffers a **1-cycle stall penalty**. 
  *   The instruction currently in the IF stage (which is the wrong instruction, `PC + 4`) is **flushed**. Its control signals are forced to `0` (turning it into a NOP bubble), and the PC is updated to the correct Branch Target address so the right instruction can be fetched on the next cycle.
  
  ---
### **Topic 3: 2-Bit Dynamic Predictors**
  
  *Note: A 2-Bit predictor has 4 states: Strongly Taken (ST), Weakly Taken (WT), Weakly Not Taken (WN), Strongly Not Taken (SN). It requires TWO consecutive mispredictions to flip its overall True/False guess.*
  
  **Problem 7: 2-Bit Predictor Trace**
  Trace the following branch sequence using a **2-Bit Dynamic Predictor**. Calculate the accuracy.
  *Assume the initial state is **Strongly Taken (ST)***.
  Sequence: `T, T, N, T, T`
  
  **Solution 7:**
  
  | Step | Current State | Actual | Result | Next State |
  | :---: | :---: | :---: | :---: | :---: |
  | 1 | Predict **T** (ST) | **T** | ✅ Correct | ST (Strongly Taken) |
  | 2 | Predict **T** (ST) | **T** | ✅ Correct | ST (Strongly Taken) |
  | 3 | Predict **T** (ST) | **N** | ❌ Incorrect | WT (Weakly Taken) |
  | 4 | Predict **T** (WT) | **T** | ✅ Correct | ST (Strongly Taken) |
  | 5 | Predict **T** (ST) | **T** | ✅ Correct | ST (Strongly Taken) |
  
  *   **Accuracy:** $4 / 5 = \mathbf{80\%}$
  *   *Observation:* Notice how the single 'N' anomaly caused an incorrect guess, but it *did not* flip the predictor to "Not Taken". It just dropped it to "Weakly Taken", saving the prediction for Step 4!
  
  ***
  
  **Problem 8: 1-Bit vs 2-Bit Comparison**
  Use the sequence from Problem 7 (`T, T, N, T, T`). If you had used a **1-Bit Predictor** starting in "Predict Taken", how many mispredictions would have occurred? Why is the 2-bit predictor superior here?
  
  **Solution 8:**
  *   **1-Bit Trace:**
    *   T $\to$ Correct (Stays T)
    *   T $\to$ Correct (Stays T)
    *   N $\to$ **Incorrect** (Flips to N)
    *   T $\to$ **Incorrect** (Flips to T)
    *   T $\to$ Correct (Stays T)
  *   *Conclusion:* The 1-bit predictor has **2 mispredictions** (Accuracy 60%). The 2-bit predictor is superior because it has "hysteresis" (memory). It forgives the single 'N' anomaly, whereas the 1-bit predictor immediately changes its mind, guaranteeing a second failure when the pattern resumes.
  
  ***
  
  **Problem 9: 2-Bit Predictor on Alternating Patterns**
  Trace the alternating pattern `T, N, T, N, T` using a **2-Bit Predictor**. 
  Assume the initial state is **Strongly Taken (ST)**. Calculate the accuracy.
  
  **Solution 9:**
  
  | Step | Current State | Actual | Result | Next State |
  | :---: | :---: | :---: | :---: | :---: |
  | 1 | Predict **T** (ST) | **T** | ✅ Correct | ST (Strongly Taken) |
  | 2 | Predict **T** (ST) | **N** | ❌ Incorrect | WT (Weakly Taken) |
  | 3 | Predict **T** (WT) | **T** | ✅ Correct | ST (Strongly Taken) |
  | 4 | Predict **T** (ST) | **N** | ❌ Incorrect | WT (Weakly Taken) |
  | 5 | Predict **T** (WT) | **T** | ✅ Correct | ST (Strongly Taken) |
  
  *   **Accuracy:** $3 / 5 = \mathbf{60\%}$
  *   *Observation:* Compare this to Problem 2! The 1-bit predictor had 16% accuracy on an alternating pattern. Because the 2-bit predictor started in "Strongly Taken", it stubbornly kept guessing "Taken", allowing it to catch half of the alternating sequence successfully.
  
  ***
  
  **Problem 10: The Branch History Table (Buffer)**
  In dynamic branch prediction, where exactly are these 1-bit or 2-bit state variables physically stored in the processor? How does the processor know which state variable corresponds to which branch instruction?
  
  **Solution 10:**
  *   They are stored in a small, fast SRAM memory array called the **Branch Prediction Buffer** (or Branch History Table).
  *   The table is **indexed by the lower bits of the branch instruction's memory address**. When the CPU fetches an instruction, it looks at the address, uses the lower bits to jump to that row in the table, and instantly reads the 1-bit or 2-bit prediction state before the instruction is even decoded!
- ---
- ---
- ---
### **Topic: Pipeline Registers \& The "Wrong Register" Bug**
  
  **Problem 1: Purpose of Pipeline Registers**
  In the transition from a single-cycle datapath to a pipelined datapath, four large registers were added: `IF/ID`, `ID/EX`, `EX/MEM`, and `MEM/WB`. 
  What is the primary physical purpose of these pipeline registers, and when do they update their stored values?
  
  **Solution 1:**
  *   **Purpose:** They act as "suitcases" or buffers that hold the data and control signals computed in one stage so they can be safely passed to the next stage. This physically isolates the 5 stages from each other, preventing the electrical signals of Instruction 2 from crashing into the electrical signals of Instruction 1.
  *   **Update Timing:** They update their stored values synchronously on every **clock edge** (tick).
  
  ***
  
  **Problem 2: The "Wrong Register" Bug (Slide 57-58)**
  Look at the `WB` (Write-Back) stage of the pipeline. The data being written back to the Register File comes from the `MEM/WB` pipeline register. 
  Why must the 5-bit destination register number (`Rd` or `Rt`) also be passed down the pipeline from `ID/EX` $\to$ `EX/MEM` $\to$ `MEM/WB`? What catastrophic error would occur if the Register File just read the destination register bits directly from the `IF/ID` register?
  
  **Solution 2:**
  *   **The Error:** If the WB stage read the destination register directly from the `IF/ID` register, it would write the data into the wrong register! 
  *   **Why:** Because of pipelining, by the time Instruction 1 reaches the WB stage, the IF and ID stages are already processing Instruction 4 and Instruction 5. If the WB stage looked at the `IF/ID` register, it would see the destination register for Instruction 5, not Instruction 1.
  *   **The Fix:** Passing the 5-bit destination register number down through the pipeline "suitcases" ensures that the instruction remembers exactly *where* it is supposed to save its data when it finally reaches the end of the pipeline.
  
  ---
### **Topic: Control Signal Propagation**
  
  **Problem 3: Grouping Control Signals (Slide 66)**
  In the pipelined datapath, the Main Control Unit sits in the **ID** stage and generates all 9 control signals at once. However, these signals are grouped into three categories: **EX**, **M**, and **WB**.
  List which specific control signals belong to the **M** (Memory) group and which belong to the **WB** (Write-Back) group.
  
  **Solution 3:**
  *   **M (Memory) Group:** `MemRead`, `MemWrite`, `Branch`. *(These travel to the EX/MEM register).*
  *   **WB (Write-Back) Group:** `RegWrite`, `MemtoReg`. *(These travel all the way to the MEM/WB register).*
  
  ***
  
  **Problem 4: Tracing a Control Signal**
  Consider the instruction `LDUR X9,[X10, #8]`. This instruction requires the `MemRead` control signal to be set to `1`. 
  1. In which pipeline stage is this `1` generated?
  2. Which pipeline register(s) does this `1` travel through?
  3. In which pipeline stage is this `1` actually used by the hardware?
  
  **Solution 4:**
  1.  Generated in the **ID** (Decode) stage by the Main Control Unit.
  2.  It travels through the **`ID/EX`** register, and then into the **`EX/MEM`** register.
  3.  It is actually used in the **MEM** stage (it triggers the Data Memory to output the requested data). After the MEM stage, it is discarded and does not travel to the `MEM/WB` register.
  
  ---
### **Topic: Cycle-by-Cycle Tracing (Pipeline Diagrams)**
  
  **Problem 5: Multi-Cycle Pipeline Diagram**
  Consider the following sequence of three independent instructions executing in a 5-stage pipeline:
  1. `ADD X1, X2, X3`
  2. `LDUR X4, [X5, #0]`
  3. `SUB X6, X7, X8`
  
  A) During Clock Cycle 4, which pipeline stage is the `LDUR` instruction in? What hardware component is it actively using?
  B) During Clock Cycle 5, which pipeline stage is the `ADD` instruction in?
  
  **Solution 5:**
  *Trace the timeline:*
  *   CC1: ADD in IF
  *   CC2: ADD in ID, LDUR in IF
  *   CC3: ADD in EX, LDUR in ID, SUB in IF
  *   CC4: ADD in MEM, **LDUR in EX**, SUB in ID
  *   CC5: **ADD in WB**, LDUR in MEM, SUB in EX
  *   **Part A:** `LDUR` is in the **EX** stage. It is actively using the **ALU** to calculate the physical memory address (Base X5 + Offset 0).
  *   **Part B:** `ADD` is in the **WB** stage.
  
  ***
  
  **Problem 6: The "Don't Cares" of the WB Stage**
  An instruction has just reached the `MEM/WB` pipeline register. The control signals stored in this register are extracted and sent to the hardware. The signals are:
  `RegWrite = 0` and `MemtoReg = X`.
  1. What physical action happens in the WB stage for this instruction?
  2. What specific instruction class (R-type, Load, Store, or Branch) could this be?
  
  **Solution 6:**
  1. **No physical action happens.** Because `RegWrite = 0`, the Register File ignores all inputs. The value of `MemtoReg` doesn't matter (X). 
  2. This is either a **Store (`STUR`)** or a **Branch (`CBZ` / `B`)**. None of these instructions write data back to the registers, so they effectively do nothing during the WB stage.
  
  ---
### **Topic: Hardware Placement for Hazards**
  
  **Problem 7: The Forwarding Unit Placement**
  In the pipelined datapath, the **Forwarding Unit** is a hardware component added to resolve Data Hazards. 
  A) In which pipeline stage is the Forwarding Unit physically located?
  B) What specific inputs does it read from the pipeline registers to determine if forwarding is necessary?
  
  **Solution 7:**
  *   **Part A:** It is physically located in the **EX** stage (right before the ALU).
  *   **Part B:** It reads the **Source Registers (`Rn`, `Rm`)** from the current `ID/EX` register, and compares them against the **Destination Registers (`Rd`)** stored in the `EX/MEM` and `MEM/WB` pipeline registers. It also checks the `RegWrite` signal from those registers to ensure the previous instructions are actually writing data.
  
  ***
  
  **Problem 8: The Hazard Detection Unit Placement**
  The **Hazard Detection Unit** resolves Load-Use hazards by inserting a stall. 
  A) In which pipeline stage is the Hazard Detection Unit physically located?
  B) What are the three specific physical actions it takes to create a "bubble" (stall) in the pipeline?
  
  **Solution 8:**
  *   **Part A:** It is located in the **ID** stage.
  *   **Part B:** 
    1. It turns all control signals currently leaving the Control Unit to **0** (creating a NOP in the `ID/EX` register).
    2. It disables the **PC write** signal (so the PC doesn't advance, re-fetching the same instruction).
    3. It disables the **`IF/ID` write** signal (so the instruction currently in the IF/ID register is preserved and decoded again on the next cycle).
  
  ---
### **Topic: Branch Hardware Migration**
  
  **Problem 9: Moving the Branch Adder**
  In the original Single-Cycle Datapath, the branch target address adder (the one connected to the Shift Left 2 unit) was located in the **EX** stage. In the final Pipelined Datapath, hardware engineers moved this branch adder into the **ID** stage.
  Why was this physical hardware change made, and how does it impact pipeline performance?
  
  **Solution 9:**
  *   **Why:** If the branch is calculated in the EX stage, the CPU doesn't know whether to jump or not until Clock Cycle 3. By that time, it has already fetched the wrong instructions in CC2 and CC3. 
  *   **Impact:** Moving the adder and a register comparator into the ID stage means the CPU knows the branch target and the branch outcome by the end of Clock Cycle 2. This reduces the branch flush penalty from **3 stall cycles down to just 1 stall cycle**, vastly improving pipeline performance!
  
  ***
  
  **Problem 10: Tracing a Branch (`CBZ`)**
  Trace the instruction `CBZ X9, Loop` through the **Pipelined** datapath (assuming the optimized datapath where branch logic is in the ID stage).
  What specific actions occur in the EX, MEM, and WB stages for this instruction?
  
  **Solution 10:**
  *   **Action:** **Absolutely nothing.**
  *   *Justification:* Because the branch condition (checking if X9 is zero) and the target address calculation were both completed early in the **ID** stage, the instruction has finished its job. 
  *   As the `CBZ` instruction moves into the EX, MEM, and WB pipeline registers, all of its control signals (`RegWrite`, `MemWrite`, etc.) are set to `0`. It passes through the rest of the pipeline as a harmless "bubble" (NOP), doing no math and writing no data.
- ---
- ---
- ---
### **Topic: Locality \& Memory Technology**
  
  **Problem 1: The Principle of Locality**
  Consider the following C code snippet:
  ```c
  int sum = 0;
  for (int i = 0; i < 1000; i++) {
    sum = sum + array[i];
  }
  ```
  Identify which variable exhibits strong **Temporal Locality** and which variable exhibits strong **Spatial Locality**. Briefly justify your answers.
  
  **Solution 1:**
  *   **Temporal Locality (Time):** The variables **`sum`** and **`i`**. 
    *   *Justification:* Temporal locality means if an item is accessed, it will likely be accessed again very soon. `sum` and `i` are accessed and updated on every single iteration of the loop.
  *   **Spatial Locality (Space):** The variable **`array`**.
    *   *Justification:* Spatial locality means if an item is accessed, items at nearby addresses will be accessed soon. The loop accesses `array[0]`, then immediately accesses the neighbor `array[1]`, `array[2]`, etc.
  
  ***
  
  **Problem 2: SRAM vs. DRAM**
  A computer uses SRAM for its Cache and DRAM for its Main Memory. 
  A) What is the physical reason DRAM is slower than SRAM?
  B) Why does DRAM require "periodic refreshing" while SRAM does not?
  
  **Solution 2:**
  *   **A)** DRAM uses 1 tiny capacitor and 1 transistor per bit. Charging and discharging this capacitor takes physical time. SRAM uses a 6-transistor cross-coupled inverter circuit that relies purely on electrical switching, making it much faster.
  *   **B)** The capacitor in a DRAM cell slowly leaks its electrical charge over time. If it isn't periodically read and rewritten (refreshed), the data fades away. SRAM holds its state statically as long as power is applied.
  
  ---
### **Topic: Cache Performance Math (AMAT \& CPI)**
  
  **Problem 3: Average Memory Access Time (AMAT)**
  A processor has a clock cycle time of $1\text{ ns}$. The L1 cache has a hit time of 1 clock cycle and a miss rate of $5\%$. The penalty to fetch data from main memory on a miss is $20\text{ cycles}$. 
  Calculate the **AMAT** in both clock cycles and nanoseconds.
  
  **Solution 3:**
  1.  **Identify Formula:** $\text{AMAT} = \text{Hit Time} + (\text{Miss Rate} \times \text{Miss Penalty})$
  2.  **Substitute Values:** $\text{AMAT} = 1 + (0.05 \times 20)$
  3.  **Calculate:** $\text{AMAT} = 1 + 1 = \mathbf{2 \text{ clock cycles}}$.
  4.  **Convert to ns:** Since 1 cycle = $1\text{ ns}$, the AMAT is **$2\text{ ns}$**.
  
  ***
  
  **Problem 4: Effective CPI with Cache Misses**
  A processor has a base CPI of $1.5$ (assuming a perfect cache). 
  *   The Instruction cache miss rate is $2\%$.
  *   The Data cache miss rate is $4\%$.
  *   Loads and Stores make up $25\%$ of all instructions.
  *   The miss penalty for any cache miss is $100\text{ cycles}$.
  Calculate the **Actual (Effective) CPI** of this processor.
  
  **Solution 4:**
  1.  **Formula:** $\text{Actual CPI} = \text{Base CPI} + \text{I-Cache Stalls} + \text{D-Cache Stalls}$
  2.  **I-Cache Stalls:** Every single instruction accesses the I-Cache. 
    *   $1.0 \times 0.02 \times 100 = \mathbf{2.0 \text{ cycles per instruction}}$.
  3.  **D-Cache Stalls:** Only Loads and Stores (25%) access the D-Cache. 
    *   $0.25 \times 0.04 \times 100 = \mathbf{1.0 \text{ cycle per instruction}}$.
  4.  **Total Actual CPI:** $1.5 + 2.0 + 1.0 = \mathbf{4.5 \text{ CPI}}$.
  *(Note: Memory stalls made the CPU 3 times slower than its ideal baseline!)*
  
  ---
### **Topic: Multilevel Caches (L1 \& L2)**
  
  **Problem 5: Multilevel CPI (Like Slide 47)**
  To improve the processor from Problem 4, engineers add an L2 cache. 
  *   Base CPI = $1.0$.
  *   L1 cache miss rate = $5\%$.
  *   L2 cache hit penalty = $10\text{ cycles}$.
  *   Global miss rate to Main Memory = $1\%$.
  *   Main Memory penalty = $100\text{ cycles}$.
  Calculate the new **Actual CPI**.
  
  **Solution 5:**
  1.  **Formula:** $\text{CPI} = \text{Base} + (\text{L1 Misses hitting L2}) + (\text{Global Misses hitting RAM})$
  2.  **L2 Hit Penalty:** $0.05 \times 10 = \mathbf{0.5 \text{ cycles}}$.
  3.  **Main Memory Penalty:** $0.01 \times 100 = \mathbf{1.0 \text{ cycle}}$.
  4.  **Actual CPI:** $1.0 + 0.5 + 1.0 = \mathbf{2.5 \text{ CPI}}$.
  
  ***
  
  **Problem 6: Local vs. Global Miss Rate**
  An L1 cache receives $1000$ memory requests and misses on $100$ of them. Those $100$ requests go to the L2 cache. The L2 cache misses on $20$ of them, which then go to Main Memory.
  A) What is the **Local** miss rate of the L2 cache?
  B) What is the **Global** miss rate of the L2 cache?
  
  **Solution 6:**
  *   **A) Local Miss Rate:** (Misses in L2) / (Total accesses *to* L2).
    *   $20 / 100 = \mathbf{20\%}$.
  *   **B) Global Miss Rate:** (Misses in L2) / (Total accesses generated by the CPU).
    *   $20 / 1000 = \mathbf{2\%}$.
  
  ---
### **Topic: Cache Organization Trade-offs**
  
  **Problem 7: Handling Writes (Write-Through vs. Write-Back)**
  A programmer is designing a cache. 
  A) If they choose a **Write-Through** policy, what additional hardware structure MUST they add to prevent massive performance stalls?
  B) If they choose a **Write-Back** policy, what extra piece of metadata (a 1-bit flag) must be added to every block in the cache?
  
  **Solution 7:**
  *   **A) Write Buffer:** Because Write-Through updates Main Memory on every single store, the CPU would stall for ~100 cycles every time a variable is updated. A write buffer holds the data and writes it to RAM in the background so the CPU can keep working immediately.
  *   **B) Dirty Bit:** Because Write-Back caches only update RAM when a block is evicted, the cache must track whether the data in the cache has been modified. If the Dirty Bit is 0, it can just be overwritten. If it is 1, it must be saved to RAM first.
  
  ***
  
  **Problem 8: Block Size Trade-offs (Slide 24)**
  Assume you have a cache with a fixed capacity of 16 KB. You decide to increase the block size from 16 bytes per block to 128 bytes per block. 
  A) Initially, why does increasing the block size *decrease* the miss rate?
  B) Eventually, if you keep making the blocks bigger and bigger, the miss rate will spike heavily. Give two reasons why.
  
  **Solution 8:**
  *   **Part A:** Larger blocks take advantage of **Spatial Locality**. When you miss on `A[0]`, a large block pulls in `A[1]` through `A[31]` for free, preventing future misses.
  *   **Part B:** 
    1.  **Pollution:** The cache pulls in massive chunks of data the program doesn't actually need, wasting space.
    2.  **Competition:** Because the total cache size is fixed at 16 KB, making blocks larger means there are *fewer total blocks*. This causes massive conflict misses, as different variables fight for the few available slots.
  
  ***
  
  **Problem 9: Memory Bandwidth \& Interleaving (Slide 31-32)**
  A cache needs to fetch a 4-word block from Main Memory. The memory bus takes 1 cycle to send the address, DRAM takes 15 cycles to find the data, and the bus takes 1 cycle to send a word of data back.
  A) If the memory is **1-word wide** (non-interleaved), how many total bus cycles does the miss penalty take?
  B) If the memory is **4-bank interleaved**, how many total bus cycles does the miss penalty take?
  
  **Solution 9:**
  *   **A) 1-Word Wide:** You must pay the 15-cycle DRAM latency for *every single word*.
    *   $1\text{ (Addr)} + [4 \text{ words} \times 15\text{ (DRAM)}] + [4 \text{ words} \times 1\text{ (Transfer)}] = 1 + 60 + 4 = \mathbf{65 \text{ cycles}}$.
  *   **B) 4-Bank Interleaved:** You send the address once, all 4 banks look up the data simultaneously (paying the 15-cycle penalty only once), and then you transfer all 4 words back-to-back.
    *   $1\text{ (Addr)} + 15\text{ (DRAM)} + [4 \text{ words} \times 1\text{ (Transfer)}] = 1 + 15 + 4 = \mathbf{20 \text{ cycles}}$.
  
  ***
  
  **Problem 10: Write Allocation Policies**
  When a CPU tries to write to a memory address that is currently NOT in the cache (a Write Miss), there are two standard ways to handle it.
  Briefly explain **"Allocate on Miss"** and **"Write Around (No-Allocate)"**. Which one is typically paired with a Write-Back cache?
  
  **Solution 10:**
  *   **Allocate on Miss:** The CPU fetches the entire block from Main Memory into the cache, and *then* writes the new data into the cache block. **(Typically paired with Write-Back).**
  *   **Write Around (No-Allocate):** The CPU bypasses the cache entirely and writes the new data directly into Main Memory. It does not fetch the block into the cache. **(Typically paired with Write-Through).**
  
  ---
- ---
- ---
- ---
### **Topic: Address Subdivision (Like HW6 Q1)**
  *Assume for all problems in this section: 32-bit addresses, Byte-addressable memory, and 1 Word = 4 Bytes.*
  
  **Problem 1: Direct-Mapped Subdivision**
  Suppose you have a cache memory that can store **4096 data words**. The cache is **Direct-Mapped with 1 word per block**. 
  Calculate the exact number of bits required for the Byte Offset, Word Offset, Index, and Tag. 
  
  **Solution 1:**
  *   **Byte Offset:** 1 word = 4 bytes. $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Word Offset:** 1 word per block. $\log_2(1) = \mathbf{0 \text{ bits}}$.
  *   **Index:** 
    *   Total blocks = $4096 \text{ words} / 1 \text{ word/block} = 4096 \text{ blocks}$.
    *   Direct-mapped means 1 block per set. Total Sets = $4096$.
    *   $\log_2(4096) = \mathbf{12 \text{ bits}}$.
  *   **Tag:** $32 - (12 + 0 + 2) = \mathbf{18 \text{ bits}}$.
  
  ***
  
  **Problem 2: Changing the Block Size**
  Suppose you have the same cache capacity (**4096 data words**), but it is configured as **Direct-Mapped with 8 words per block**. 
  Calculate the bits for Byte Offset, Word Offset, Index, and Tag. 
  
  **Solution 2:**
  *   **Byte Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$. (Never changes for 4-byte words).
  *   **Word Offset:** 8 words per block. $\log_2(8) = \mathbf{3 \text{ bits}}$.
  *   **Index:** 
    *   Total blocks = $4096 \text{ words} / 8 \text{ words/block} = 512 \text{ blocks}$.
    *   Direct-mapped means Total Sets = $512$.
    *   $\log_2(512) = \mathbf{9 \text{ bits}}$.
  *   **Tag:** $32 - (9 + 3 + 2) = \mathbf{18 \text{ bits}}$.
  
  ***
  
  **Problem 3: Set-Associative Subdivision**
  Suppose you have the same cache capacity (**4096 data words**), but it is configured as **4-Way Set Associative with 2 words per block**. 
  Calculate the bits for Byte Offset, Word Offset, Index, and Tag.
  
  **Solution 3:**
  *   **Byte Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Word Offset:** 2 words per block. $\log_2(2) = \mathbf{1 \text{ bit}}$.
  *   **Index:** 
    *   Total blocks = $4096 \text{ words} / 2 \text{ words/block} = 2048 \text{ blocks}$.
    *   4-Way means 4 blocks per set. Total Sets = $2048 / 4 = 512 \text{ Sets}$.
    *   $\log_2(512) = \mathbf{9 \text{ bits}}$.
  *   **Tag:** $32 - (9 + 1 + 2) = \mathbf{20 \text{ bits}}$.
  
  ***
  
  **Problem 4: Fully Associative Subdivision**
  Suppose you have the same cache capacity (**4096 data words**), but it is configured as **Fully Associative with 4 words per block**. 
  Calculate the bits for Byte Offset, Word Offset, Index, and Tag.
  
  **Solution 4:**
  *   **Byte Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Word Offset:** 4 words per block. $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Index:** Fully Associative means there is only 1 massive set. $\log_2(1) = \mathbf{0 \text{ bits}}$.
  *   **Tag:** $32 - (0 + 2 + 2) = \mathbf{28 \text{ bits}}$.
  
  ---
### **Topic: Block Mapping Math (Like Slide 23)**
  
  **Problem 5: Calculating Block Address and Index**
  A cache has **32 blocks** and each block holds **16 bytes**. 
  If the CPU requests data at **Byte Address 1000**, answer the following:
  A) What is the memory Block Address?
  B) To what specific Cache Block Number (Index) does this address map? (Assume Direct-Mapped).
  
  **Solution 5:**
  *   **A) Block Address:** $\lfloor \text{Byte Address} / \text{Bytes per Block} \rfloor$
    *   $\lfloor 1000 / 16 \rfloor = 62.5 \rightarrow \mathbf{62}$.
    *   *(Mental math: $16 \times 60 = 960$. $16 \times 2 = 32$. $960+32 = 992$. $1000-992=8$, so it is Block 62, byte 8).*
  *   **B) Cache Index:** $\text{Block Address} \pmod{\text{Total Cache Blocks}}$
    *   $62 \pmod{32} = \mathbf{30}$.
    *   *(It maps to Cache Index 30).*
  
  ---
### **Topic: Cache Tracing (Like HW6 Q2)**
  *For Problems 6-8, assume a simple memory hierarchy:*
  *   Cache stores **4 one-word blocks**. 
  *   Addresses provided are **WORD addresses** (meaning no byte offset needed; Address = [Tag][Index]).
  *   Word Address Sequence: **`1, 3, 1, 5, 4, 1`** (Binary: `001, 011, 001, 101, 100, 001`).
  
  **Problem 6: Direct Mapped Trace**
  Trace the sequence for a **Direct Mapped** organization. Calculate the Hit Ratio. 
  *(Hint: Index is 2 bits. Tag is 1 bit).*
  
  **Solution 6:**
  *Index is Address mod 4 (the last 2 bits).*
  *   `001`: Index `01`, Tag `0`. Miss. (Cache[1] = M[1])
  *   `011`: Index `11`, Tag `0`. Miss. (Cache[3] = M[3])
  *   `001`: Index `01`, Tag `0`. **Hit.** 
  *   `101`: Index `01`, Tag `1`. Miss. (Cache[1] = M[5] $\to$ *Evicts M[1]!*)
  *   `100`: Index `00`, Tag `1`. Miss. (Cache[0] = M[4])
  *   `001`: Index `01`, Tag `0`. Miss. (Cache[1] = M[1] $\to$ *Evicts M[5]!*)
  *   **Hit Ratio: 1/6 (16.67%)**
  
  ***
  
  **Problem 7: 2-Way Set Associative Trace**
  Trace the same sequence for a **2-Way Set Associative** organization using LRU replacement. Calculate the Hit Ratio.
  *(Hint: 4 blocks / 2 ways = 2 Sets. Index is 1 bit. Tag is 2 bits).*
  
  **Solution 7:**
  *Index is Address mod 2 (the last 1 bit). Each set holds 2 blocks.*
  *   `001`: Set `1`. Miss. (Set 1: M[1], empty)
  *   `011`: Set `1`. Miss. (Set 1: M[1], M[3])
  *   `001`: Set `1`. **Hit.** (Set 1: M[3], M[1] $\to$ *M[1] is now Most Recently Used! M[3] is LRU*)
  *   `101`: Set `1`. Miss. (Set 1: M[5], M[1] $\to$ *Evicts M[3] because it was LRU!*)
  *   `100`: Set `0`. Miss. (Set 0: M[4], empty)
  *   `001`: Set `1`. **Hit.** (Set 1: M[5], M[1] $\to$ *Still in cache!*)
  *   **Hit Ratio: 2/6 (33.3%)**
  
  ***
  
  **Problem 8: Fully Associative Trace**
  Trace the same sequence for a **Fully Associative** organization using LRU. Calculate the Hit Ratio.
  *(Hint: 1 Set containing all 4 blocks. No Index bits. Tag is all 3 bits).*
  
  **Solution 8:**
  *Blocks go into the first empty slot. LRU kicks out the oldest un-accessed block.*
  *   `001`: Miss. (Cache: M[1], -, -, -)
  *   `011`: Miss. (Cache: M[1], M[3], -, -)
  *   `001`: **Hit.** (Cache: M[3], M[1], -, - $\to$ *M[3] is now LRU, M[1] is MRU*)
  *   `101`: Miss. (Cache: M[3], M[1], M[5], -)
  *   `100`: Miss. (Cache: M[3], M[1], M[5], M[4])
  *   `001`: **Hit.** (Cache: M[3], M[5], M[4], M[1])
  *   **Hit Ratio: 2/6 (33.3%)**
  
  ---
### **Topic: Architecture \& Implementation**
  
  **Problem 9: The Associativity Hardware Cost**
  Your team is deciding between building a Direct Mapped cache and a 4-Way Set Associative cache. Both have 1024 blocks.
  A) When a memory address arrives at the Direct Mapped cache, how many hardware comparators are needed to check the Tag?
  B) When a memory address arrives at the 4-Way Set Associative cache, how many hardware comparators are needed? Why?
  
  **Solution 9:**
  *   **A) 1 Comparator.** The hardware uses the Index to go to the exactly 1 slot where the data *must* be. It only has to compare the Address Tag against the single Tag stored in that one slot.
  *   **B) 4 Comparators.** The hardware uses the Index to go to the correct Set. Because that Set contains 4 different blocks, the requested data could be in *any* of those 4 slots. To be fast, the hardware must read all 4 tags simultaneously and compare them all at once. This extra hardware is why Set Associative caches have slightly longer Hit Times.
  
  ***
  
  **Problem 10: The "Word-Addressable" Trap**
  You are given a cache memory with 8 blocks. The block size is 1 word (4 bytes).
  The CPU generates the **Byte Address `24`**. 
  A) What is the binary cache Index this address maps to? 
  B) If the problem stated the CPU generated the **Word Address `24`**, what would the binary cache Index be?
  
  **Solution 10:**
  *   **A) Byte Address 24:** 
    *   Because it's a byte address, we must remove the byte offset first!
    *   Block Address = $\lfloor 24 / 4 \rfloor = 6$.
    *   Index = $6 \pmod 8 = 6$. Binary = **`110`**.
  *   **B) Word Address 24:**
    *   Because it's *already* a word address, there is no byte offset to remove. We just map it directly!
    *   Block Address = 24.
    *   Index = $24 \pmod 8 = 0$. Binary = **`000`**.
- ---
- ---
- ---
### **Topic: The Core Tracing Exercise (Like HW6 Q2)**
  *For Problems 1–3, assume a simple memory hierarchy:*
  *   Cache stores **4 one-word data blocks**.
  *   Main memory is word-addressable.
  *   Word address sequence (in decimal): **`0, 4, 0, 5, 2, 4, 1`**
  
  **Problem 1: Direct Mapped Trace**
  Complete the trace table for a **Direct Mapped** cache. Show the Hit/Miss status and the final cache contents. Calculate the Hit Ratio. 
  
  **Solution 1:**
  *   **Index Calculation:** Block Address $\pmod 4$. (Or in binary, the last 2 bits).
  *   *Math:* $0 \bmod 4 = \mathbf{0}$, $4 \bmod 4 = \mathbf{0}$, $5 \bmod 4 = \mathbf{1}$, $2 \bmod 4 = \mathbf{2}$, $1 \bmod 4 = \mathbf{1}$.
  
  | Addr | Index | Hit/Miss | Cache Content (Index 00, 01, 10, 11) |
  | :--- | :---: | :---: | :--- |
  | **0** | 00 | Miss | Mem[0], --, --, -- |
  | **4** | 00 | Miss | Mem[4], --, --, -- *(Evicts 0)* |
  | **0** | 00 | Miss | Mem[0], --, --, -- *(Evicts 4)* |
  | **5** | 01 | Miss | Mem[0], Mem[5], --, -- |
  | **2** | 10 | Miss | Mem[0], Mem[5], Mem[2], -- |
  | **4** | 00 | Miss | Mem[4], Mem[5], Mem[2], -- *(Evicts 0)* |
  | **1** | 01 | Miss | Mem[4], Mem[1], Mem[2], -- *(Evicts 5)* |
  
  *   **Hit Ratio: 0/7 (0%)**. *(This is a classic "Ping-Pong" conflict miss problem!)*
  
  ***
  
  **Problem 2: 2-Way Set Associative Trace**
  Trace the exact same sequence (`0, 4, 0, 5, 2, 4, 1`) for a **2-Way Set Associative** cache using LRU replacement. Calculate the Hit Ratio.
  
  **Solution 2:**
  *   **Index Calculation:** 4 blocks $\div$ 2 ways = 2 Sets. Block Address $\pmod 2$.
  *   *Math:* $0 \to Set 0$, $4 \to Set 0$, $5 \to Set 1$, $2 \to Set 0$, $1 \to Set 1$.
  
  | Addr | Index | Hit/Miss | Cache Content (Set 0) | Cache Content (Set 1) |
  | :--- | :---: | :---: | :--- | :--- |
  | **0** | 0 | Miss | M[0], -- | --, -- |
  | **4** | 0 | Miss | M[0], M[4] | --, -- |
  | **0** | 0 | **Hit** | M[4], M[0] *(0 is now MRU)* | --, -- |
  | **5** | 1 | Miss | M[4], M[0] | M[5], -- |
  | **2** | 0 | Miss | M[0], M[2] *(Evicts 4!)* | M[5], -- |
  | **4** | 0 | Miss | M[2], M[4] *(Evicts 0!)* | M[5], -- |
  | **1** | 1 | Miss | M[2], M[4] | M[5], M[1] |
  
  *   **Hit Ratio: 1/7 (14.3%)**.
  
  ***
  
  **Problem 3: Fully Associative Trace**
  Trace the exact same sequence (`0, 4, 0, 5, 2, 4, 1`) for a **Fully Associative** cache using LRU replacement. Calculate the Hit Ratio.
  
  **Solution 3:**
  *   **Index Calculation:** None! Everything goes into the single 4-block set.
  
  | Addr | Hit/Miss | Cache Content (Listed from LRU to MRU) |
  | :--- | :---: | :--- |
  | **0** | Miss | M[0] |
  | **4** | Miss | M[0], M[4] |
  | **0** | **Hit** | M[4], M[0] *(0 pushed to front)* |
  | **5** | Miss | M[4], M[0], M[5] |
  | **2** | Miss | M[4], M[0], M[5], M[2] |
  | **4** | **Hit** | M[0], M[5], M[2], M[4] *(4 pushed to front)* |
  | **1** | Miss | M[5], M[2], M[4], M[1] *(Cache was full, evicted oldest: 0)* |
  
  *   **Hit Ratio: 2/7 (28.5%)**.
  
  ---
### **Topic: The Traps \& Variations**
  
  **Problem 4: Multi-Word Block Tracing (Spatial Locality)**
  Assume a Direct Mapped cache has **4 blocks**, but each block holds **2 words**. 
  Trace the word address sequence: **`0, 1, 2, 3, 4, 5`**. Calculate the hit ratio.
  
  **Solution 4:**
  *   **Block Address Math:** Because there are 2 words per block, we must divide the word address by 2 to find the Block Address! $\lfloor \text{Word Addr} / 2 \rfloor$.
  *   *Block Addrs:* 0 $\to$ Block 0. 1 $\to$ Block 0. 2 $\to$ Block 1. 3 $\to$ Block 1. 4 $\to$ Block 2. 5 $\to$ Block 2.
  *   *Index Math:* Block Addr $\pmod 4$.
  
  | Addr | Blk Addr | Index | Hit/Miss | Action |
  | :--- | :---: | :---: | :---: | :--- |
  | **0** | 0 | 0 | Miss | Fetches block containing Words [0, 1] |
  | **1** | 0 | 0 | **Hit** | Word 1 is already in the cache! |
  | **2** | 1 | 1 | Miss | Fetches block containing Words [2, 3] |
  | **3** | 1 | 1 | **Hit** | Word 3 is already in the cache! |
  | **4** | 2 | 2 | Miss | Fetches block containing Words [4, 5] |
  | **5** | 2 | 2 | **Hit** | Word 5 is already in the cache! |
  
  *   **Hit Ratio: 3/6 (50%)**. *(This demonstrates why making blocks larger than 1 word improves spatial locality!)*
  
  ***
  
  **Problem 5: The Byte Address Trap**
  Assume a cache has **4 one-word blocks** and is Direct Mapped. 
  The CPU generates the following **BYTE** address sequence: `0, 4, 8, 12, 16`. 
  (Remember, 1 Word = 4 Bytes). Are there any hits in this sequence? Show your math.
  
  **Solution 5:**
  *   **Step 1:** Convert Byte Addresses to Word Addresses (divide by 4).
    *   $0 \to$ Word 0
    *   $4 \to$ Word 1
    *   $8 \to$ Word 2
    *   $12 \to$ Word 3
    *   $16 \to$ Word 4
  *   **Step 2:** Calculate Index (Word Addr $\pmod 4$).
    *   0 $\pmod 4 = 0$ (Miss)
    *   1 $\pmod 4 = 1$ (Miss)
    *   2 $\pmod 4 = 2$ (Miss)
    *   3 $\pmod 4 = 3$ (Miss)
    *   4 $\pmod 4 = 0$ (Miss, evicts Word 0).
  *   **Answer: 0 Hits.** If you forgot to divide by 4, you would have calculated `4 mod 4 = 0`, `8 mod 4 = 0`, `12 mod 4 = 0` and incorrectly assumed they were ping-ponging in the same slot.
  
  ---
### **Topic: Conceptual \& Replacement Theory**
  
  **Problem 6: Compulsory vs. Conflict Misses**
  Look back at the Direct Mapped trace in Problem 1. 
  A) When address `4` was accessed the *first* time, what type of miss was it? 
  B) When address `0` was accessed the *second* time, what type of miss was it?
  
  **Solution 6:**
  *   **A) Compulsory Miss (Cold Miss):** Address 4 had never been seen by the CPU before. It missed simply because the cache started empty. 
  *   **B) Conflict Miss:** Address 0 *was* in the cache, but it was kicked out because address 4 was forced to map to the exact same index (`00`). It missed due to competition for a slot.
  
  ***
  
  **Problem 7: LRU Hardware Logic**
  In a 4-Way Set Associative cache, a specific set currently holds blocks A, B, C, and D. 
  The current LRU order (from Most Recently Used to Least Recently Used) is: **`C, A, B, D`**.
  If the CPU requests block **`B`**, which results in a Cache Hit, what is the new LRU order for this set?
  
  **Solution 7:**
  *   Because `B` was accessed, it gets pulled from the middle of the line and placed at the very front as the Most Recently Used. The items previously in front of it slide down one spot.
  *   **New Order:** **`B, C, A, D`**. *(Notice D remains the LRU because it wasn't touched).*
  
  ***
  
  **Problem 8: Replacement Policy Trade-offs**
  If you are designing a highly associative cache (e.g., 8-Way Set Associative), why might you choose a **Random** replacement policy instead of **LRU (Least Recently Used)**?
  
  **Solution 8:**
  *   Tracking the exact age order of 8 different blocks for every single set requires a lot of extra bits and complex, slow tracking hardware. 
  *   **Random** replacement requires almost zero hardware overhead. For highly associative caches, statistically, evicting a random block performs almost exactly as well as LRU, saving significant hardware costs without sacrificing performance.
  
  ***
  
  **Problem 9: Loop Tracing (Real Code Application)**
  Consider the following pseudo-code that runs exactly once:
  `for (int i = 0; i < 4; i++) { A[i] = A[i] * 2; }`
  Assume `A[0]` starts at word address `0`. The CPU must READ the value, then WRITE the new value. 
  If the cache is Direct Mapped with **2 words per block**, how many total cache misses will occur during this loop?
  
  **Solution 9:**
  *   **Access Pattern:** Read 0, Write 0, Read 1, Write 1, Read 2, Write 2, Read 3, Write 3.
  *   **Trace:**
    *   Read 0: **Miss**. (Fetches Block 0, which brings in Words `0` and `1`).
    *   Write 0: **Hit**.
    *   Read 1: **Hit**. (Word 1 was brought in for free!).
    *   Write 1: **Hit**.
    *   Read 2: **Miss**. (Fetches Block 1, which brings in Words `2` and `3`).
    *   Write 2: **Hit**.
    *   Read 3: **Hit**. (Brought in for free!).
    *   Write 3: **Hit**.
  *   **Answer:** There will be exactly **2 cache misses** (out of 8 memory accesses).
  
  ***
  
  **Problem 10: Associativity Trade-offs**
  Processor X uses a Direct Mapped cache. Processor Y uses a Fully Associative cache. Both caches hold exactly 1024 blocks.
  A) Which processor will likely have the higher **Hit Ratio**? Why?
  B) Which processor will likely have the faster **Hit Time** (clock speed)? Why?
  
  **Solution 10:**
  *   **A) Processor Y (Fully Associative)** will have the higher Hit Ratio. Because any block can go anywhere, it completely eliminates Conflict Misses.
  *   **B) Processor X (Direct Mapped)** will have the faster Hit Time. The CPU instantly checks exactly 1 index and 1 tag. Processor Y requires massive, slow hardware to search 1024 tags simultaneously. (This is why L1 caches are often direct-mapped or low-associativity to keep the clock speed high!).
- ---
- ---
- ---
### **Practice Problem 1: The "Traffic Jam" (2-Way Set Associative)**
  **Setup:** You have a **2-Way Set Associative** cache that holds a total of **4 one-word blocks**. (This means 2 Sets: Set 0 and Set 1). 
  **Sequence:** The CPU generates the following **Word** addresses (in decimal):
  `0, 2, 4, 0, 2`
  **Task:** Trace the sequence using LRU replacement. Show the final cache content for each set and calculate the Hit Ratio.
### **Practice Problem 2: The "LRU Savior" (Fully Associative)**
  **Setup:** You have a **Fully Associative** cache that holds **4 one-word blocks**. (This means 1 giant Set, no Index).
  **Sequence:** The CPU generates the following **Word** addresses:
  `1, 2, 3, 4, 1, 5, 2`
  **Task:** Trace the sequence using LRU replacement. Track the exact order of your LRU "stack" to see who gets evicted. Calculate the Hit Ratio.
### **Practice Problem 3: The "Ping-Pong" (Direct Mapped)**
  **Setup:** You have a **Direct Mapped** cache that holds **4 one-word blocks**. 
  **Sequence:** The CPU generates the following **Word** addresses:
  `0, 1, 2, 4, 0, 4`
  **Task:** Trace the sequence. Calculate the Hit Ratio. Pay close attention to what happens at Index 0.
### **Practice Problem 4: The Byte Address Trap**
  **Setup:** You have a **2-Way Set Associative** cache that holds a total of **4 one-word blocks**. (2 Sets). 
  **Sequence:** The CPU generates the following **BYTE** addresses (in decimal):
  `0, 4, 16, 20, 8, 16`
  *(Hint: 1 Word = 4 Bytes. Do NOT use these numbers to find the Index yet!)*
  **Task:** Convert to Word addresses, then trace the sequence using LRU replacement. Calculate the Hit Ratio.
  
  ***
  ***
  ***
  ***
# **Answer Key & Explanations**
### **Solution 1: The "Traffic Jam" (2-Way)**
  *   **Math:** 4 blocks / 2 ways = 2 Sets. Index = Word Address $\pmod 2$.
  *   *Notice something?* $0 \pmod 2 = 0$. $2 \pmod 2 = 0$. $4 \pmod 2 = 0$. **Every single address maps to Set 0!** Set 1 remains completely empty.
  
  | Word Addr | Index | Hit/Miss | Set 0 Content (Left is MRU, Right is LRU) | Set 1 Content |
  | :---: | :---: | :---: | :--- | :--- |
  | **0** | 0 | Miss | M[0], -- | --, -- |
  | **2** | 0 | Miss | M[2], M[0] | --, -- |
  | **4** | 0 | Miss | M[4], M[2] *(Evicted 0 because it was LRU)* | --, -- |
  | **0** | 0 | Miss | M[0], M[4] *(Evicted 2 because it was LRU)* | --, -- |
  | **2** | 0 | Miss | M[2], M[0] *(Evicted 4 because it was LRU)* | --, -- |
  
  *   **Hit Ratio:** $0 / 5 = \mathbf{0\%}$.
  *   *Takeaway:* Even with a set-associative cache, if all your addresses happen to be multiples of your Set Count, you still get devastating conflict misses!
  
  ---
### **Solution 2: The "LRU Savior" (Fully Associative)**
  *   **Math:** No Index. Everything goes onto one big 4-slot clipboard.
  
  | Word Addr | Hit/Miss | Cache Content (Ordered from MRU to LRU) |
  | :---: | :---: | :--- |
  | **1** | Miss | M[1] |
  | **2** | Miss | M[2], M[1] |
  | **3** | Miss | M[3], M[2], M[1] |
  | **4** | Miss | M[4], M[3], M[2], M[1] *(Cache is full. 1 is at the bottom).* |
  | **1** | **Hit!** | M[1], M[4], M[3], M[2] *(1 is pulled from the bottom and saved!)* |
  | **5** | Miss | M[5], M[1], M[4], M[3] *(Because 1 was saved, 2 was at the bottom and gets evicted).* |
  | **2** | Miss | M[2], M[5], M[1], M[4] *(Evicted 3).* |
  
  *   **Hit Ratio:** $1 / 7 \approx \mathbf{14.3\%}$.
  *   *Takeaway:* If we hadn't accessed `1` on that 5th step, it would have been the one evicted when `5` came in! Accessing data resets its "age" to zero.
  
  ---
### **Solution 3: The "Ping-Pong" (Direct Mapped)**
  *   **Math:** 4 Blocks. Index = Word Address $\pmod 4$.
  
  | Word Addr | Index | Hit/Miss | Cache Content (Index 00, 01, 10, 11) |
  | :---: | :---: | :---: | :--- |
  | **0** | 0 | Miss | M[0], --, --, -- |
  | **1** | 1 | Miss | M[0], M[1], --, -- |
  | **2** | 2 | Miss | M[0], M[1], M[2], -- |
  | **4** | 0 | Miss | M[4], M[1], M[2], -- *(Evicted 0)* |
  | **0** | 0 | Miss | M[0], M[1], M[2], -- *(Evicted 4)* |
  | **4** | 0 | Miss | M[4], M[1], M[2], -- *(Evicted 0)* |
  
  *   **Hit Ratio:** $0 / 6 = \mathbf{0\%}$.
  *   *Takeaway:* In Direct Mapped, there is no LRU stack to save you. Address 0 and Address 4 both map to Index 0. If you alternate them, they instantly destroy each other every time.
  
  ---
### **Solution 4: The Byte Address Trap**
  *   **Step 1 (The Trap):** Convert Byte addresses to Word addresses by dividing by 4!
    *   Byte Addrs: `0, 4, 16, 20, 8, 16`
    *   Word Addrs: **`0, 1, 4, 5, 2, 4`**
  *   **Step 2 (The Math):** 4 blocks / 2 ways = 2 Sets. Index = Word Address $\pmod 2$.
  
  | Word Addr | Index | Hit/Miss | Set 0 Content (MRU, LRU) | Set 1 Content (MRU, LRU) |
  | :---: | :---: | :---: | :--- | :--- |
  | **0** | 0 | Miss | M[0], -- | --, -- |
  | **1** | 1 | Miss | M[0], -- | M[1], -- |
  | **4** | 0 | Miss | M[4], M[0] | M[1], -- |
  | **5** | 1 | Miss | M[4], M[0] | M[5], M[1] |
  | **2** | 0 | Miss | M[2], M[4] *(Evicted 0)* | M[5], M[1] |
  | **4** | 0 | **Hit!** | M[4], M[2] *(4 pulled to front)* | M[5], M[1] |
  
  *   **Hit Ratio:** $1 / 6 \approx \mathbf{16.67\%}$.
  *   *Takeaway:* If you didn't divide by 4 at the start, your indices would have been `0%2=0`, `4%2=0`, `16%2=0`... and everything would have falsely collided in Set 0! Because you correctly converted to Word Addresses, you saw that `4` mapped to Set 0, while `5` mapped to Set 1.