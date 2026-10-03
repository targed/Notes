#### **1. The Pipeline Registers**
  In a Single-Cycle processor, electricity flows from the PC all the way to the Register Write-Back in one clock tick. In a Pipelined processor, we must separate the stages so they don't interfere with each other.
  
  *   **The Solution:** Insert large registers (buffers) between the stages.
  *   **The "Suitcase" Analogy:** Think of the instruction as a traveler. Between every stage (Check-in, Security, Gate), there is a holding area. The traveler packs everything they computed in the previous stage into a "Suitcase" (Pipeline Register) and hands it to the next stage.
  
  **The Four Pipeline Registers:**
  1.  **IF/ID:** Holds the Instruction and the PC.
  2.  **ID/EX:** Holds values read from Registers (`Rn`, `Rm`), Immediate values, and Control Signals.
  3.  **EX/MEM:** Holds the ALU Result (Address or Math) and the value to be stored (for `STUR`).
  4.  **MEM/WB:** Holds the data read from Memory or the ALU Result.
  
  *Note: These registers are updated on every clock edge.*
#### **2. Instruction Flow: Load (LDUR)**
  Let's trace `LDUR X1, [X2, #100]` through the pipe.
  
  1.  **IF Stage:** Fetch instruction. Save it in **IF/ID**.
  2.  **ID Stage:**
    *   Read Register `X2` (Base).
    *   Sign-extend the immediate `#100`.
    *   Save these values in **ID/EX**.
  3.  **EX Stage:**
    *   ALU adds Base (`X2`) + Offset (`100`).
    *   Pass this sum (Address) to **EX/MEM**.
  4.  **MEM Stage:**
    *   Read Data Memory at that Address.
    *   Pass the data to **MEM/WB**.
  5.  **WB Stage:**
    *   Take the data from **MEM/WB** and write it into Register `X1`.
#### **3. The "Wrong Register" Bug**
  **This is a critical hardware detail.**
  *   **The Problem:**.
    *   In the **ID** stage, we decode the instruction and see the destination is `X1`.
    *   However, the write to `X1` happens in the **WB** stage (3 cycles later).
    *   By the time this instruction reaches **WB**, the **ID** stage is processing a *new* instruction (say, one that writes to `X9`).
    *   If the **WB** stage looks back at the "Instruction Decode" wires to see which register to write to, it will see `X9` (the current ID instruction), not `X1` (the instruction actually finishing).
  *   **The Fix (Slide 58):** We must pass the Destination Register Number (`Rd` or `Rt`) **down the pipeline**.
    *   It travels from **ID/EX** $\to$ **EX/MEM** $\to$ **MEM/WB**.
    *   The "Write Register" input on the Register File comes from the **MEM/WB** pipeline register, ensuring we write to the correct place.
#### **4. Pipelined Control**
  How do we handle the "Brain" (Control Unit)?
  In Single-Cycle, the Control Unit set all wires at once. We can't do that here because different stages are executing different instructions simultaneously.
  
  **The Solution: Carrying Control Signals**
  The Control Unit is located in the **ID** stage. It generates *all* control signals for an instruction at once. We group them by stage and pass them down the pipeline registers.
  
  *   **ID/EX Register holds:** (Execution Signals)
    *   `ALUSrc`, `ALUOp`, `RegDst`.
  *   **EX/MEM Register holds:** (Memory Signals)
    *   `MemRead`, `MemWrite`, `Branch`.
  *   **MEM/WB Register holds:** (Write-Back Signals)
    *   `RegWrite`, `MemtoReg`.
  
  **The Logic:**
  As an instruction moves from ID to EX, it brings its "Execution Signals" with it. When it moves to MEM, it leaves the EX signals behind (they are used/discarded) and carries the MEM and WB signals forward.
#### **5. Pipeline Diagrams**
  There are two ways to draw pipelines for analysis:
  1.  **Multi-cycle Diagram:** Instructions on Y-axis, Time on X-axis. Used to see the "staircase" effect and visualize Stalls.
  2.  **Single-cycle Diagram:** A snapshot of the whole datapath at one specific clock cycle. Useful for debugging "What is happening in the ALU right now?"
  
  ---
### **Student "Check Your Understanding"**
  
  1.  **Pipeline Registers:** Why is the immediate value (offset) stored in the **ID/EX** register? Why can't the ALU just read it directly from the **IF/ID** register?
  2.  **Control:** Look at the `MemWrite` signal (used for `STUR`).
    *   It is generated in the **ID** stage.
    *   It is used in the **MEM** stage.
    *   How many pipeline registers does it pass through before it is used?
  3.  **Hardware Design:** Why don't we need a "Write Back" signal in the **ID/EX** register? (Hint: Does the EX stage do any writing to the register file?)