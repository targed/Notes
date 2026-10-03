#### **1. The Two Parts of a CPU**
  The processor is split into two distinct sections. You must understand the difference:
  1.  **Datapath (The Brawn):** The collection of functional units that actually process data. (ALU, Registers, Memory, Wires, Muxes).
  2.  **Control (The Brain):** The logic that tells the datapath *what* to do. It switches the Muxes, turns on/off Write signals, and selects ALU operations.
#### **2. Logic Design Basics**
  To build a CPU, we use two types of digital logic elements:
  *   **Combinational Elements (ALU, Adder, Mux):**
    *   Outputs depend *only* on current inputs.
    *   They have no "memory." $3+2$ is always $5$.
    *   **Multiplexer (Mux):** The most important component for Datapath design. It is a digital switch. If `Control == 0`, allow Input A to pass. If `Control == 1`, allow Input B to pass.
  *   **Sequential Elements (Registers, Memory):**
    *   They have "state" (memory).
    *   **Edge-Triggered Clocking:** Updates happen only when the clock ticks (goes from 0 to 1). This allows us to read a value, change it in the ALU, and write it back to the same register in one cycle without a race condition.
#### **3. Building the Datapath Step-by-Step**
  The slides build the machine incrementally.
  
  **A. Instruction Fetch:**
  *   **Goal:** Get the instruction from memory.
  *   **Mechanism:**
    1.  Use **PC** (Program Counter) to read Instruction Memory.
    2.  Use an **Adder** to calculate `PC + 4` (to point to the next instruction).
  
  **B. R-Format Instructions (ADD, SUB, AND) (Slide 15):**
  *   **Goal:** Read two registers, do math, write back.
  *   **Mechanism:**
    1.  Read `Rn` and `Rm` from the Register File.
    2.  Feed them into the **ALU**.
    3.  Write the result back to `Rd` in the Register File.
    4.  *Control Signal:* `RegWrite` must be 1.
  
  **C. Load/Store Instructions (LDUR, STUR) (Slide 16):**
  *   **Goal:** Move data between Registers and Memory.
  *   **Mechanism:**
    1.  Read base address (`Rn`).
    2.  Sign-extend the 9-bit offset to 64 bits.
    3.  **ALU** adds Base + Offset to calculate the address.
    4.  **Load:** Read from Data Memory, write to Register.
    5.  **Store:** Read from Register, write to Data Memory.
  
  **D. Branch Instructions (CBZ - Compare Branch Zero) (Slide 17-18):**
  *   **Goal:** If a register is 0, jump to a new address.
  *   **Mechanism:**
    1.  Read register `Rt`. Pass it to ALU.
    2.  Check the **Zero Flag** output of the ALU.
    3.  **Target Address Calculation:** Take the offset from the instruction, shift it left by 2 (because instructions are 4 bytes aligned), and add it to the current PC.
    4.  **Mux Decision:** If Zero Flag is 1, update PC to Target. If Zero Flag is 0, update PC to `PC + 4`.
#### **4. The Full Datapath & Control**
  We cannot have separate ALUs and Memories for every instruction type. We combine them into one **Single-Cycle Datapath**.
  *   **The Challenge:** Different instructions need different paths.
  *   **The Solution:** **Multiplexers.**
    *   *ALUSrc Mux:* Does the ALU take the second input from a Register (R-Format) or from the Immediate field (Load/Store/Addi)?
    *   *MemtoReg Mux:* Does the data going back to the register come from the ALU (Math) or Memory (Load)?
  
  **The Control Unit:**
  The Control Unit looks at the **Opcode** (the bits that identify the instruction) and sets the wires (flags) to 1 or 0.
  *   **ALUOp:** A 2-bit code sent to the "ALU Control" to tell it what to do (Add, Sub, And, Or).
  *   **Main Control Signals:**
    *   `Reg2Loc`: Which register bit field is read?
    *   `RegWrite`: Are we writing to a register?
    *   `MemRead`/`MemWrite`: Are we using the Data Memory?
    *   `Branch`: Is this a conditional jump?
#### **5. The Unconditional Branch (B)**
  *   The `B` instruction uses a specific format.
  *   It has a large 26-bit offset.
  *   We shift it left by 2 bits $\rightarrow$ 28 bits.
  *   We Sign-extend it to 64 bits.
  *   Add to PC.
  
  ---
### **Student "Check Your Understanding"**
  
  1.  **The Mux:** In the full datapath, there is a Multiplexer before the Register File's "Write Data" input. What two inputs does this Mux switch between? Which instruction uses which input?
  2.  **Clocking:** Why do we need the "PC + 4" adder? Why can't we just read instructions sequentially?
  3.  **Control Logic:** For a **STUR** (Store Register) instruction:
    *   What should `RegWrite` be? (0 or 1?)
    *   What should `MemWrite` be? (0 or 1?)
    *   What should `ALUSrc` be? (Register or Immediate?)