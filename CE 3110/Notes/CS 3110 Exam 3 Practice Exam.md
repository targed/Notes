# **CpE 3110 - Final Exam (Practice)**
  **Allowed Materials:** Pencil, Eraser, 2-page Crib Sheet, LEGv8 Green Card. No calculators.
### **Question 1: Pipelining Performance (15 pts)**
  You are comparing two implementations of the LEGv8 architecture. 
  *   **Processor A** is a single-cycle processor with a clock cycle time of $1000\text{ ps}$.
  *   **Processor B** is a 5-stage pipelined processor with a clock cycle time of $200\text{ ps}$.
  You need to execute a program containing **1000 instructions**. Assume there are absolutely no pipeline hazards.
  1. Calculate the total execution time (in ps) for Processor A.
  2. Calculate the total execution time (in ps) for Processor B.
  3. Calculate the overall speedup of Processor B over Processor A.
### **Question 2: Pipeline Hazards \& Forwarding (15 pts)**
  Consider the following LEGv8 instruction sequence running on a 5-stage pipeline equipped with full forwarding and a Hazard Detection Unit:
  1. `LDUR X1, [X2, #0]`
  2. `ADD  X3, X1, X4`
  3. `SUB  X5, X3, X1`
  
  1. Clearly identify all data hazards in this sequence (specify the two instructions and the register involved).
  2. For each hazard, specify whether a **stall** is required (Yes/No).
  3. For each hazard, specify which forwarding path is used to resolve it (**EXE-forwarding** or **MEM-forwarding**).
### **Question 3: Branch Prediction (15 pts)**
  Trace the following branch outcome sequence using a **2-Bit Dynamic Predictor**. 
  Assume the initial state is **Weakly Taken (WT)**. 
  Sequence (T = Taken, N = Not Taken): `T, N, N, T, T, N`
  Construct a trace table showing the Iteration, Current State, Actual Outcome, Prediction Result (Correct/Incorrect), and Next State. Calculate the final Prediction Accuracy.
### **Question 4: Cache Subdivision Math (15 pts)**
  Main memory is byte-addressable and accessed via a **32-bit address**. You have a cache memory that can store exactly **8192 data words**. (Assume 1 word = 4 bytes).
  Calculate the exact number of bits required for the **Byte Offset, Word Offset, Index, and Tag** for the following two configurations:
  1. Direct-mapped cache with **1 word** per block.
  2. 4-way set associative cache with **4 words** per block.
### **Question 5: Cache Tracing (15 pts)**
  Assume you have a **2-Way Set Associative** cache that can store a total of **4 one-word data blocks**. Main memory is word-addressable. 
  Trace the following **word address** sequence: `0, 4, 2, 0, 5, 2`
  Construct a trace table showing the Address, Tag, Index, Hit/Miss, and the final Cache Content for each set. Use the **LRU (Least Recently Used)** replacement policy. Calculate the overall Hit Ratio.
### **Question 6: Cache Performance Math (10 pts)**
  A pipelined processor has a base CPI of **$1.5$** (assuming a perfect, 100% hit rate cache).
  *   The Instruction Cache (I-Cache) miss rate is **$2\%$**.
  *   The Data Cache (D-Cache) miss rate is **$5\%$**.
  *   Loads and Stores make up **$20\%$** of all instructions.
  *   The miss penalty for any cache miss is **$50\text{ cycles}$**.
  
  Calculate the **Actual (Effective) CPI** of this processor. Show your math.
### **Question 7: Architectural Concepts (15 pts)**
  Answer the following concept questions briefly:
  1. **Write-Through vs. Write-Back:** What is the specific purpose of the "Dirty Bit", and which of these two write policies requires it?
  2. **Hazard Detection Logic:** When the Hazard Detection Unit detects a Load-Use hazard in the ID stage, what three physical actions does it take to inject a "bubble" (stall) into the pipeline?
  3. **Locality:** Iterating sequentially through a large array in a `for` loop primarily exploits which type of locality (Temporal or Spatial)? Why?
  
  ***
  ***
  *(Stop here! Do not scroll down until you have completed the exam).*
  ***
  ***
  
  <br><br><br><br><br>
# **Practice Final Exam Answer Key \& Explanations**
### **Question 1 Solution**
  1.  **Processor A (Single-Cycle):**
    *   $\text{Execution Time} = \text{Instruction Count} \times \text{Clock Cycle Time}$
    *   $\text{Time}_A = 1000 \times 1000\text{ ps} = \mathbf{1,000,000\text{ ps}}$.
  2.  **Processor B (Pipelined):**
    *   $\text{Total Cycles} = k + N - 1 = 5 + 1000 - 1 = 1004 \text{ cycles}$.
    *   $\text{Time}_B = 1004 \times 200\text{ ps} = \mathbf{200,800\text{ ps}}$.
  3.  **Speedup:**
    *   $\text{Speedup} = \text{Time}_A / \text{Time}_B = 1,000,000 / 200,800$.
    *   *(Mental math: $1000 / 200.8 \approx \mathbf{4.98}$)*. Processor B is ~4.98 times faster.
### **Question 2 Solution**
  1.  **Hazards Identified:**
    *   **Hazard A:** Between `LDUR` (Inst 1) and `ADD` (Inst 2) on register **`X1`**.
    *   **Hazard B:** Between `ADD` (Inst 2) and `SUB` (Inst 3) on register **`X3`**.
    *   **Hazard C:** Between `LDUR` (Inst 1) and `SUB` (Inst 3) on register **`X1`**.
  2.  **Stalls Required:**
    *   Hazard A: **Yes** (1 cycle). This is a Load-Use hazard.
    *   Hazard B: **No**.
    *   Hazard C: **No**. 
  3.  **Forwarding Paths Used:**
    *   Hazard A: Because of the 1-cycle stall, the `LDUR` reaches the WB stage just as `ADD` begins the EX stage. This requires **MEM-forwarding** (from the MEM/WB register).
    *   Hazard B: The `ADD` finishes computing `X3` in EX, and `SUB` needs it immediately in EX. This requires **EXE-forwarding** (from the EX/MEM register).
    *   Hazard C: Because the stall pushed the `SUB` instruction back, the `LDUR` instruction has completely exited the pipeline by the time `SUB` reaches the EX stage. The value of `X1` has been safely written to the Register File, so **No forwarding is needed** (it reads it naturally from the register file in ID).
### **Question 3 Solution**
  *Recall the 4 states: Strongly Taken (ST), Weakly Taken (WT), Weakly Not Taken (WN), Strongly Not Taken (SN).*
  
  | Iter | Current State | Actual | Result | Next State |
  | :---: | :---: | :---: | :---: | :---: |
  | 1 | Predict **T** (WT) | **T** | ✅ Correct | ST |
  | 2 | Predict **T** (ST) | **N** | ❌ Incorrect | WT |
  | 3 | Predict **T** (WT) | **N** | ❌ Incorrect | WN |
  | 4 | Predict **N** (WN) | **T** | ❌ Incorrect | WT |
  | 5 | Predict **T** (WT) | **T** | ✅ Correct | ST |
  | 6 | Predict **T** (ST) | **N** | ❌ Incorrect | WT |
  
  *   **Total Predictions:** 6
  *   **Correct Predictions:** 2
  *   **Accuracy:** $2 / 6 = \mathbf{33.3\%}$
### **Question 4 Solution**
  *Baseline facts: Address = 32 bits. Capacity = 8192 words. 1 word = 4 bytes.*
  **1. Direct-Mapped, 1 word per block:**
  *   **Byte Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Word Offset:** $\log_2(1) = \mathbf{0 \text{ bits}}$.
  *   **Index:** $8192 \text{ words} / 1 \text{ word/block} = 8192 \text{ blocks}$. Total Sets = 8192. $\log_2(8192) = \mathbf{13 \text{ bits}}$.
  *   **Tag:** $32 - (13 + 0 + 2) = \mathbf{17 \text{ bits}}$.
  
  **2. 4-Way Set Associative, 4 words per block:**
  *   **Byte Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Word Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Index:** $8192 \text{ words} / 4 \text{ words/block} = 2048 \text{ blocks}$. $2048 \text{ blocks} / 4 \text{ ways} = 512 \text{ Sets}$. $\log_2(512) = \mathbf{9 \text{ bits}}$.
  *   **Tag:** $32 - (9 + 2 + 2) = \mathbf{19 \text{ bits}}$.
### **Question 5 Solution**
  *Configuration: 4 blocks / 2 Ways = 2 Sets. Index = Block Address mod 2.*
  *Address Sequence: `0, 4, 2, 0, 5, 2`*
  
  | Addr | Index | Hit/Miss | Cache Content (Set 0) | Cache Content (Set 1) |
  | :--- | :---: | :---: | :--- | :--- |
  | **0** | 0 | Miss | M[0], -- | --, -- |
  | **4** | 0 | Miss | M[0], M[4] *(4 is MRU)*| --, -- |
  | **2** | 0 | Miss | M[2], M[4] *(Evicts 0!)* | --, -- |
  | **0** | 0 | Miss | M[2], M[0] *(Evicts 4!)* | --, -- |
  | **5** | 1 | Miss | M[2], M[0] | M[5], -- |
  | **2** | 0 | **Hit** | M[0], M[2] *(2 becomes MRU)* | M[5], -- |
  
  *   **Hit Ratio:** $1 / 6 = \mathbf{16.67\%}$
### **Question 6 Solution**
  *   **Formula:** $\text{Effective CPI} = \text{Base CPI} + \text{I-Cache Stalls} + \text{D-Cache Stalls}$
  *   **I-Cache Stalls:** $100\%$ of instructions fetch from I-Cache. 
    *   $1.0 \times 0.02 \times 50 = \mathbf{1.0 \text{ cycle}}$.
  *   **D-Cache Stalls:** $20\%$ of instructions access D-Cache. 
    *   $0.20 \times 0.05 \times 50 = \mathbf{0.5 \text{ cycles}}$.
  *   **Effective CPI:** $1.5 + 1.0 + 0.5 = \mathbf{3.0 \text{ CPI}}$.
### **Question 7 Solution**
  1.  **Dirty Bit:** The Dirty Bit tracks whether a block inside the cache has been modified (written to) and no longer matches the data in Main Memory. It is required for the **Write-Back** policy, as this policy delays updating Main Memory until the dirty block is finally evicted.
  2.  **Hazard Detection Unit Actions:** 
    *   1) It forces all control signals in the `ID/EX` pipeline register to `0` (creating a NOP bubble). 
    *   2) It prevents the update of the `PC` register.
    *   3) It prevents the update of the `IF/ID` pipeline register (forcing the same instruction to be decoded again).
  3.  **Locality:** It exploits **Spatial Locality**. An array is a contiguous block of memory. When `A[0]` misses, the cache fetches an entire block containing `A[0], A[1], A[2]...` Therefore, future sequential accesses immediately hit in the cache because their addresses are physically close to the original miss.
- ---
- ---
- ---
# **CpE 3110 - Final Exam (Practice C)**
  **Allowed Materials:** Pencil, Eraser, 2-page Crib Sheet, LEGv8 Green Card. No calculators.
### **Question 1: Pipelining \& Speedup Math (10 pts)**
  You are given a program with exactly **5000 instructions**. 
  Processor 1 is a single-cycle processor with a clock cycle time of **$800\text{ ps}$**.
  Processor 2 is a 5-stage pipelined processor with a clock cycle time of **$200\text{ ps}$**. 
  Assuming there are absolutely no pipeline hazards or stalls:
  1. Calculate the total execution time (in ps) for Processor 1.
  2. Calculate the total execution time (in ps) for Processor 2.
  3. Calculate the overall speedup of Processor 2 over Processor 1.
### **Question 2: Hazards \& Forwarding Paths (20 pts)**
  Consider the following LEGv8 instruction sequence running on a 5-stage pipeline equipped with full forwarding and a Hazard Detection Unit:
  1. `ADD X1, X2, X3`
  2. `LDUR X4, [X1, #8]`
  3. `SUB X5, X4, X1`
  
  1. Clearly identify the **two** data hazards in this sequence (specify the two instructions and the register involved).
  2. For each hazard, specify whether a **stall** is required (Yes/No).
  3. For each hazard, specify exactly which forwarding path is used (**EXE-forwarding** or **MEM-forwarding**).
### **Question 3: Branch Prediction (15 pts)**
  Assume a pipelined processor has a built-in **1-Bit Dynamic Predictor**. 
  Trace the following branch outcome sequence. Assume the initial state of the predictor is **Predict Taken (T)**. 
  Sequence (T = Taken, N = Not Taken): `T, N, T, T, N, N, T`
  Construct a trace table showing the Step, Current Prediction, Actual Outcome, Prediction Result (Correct/Incorrect), and Next Prediction. Calculate the final Prediction Accuracy.
### **Question 4: Cache Subdivision Math (15 pts)**
  Main memory is **byte-addressable** and accessed via a **32-bit address**. 
  You are given a cache memory that can store exactly **1024 data words**. (Assume 1 word = 4 bytes).
  Calculate the exact number of bits required for the **Byte Offset, Word Offset, Index, and Tag** for the following two configurations:
  1. **Direct-mapped** cache with **4 words per block**.
  2. **2-way set associative** cache with **2 words per block**.
### **Question 5: Cache Tracing (The Byte Trap) (15 pts)**
  Assume you have a **Direct-Mapped** cache that can store exactly **4 one-word data blocks**. 
  The CPU generates the following sequence of **BYTE addresses**: 
  `0, 4, 16, 4, 8, 20, 16`
  Construct a trace table showing the corresponding Word Address, Index, Hit/Miss, and the final Cache Content for each access. Calculate the overall Hit Ratio.
### **Question 6: Multilevel CPI Math (10 pts)**
  A pipelined processor has a base CPI of **$1.2$** (assuming a perfect L1 cache).
  *   The Instruction Cache (I-Cache) miss rate is **$3\%$**.
  *   The Data Cache (D-Cache) miss rate is **$6\%$**.
  *   Loads and Stores make up **$30\%$** of all instructions.
  *   The penalty for an L1 cache miss that hits in the L2 cache is **$10\text{ cycles}$**.
  *   The global miss rate to Main Memory is **$1\%$** (per instruction).
  *   The penalty for accessing Main Memory is **$100\text{ cycles}$**.
  
  Calculate the **Effective (Actual) CPI** of this processor. Show your math.
### **Question 7: Architectural Concepts (15 pts)**
  Answer the following concept questions briefly:
  1. **Memory Technology:** Why do computer architects use SRAM for Cache Memory and DRAM for Main Memory, instead of just using one technology for everything?
  2. **Structural Hazards:** If a pipelined CPU uses a single, unified memory module instead of a split I-Cache and D-Cache, which two pipeline stages will structurally collide?
  3. **Branch Target Math:** In the ID stage of the datapath, what physical hardware unit converts a branch instruction's 19-bit or 26-bit offset into a **byte offset**, and mathematically, what is it multiplying the value by?
  
  ***
  ***
  ***
  ***
# **Practice Exam C Answer Key \& Explanations**
### **Question 1 Solution**
  1.  **Processor 1 (Single-Cycle):**
    *   $\text{Execution Time} = \text{Instruction Count} \times \text{Clock Cycle Time}$
    *   $\text{Time}_{P1} = 5000 \times 800\text{ ps} = \mathbf{4,000,000\text{ ps}}$.
  2.  **Processor 2 (Pipelined):**
    *   $\text{Total Cycles} = k + N - 1 = 5 + 5000 - 1 = 5004 \text{ cycles}$.
    *   $\text{Time}_{P2} = 5004 \times 200\text{ ps} = \mathbf{1,000,800\text{ ps}}$.
  3.  **Speedup:**
    *   $\text{Speedup} = \text{Time}_{P1} / \text{Time}_{P2} = 4,000,000 / 1,000,800$.
    *   *(Mental math: $4000 / 1000.8 \approx \mathbf{3.996}$)*. Processor 2 is nearly 4 times faster.
### **Question 2 Solution**
  1.  **Hazards Identified:**
    *   **Hazard A:** Between `ADD` (Inst 1) and `LDUR` (Inst 2) on register **`X1`**.
    *   **Hazard B:** Between `LDUR` (Inst 2) and `SUB` (Inst 3) on register **`X4`**.
    *   *(Note: There is also technically a dependency on `X1` between `ADD` and `SUB`, but the stall caused by Hazard B will push `SUB` far enough back that it reads `X1` directly from the register file safely).*
  2.  **Stalls Required:**
    *   Hazard A (`ADD` to `LDUR`): **No Stall.**
    *   Hazard B (`LDUR` to `SUB`): **Yes (1 cycle).** This is a Load-Use hazard.
  3.  **Forwarding Paths Used:**
    *   Hazard A: The `LDUR` needs `X1` in the EX stage to calculate the address. The `ADD` computes it in the EX stage. This is a standard 1-cycle gap resolved by **EXE-forwarding**.
    *   Hazard B: Because of the 1-cycle stall, `LDUR` reaches the WB stage just as `SUB` begins the EX stage. This requires **MEM-forwarding**.
### **Question 3 Solution**
  *Rule: 1-Bit Predictor always guesses whatever the last actual outcome was.*
  
  | Step | Current Prediction | Actual | Result | Next Prediction |
  | :---: | :---: | :---: | :---: | :---: |
  | 1 | Predict **T** | **T** | ✅ Correct | Predict T |
  | 2 | Predict **T** | **N** | ❌ Incorrect | Predict N |
  | 3 | Predict **N** | **T** | ❌ Incorrect | Predict T |
  | 4 | Predict **T** | **T** | ✅ Correct | Predict T |
  | 5 | Predict **T** | **N** | ❌ Incorrect | Predict N |
  | 6 | Predict **N** | **N** | ✅ Correct | Predict N |
  | 7 | Predict **N** | **T** | ❌ Incorrect | Predict T |
  
  *   **Total Predictions:** 7
  *   **Correct Predictions:** 3
  *   **Accuracy:** $3 / 7 = \mathbf{42.8\%}$
### **Question 4 Solution**
  *Baseline facts: Total Capacity = 1024 words. 1 word = 4 bytes. Address = 32 bits.*
  **1. Direct-Mapped, 4 words per block:**
  *   **Byte Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Word Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Index:** $1024 \text{ words} / 4 \text{ words/block} = 256 \text{ blocks}$. Total Sets = 256. $\log_2(256) = \mathbf{8 \text{ bits}}$.
  *   **Tag:** $32 - (8 + 2 + 2) = \mathbf{20 \text{ bits}}$.
  
  **2. 2-Way Set Associative, 2 words per block:**
  *   **Byte Offset:** $\log_2(4) = \mathbf{2 \text{ bits}}$.
  *   **Word Offset:** $\log_2(2) = \mathbf{1 \text{ bit}}$.
  *   **Index:** $1024 \text{ words} / 2 \text{ words/block} = 512 \text{ blocks}$. 
    $512 \text{ blocks} / 2 \text{ ways} = 256 \text{ Sets}$. $\log_2(256) = \mathbf{8 \text{ bits}}$.
  *   **Tag:** $32 - (8 + 1 + 2) = \mathbf{21 \text{ bits}}$.
### **Question 5 Solution (The Byte Trap)**
  *Because these are BYTE addresses and blocks hold 1 word (4 bytes), we must first divide the addresses by 4 to get the Word Address!*
  *   **Word Addresses:** $0/4=\mathbf{0}$, $4/4=\mathbf{1}$, $16/4=\mathbf{4}$, $4/4=\mathbf{1}$, $8/4=\mathbf{2}$, $20/4=\mathbf{5}$, $16/4=\mathbf{4}$.
  *   **Index Calculation:** Word Address $\pmod 4$.
  
  | Byte Addr | Word Addr | Index | Hit/Miss | Cache Content (Index 00, 01, 10, 11) |
  | :---: | :---: | :---: | :---: | :--- |
  | **0** | **0** | 00 | Miss | Mem[0], --, --, -- |
  | **4** | **1** | 01 | Miss | Mem[0], Mem[1], --, -- |
  | **16** | **4** | 00 | Miss | Mem[4]*, Mem[1], --, -- *(Evicts 0)* |
  | **4** | **1** | 01 | **Hit** | Mem[4], Mem[1], --, -- |
  | **8** | **2** | 10 | Miss | Mem[4], Mem[1], Mem[2], -- |
  | **20** | **5** | 01 | Miss | Mem[4], Mem[5]*, Mem[2], -- *(Evicts 1)*|
  | **16** | **4** | 00 | **Hit** | Mem[4], Mem[5], Mem[2], -- |
  
  *   **Hit Ratio:** $2 / 7 = \mathbf{28.5\%}$
### **Question 6 Solution**
  *   **Formula:** $\text{CPI} = \text{Base} + (\text{L1 I-Stalls}) + (\text{L1 D-Stalls}) + (\text{Main Mem Stalls})$
  *   **L1 I-Cache Penalty:** $100\%$ of instructions hit I-Cache. $1.0 \times 0.03 \times 10 = \mathbf{0.3 \text{ cycles}}$.
  *   **L1 D-Cache Penalty:** $30\%$ of instructions hit D-Cache. $0.30 \times 0.06 \times 10 = \mathbf{0.18 \text{ cycles}}$.
  *   **Main Memory Penalty:** Global miss rate applies to all instructions. $0.01 \times 100 = \mathbf{1.0 \text{ cycles}}$.
  *   **Effective CPI:** $1.2 + 0.3 + 0.18 + 1.0 = \mathbf{2.68 \text{ CPI}}$.
### **Question 7 Solution**
  1.  **Memory Tech:** SRAM is extremely fast but takes up a lot of physical space (6 transistors) and is very expensive. DRAM is very dense and cheap (1 capacitor), but slow. The hierarchy gives us the illusion of having a memory that is as fast as SRAM but as large and cheap as DRAM.
  2.  **Structural Hazards:** The **IF (Instruction Fetch)** stage and the **MEM (Data Memory)** stage would collide, as they would both try to read/write from the single memory module simultaneously.
  3.  **Branch Target Math:** The **Shift Left 2** unit. It shifts the binary number to the left by 2 bits, which mathematically **multiplies it by 4**, converting the instruction offset into a byte offset.