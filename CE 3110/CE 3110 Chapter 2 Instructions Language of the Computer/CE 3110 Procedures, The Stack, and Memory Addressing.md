#### **1. Procedures (Functions)**
  A procedure is a stored subroutine (like a method in Java or function in C).
  When function A calls function B, a strict protocol must be followed so B knows where to get arguments and where to return.
  
  **The 6 Steps of Execution:**
  1.  **Place Parameters:** Put arguments in registers **X0 – X7**.
  2.  **Transfer Control:** Jump to the procedure code.
  3.  **Acquire Storage:** If the procedure needs memory (local variables), reserve space on the Stack.
  4.  **Perform Operations:** Do the work.
  5.  **Place Result:** Put the return value in registers **X0 – X7**.
  6.  **Return:** Jump back to the instruction immediately following the original call.
  
  **The Instructions:**
  *   **`BL Label` (Branch with Link):** Used to *call* a function.
  *   It jumps to `Label`.
  *   **Crucial:** It saves the address of the *next* instruction into register **X30 (LR - Link Register)**. This is the "breadcrumb" so the CPU knows where to go back.
  *   **`BR LR` (Branch to Register):** Used to *return* from a function.
  *   It looks at **X30 (LR)** and jumps to that address.
#### **2. The Stack**
  We only have ~30 registers. What if a function has 50 local variables? Or what if Function A calls Function B, and Function B needs to use the same registers A was using? We can't let B overwrite A's data.
  
  **The Solution:** Spill registers to memory.
  *   **The Stack:** A specific area of memory used for temporary storage during function calls.
  *   **SP (Stack Pointer - X28):** A register that holds the memory address of the *top* of the stack.
  *   **Direction:** The stack grows **down** (from high addresses to low addresses).
    *   **To Push (Save):** Subtract from SP (`SUBI SP, SP, #16`), then Store (`STUR`).
    *   **To Pop (Restore):** Load (`LDUR`), then Add to SP (`ADDI SP, SP, #16`).
  
  **Leaf vs. Non-Leaf Procedures:**
  *   **Leaf Procedure:** A function that *does not* call another function.
    *   *Simple:* It doesn't need to save the Link Register (LR) because no one will overwrite it.
  *   **Non-Leaf Procedure:** A function that calls other functions (e.g., `main()` calls `printf()`, or a recursive function).
    *   *Complex:* Before it calls someone else, it **must** save its own X30 (LR) to the stack. If it doesn't, the `BL` instruction inside will overwrite X30, and the function will never know how to get back to the start.
#### **3. Memory Layout**
  Your program's memory is divided into four dynamic sections:
  1.  **Text:** The program code (instructions). Read-only.
  2.  **Static Data:** Global variables (constants, static arrays).
  3.  **Dynamic Data (Heap):** Structures that grow/shrink (e.g., `malloc` in C, `new` objects in Java). Grows **Up**.
  4.  **Stack:** Automatic storage for function calls. Grows **Down**.
#### **4. Characters & Strings**
  Not all data is a 64-bit integer.
  *   **Bytes:** Characters are usually 8 bits (ASCII).
  *   **Instructions:**
    *   `LDURB` (Load Byte): Loads just 8 bits.
    *   `STURB` (Store Byte): Stores just 8 bits.
  *   **String Copy (`strcpy`):**
    *   Strings in C are arrays of bytes ending with a `null` byte (`0`).
    *   The loop (Slide 61) loads a byte, checks if it is 0 (`CBZ`), stores it, increments the pointer by **1** (not 8), and repeats.
#### **5. Large Constants**
  *   **The Problem:** Instructions are 32 bits long. You physically cannot fit a 64-bit constant number inside a 32-bit instruction (where would the Opcode go?).
  *   **The Limit:** Regular instructions like `ADDI` only accept 12-bit constants.
  *   **The Solution:** Two special instructions to build big numbers in chunks.
    1.  **`MOVZ` (Move with Zeros):** Loads a 16-bit number and zeros out the rest of the register.
    2.  **`MOVK` (Move with Keep):** Loads a 16-bit number into a specific spot, *keeping* the existing data in the rest of the register.
    *   *Shift:* You specify where the 16 bits go (Shift 0, 16, 32, or 48).
#### **6. Addressing Modes Summary**
  There are 4 ways the CPU can calculate an address:
  1.  **Immediate:** The data is right there in the instruction (`ADDI`).
  2.  **Register:** The data is in a register (`ADD`).
  3.  **Base (Displacement):** Memory access. Register + Constant Offset (`LDUR`).
  4.  **PC-Relative:** Branching. PC + Constant Offset (`B`, `CBZ`).
  
  ---
### **Student "Check Your Understanding"**
  *This completes Chapter 2. Test yourself with these final concepts:*
  
  1.  **Stack Mechanics:** If the Stack Pointer (`SP`) is at address 2000, and you push two 64-bit integers onto the stack, what is the new value of `SP`? (Remember: The stack grows *down*).
  2.  **Procedure Calls:** Why **must** a recursive function save register X30 (LR) to the stack at the start of the function?
  3.  **Large Constants:** You need to load the value `0x1234567812345678` into a register. Can you do this with one instruction? If not, which instructions (and how many) would you use?