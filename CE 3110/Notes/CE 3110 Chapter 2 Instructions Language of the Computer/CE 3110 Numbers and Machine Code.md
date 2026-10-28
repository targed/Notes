#### **1. Signed vs. Unsigned Integers**
  Computers are built of switches (0 and 1). They don't natively know what a "negative" number is. We have to agree on a system to represent them.
  
  *   **Unsigned Integers:**
  *   Used for memory addresses or data that can never be negative.
  *   Range: $0$ to $2^n - 1$.
  *   **Signed Integers (2's Complement):**
  *   This is the universal standard for integers in computers.
  *   **The Sign Bit:** The Most Significant Bit (MSB - the leftmost bit) tells you the sign.
      *   `0` = Positive.
      *   `1` = Negative.
  *   **The Trick:** To make a number negative, you don't just flip the first bit. You must:
      1.  Invert all bits (0 becomes 1, 1 becomes 0).
      2.  Add 1 to the result.
  *   **Example (Slide 19):** Negating +2.
      *   +2 in binary: `...0010`
      *   Invert: `...1101`
      *   Add 1: `...1110` (This is -2).
  
  **Why 2's Complement?**
  It allows the hardware to use the **same circuit** for Addition and Subtraction. Subtraction ($A - B$) is just treated as Addition ($A + (-B)$).
#### **2. Sign Extension**
  *   **The Problem:** You load a byte (8 bits) from memory, say `1111 1110` (-2). You put it into a **64-bit** register. If you fill the remaining 56 bits with zeros, the number becomes a huge positive integer.
  *   **The Solution:** You must "extend" the sign bit to fill the empty space.
    *   **Unsigned Load (`LDURB`):** Fills the upper bits with **0s**. (Used for characters, pixels).
    *   **Signed Load (`LDURSB`):** Fills the upper bits with **1s** if the number was negative, or **0s** if positive. (Used for math values).
#### **3. Instruction Representation (The Formats)**
  This is **Machine Code**. Every instruction in LEGv8 is exactly **32 bits** long. However, how we chop up those 32 bits depends on the *type* of instruction.
  
  **A. R-Format (Register Format) – Slide 23**
  Used for arithmetic involving **only registers** (e.g., `ADD X1, X2, X3`).
  
  | Field | Bits | Description |
  | :--- | :--- | :--- |
  | **Opcode** | 11 | The ID number of the instruction (e.g., "Add" has a specific ID). |
  | **Rm** | 5 | The *Second* Source Register. |
  | **shamt** | 6 | Shift Amount (used for shifting bits, `0` for normal math). |
  | **Rn** | 5 | The *First* Source Register. |
  | **Rd** | 5 | The **Destination** Register (where the result goes). |
  
  **B. D-Format (Data Transfer Format)**
  Used for moving data between Memory and Registers (`LDUR`, `STUR`).
  
  | Field | Bits | Description |
  | :--- | :--- | :--- |
  | **Opcode** | 11 | Instruction ID. |
  | **Address** | 9 | The **Offset** (constant). Note: This is a signed 9-bit number! |
  | **Op2** | 2 | Extension bits (usually `00` for LEGv8 basic loads). |
  | **Rn** | 5 | The **Base** Register (holds the memory address). |
  | **Rt** | 5 | The **Target** Register (source for Store, dest for Load). |
  
  *   *Note the name change:* In D-Format, the register being written/read is called `Rt`, not `Rd`.
  
  **C. I-Format (Immediate Format)**
  Used for math with constants (`ADDI`, `SUBI`).
  
  | Field | Bits | Description |
  | :--- | :--- | :--- |
  | **Opcode** | 10 | Instruction ID (Slightly shorter to make room for immediate). |
  | **Immediate**| 12 | The constant number (e.g., the "4" in `ADDI X1, X2, #4`). |
  | **Rn** | 5 | Source Register. |
  | **Rd** | 5 | Destination Register. |
#### **4. Machine Code Examples**
  Let's walk through (`ADDI X9, X9, #1`) to see how translation works.
  
  1.  **Instruction:** `ADDI X9, X9, #1`
  2.  **Format:** I-Format (because of the immediate `#1`).
  3.  **Breakdown:**
    *   **Opcode:** Look up `ADDI` in the "Green Card" (reference sheet). It's `488` (hex) or `1001000100` (binary).
    *   **Immediate:** The number is `1`. In 12-bit binary: `0000 0000 0001`.
    *   **Rn (Source):** `X9`. 9 in binary is `01001`.
    *   **Rd (Dest):** `X9`. 9 in binary is `01001`.
  4.  **Assemble:**
    `1001000100` (Op) | `000000000001` (Imm) | `01001` (Rn) | `01001` (Rd)
  5.  **Hexadecimal:** Group them into 4s to convert to Hex.
  
  ---
### **Student "Check Your Understanding"**
  *These questions test if you can "be the assembler".*
  
  1.  **2's Complement:** What is the binary representation of **-1** in a 32-bit system? (Hint: Invert 0 and add 1).
  2.  **Format ID:** Look at the following instructions. Which format (R, D, or I) does each use?
    *   `SUB X1, X2, X3`
    *   `LDUR X1, [X2, #100]`
    *   `ADDI X1, X2, #50`
  3.  **Bit Constraint:** In D-Format instructions (`LDUR`), the "Address" (Offset) field is only **9 bits** wide. What is the maximum positive offset you can type before the assembler gives you an error? (Hint: $2^8 - 1$).