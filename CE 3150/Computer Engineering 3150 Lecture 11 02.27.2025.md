### Arithmetic Instructions
#### Multiplication
  
  The AVR has dedicated instructions for performing multiplication. The handwritten notes showcase the core concept, but let's expand on it using information from Chapter 5 of the text book, specifically section 5.
  
  *   **`MUL Rd, Rr` (Multiply Unsigned)**
    *   Performs an 8-bit x 8-bit unsigned multiplication.
    *   Operands (`Rd` and `Rr`) must be general-purpose registers.
    *   Result (16-bit) is placed in registers `R1:R0`. `R1` holds the high byte, and `R0` holds the low byte.
    *   Flags Affected:
        *   `Z` (Zero flag): Set if the result is zero.
        *   `C` (Carry flag): Set if bit 15 of the result is set.
        *Handwritten note emphasizes to avoid overwriting R0 when loading in numbers to avoid overwriting data*
  
  *   **`MULS Rd, Rr` (Multiply Signed)**
    *   Performs an 8-bit × 8-bit signed multiplication.
    *   Operands must be registers R16-R31.
    *   Result is placed in R1:R0.
    * The handwritten example that is shown to multiply (+3) and (-5) with a result of 00000 (-15).
  
  *	**`MULSU Rd, Rr` (Multiply Signed Unsigned)**
  *	Multiply signed with unsigned.
  
  *	**FMUL, FMULS, FMULSU:**
  *	Fractional Multiplication variations.
  
  *	**Example from Handwritten Notes**
  ```assembly
  ;* DESCRIPTION
  ;* Signed fractional multiply of two 16-bit numbers with 32-bit result.
  ;* r19:r18:r17:r16 = ( r23:r22 * r21:r20 ) << 1
  
  fmuls16x16_32:
    clr   r2
    fmuls r23, r21    ; ((signed)ah * (signed)bh) << 1
    movw  r19:r18, r1:r0
    fmul  r22, r20    ;(al  * bl) << 1
    add   r18, r2
    movw  r17:r16, r1:r0
    fmulsu  r23, r20    ; ((signed)ah * bl) << 1
    sbc   r19, r2
    add   r17, r0
    adc   r18, r1
    adc   r19, r2
    fmulsu  r21, r22    ;((signed)bh * al) << 1
    sbc   r19, r2
    add   r17, r0
    adc   r18, r1
    adc   r19, r2
  ```
#### Division
  
  *   **No Dedicated Division Instruction:**  The AVR *does not* have a single instruction for division. Division must be implemented through repeated subtraction.
  
  *   **Handwritten Note Algorithm:**
    1.  Load the Dividend (number to be divided) and Divisor.
    2.  Initialize a Quotient to 0.
    3.  Repeatedly:
        *   Increment the Quotient.
        *   Subtract the Divisor from the Dividend.
        *   Branch back to step 3 if the Carry flag is NOT set (meaning no borrow occurred, subtraction was successful).
        *If there is a carry, then decrement the quotient (because the operation went too far).
        *Restore the number by adding the Denominator, and putting it in the remainder*
    4.  The Quotient now holds the integer result of the division.
    5.  The Remainder is left in the Dividend register (the original number).
  
  *   **Example (Handwritten Note):** Divide 75 by 10.
  ```assembly
    .DEF NUM=R20 ;
    .DEF DENOM=R21 ;
    .DEF QUOT=R22
    
    LDI NUM, 75
    LDI DENOM, 10
    CLR QUOT; QUOT = 0
  
  loop: INC QUOT
    SUB NUM, DENOM
    BRCC loop; if C=0
            ;no borrow
            ;so continue
            ;iteration too
            ;far
    DEC QUOT
    ADD NUM, DENOM ; get remainder
            ;in NUM
    
    HERE: JMP HERE
  ```
### Addition and Subtraction
  
  *   **Basic Instructions:**
    *   `ADD Rd, Rr`: Adds two registers.  `Rd ← Rd + Rr`
    *   `ADC Rd, Rr`: Adds two registers + Carry flag. `Rd — Rd + Rr + C`
    *   `SUB Rd, Rr`: Subtracts two registers. `Rd ← Rd - Rr`
    *   `SBC Rd, Rr`: Subtracts with Carry (Borrow). `Rd ← Rd - Rr - C`
    *   `SUBI Rd, K`: Subtracts an immediate value. `Rd ← Rd - K`
  * *SBCI Rd, K* -Substract Immediate with Carry
  * *SBIW Rdh:Rd1, K*- Substract Immediate from word.
  
  *   **Flags Affected (Important for Conditional Jumps):**
    *   `Z` (Zero Flag): Set if the result is 0.
    *   `C` (Carry/Borrow Flag): Set if there's a carry-out from the most significant bit (addition) or a borrow (subtraction).
    *   `N` (Negative Flag): Set if the result is negative (MSB is 1).
    *   `V` (Overflow Flag): Set if a signed arithmetic overflow occurs.
    *   `H` (Half Carry Flag): Set if there's a carry from bit 3 to bit 4.  Used in BCD arithmetic.
    *	`S`(Sign flag): N xor V
  
  * **Subtraction Details**
  The AVR, like most CPUs, uses the 2's complement method for subtraction. This is done internally. The steps the hardware implicitly follows are:
    1.  Take the 2's complement of the subtrahend (the number being subtracted).
    2.  Add the 2's complement to the minuend (the number being subtracted from).
    3.  Invert the carry flag.  *(This is crucial. AVR inverts the carry after a subtraction.)*
  
  * **Example (Handwritten Note):** 27h - 12h
  ```assembly
  LDI R16,0x27  
  LDI R17,0x12
  SUB R16, R17
  ```
  
    *   `27H` becomes `0010 0111` in binary.
    *   `12H` becomes `0001 0010` in binary.
    *   2's Complement of `12H`:  Invert bits (`1110 1101`) and add 1 (`1110 1110`).
    *   Addition: `0010 0111 + 1110 1110 = 1 0001 0101` (Carry = 1).
    *   AVR *Inverts* Carry: C becomes 0. Result is positive.
  
  * **Example (Handwritten Note):** R17-R19, where R17 = 0x62, and R19 = 0x96.
  ```assembly
  LDI R16,0x62  
  LDI R17,0x96
  SUB R16, R17
  ```
    *   0x62 is 0110 0010
    *   0x96 is 1001 0110
    *	2s compliment of 0x96
       0110 1001
    *	The result is then 1 1100 1100 (CCh), but a carry occurs, so C = 0.
  
  * **Example (Handwritten Note):**
  ```assembly
  LDI R16, 0x3B
  LDI R17, 0x2D
  SUB R16,R17
  ```
  
    *   `3BH` is `0011 1011`
    *	0x2D is 0010 1101
    * 2s complement of 0x2D is 11010010, +1. 1101 0011.
    *	The result is then 0000 1110, and carry = 1.
    *	This means that the actual C = 0.
  
  *	**Example (Handwritten):** Add the values of 0x2567 to those of location 0x248 and 0x249 of RAM.
  ```assembly
  . DEF Val Low = R16
  . DEF Val High = R17
    LDI Val Low, 0x67
    LDI Val High, 0x25
    LDS R18, 0x248 ; Loads R18 with the value on the memory address, does not support the immediate form of operation.
    LDS R19, 0x249 ; Loads R19 with the value on the memory address, does not support the immediate form of operation.
    ADD Val Low, R18 ; Add the low bytes together.
    ADDC Val High, R19 ; Adds the high bytes together, as well as the potential carry from the previous operation.
    STS 0x250, Val Low ; Stores the low result.
    STS 0x251, Val High; Stores the high result.
  ```
### Bit and Bitwise Manipulation
  *SBR, and CBR.*
  
  *   **`SBR Rd, K` (Set Bits in Register)**
    *   Sets specified bits in register `Rd` to 1.  Uses a mask `K`.  (`Rd ← Rd OR K`)
  
  *   **`CBR Rd, K` (Clear Bits in Register)**
    *   Clears specified bits in register `Rd`. Uses a mask `K`. (`Rd ← Rd AND (NOT K)`)
  
  *   **`SBI A, b` (Set Bit in I/O Register)**
    *   Sets a specific bit (`b`) in an I/O register (`A`) to 1.  *Only works on the lower 32 I/O registers*.
  
  *   **`CBI A, b` (Clear Bit in I/O Register)**
    *   Clears a specific bit (`b`) in an I/O register (`A`) to 0. *Only works on the lower 32 I/O registers*.
  
  *   **`SBIS A, b` (Skip if Bit in I/O Register is Set)**
    *   Skips the *next* instruction if bit `b` in I/O register `A` is 1.
  
  *   **`SBIC A, b` (Skip if Bit in I/O Register is Cleared)**
    *   Skips the *next* instruction if bit `b` in I/O register `A` is 0.
  * **Handwritten Note:** Examples of using `SBI` and `CBI`.
  ```assembly
  SBI PORTC, 3 ; PORTC.3 = 1 (Set)
  SBI 0x15,3 ; 3 equivalent
  
  CBI PORTC, 3; PORTC.3 = 0 (Clear)
  ```
  *Important: These work on I/O registers, not general-purpose registers.*
  *	**Example (Handwritten):**
    ```assembly
    SBIS PINC, 2 ; check to see if door is open
    RJMP HERE ; PC+1, continuously
    LDI R16,30 ; PC+2 
    SBI PORTC,3 ;
    Call Delay ;
    CBI PORTC, 3 ;
    Call Delay ;
    DEC R16 ; dec loop count
    BRNE loop
    RJMP Here; back to monitoring the
            ; door condition
    ```
    *  The code waits until pin 2 of port C is set high.
### I2C (Inter-Integrated Circuit)
  
  *   **Concept:** A serial communication protocol developed by Philips. Allows multiple devices to communicate over a shared 2-wire bus.
  *   **TWI (Two-Wire Interface):** The AVR's implementation of I2C.  Uses two pins:
    *   **SDA (Serial Data):**  Data transfer.
    *   **SCL (Serial Clock):**  Clock signal for synchronization.
  * **Registers:**
    *   **TWBR (TWI Bit Rate Register):**  Controls the SCL clock frequency.
    *   **TWSR (TWI Status Register):**  Provides status information about the TWI bus and current operation.
    *   **TWDR (TWI Data Register):**  Holds the data to be transmitted or the received data.
    *   **TWAR (TWI Address Register):**  Holds the 7-bit slave address of the AVR when it's acting as a slave.