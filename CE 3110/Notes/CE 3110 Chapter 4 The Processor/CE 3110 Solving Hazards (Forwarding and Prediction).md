#### **1. The Forwarding Unit**
  **Goal:** Solve Data Hazards without stopping the processor.
  *   **The Scenario:**
  1.  `ADD X1, ...` (Writes to X1).
  2.  `SUB ..., X1, ...` (Reads X1).
  *   **The Hardware Fix:** We add **Multiplexers** to the inputs of the ALU.
  *   *Normal Input:* Comes from `ID/EX` register (the value read from the Register File).
  *   *Forwarded Input A:* Comes from `EX/MEM` (result of the immediately preceding instruction).
  *   *Forwarded Input B:* Comes from `MEM/WB` (result of the instruction 2 cycles ago).
  
  **Detecting the Hazard (The Logic):**
  The Forwarding Unit compares the **Source Registers** of the current instruction (`Rn`, `Rm`) with the **Destination Registers** of the previous instructions (`Rd`).
  
  *   **EX Hazard (1 cycle gap):**
  `if (EX/MEM.RegWrite and EX/MEM.Rd == ID/EX.Rn)`
  *   *Meaning:* The instruction just ahead of me is writing to a register that I need to read.
  *   *Action:* Flip the Mux to take data from `EX/MEM`.
  *   **MEM Hazard (2 cycle gap):**
  `if (MEM/WB.RegWrite and MEM/WB.Rd == ID/EX.Rn)`
  *   *Meaning:* The instruction 2 steps ahead is writing to a register I need.
  *   *Action:* Flip the Mux to take data from `MEM/WB`.
  
  **The Double Hazard:**
  What if *both* previous instructions write to `X1`?
  *   We want the **most recent** version.
  *   *Rule:* Check for EX Hazard first. If found, forward from EX. Only check for MEM Hazard if there is no EX Hazard.
#### **2. The Hazard Detection Unit (Stalling)**
  **Goal:** Solve "Load-Use" Hazards where forwarding is impossible.
  *   **The Scenario:**
    1.  `LDUR X1, ...` (Result ready in MEM stage).
    2.  `SUB ..., X1, ...` (Needs result in EX stage).
  *   **The Logic:**
    The Hazard Detection Unit sits in the **ID Stage**. It checks:
    `if (ID/EX.MemRead and (ID/EX.Rd == IF/ID.Rn or ID/EX.Rd == IF/ID.Rm))`
    *   *Translation:* Is the instruction currently in EX a "Load"? AND is the destination of that Load the same as one of the sources of the instruction currently in ID?
  
  **How to Stall:**
  If the condition is true, the unit inserts a **Bubble**:
  1.  **Force Control Signals to 0:** The instruction currently in ID is effectively turned into a `NOP` (No Operation). It moves down the pipe doing nothing.
  2.  **Disable PC Write:** Don't fetch a new instruction.
  3.  **Disable IF/ID Write:** Keep the current instruction in the Decode stage so we can try again next cycle.
#### **3. Solving Control Hazards**
  **The Problem:** Standard branches are decided in the **EX** stage (Slide 45). This causes a 3-cycle penalty if we guess wrong.
  **The Fix:** **Move the decision earlier.**
  *   Add a comparator and an address adder to the **ID Stage**.
  *   Now, we know if we are branching by the end of the Decode stage.
  *   **Benefit:** Reduces the penalty ("flush") from 3 instructions to just **1 instruction**.
#### **4. Branch Prediction**
  Even with the hardware fix, losing 1 cycle every loop is annoying. We want 0 penalty.
  
  *   **1-Bit Predictor:**
    *   A table of bits. `0` = Predict Not Taken, `1` = Predict Taken.
    *   *Flaw:* In a loop (runs 10 times), it mispredicts twice. Once at the end (loop exits, bit says "Take"), and once at the start (bit says "Don't Take" because it exited last time).
  *   **2-Bit Predictor:**
    *   A state machine with 4 states:
        *   Strongly Taken
        *   Weakly Taken
        *   Weakly Not Taken
        *   Strongly Not Taken
    *   *Benefit:* It requires **two** wrong guesses to change its mind. It tolerates the single anomaly at the end of a loop without flipping the prediction for the next run.
#### **5. Advanced Concepts**
  *   **Instruction Level Parallelism (ILP):** Pipelining overlaps execution. To get even faster, we want to issue *multiple* instructions per clock cycle.
  *   **Multiple Issue (Superscalar):**
    *   The CPU has multiple pipelines (e.g., 2 ALUs, 1 Load Unit).
    *   It fetches 2-4 instructions at once and launches them all simultaneously.
    *   *Constraint:* Dependencies become very hard to manage (complex hardware).
  
  ---
### **Student "Check Your Understanding"**
  
  1.  **Forwarding Logic:**
    Instruction sequence:
    1. `ADD X1, X2, X3`
    2. `SUB X4, X1, X5`
    When `SUB` is in the **EX** stage, where is `ADD`? Which forwarding mux input does `SUB` use?
  2.  **Stalling:** Why does the Hazard Detection Unit have to check `ID/EX.MemRead`? Why doesn't it just check if the previous instruction writes to a register?
  3.  **Branch Prediction:** Why is a 2-bit predictor better than a 1-bit predictor for a `for` loop that runs 1000 times?