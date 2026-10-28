### **Topic 1: Integer Arithmetic (Chapter 3)**
  *This covers how the ALU physically calculates values.*
  
  *   **Signed vs. Unsigned Addition & Subtraction:**
  *   Know how to do binary addition and subtraction by hand.
  *   **Overflow Rules:** You must be able to state whether an overflow occurred and *justify* it (Like HW3 Q1 & Q2).
      *   *Unsigned Overflow:* Occurs if there is a Carry-Out from the Most Significant Bit (MSB).
      *   *Signed Overflow:* Occurs if adding two positive numbers yields a negative, or adding two negative numbers yields a positive. (Also detected if Carry-In to MSB $\neq$ Carry-Out of MSB).
  *   **Multiplication Hardware Algorithms:**
  *   You need to know how to construct a step-by-step trace table for multiplication (Like HW3 Q3 & Q4).
  *   **Unoptimized Multiplier:** 64-bit ALU, 64-bit Product register, 32-bit Multiplicand. (Multiplicand shifts left, Multiplier shifts right).
  *   **Optimized Multiplier:** 32-bit ALU, 64-bit Product register. (Multiplicand stays still, Product shifts right. Multiplier is initially placed in the lower half of the Product register).
  *   **Division Concepts:**
  *   Know the restoring division algorithm: subtract the divisor, if the result is negative, put a `0` in the quotient and "restore" (add back) the divisor.
### **Topic 2: Floating-Point Math - IEEE 754 (Chapter 3)**
  *This is the heaviest math section. Expect conversion and addition problems.*
  
  *   **The IEEE 754 Format (Single & Double):**
    *   Formula: $(-1)^S \times (1 + \text{Fraction}) \times 2^{(\text{Exponent} - \text{Bias})}$
    *   **Single Precision:** 1 Sign bit, 8 Exponent bits, 23 Fraction bits. Bias = **127**.
    *   **Double Precision:** 1 Sign bit, 11 Exponent bits, 52 Fraction bits. Bias = **1023**.
  *   **Conversions (Like HW3 Q5):**
    *   Decimal to Binary Floating Point. (e.g., converting -15.25 to 32-bit hex/binary).
    *   Binary to Decimal (e.g., extracting the exponent, subtracting the bias, and shifting the decimal point).
  *   **Floating-Point Addition Algorithm (Like HW3 Q6):**
    *   Know the 4 exact steps:
        1. Align binary points (shift the significand of the *smaller* exponent to the right).
        2. Add the significands.
        3. Normalize the result (shift left/right to get $1.\text{XXXX}$) and update the exponent.
        4. Round if necessary.
  *   **Special Cases:**
    *   Know how the hardware identifies special numbers:
        *   **Zero:** Exponent = `00...0`, Fraction = `00...0`.
        *   **Denormalized:** Exponent = `00...0`, Fraction $\neq 0$. (No hidden '1').
        *   **Infinity:** Exponent = `11...1`, Fraction = `00...0`.
        *   **NaN (Not a Number):** Exponent = `11...1`, Fraction $\neq 0$.
### **Topic 3: The Single-Cycle Datapath (Chapter 4, Slides 1-21)**
  *You need to know the physical blueprint of the processor.*
  
  *   **Logic Components (Like HW4 Q1 & Q2):**
    *   **Combinational Logic** (no memory, relies on current inputs): ALU, Adders, Multiplexors, Sign-Extend, Shift-Left-2.
    *   **Sequential Logic** (stores state, updates on clock edge): Program Counter (PC), Register File, Instruction Memory, Data Memory.
  *   **Tracing Instructions:**
    *   If given a diagram of the datapath, you must be able to trace the physical path that data takes for specific instructions (`ADD`, `LDUR`, `STUR`, `CBZ`, `B`).
    *   *Example:* For `LDUR`, know that data comes out of the Instruction Memory, goes to the Register file to read the base address, the offset goes through the Sign-Extend, they both go into the ALU to calculate an address, that address goes into Data Memory, and the output of Data Memory goes back to the Register file.
### **Topic 4: Datapath Control Logic (Chapter 4, Slides 22-31)**
  *This is about how the "brain" of the CPU controls the multiplexors.*
  
  *   **The Control Signal Table (Like HW4 "Find appropriate values..."):**
    *   You must be able to generate the 0s, 1s, and Xs (Don't Cares) for every control wire based on an instruction.
    *   **The 9 Main Signals:** `Reg2Loc`, `Uncondbranch`, `Branch`, `MemRead`, `MemtoReg`, `ALUOp`, `MemWrite`, `ALUSrc`, `RegWrite`.
    *   *Crucial Skill:* Knowing *why* something is an `X` (Don't Care). For example, if `RegWrite = 0` (like in `STUR` or `CBZ`), then `MemtoReg` is `X` because we aren't saving anything to the registers anyway.
  *   **Branch Target Calculation (Like HW4 final question):**
    *   Know how to calculate the 64-bit output of the **"Shift left 2"** unit for `CBZ` and `B` instructions.
    *   *The Steps:* Find the instruction offset (Target line - Current line) $\to$ Sign-extend to 64 bits (using 2's complement if jumping backward) $\to$ Shift left by 2 (append `00` to the right).
### **Topic 5: Required Carryover from Chapters 1 & 2**
  *The professor explicitly stated to review Ch 1 & 2 because you cannot do Ch 3 & 4 without them.*
  
  *   **Green Card Usage:** You still need to know how to pull the Opcode out of a 32-bit hex instruction to figure out what instruction is running. 
  *   **Instruction Formats:** You need to know the difference between R, D, I, CB, and B formats so you know where the offset or immediate fields are located in the 32-bit string.
  *   **Two's Complement Math:** If you can't do 2's complement negation (Invert bits + 1), you will fail both the subtraction overflow problems and the backward-jumping branch calculation problems.
  
  ---
- ---
- ---
### **Topic: Generating Control Signals**
  
  **Problem 1: The R-Format Control Signals**
  Generate the 9 main control signals and the 4-bit ALU control for the instruction `SUB X1, X2, X3`. 
  *(Signals: Reg2Loc, Uncondbranch, Branch, MemR/W (2-bit), MemtoReg, ALUOp, ALUSrc, RegWrite, ALU Control)*
  
  **Solution 1:**
  *   **Reg2Loc = 0**: We need to read `Rm` (bits 20-16) as the second source register.
  *   **Uncondbranch = 0**: Not a `B` instruction.
  *   **Branch = 0**: Not a `CBZ` instruction.
  *   **MemR/W = 00**: We are doing math, so we hold/ignore Data Memory.
  *   **MemtoReg = 0**: We want to send the ALU result back to the Register File (not memory data).
  *   **ALUOp = 10**: This code tells the ALU Control unit "Look at the instruction opcode to figure out the math."
  *   **ALUSrc = 0**: We want the second ALU input to be the register data we just read, not an immediate offset.
  *   **RegWrite = 1**: We are writing the final answer into `X1`.
  *   **ALU Control = 0110**: The specific 4-bit code to command the ALU to subtract.
  
  ***
  
  **Problem 2: The "Don't Cares" of LDUR**
  For the instruction `LDUR X9,[X10, #24]`, determine the values of the **Reg2Loc** and **MemtoReg** control signals. If either is a Don't Care (X), clearly explain *why* the hardware doesn't care.
  
  **Solution 2:**
  *   **Reg2Loc = X (Don't Care).** `LDUR` only reads one register from the Register File: the base address (`X10`, which is hardwired into Read Register 1). It does not read a second register at all. Because we don't care what comes out of the "Read Data 2" port, we don't care what the `Reg2Loc` multiplexor selects.
  *   **MemtoReg = 1.** `LDUR` pulls data out of the Data Memory and writes it to a register. The `MemtoReg` mux must be set to `1` to route the Data Memory output back to the Register File.
  
  ***
  
  **Problem 3: Reverse Engineering the Datapath**
  You freeze the CPU during a clock cycle and observe the following control signals leaving the Main Control Unit:
  `Uncondbranch = 0`, `Branch = 0`, `MemR/W = 01`, `ALUSrc = 1`, `RegWrite = 0`.
  What specific LEGv8 instruction is currently executing? Justify your answer.
  
  **Solution 3:**
  *   **Instruction:** **`STUR` (Store Register)**
  *   *Justification:* 
    *   `MemR/W = 01` tells us the CPU is actively writing to the Data Memory. Only `STUR` does this.
    *   `RegWrite = 0` confirms we are not loading data into a register.
    *   `ALUSrc = 1` confirms the ALU is using a sign-extended offset to calculate a memory address.
  
  ---
### **Topic: Multiplexor Logic & PC Routing**
  
  **Problem 4: The PC Source Logic**
  Look at the top-right corner of the Single-Cycle Datapath. There is a multiplexor that chooses what address goes into the PC next. It chooses between `PC + 4` and the `Branch Target Address`. 
  Write the Boolean logic equation that controls this multiplexor using the signals `Uncondbranch`, `Branch`, and the ALU's `Zero` flag.
  
  **Solution 4:**
  The multiplexor switches to the Branch Target Address (1) if the instruction is an unconditional branch, OR if it's a conditional branch AND the condition is met.
  *   **Logic Equation:** `Mux Control = Uncondbranch OR (Branch AND Zero)`
  
  ***
  
  **Problem 5: The Reg2Loc Multiplexor**
  Why do `STUR` and `CBZ` require the `Reg2Loc` control signal to be `1`, while `ADD` requires it to be `0`? 
  
  **Solution 5:**
  *   In the LEGv8 machine code, the 5 bits representing the second register to be read are located in different places depending on the instruction format.
  *   For an `ADD` (R-Format), the second source register is `Rm`, located at bits **20-16**. Setting `Reg2Loc = 0` routes these bits to the Register File.
  *   For `STUR` (D-Format) and `CBZ` (CB-Format), the register whose data we need to read (to either store into memory or check for zero) is `Rt`, located at bits **4-0**. Setting `Reg2Loc = 1` routes these bits to the Register File instead.
  
  ---
### **Topic: Debugging "Stuck-At" Faults**
  *These questions test your deep understanding by breaking the datapath.*
  
  **Problem 6: Stuck ALUSrc**
  Suppose a manufacturing defect causes the `ALUSrc` control wire to be permanently stuck at `0`. 
  If the CPU attempts to execute `LDUR X1, [X2, #40]`, what specifically goes wrong in the datapath, and what garbage data ends up in register `X1`?
  
  **Solution 6:**
  *   *Intended behavior:* `ALUSrc` should be `1` to route the immediate offset (`#40`) into the ALU, allowing it to calculate the address `X2 + 40`.
  *   *Fault behavior:* Because `ALUSrc` is stuck at `0`, the ALU receives **Read Data 2** from the Register File instead of the offset. 
  *   *The Result:* The ALU will add the base register (`X2`) to whatever random garbage register happens to be decoded by bits 20-16. It will send this garbage address to the Data Memory. `X1` will be loaded with random, unpredictable data from the wrong memory location (or cause a segmentation fault).
  
  ***
  
  **Problem 7: Stuck MemtoReg**
  Suppose the `MemtoReg` control wire is permanently stuck at `0`. 
  If the CPU attempts to execute `LDUR X5,[X6, #8]`, what value is actually written into register `X5`?
  
  **Solution 7:**
  *   *Intended behavior:* `MemtoReg` should be `1` to route the data fetched from Data Memory back to the register file.
  *   *Fault behavior:* Because it is stuck at `0`, the multiplexor will route the **ALU Result** directly to the register file, completely ignoring the output of the Data Memory.
  *   *The Result:* Register `X5` will be written with the calculated physical memory address ($X6 + 8$) instead of the actual data stored at that address in RAM!
  
  ***
  
  **Problem 8: Stuck Branch Logic**
  Suppose the `Zero` flag output from the ALU is permanently stuck at `1`. 
  How will this affect a `CBNZ X9, Loop` instruction? (Assume X9 contains the value 5).
  
  **Solution 8:**
  *   *Intended behavior:* `CBNZ` should branch if X9 is NOT zero. Since X9 is 5, the branch *should* be taken.
  *   *Fault behavior:* `CBNZ` works by passing the register value through the ALU. Because it's an inverted check, the control unit actually wants the `Zero` flag to be `0` to trigger a jump for `CBNZ`. 
  *   *The Result:* Because the `Zero` flag is stuck at `1` (which physically means "the value IS zero"), the hardware logic thinks the condition failed. The PC multiplexor will not switch, and the `CBNZ` instruction will **fail to branch**, falling through to `PC + 4`.
  
  ---
### **Topic: ALU Control Logic**
  
  **Problem 9: The Two-Level ALU Control**
  The Datapath uses a 2-level control scheme for the ALU: The Main Control generates a 2-bit `ALUOp`, and the ALU Control Unit turns that into a 4-bit command for the actual ALU. 
  Why doesn't the Main Control unit just generate the 4-bit ALU command directly?
  
  **Solution 9:**
  *   It simplifies the Main Control hardware. 
  *   For Load/Store instructions, the ALU *always* adds. For `CBZ`, it *always* passes Input B. The Main Control can just send a generic `ALUOp` (`00` or `01`) to handle these.
  *   For R-type instructions (`ADD`, `SUB`, `AND`, `ORR`), the Main Control doesn't know what math to do just by looking at the 11-bit opcode. It sends `ALUOp = 10` to the ALU Control Unit, delegating the job. The ALU Control Unit then looks at the specific instruction bits to finalize the exact 4-bit math command, keeping the Main Control Unit smaller and faster.
  
  ***
  
  **Problem 10: Adding ADDI to the Control Table**
  The professor's HW4 table only included `ADD` (R-format). If we wanted to execute the I-Format instruction `ADDI X1, X2, #15`, what would the values of the following 5 control signals be? 
  `Reg2Loc`, `ALUSrc`, `MemtoReg`, `RegWrite`, `MemR/W`.
  
  **Solution 10:**
  *   **Reg2Loc = X**: `ADDI` only reads one source register (`Rn`). It does not read a second register, so the mux selection doesn't matter.
  *   **ALUSrc = 1**: The second input to the ALU must be the sign-extended immediate offset (`#15`), not a register.
  *   **MemtoReg = 0**: We want to send the result of the ALU math directly back to the register.
  *   **RegWrite = 1**: We are updating register `X1`.
  *   **MemR/W = 00**: We are strictly doing math inside the CPU; we hold/ignore the Data Memory.