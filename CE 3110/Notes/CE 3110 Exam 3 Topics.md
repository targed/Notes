### **Part 1: Pipelining Basics & Performance (Ch. 4)**
  id:: 6a013c81-0705-474f-9720-f0506f39304c
  
  *   **The 5 Stages of the Pipeline:** 
  *   Instruction Fetch (IF), Instruction Decode (ID), Execute (EX), Memory (MEM), Write-Back (WB).
  *   **Pipeline Performance Math (Like HW5 Q1):**
  *   **Single-Cycle Time:** $\text{Total Time} = N \times \text{Clock Cycle Time}$
  *   **Pipelined Time:** $\text{Total Time} = (\text{Stages} + N - 1) \times \text{Clock Cycle Time}$
  *   **Speedup Calculation:** $\text{Speedup} = \frac{\text{Time}_{\text{Single}}}{\text{Time}_{\text{Pipelined}}}$
  *   **Pipeline Diagrams:**
  *   *Multi-cycle diagrams:* Drawing the "staircase" of instructions over time.
  *   *Single-cycle diagrams:* Looking at a snapshot of the datapath to see which instruction is in which stage at a specific clock cycle.
### **Part 2: Pipeline Hazards & Solutions (Ch. 4)**
  
  *   **Structural Hazards:** Hardware cannot support a combination of instructions (e.g., trying to read and write to the exact same memory at the exact same time). *Solution:* Separate Instruction and Data memories.
  *   **Data Hazards (Like HW5 Q2):**
    *   An instruction needs data that a previous instruction hasn't finished writing yet.
    *   **Forwarding (Bypassing):** Grabbing data from the `EX/MEM` or `MEM/WB` pipeline registers and feeding it directly into the ALU.
    *   **Load-Use Hazard:** You cannot forward backward in time. If a `LDUR` is followed immediately by an instruction that needs that data, you **MUST insert 1 Stall (Bubble)**, and *then* forward.
    *   **Code Scheduling:** Reordering assembly instructions (without changing the math) to put unrelated instructions between a Load and a Use to avoid stalls.
  *   **Double Data Hazards:** When two previous instructions write to the same register. *Rule:* Always forward from the most recent instruction (the EX hazard takes priority over the MEM hazard).
### **Part 3: Branch Prediction (Ch. 4)**
  
  *   **The Branch Penalty:** If the CPU waits until the MEM stage to decide on a branch, it has to flush 3 instructions. Moving the branch adder to the ID stage reduces the penalty to 1 stall cycle.
  *   **Static Prediction:** Always guessing "Taken" or "Not Taken". (Like HW5 Q4).
  *   **Dynamic Prediction (Like HW5 Q3):**
    *   **1-Bit Predictor:** Guesses whatever happened last time. Fails badly on alternating loops.
    *   **2-Bit Predictor:** A 4-state machine (Strongly Taken, Weakly Taken, Weakly Not Taken, Strongly Not Taken). Requires *two* wrong guesses to change its core prediction.
    *   You must be able to trace a sequence of branches (T, N, T, T...) and calculate the **Prediction Accuracy**.
### **Part 4: The Pipelined Datapath Hardware (Ch. 4)**
  
  *   **Pipeline Registers:** The "suitcases" between stages (`IF/ID`, `ID/EX`, `EX/MEM`, `MEM/WB`) that hold data and control signals.
  *   **The Forwarding Unit:** Sits in the EX stage. Compares `Rs1`/`Rs2` of the current instruction with the `Rd` of the instructions in the EX/MEM and MEM/WB registers.
  *   **The Hazard Detection Unit:** Sits in the ID stage. Checks for Load-Use hazards. If detected, it stalls the PC, stalls the `IF/ID` register, and forces all control signals to `0` (creating a NOP bubble).
  
  ---
### **Part 5: Memory Hierarchy & Cache Basics (Ch. 5)**
  
  *   **The Memory Gap & Technologies:** SRAM (Cache, fast/expensive) vs. DRAM (Main Memory, slow/cheap/needs refreshing) vs. Magnetic/SSD (Storage).
  *   **Principle of Locality:**
    *   *Temporal Locality:* If accessed now, it will likely be accessed again soon (e.g., loops).
    *   *Spatial Locality:* If accessed now, items nearby will likely be accessed soon (e.g., arrays).
  *   **Cache Math & Performance:**
    *   **AMAT (Average Memory Access Time):** $\text{AMAT} = \text{Hit Time} + (\text{Miss Rate} \times \text{Miss Penalty})$
    *   **Memory Stall Cycles:** $\text{Instructions} \times \text{Misses/Instruction} \times \text{Miss Penalty}$
    *   **Overall CPU Time:** $(\text{Execution Cycles} + \text{Stall Cycles}) \times \text{Clock Cycle Time}$
### **Part 6: Cache Organization & Address Mapping (Ch. 5)**
  
  You must know how to slice a 32-bit (or 64-bit) address into **Tag**, **Index**, **Word Offset**, and **Byte Offset**.
  *   **1. Direct Mapped:** Each memory block maps to exactly one cache slot.
    *   $\text{Index Bits} = \log_2(\text{Number of Cache Blocks})$
  *   **2. Fully Associative:** No index. A block can go anywhere. You must search all tags simultaneously.
  *   **3. N-Way Set Associative:** Cache is divided into "Sets" containing $N$ blocks each.
    *   $\text{Index Bits} = \log_2(\text{Number of Sets})$.
    *   $\text{Number of Sets} = \text{Total Blocks} / N$.
### **Part 7: Cache Tracing & Replacement (Ch. 5)**
  
  *   **Trace Tables:** Given a sequence of memory addresses (e.g., `000`, `110`, `010`), determine if each is a Hit or Miss, and show the resulting contents of the cache.
  *   **Replacement Policies:**
    *   **LRU (Least Recently Used):** When a Set is full, kick out the block that hasn't been touched in the longest time.
    *   **Random:** Used in highly associative caches because LRU is too hard to build in hardware.
### **Part 8: Handling Writes & Advanced Cache (Ch. 5)**
  
  *   **Write-Through:** Update cache and Main Memory simultaneously. (Slower, requires a Write Buffer to prevent massive stalls).
  *   **Write-Back:** Update *only* the cache. Add a **"Dirty Bit"** to the block. Only update Main Memory when the dirty block gets evicted. (Faster, complex hardware).
  *   **Write Allocation:** On a Write Miss, do you fetch the block into cache first (Write Allocate) or just bypass cache and write directly to RAM (Write Around)?
  *   **Multilevel Caches:** L1 minimizes hit time; L2 minimizes miss rate. Know how to calculate CPI when both L1 and L2 penalties are involved.
  
  ---
- ---
- ---
- ---
### **Topic: Handling Cache Writes**
  
  **Problem 1: Write-Through vs. Write-Back Trade-offs**
  You are designing a memory hierarchy for a new processor. 
  A) Describe the core difference between a **Write-Through** policy and a **Write-Back** policy.
  B) Which policy generally yields faster execution times for programs that frequently update the same variable (e.g., a loop counter)? Why?
  
  **Problem 2: The Write Buffer Necessity**
  Assume a processor uses a **Write-Through** cache. The base CPI is $1.0$, and $15\%$ of all instructions are `STUR` (Store) instructions. Writing to main memory takes $100$ clock cycles.
  A) If this processor does **NOT** have a write buffer, what is the Effective (Actual) CPI?
  B) If you add a deep write buffer, how does the CPU handle a `STUR` instruction, and ideally, what does the Effective CPI drop to?
  
  **Problem 3: The Dirty Bit**
  A **Write-Back** cache requires an extra 1-bit piece of metadata for every block, called the **Dirty Bit**. 
  A) What does it mean if the Dirty Bit is `1`? What does it mean if it is `0`?
  B) At what exact moment in the CPU's execution does a "dirty" block finally get written back to Main Memory?
  
  **Problem 4: Write Miss Allocation Policies**
  The CPU executes a `STUR` instruction, but the target memory address is currently **not** in the cache (a Write Miss). 
  A) If the cache uses a **Write Allocate** policy, what two physical steps happen next?
  B) If the cache uses a **Write Around (No-Allocate)** policy, what happens?
  
  ---
### **Topic: Multilevel Caches (L1 \& L2)**
  
  **Problem 5: Local vs. Global Miss Rate**
  During the execution of a program, the CPU generates $10,000$ memory access requests. 
  *   The L1 cache misses $500$ times. 
  *   Those $500$ requests are sent to the L2 cache. The L2 cache misses $50$ times.
  A) What is the **Local** miss rate of the L2 cache?
  B) What is the **Global** miss rate of the L2 cache?
  
  **Problem 6: Calculating Multilevel AMAT**
  You are given a processor with the following specifications:
  *   L1 Cache Hit Time = $1\text{ cycle}$
  *   L1 Cache Miss Rate = $10\%$
  *   L2 Cache Hit Time = $10\text{ cycles}$
  *   L2 Cache Local Miss Rate = $20\%$
  *   Main Memory Access Time = $100\text{ cycles}$
  
  Calculate the **Overall Average Memory Access Time (AMAT)** for this CPU in clock cycles. 
  *(Hint: First calculate the L1 Miss Penalty, which is just the AMAT of the L2 cache).*
  
  **Problem 7: Calculating Multilevel CPI (Like Slide 47)**
  A CPU executes a program with the following characteristics:
  *   Base CPI (assuming perfect cache) = $1.0$
  *   L1 Miss Rate = $4\%$ (per instruction)
  *   L2 Hit Penalty = $15\text{ cycles}$
  *   Global Miss Rate to Main Memory = $1\%$ (per instruction)
  *   Main Memory Penalty = $100\text{ cycles}$
  
  Calculate the **Effective (Actual) CPI** of this processor.
  
  ---
### **Topic: Advanced Cache Architectures**
  
  **Problem 8: Split Caches (Harvard Architecture)**
  Modern processors rarely use a single, unified L1 cache. Instead, they feature a **Split Cache**: an L1 I-Cache (Instruction Cache) and an L1 D-Cache (Data Cache).
  If the designers tried to use a single Unified L1 Cache for a 5-stage pipelined processor, what specific pipeline hazard would constantly occur, and which two pipeline stages would collide?
  
  **Problem 9: Multilevel Design Philosophy (Slide 48)**
  Because L1 and L2 caches serve different purposes in the hierarchy, hardware engineers design them with different priorities.
  A) What is the primary design focus for an **L1 Cache**, and how does this affect its physical size and associativity?
  B) What is the primary design focus for an **L2 Cache**, and how does this affect its physical size and block size?
  
  **Problem 10: Interleaved Memory Bandwidth (Slide 32)**
  A cache miss requires fetching a 4-word block from Main Memory. The memory bus takes $1\text{ cycle}$ to send the address, DRAM takes $20\text{ cycles}$ to process a read, and the bus takes $1\text{ cycle}$ to transfer a word of data back.
  A) Calculate the total miss penalty (in cycles) if the Main Memory is **1-word wide (non-interleaved)**.
  B) Calculate the total miss penalty (in cycles) if the Main Memory is **4-bank interleaved**.
  
  ***
  ***
  ***
  ***
# **Practice Answer Key \& Explanations**
### **Question 1 Solution**
  *   **A) Differences:** A **Write-Through** cache updates both the cache block and the main memory block simultaneously so they are always perfectly identical. A **Write-Back** cache updates *only* the cache block; main memory is left with "stale" (outdated) data until the cache block is eventually evicted.
  *   **B) Write-Back is faster for loops.** If a loop updates `sum` 1000 times, Write-Back performs 1000 fast cache writes (1 cycle each) and only 1 slow memory write at the very end when `sum` is evicted. Write-Through would perform 1000 slow memory writes (100 cycles each), heavily stalling the CPU.
### **Question 2 Solution**
  *   **A) Effective CPI without buffer:** 
    *   $\text{Stall cycles per instruction} = \text{Store frequency} \times \text{Memory Penalty}$
    *   $\text{Stalls} = 0.15 \times 100 = \mathbf{15\text{ cycles}}$.
    *   $\text{Effective CPI} = \text{Base CPI} + \text{Stalls} = 1.0 + 15 = \mathbf{16.0}$. *(The CPU is 16 times slower!)*
  *   **B) With a write buffer:** The CPU dumps the store data into the fast write buffer and immediately moves on to the next instruction. Assuming the buffer doesn't fill up, the store penalty drops to $0$. The Effective CPI drops back down to the ideal **$1.0$**.
### **Question 3 Solution**
  *   **A) Dirty Bit Meaning:** If `1` (Dirty), the data in the cache block has been modified and no longer matches Main Memory. If `0` (Clean), the data perfectly matches Main Memory (meaning it can be safely overwritten without losing changes).
  *   **B) The Eviction Moment:** A dirty block is written to Main Memory ONLY when a cache miss occurs and the hardware decides to replace (evict) that specific dirty block to make room for the new incoming data.
### **Question 4 Solution**
  *   **A) Write Allocate:** 1) Fetch the requested block from Main Memory into the cache. 2) Write the new data into that newly loaded cache block.
  *   **B) Write Around (No-Allocate):** Bypass the cache completely and just write the new data straight into Main Memory. The cache contents remain unchanged.
### **Question 5 Solution**
  *   **A) Local Miss Rate:** The miss rate *from the perspective of the L2 cache itself*.
    *   $\text{Local L2 Miss Rate} = (\text{L2 Misses}) / (\text{Total L2 Accesses}) = 50 / 500 = \mathbf{10\%}$.
  *   **B) Global Miss Rate:** The miss rate *from the perspective of the CPU/Program*.
    *   $\text{Global L2 Miss Rate} = (\text{L2 Misses}) / (\text{Total CPU Memory Accesses}) = 50 / 10,000 = \mathbf{0.5\%}$.
### **Question 6 Solution**
  *   **Step 1: Calculate L2 AMAT (which acts as the L1 Miss Penalty):**
    *   $\text{L2 AMAT} = \text{L2 Hit Time} + (\text{L2 Local Miss Rate} \times \text{Main Mem Time})$
    *   $\text{L2 AMAT} = 10 + (0.20 \times 100) = 10 + 20 = \mathbf{30\text{ cycles}}$.
  *   **Step 2: Calculate Overall AMAT:**
    *   $\text{Overall AMAT} = \text{L1 Hit Time} + (\text{L1 Miss Rate} \times \text{L1 Miss Penalty})$
    *   $\text{Overall AMAT} = 1 + (0.10 \times 30) = 1 + 3 = \mathbf{4\text{ cycles}}$.
### **Question 7 Solution**
  *   **Formula:** $\text{Effective CPI} = \text{Base} + (\text{L1 misses that hit L2}) + (\text{Global misses that hit RAM})$
  *   **Penalty 1 (L2 Hits):** $0.04 \times 15 = \mathbf{0.6\text{ cycles}}$.
  *   **Penalty 2 (RAM Hits):** $0.01 \times 100 = \mathbf{1.0\text{ cycle}}$.
  *   **Effective CPI:** $1.0 + 0.6 + 1.0 = \mathbf{2.6\text{ CPI}}$.
### **Question 8 Solution**
  *   **The Hazard:** A **Structural Hazard** would occur.
  *   **The Colliding Stages:** The **IF** (Instruction Fetch) stage and the **MEM** (Data Memory Access) stage. 
  *   *Explanation:* If instruction 1 is a `LDUR` in the MEM stage, it needs to read data from the cache. At the exact same time, Instruction 4 is in the IF stage trying to fetch its instruction code from the cache. A single memory module cannot handle two requests in one clock cycle, forcing the pipeline to stall.
### **Question 9 Solution**
  id:: 6a0142e1-2bad-45f2-a83f-c1feb6a4de18
  *   **A) L1 Cache Focus:** The primary goal is **minimal hit time** to ensure it can keep up with the blindingly fast CPU clock speed. Therefore, L1 caches are kept physically **small** and use simpler configurations (like Direct-Mapped or 2-Way Associative) so the hardware doesn't slow down the pipeline.
  *   **B) L2 Cache Focus:** The primary goal is **minimal miss rate** to prevent the CPU from having to access the painfully slow Main Memory. Therefore, L2 caches are **large**, highly associative (e.g., 8-Way), and use larger block sizes. The hit time takes longer (e.g., 10-15 cycles), but it is a worthy trade-off to avoid a 100+ cycle RAM penalty.
### **Question 10 Solution**
  *   **A) Non-interleaved (1-word wide):** The CPU must wait for the DRAM to find the data 4 separate times.
    *   $1 \text{ (Addr)} + [4 \times 20 \text{ (DRAM)}] +[4 \times 1 \text{ (Transfer)}] = 1 + 80 + 4 = \mathbf{85\text{ cycles}}$.
  *   **B) 4-Bank Interleaved:** The CPU sends the address once. All 4 banks look up their respective words simultaneously (paying the 20-cycle delay only once). Then, the 4 words are transferred consecutively.
    *   $1 \text{ (Addr)} + 20 \text{ (DRAM)} +[4 \times 1 \text{ (Transfer)}] = 1 + 20 + 4 = \mathbf{25\text{ cycles}}$.