### Arithmetic Instructions
  
  These instructions perform arithmetic operations on data stored in registers. The AVR's ALU (Arithmetic Logic Unit) is directly connected to the general-purpose registers (R0-R31), allowing for fast, single-cycle execution of many arithmetic instructions.
  
  **Unsigned Numbers:**
  
  *   Unsigned numbers represent only positive values and zero.  For an 8-bit value, the range is 0 to 255 (0x00 to 0xFF).
  
  **Addition:**
  
  *   **`ADD Rd, Rr`**:  Adds the contents of register `Rr` to register `Rd`, storing the result in `Rd`.
  *   $Rd \leftarrow Rd + Rr$
  *   Flags Affected: H, S, V, N, Z, C
  
  *   **`ADC Rd, Rr`**:  Adds the contents of register `Rr` to register `Rd`, *and* adds the Carry flag (C) from a previous operation. This is crucial for multi-byte addition.
  *   $Rd \leftarrow Rd + Rr + C$
  *   Flags Affected: H, S, V, N, Z, C
  
  **Example (8-bit addition):**
  
  ```assembly
  LDI R20, 0xF5   ; R20 = 0xF5
  LDI R21, 0x0B   ; R21 = 0x0B
  ADD R20, R21   ; R20 = R20 + R21 = 0xF5 + 0x0B = 0x00, C = 1, Z = 1
  ```
  
  *Explanation:*
  
  1.  0xF5 + 0x0B = 0x100.  Since R20 is an 8-bit register, it can only hold 0x00. The '1' is carried over.
  2.  The Carry flag (C) is set to 1 because there was a carry out of bit 7.
  3.  The Zero flag (Z) is set to 1 because the result in R20 is zero.
  
  **Example (16-bit addition with carry):**
  
  ```assembly
  ; Add two 16-bit numbers: 0x3CE7 + 0x3B8D
  ; Store the result in R21:R20
  
  LDI R20, 0xE7    ; R20 = Low byte of first number (0xE7)
  LDI R21, 0x3C    ; R21 = High byte of first number (0x3C)
  LDI R18, 0x8D    ; R18 = Low byte of second number (0x8D)
  LDI R19, 0x3B    ; R19 = High byte of second number (0x3B)
  
  ADD R20, R18    ; Add low bytes (E7 + 8D = 174, C=1)
  ADC R21, R19    ; Add high bytes + carry (3C + 3B + 1 = 78)
  
  ; Result: R21:R20 = 0x7874
  ```
  
  *Explanation:*
  
  1.  `ADD R20, R18` adds the low bytes. The carry generated from this addition is stored in the Carry flag (C).
  2.  `ADC R21, R19` adds the high bytes *and* the Carry flag from the previous addition.
  
  **Subtraction:**
  
  *   **`SUB Rd, Rr`**: Subtracts the contents of register `Rr` from register `Rd`, storing the result in `Rd`.
  *   $Rd \leftarrow Rd - Rr$
  *   Flags Affected: H, S, V, N, Z, C  (Note: The Carry flag acts as a *borrow* flag in subtraction).
  
  *   **`SBC Rd, Rr`**: Subtracts the contents of register `Rr` from register `Rd`, *and* subtracts the Carry flag (C).  Used for multi-byte subtraction.
  *   $Rd \leftarrow Rd - Rr - C$
  *    Flags Affected: H, S, V, N, Z, C
  
  *  **`SUBI Rd, K`**: Subtract immediate. Subtracts constant from register Rd.
  * $Rd \leftarrow Rd - K$
  *   Flags Affected: H, S, V, N, Z, C
  
  * **`SBCI Rd,K`**: Subtract Immediate with Carry, used for multi-word subtraction.
  *    $Rd \leftarrow Rd - K - C$
  *    Flags Affected: H, S, V, N, Z, C
  
  *   **`SBIW Rd+1:Rd, K`**: Subtract immediate from word. Works on register pairs R25:R24, R27:R26, R29:R28, R31,R30.
  *    $Rd+1: Rd \leftarrow Rd+1:Rd -K$
  *    Flags Affected: S, V, N, Z, C
  
  **Example (Subtraction with SUB and SUBI):**
  
  ```assembly
  ; Example showing the steps in subtraction
  LDI R20, 0x23    ;Load 0x23 into R20
  LDI R21, 0x3F    ;Load 0x3F into R21
  SUB R21, R20    ;R21 <- R21-R20
  
  ; Solution
  ; R21 = 3F = 0011 1111
  ; R20 = 23 = 0010 0011
  ;            - 
  ;         1C 0010 0011
  
  ;            0011 1111
  ;         +  1101 1101 (2's comp)
  ;      (1)  0001 1100
  
  ; C=0, D7=0 N=0 (Result is positive)
  
  ; Subtract 18H from 29H and store result in R21, using SUBI, (a) without using SUBI, and (b) with.
  ; (a)
  LDI R21, 0x29    ;R21 = 29H
  LDI R22, 0x18    ;R22 = 18H
  SUB R21, R22    ;R22 = 29 -18 = 11H
  
  ; (b)
  LDI R21, 0x29
  SUBI R21, 0x18    ;R21 = 18 -29 = 18 -11 H
  ```
  In this case, since the 2nd operand is being loaded into R22.
  
  **Multiplication (Unsigned):**
  
  *   **`MUL Rd, Rr`**: Multiplies two *unsigned* 8-bit registers.
  *   Result:  A 16-bit result is placed in registers R1 (high byte) and R0 (low byte).
  *   Operands: Rd and Rr can be any of the general-purpose registers (R0-R31).  However, if *either* operand is R0 or R1, the result will *overwrite* one of the operands.
  *   Flags Affected: Z, C (C is set if bit 15 of the result is set, cleared otherwise).
  
  **Example (Multiplication):**
  
  ```assembly
  LDI R23, 0x25    ; Load 25H into R23
  LDI R24, 0x65    ; Load 65H into R24
  MUL R23, R24    ; Multiply R23 * R24
              ; Result: R1:R0 = 0E99H (C=0)
  ```
  
  **Division (Unsigned):**
  
  *   The AVR *does not* have a dedicated division instruction.
  *   Division must be implemented using repeated subtraction or other algorithms (shift-and-subtract, etc.).
  
  **Example (Division by Repeated Subtraction - Conceptual):**
  
  ```assembly
  ; Divide R20 by R21, placing the quotient in R22 and remainder in R23
  
  .DEF NUM = R20       ; Dividend
  .DEF DENOMINATOR = R21  ; Divisor
  .DEF QUOTIENT = R22    ; Quotient
  .DEF REMAINDER = R23   ; Remainder
  
  LDI NUM, 95           ; Example: Divide 95 by 10
  LDI DENOMINATOR, 10
  CLR QUOTIENT          ; Initialize quotient to 0
  
  DIVISION_LOOP:
  INC QUOTIENT          ; Increment quotient
  SUB NUM, DENOMINATOR ; Subtract divisor from dividend
  BRCC DIVISION_LOOP   ; Branch back if Carry flag is NOT set (i.e., result is still positive or zero)
  
  ; If we get here, the Carry flag was set, meaning we went too far
  DEC QUOTIENT          ; Decrement quotient (we subtracted one too many times)
  ADD NUM, DENOMINATOR ; Add the divisor back to get the remainder
  
  ; Now: QUOTIENT contains the quotient, and NUM contains the remainder.
  
  ; ... (Rest of your program) ...
  HERE: JMP HERE
  ```
  
  *Explanation:*
  
  1.  We repeatedly subtract the divisor (DENOMINATOR) from the dividend (NUM) and increment the quotient (QUOTIENT).
  2.  We continue subtracting as long as the result is non-negative (Carry flag is not set).
  3.  When the subtraction results in a negative value (Carry flag is set), we've subtracted one too many times.  We decrement the quotient and add the divisor back to the dividend to get the remainder.
  
  *Important Note:* This is a very basic example. It does not handle division by zero, and it is not optimized for speed.  Real-world division routines are more complex.
### Logic Instructions
  
  **AND:**
  
  *   **`AND Rd, Rr`**:  Performs a bitwise logical AND between `Rd` and `Rr`, storing the result in `Rd`.
    *   $Rd \leftarrow Rd \cdot Rr$  (where '.' represents the bitwise AND operation)
    *   Flags Affected: S, V (cleared), N, Z
    *   Uses:
        *   **Masking:** Setting specific bits to 0 while leaving others unchanged.
        *   **Testing Bits:** Checking if specific bits are 0.
  
  **Example (Masking):**
  
  ```assembly
  ; Mask out (clear) the upper nibble of R20, leaving the lower nibble unchanged.
  LDI R20, 0x35   ; R20 = 0011 0101
  ANDI R20, 0x0F  ; R20 = R20 AND 0000 1111 = 0000 0101 (0x05)
  ```
  
  **OR:**
  
  *   **`OR Rd, Rr`**:  Performs a bitwise logical OR between `Rd` and `Rr`, storing the result in `Rd`.
    *   $Rd \leftarrow Rd + Rr$ (where '+' represents the bitwise OR operation, though a vertical bar is typically used: $Rd \leftarrow Rd \vert Rr$ )
    *   Flags Affected: S, V (cleared), N, Z
    *   Uses:
        *   **Setting Bits:** Setting specific bits to 1 while leaving others unchanged.
  
  **Example (Setting Bits):**
  
  ```assembly
  ; Set bits 2 and 5 of R20 to 1, leaving other bits unchanged.
  LDI R20, 0x04   ; R20 = 0000 0100
  ORI R20, 0b00100100 ; R20 = R20 OR 0010 0100 = 0010 0100
  ```
  
  **EX-OR (Exclusive OR):**
  
  *   **`EOR Rd, Rr`**:  Performs a bitwise exclusive OR between `Rd` and `Rr`, storing the result in `Rd`.
    *   $Rd \leftarrow Rd \oplus Rr$
    *   Flags Affected: S, V (cleared), N, Z
    *   Uses:
        *   **Toggling Bits:**  Flipping the state of specific bits.
        *   **Comparing Registers:**  Checking if two registers are equal (result will be 0 if they are).
  
  **Example (Toggling Bits):**
  
  ```assembly
  ; Toggle all bits of R20
  LDI R20, 0x54
  LDI R21, 0x7B
  EOR R20, R21 ; 
  ```
  *Solution: R20 becomes 0x2C*
  
  **COM (Complement):**
  
  *   **`COM Rd`**:  Performs a one's complement (bitwise NOT) on `Rd`.  Each bit is flipped (0 becomes 1, 1 becomes 0).
    *   $Rd \leftarrow \text{ \$FF} - Rd$
    *   Flags Affected: S, V (cleared), N, Z, C (set)
  
  **Example (Complement):**
  ```assembly
  LDI R20, 0xAA
  COM R20 ;Inverts all the bits in register R20
  ```
  
  **NEG (Negate):**
  
  *   **`NEG Rd`**:  Performs a two's complement (negation) on `Rd`. This is equivalent to finding the negative of the number in two's complement representation.
    *   $Rd \leftarrow \$00 - Rd$
    *   Flags Affected: H, S, V, N, Z, C
  
  **Example (Negate):**
  
  ```assembly
  LDI R21, 0x85 ;Load 0x85 into R21, and find the 2's complement
  NEG R21       ; Now: R21 = 0x7B
  ```
  
  **Compare Instructions:**
  
  *   **`CP Rd, Rr`**:  Compares `Rd` and `Rr` by performing a subtraction (`Rd - Rr`), but *does not* store the result.  Only the flags in the status register are affected.
  
  *   **`CPI Rd, K`**:  Compares `Rd` with an immediate value `K` (a constant).
  
  **Conditional Branch Instructions (Using Compare Results):**
  
  These instructions are typically used *after* a `CP` or `CPI` instruction to make decisions based on the comparison result.
  
  *   **`BREQ k`**: Branch if equal (Z flag is set).
  *   **`BRNE k`**: Branch if not equal (Z flag is cleared).
  *   **`BRSH k`**: Branch if same or higher (unsigned comparison, C flag is cleared).
  *   **`BRLO k`**: Branch if lower (unsigned comparison, C flag is set).
  *   **`BRGE k`**: Branch if greater than or equal (signed comparison, S flag = 0).
  *   **`BRLT k`**: Branch if less than (signed comparison, S flag = 1).
  *  **`BRVS k`**: Branch if overflow.
  * **`BRVC K`**: Branch if no overflow.
  
  **Example (Compare and Branch):**
  
  ```assembly
  ; Check if PORTB has the value 0x63. If so, stop.
  
    LDI R20, 0x00 ;Configure PORTB to be an input
    OUT DDRB, R20
  AGAIN:
    IN  R20, PINB   ; Read PORTB
    CPI R20, 0x63  ; Compare with 0x63
    BRNE AGAIN    ; If not equal, loop back
    ; If we get here, PORTB == 0x63
    ; ... (Do something) ...
  ```
  ***
## Advanced Assembly Language Programming (Lecture 6 Topics)
### Assembler Directives
  
  These are commands to the assembler, not instructions executed by the CPU.
  
  *   **`.EQU` (Equate):**  Defines a symbolic constant or a fixed address.  The value *cannot* be changed later.
  
    ```assembly
    .EQU COUNT = 0x25
    LDI R21, COUNT  ; R21 will be loaded with 0x25
    ```
  
  *   **`.SET`:**  Similar to `.EQU`, but the value *can* be reassigned later in the program.
  
  *   **`.ORG` (Origin):**  Specifies the starting address for the following code or data.
    ```assembly
      .ORG 0x0100   ; Start assembling code at address 0x0100
      LDI R16, 0xFF ; This instruction will be placed at 0x0100
    ```
  *    **.INCLUDE:**  This directive tells the AVR assembler to add the contents of a file to the program (like #include in C++)
    ```assembly
    .INCLUDE "M32DEF.INC"
    ```
  *   **`.DB` (Define Byte):**  Allocates ROM space for byte-sized data.
  
  *   `.DW` Define a word in memory.
### Arithmetic and Logic Expressions
  
  *   The AVR assembler can evaluate constant expressions using arithmetic and logic operators.
  
    ```assembly
    .EQU VAL1 = 10
    .EQU VAL2 = 20
    .EQU RESULT = (VAL1 + VAL2) * 2  ; RESULT will be 60
    ```
  
    *   **Arithmetic Operators:** +, -, *, /, % (modulo)
    *   **Logic Operators:** & (bitwise AND), | (bitwise OR), ^ (bitwise XOR), ~ (bitwise NOT)
    *   **Shift Operators:** << (left shift), >> (right shift)
### HIGH() and LOW() functions
  These are used to get the higher and lower bytes of 16-bit numbers.
### Addressing Modes
  
  *  **Single-Register (Immediate) Addressing:**
  
    ```assembly
    NEG R18	;Negate the contents of R18
    COM R19	;Complement the contents of R19
    INC R20	;Increment R20
    DEC R21	;Decrement R21
    ROR R22	;Rotate right R22
    ```
  
  *  **Two-Register Addressing:**
  
    ```assembly
        ;Example of two-register address modes are as follows
    ADD R23, R20 ;ADD R23 to R20
    SUB R29, R23 ;Subtract R29 from R23
    AND R16, R24 ;AND R16 with R24
    MOV R23, R19 ;copy the contents of R19 to R23
    ```
  
  *   **Direct Addressing:** Accessing data in RAM memory using its 16-bit address.
    *   `LDS Rd, k`: Load Direct from SRAM.  Loads a byte from the SRAM address `k` into register `Rd`.
    *   `STS k, Rr`: Store Direct to SRAM. Stores a byte from register `Rr` to the SRAM address `k`.
  
  *  **I/O Direct Addressing:** Used to access I/O registers (like `PORTB`, `DDRC`, etc).
    *    `IN Rd, A`:  Loads a byte from the I/O port at address `A` into register `Rd`.
    *   `OUT A, Rr`:  Stores a byte from register `Rr` to the I/O port at address `A`.
  
  *  **Register Indirect Addressing:** Use X,Y,or Z register.
    *  `LD Rd, X/Y/Z`:  Loads data to a specified register from the address contained in the specified register.
    *  `ST X/Y/Z, Rd`: Stores data to a memory address contained in one of the registers from a specified register.
    *  `LD Rd, X+/Y+/Z+`: Same as above, but increments after loading.
    *  `LD Rd, -X/-Y/-Z`: Same as above, but increments before loading.
### Look-Up Tables and Table Processing
  
  *   **Look-up Table:** A pre-calculated table of values stored in program memory (Flash ROM).
  *  **.DB (Define Byte):** This is the assembler directive to allocate memory and burn in data into the program.
  
    ```assembly
    ; Example look-up table for squares of numbers 0-9
    .ORG 0x200  ; Start table at address 0x200
    SQUARE_TABLE:
        .DB 0, 1, 4, 9, 16, 25, 36, 49, 64, 81
    ```
  * **Reading Table Elements:**
  The Z register is typically used as a pointer to access elements in the look-up table.
  
    *   **`LPM Rd, Z`:** Load Program Memory. Loads a byte from the program memory location pointed to by the Z register into register `Rd`.  Z points to the *byte* address, *not* the word address.
  
  **Example (Look-up Table):**
  ```assembly
  ; Read the table elements from the AVR
  LDI ZH, HIGH(TABLE<<1)
  LDI ZL, LOW(TABLE<<1)
  LPM R16, Z
  ```
  
    *   **`LPM Rd, Z+`:** Load Program Memory with post-increment.  Loads a byte and then increments the Z register.
### Bit-Addressability (Review and Extension)
  
  *   **General-Purpose Registers:** *Not* bit-addressable. You can use `SBR` (Set Bits in Register) and `CBR` (Clear Bits in Register) to set or clear specific bits, but these are actually aliases for `ORI` and `ANDI`.
  *   **I/O Registers (Lower 32):** Bit-addressable using `SBI`, `CBI`, `SBIS`, `SBIC`.
  *   **Status Register (SREG):** Bit-addressable using instructions like `BSET`, `BCLR`, `BRBS` (Branch if Bit in SREG Set), `BRBC` (Branch if Bit in SREG Cleared).
  *    **Internal RAM: Not bit-addressable.**
### Macros
  
  *   **Macro:** A named block of assembly code that can be inserted into the program multiple times by using its name.  Similar to a function, but *expanded inline* during assembly (no function call overhead).
  
  *   **`.MACRO` and `.ENDMACRO`:** Directives used to define a macro.
  
    ```assembly
    .MACRO  MY_MACRO
        ; ... code ...
    .ENDMACRO
    ```
  
  * **Benefits of Macros:**
  * Code Reusability
  * Increased readability
### Accessing EEPROM in AVR
  
  *   **EEPROM (Electrically Erasable Programmable Read-Only Memory):** Non-volatile memory used to store data that needs to be preserved even when power is off.
  
  *   **Registers for EEPROM Access:**
    *   `EEARH:EEARL`:  EEPROM Address Register (16 bits).
    *   `EEDR`: EEPROM Data Register (8 bits).
    *   `EECR`: EEPROM Control Register (8 bits).
  
  *   **Writing to EEPROM:**
    1.  Wait until any previous write is complete.
    2.  Write the EEPROM address to `EEARH:EEARL`.
    3.  Write the data to be written to `EEDR`.
    4.  Set the `EEMWE` bit in `EECR`.
    5.  Set the `EEWE` bit in `EECR` within four clock cycles of setting `EEMWE`.
  
  *   **Reading from EEPROM:**
    1.  Wait until any previous write is complete.
    2.  Write the EEPROM address to `EEARH:EEARL`.
    3.  Set the `EERE` bit in `EECR`.
    4.  Read the data from `EEDR`.
### Checksum Byte in EEPROM
  
  *   **Checksum:** A calculated value used to verify data integrity.
  *   **Generation:** A checksum is typically calculated by summing all the bytes in a block of data and then taking the two's complement of the sum (or a portion of it).
  *   **Verification:**  To verify the data, the same calculation is performed, *including* the checksum byte. If the result is zero, the data is likely to be correct.