## Stack Operations
  
  *   **Stack:** A section of SRAM (data memory) used by the CPU for temporary storage of data or addresses. The defining characteristic of a stack is LIFO (Last-In, First-Out).
  
  *   **Stack Pointer (SP):**  A 16-bit register that points to the *top* of the stack.
  *   The SP is an I/O register (actually two 8-bit registers: `SPL` (low byte) and `SPH` (high byte)).
  *   **Default:**  SP = 0x00 (Upon reset, the SP is initialized to 0, which is *not* a valid RAM location.  You *must* initialize the SP in your program.)
  *   Needed because there are a relatively limited number of registers.
  *   Two main stack operations:
      1.  **PUSH:**
          *   a) GPR contents saved where SP points to in SRAM.
          *   b) SP is decremented (SP = SP - 1).
          *Example:* PUSH R5 (content of R5 copied onto stack, then SP = SP - 1)
      2.  **POP:**
          *   a) SP is incremented (SP = SP + 1).
          *   b) Contents of SRAM where SP points are copied to GPR.
  
  **Initializing the Stack Pointer:**
  
  The stack pointer *must* be initialized before any stack operations (PUSH, POP, CALL, RET) are used. It's common practice to initialize the SP to the *end* of available RAM, as the stack grows *downward*.
  
  ```assembly
  LDI R16, HIGH(RAMEND)  ; Load high byte of RAMEND into R16
  OUT SPH, R16          ; Set SPH
  LDI R16, LOW(RAMEND)   ; Load low byte of RAMEND into R16
  OUT SPL, R16          ; Set SPL
  ```
  
  *Explanation:*
  
  *   `RAMEND` is a predefined constant in the ATmega32 include file ("M32DEF.INC") representing the last address of internal SRAM.
  *   `HIGH()` and `LOW()` are assembler functions that extract the high and low bytes of a 16-bit value.
  *   We write to `SPH` *before* `SPL` because the AVR is an 8-bit architecture, and the SP is 16 bits.  There's a temporary register that handles this.
  
  **Example (PUSH and POP):**
  
  ```assembly
  ; Example showing stack operations and register values
  
  ; Initialize stack pointer to the end of RAM
  LDI R18, HIGH(RAMEND)  ; $085F
  OUT SPH, R18
  LDI R18, LOW(RAMEND)
  OUT SPL, R18            ; $085F
  
  ; Push some values onto the stack
  LDI R28, 0x04
  LDI R27, 0x1F
  LDI R26, 0x43
  PUSH R28      ; SP = 0x085E, Memory[0x085F] = 0x04
  PUSH R27      ; SP = 0x085D, Memory[0x085E] = 0x1F
  LDI R27, 0
  LDI R28, 0
  PUSH R26      ; SP = 0x085C, Memory[0x085D] = 0x43
  POP  R28       ; R28 = 0x43, SP = 0x085D
  POP  R27       ; R27 = 0x1F, SP = 0x085E
  POP  R26       ; R26 = 0x04, SP = 0x085F.
  ```
  
  **Stack and Data Memory:**
  
  The stack resides within the internal SRAM.  You *must* ensure that the stack does not overflow into memory used for other data.
  
  *Note: Refer to Figure 2-3.  The Data Memory for AVRs with No Extended I/O Memory.  Shows the layout of general-purpose registers, I/O registers (SFRs), and internal SRAM.*
  
  *Note: Refer to Figure 2-7. I/O Registers of the ATmega32 and Their Data Memory Address Locations. This table shows the addresses of various SFRs, including SREG, SPL, SPH.*
## Subroutines (CALL and RET)
  
  *   **CALL:** Calls a subroutine (like a function call in higher-level languages).
    *   4 bytes (2 16-bit words).
    *   10 bits for opcode (machine code).
    *   22 bits for address:  $2^{22} \Rightarrow 4M$.
    *   ATmega32: 16K x 16 EPROM/PC = 14 bits
    *   The `CALL` instruction pushes the address of the *next* instruction (the return address) onto the stack and then jumps to the subroutine's starting address.
    *   For CALL:
        1.  PUSH high byte of PC* onto the stack.
        2.  PUSH low byte of PC* onto the stack.
  
       \*PC = 0x100
  
    *   Example: `CALL DELAY`
  
  *   **RET:** Returns from a subroutine.
    *   Pops the return address from the stack and loads it into the Program Counter (PC).  Execution continues from the instruction after the `CALL`.
    *  4 byte instructions.
    1. POP low byte of PC*
    2.  POP high byte of PC*
    *   Example: `RET`
  
  *   **RCALL (Relative Call):**  A 2-byte (16-bit word) version of `CALL`.  It uses a relative offset from the current PC, allowing for calls within a smaller range ( -2048 to +2047 words from the current PC).  More efficient for short jumps.
  
    $PC_{new} = PC_{current} + \text{offset} + 1$
  
  *  **ICALL (Indirect Call):**
  PC = Z register, which is indirect calling
  ATmel AVR = 23 bits, but for ATmega32 -> PC = 14 bits
## Branching Instructions and Looping
  
  *   **Branching Instructions:** Change the flow of program execution based on conditions.
  
  *   **Unconditional Branch:**
    *   `JMP`: Jump to a specified address. (4 bytes)
    *   `RJMP`: Relative jump (2 bytes).  Jump to an address relative to the current PC (-2048 to +2047 words).
    *  `IJMP`: Uses a 16-bit address from the Z register.
  
  *   **Conditional Branch:** Branch occurs only if a specific condition is met (based on flags in the SREG).
  
    *   **BRNE (Branch if Not Equal):** Branches if the Zero flag (Z) is 0 (meaning the previous operation's result was *not* zero).
  
    ```assembly
    ; Example of a simple loop using BRNE
  
    LDI R17, 5    ; Initialize loop counter to 5
    LDI R18, 0    ; Initialize sum to 0
    LDI R19, 6    ; Value to increment
    AGAIN:
        ADD R18, R19     ; Add increment value to sum
        DEC R17          ; Decrement counter
        BRNE AGAIN      ; See if counter = 0
        OUT PORTC, R18   ; Out sum to PortC
    ```
    *The number of times it runs is calculated as 5x4=20, so it will be executed 20 times until it stops*
  
  *  **BRSH (Branch if same or higher)**: $C=1$
  *  **BRLO (Branch if lower)**: $C=0$
  
  *   **Other Conditional Branches:**  `BREQ` (Branch if Equal, Z=1), `BRMI` (Branch if Minus, N=1), `BRPL` (Branch if Plus, N=0), `BRVS` (Branch if Overflow Set, V=1), `BRVC` (Branch if Overflow Clear, V=0), and many more.
  
  *   **CPI (Compare Immediate):**  Compares a register with an immediate value and sets the flags in SREG, but *doesn't* store the result of the subtraction.  This is frequently used before conditional branch instructions.
  
  **Looping:**
  
  Loops are fundamental in programming.  They allow a block of code to be executed repeatedly.
  
  *   **Basic Loop Structure:**
    1.  Initialize a counter register.
    2.  Perform the operations within the loop.
    3.  Decrement (or increment) the counter.
    4.  Check the counter's value (often using `BRNE` or `BREQ`).  If the condition is met (e.g., counter is not zero), branch back to the beginning of the loop.
  
  **BRNE Instruction Format:**
  
  | 15 | 14 | 13 | 12 | 11 | 10 |  9 |  8 |  7 |  6 |  5 |  4 |  3 |  2 | 1 | 0 |
  |:--:|:--:|:--:|:--:|:--:|:---:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:-:|:-:|
  | 1  | 1  | 1  | 1  | 0  |  1  |  k | k | k | k | k | k | k | 0 | 0 | 1|
  
  *   `k`: Represents a 7-bit two's complement value, giving a relative branch range of -64 to +63 from the instruction *after* the `BRNE`.
  
  **Example (BRNE with Negative Offset):**
  
  ```
  PC_new = PC_old + k + 1
  ```
  Suppose we're at the instruction `BRNE AGAIN`.
  PC_old = 0x0005 (location of the BRNE)
  If we go back three lines, to again the address is 0x0003
  
  ```
     k = -3 => 1111101
  ```
  
  If we plug this number into the BRNE binary number format we have:
  1111011111101001
  Which will branch back 3 instructions.
  
  **Example (Looping 250,000 Times):**
  Looping can be nested to achieve large numbers of iterations. For example, an outer loop repeating 100 times, which contains a nested loop that repeats 25 times, and within the loop is an instruction that is repeated 100 times. In assembly:
  
  ```assembly
  LDI R17, 0x9F ;R17 is output to PortA
  com R17       ;
  LDI R20, 25
  loop-3:
    LDI R21, 100
  loop-2:
    LDI R22, 100
  loop-1:
    com R17         ;Flip the bits of R17
    out porta, R17  ;send it to port A
    DEC R22
    BRNE Loop_1     ;R22=0?
    DEC R21
    BRNE Loop_2    ;R21=0?
    DEC R20
    BRNE Loop_3    ;R20=0?
  ```
  Here, R20 repeats the outer loop 25 times, R21 repeats the middle loop 100 times, and R22 repeats the inner loop 100 times.
  
  **Example (Summing Values):**
  
  ```assembly
  ; Find the sum of the values 0x62, 0xA3, and 0xC9.
  ; Put the sum into R22 (low byte) and R23 (high byte).
  
  .INCLUDE "M32DEF.INC"
  
  .ORG 0
    LDI R22, 0    ; Initialize R23:R22
    LDI R23, 0    ; to zero
    LDI R26, 0x62 ; R26 = 0x62
    ADD R22, R26  ; Adding R26 to low byte of sum
    BRSH Nxt_Number ; if C = 0, go to the next
                    ; next number to add
    INC R23      ; if C=1, inc high byte
                    ; of sum
  Nxt_Number:
    LDI R26, 0xA3 ; R26 = 0xA3
    ADD R22, R26  ; add next number to the sum low byte
    BRSH Nxt_Number1 ;If C=1, inc the high
                    ; byte
    INC R23
  Nxt_Number1:
    LDI R26, 0xC9 ; R26 = 0xC9
    ADD R22, R26   ; add next number to the low byte of the sum.
    BRSH Done
    INC R23
  Done:   JMP Done   ; terminal loop
  ```
  
  This example demonstrates adding multiple values, handling carries between bytes (using `BRSH` to check the Carry flag), and storing a 16-bit result in a register pair.
  **Unconditional Branch Instructions**
  
  *   **JMP (Jump):**  4 bytes, can go to any memory location in the 4M (word) address space of the AVR.
  
  *   **RJMP (Relative Jump):** 2 bytes, relative address range of -2048 to +2047 words relative to the *current* PC.
    *   `RJMP DELAY`
  
  *   **IJMP (Indirect Jump):** Uses the Z register to jump to a location within the lower 64K words of program memory.
    *   `IJMP`  (Z register holds the address).