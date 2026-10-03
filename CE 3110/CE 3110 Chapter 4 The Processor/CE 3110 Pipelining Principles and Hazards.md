#### **1. The Pipelining Concept**
  *   **The Analogy (Laundry):**
  *   *Sequential:* Wash(30m) $\to$ Dry(40m) $\to$ Fold(20m). Total = 90m per load. You wait for Load 1 to be folded before starting Load 2.
  *   *Pipelined:* As soon as Load 1 moves to the Dryer, put Load 2 in the Washer.
  *   *Result:* You finish one load every 40 minutes (determined by the slowest stage), instead of every 90 minutes.
  *   **The 5 Stages of LEGv8:**
  We break instruction execution into 5 discrete steps. Each step takes 1 clock cycle.
  1.  **IF:** Instruction Fetch (Get code from memory).
  2.  **ID:** Instruction Decode & Register Read.
  3.  **EX:** Execute (ALU operation) or Address Calculation.
  4.  **MEM:** Data Memory Access (Load/Store).
  5.  **WB:** Write Back (Write result into Register File).
#### **2. Performance Math**
  *   **Latency vs. Throughput:**
    *   **Latency:** Time to finish *one* instruction. Pipelining **does not** improve this (it actually gets slightly worse due to register overhead).
    *   **Throughput:** Number of instructions finished per second. Pipelining **improves** this efficiently.
  *   **Ideal Speedup:**
    $$ \text{Speedup} = \frac{\text{Time per instruction (non-pipelined)}}{\text{Time per instruction (pipelined)}} $$
    *   In a perfect world, if you have 5 stages, you get **5x speedup**.
    *   *Reality:* Stages aren't perfectly balanced, and "Hazards" cause delays.
#### **3. Designing for Pipelining**
  The LEGv8 ISA was specifically designed to make pipelining easy (RISC philosophy).
  *   **Fixed Length Instructions (32-bit):** We can Fetch (IF) and Decode (ID) easily in one cycle. We know exactly where the instruction ends. (Compare to x86, where instructions vary from 1 to 17 bytes).
  *   **Regular Formats:** The register fields (`Rn`, `Rm`, `Rd`) are always in the same place. We can read the Register File *while* we are decoding the instruction.
  *   **Memory Alignment:** Simplifies the MEM stage to exactly one cycle.
#### **4. Hazards: The Enemies of Speed**
  A **Hazard** is a situation where the next instruction cannot start in the next clock cycle. This forces us to **Stall** (pause) the pipeline, inserting a "Bubble" (no operation).
  
  **Type A: Structure Hazards**
  *   *Problem:* Two instructions fight for the same piece of hardware at the same time.
  *   *Example:* If we had only *one* memory unit, the instruction in **IF** (fetching code) and the instruction in **MEM** (loading data) would collide.
  *   *Solution:* Separate Instruction Memory and Data Memory (or separate L1 Caches).
  
  **Type B: Data Hazards**
  *   *Problem:* An instruction needs data that has not yet been written back.
    *   Instr 1: `ADD X1, X2, X3` (Computes X1 in **EX** stage, writes it in **WB** stage).
    *   Instr 2: `SUB X4, X1, X5` (Needs X1 in **ID** stage).
    *   *The Gap:* Instr 2 tries to read X1 *before* Instr 1 has written it. It reads the old, wrong value.
  *   *Solution 1: Stalling (Bubbles):* Pause Instr 2 for 2 cycles. (Safe, but kills performance).
  *   *Solution 2: Forwarding (Bypassing):*
    *   The value of `X1` is actually ready at the end of the **EX** stage of Instr 1.
    *   We add extra wires to grab that value and feed it *directly* into the ALU for Instr 2. No stall needed!
  *   *The "Load-Use" Hazard:*
    *   If Instr 1 is a `LDUR` (Load), the data comes from Memory (**MEM** stage).
    *   If Instr 2 needs that data immediately, we cannot forward from MEM backward to EX in time.
    *   *Result:* **1 Cycle Stall is unavoidable**, followed by forwarding.
    *   *Optimization:* **Code Scheduling**. The compiler reorders code to put an unrelated instruction between the Load and the Use to fill the slot.
  
  **Type C: Control Hazards (Branch Hazards)**
  *   *Problem:* `CBZ X1, Label`.
    *   The decision to branch happens in the **EX** stage (or **ID** stage if optimized).
    *   By the time we know *if* we should branch, the pipeline has already fetched the next 1 or 2 instructions.
    *   If the branch is taken, those fetched instructions are wrong and must be "flushed" (thrown away).
  *   *Solution 1: Stall:* Wait until we know the answer. (Too slow).
  *   *Solution 2: Prediction:* Guess.
    *   **Static Prediction:** Always guess "Not Taken" (just keep fetching sequentially). If wrong, flush.
    *   **Dynamic Prediction:** Use a hardware history table to remember what happened last time (e.g., Loop branches are usually taken).
  
  ---
### **Student "Check Your Understanding"**
  
  1.  **Pipeline Theory:** If a processor has 5 stages, and each stage takes 200ps, how long does it take to finish **one** instruction? How long is the time **between** instructions finishing (throughput)?
  2.  **Data Hazards:** Look at this code. Where are the dependencies? Can forwarding solve them, or do we need a stall?
    ```assembly
    ADD X1, X2, X3
    SUB X4, X1, X5
    AND X6, X1, X7
    ```