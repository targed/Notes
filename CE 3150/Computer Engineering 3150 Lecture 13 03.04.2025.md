### I. Bit-Oriented Instructions (Handwritten Notes & Textbook Ch. 6)
  
  The handwritten notes emphasize the importance of bit manipulation in AVR assembly.  We'll break down the key instructions and concepts.
  
  *   **SREG (Status Register) and the T-Bit:**
  * The handwritten note specifically mentions the `T` bit in the SREG. This bit is used as a temporary storage location for bit operations.
  * The handwritten notes has this structure of SREG: I, T, H, S, V, N, Z, C.
  
  *   **Bit Manipulation Instructions:**
  
  *   **`SBI A, b` (Set Bit in I/O Register):** Sets bit `b` in I/O register `A` to 1.  *Only works on the lower 32 I/O registers (0-31)*.  The syntax, according to the textbook (pg. 698, Appendix A), is:
      ```assembly
      SBI P,b ; Set Bit in I/O Register
      ; I/O(P, b) ← 1
      ```
      Where:
          *   `P` is the I/O register address (0-31).
          *   `b` is the bit number (0-7).
  
  *   **`CBI A, b` (Clear Bit in I/O Register):** Clears bit `b` in I/O register `A` to 0. *Only works on the lower 32 I/O registers*. Syntax from textbook (pg. 698):
   ```assembly
      CBI P,b ;Clear Bit in I/O Register
      ;I/O(P, b) ← 0
      ```
  
  *   **`SBIS A, b` (Skip if Bit in I/O Register is Set):**  Skips the *next* instruction if bit `b` in I/O register `A` is 1.  *Only works on the lower 32 I/O registers*. Textbook (pg. 698):
      ```assembly
      SBIS P,b ; Skip if Bit in I/O Register is Set
      ; if (P(b)=1) PC ← PC + 2 or 3 else PC ← PC + 1
      ```
  
  *   **`SBIC A, b` (Skip if Bit in I/O Register is Cleared):** Skips the *next* instruction if bit `b` in I/O register `A` is 0. *Only works on the lower 32 I/O registers*. Textbook (pg. 698):
       ```assembly
      SBIC P,b ; Skip if Bit in I/O Register is Cleared
      ; if (P(b)=0) PC ← PC + 2 or 3 else PC ← PC + 1
      ```
  
  *   **`BRBS s, k` (Branch if Bit in SREG is Set):**  Branches to a relative address `k` if bit `s` in the SREG is set (1). (Textbook pg. 697).
    ```assembly
    BRBS s,k ;Branch if Status Flag Set
    ;if (SREG(s)=1) then PC<- PC+k+1
    ```
  
  *   **`BRBC s, k` (Branch if Bit in SREG is Cleared):** Branches to a relative address `k` if bit `s` in the SREG is cleared (0). (Textbook pg. 697).
   ```assembly
    BRBC s,k ;Branch if Status Flag cleared
    ;if (SREG(s)=0) then PC<- PC+k+1
    ```
  
  *   **Handwritten Note Example (Switch and LED):**
  ```assembly
  ; A switch(SW) is connected to pin PC0
  ; and an LED to pin PC3. Write a program
  ; to get the status of SW and send it to
  ; the LED.
  .ORG 0
  CBI DDRC,0 ; PCO input
  SBI DDRC, 3; PC3 output
  SBI  PORTC,0; pullup resistor
  
  AGAIN:SBIC PINCO,0 ; SW=0?
  CALL LEDON ; SW=1
  SBIS PINCO,0 ; SW=1?
  CALL LEDOFF
  RJMP AGAING
  
  .ORG 0x30
  LEDON: SBI PORTC,3
  RET
  
  .ORG 0x40
  LEDOFF: CBI PORTC,3
  RET
  ```
  
  *   **Explanation:**
      *   `CBI DDRC, 0`: Configures pin PC0 as input (switch).
      *   `SBI DDRC, 3`: Configures pin PC3 as output (LED).
      *    SBI PORTC,0 Pull-up resistor at PC0.
      *   `AGAIN` loop: Continuously checks the state of PC0.
      *   `SBIC PINCO, 0`:  Skip the next instruction if PC0 is clear (0, switch pressed, grounded).
      *   `CALL LEDON`: If PC0 is 0, call subroutine `LEDON`.
       *   `SBIS PINC0,0`: Skip if PC0 is 1.
       * `CALL LEDOFF`: If PC0 is 1, call subroutine `LEDOFF`.
      *   `LEDON` subroutine: Sets bit 3 of PORTC (`SBI PORTC, 3`), turning the LED ON.
      *   `LEDOFF` subroutine: Clears bit 3 of PORTC (`CBI PORTC, 3`), turning the LED OFF.
  
  *   **Handwritten Note: Bit Operators in C:**
  *    Modulus %
  *	  Bitwise AND &
  *	 Bitwise OR |
  *	 Bitwise XOR ^
  *	 Bitwise NOT ~
  *    Shift Left <<, the expression by the number of places on the right of the operation.
  *    Shift Right >>
  
  *   **Handwritten Note Example:**
  ```assembly
  LDI R16, 0b0111<<3
  ;R16 = 0b0111000
  LDI R16, 0x48;
  >> 4;
  ;R16 = 0x04
  ```
### II. Addressing Modes (Review and Expansion)
  The handwritten notes provide another example related to the addressing mode.
  
  *   **Handwritten Note Example (Data Memory Access):**
  
  ```asy
  ; Assume that data memory locations
    ; $160-$163 have hex data. Write a program
    ; to access these values and place the result
    ; (add)
    ; in locations $170 (LSB) and $171 (MSB)
    .EQU Low_Byte = 0x170
    .EQU High_Byte = 0x171
  
    .ORG 0
        LDI R17,4  ; # bytes to add
        LDI R18,0
        LDI R19,0  ; initialize 16 bit sum R19R18
        LD R20, x-
        LD R20,+x LDI XL,0x60
                LDI XH, 0x01; X=0x0160
    Next_Num: LD R20, Xt ; get data pointed to by
                ; x copy into R20
        ADD R18,R20  ; add to low byte sum
        BRCC No_Carry ; carry?
        INC R19 ; yes> inc.high byte of Sum
    No_Carry: DEC R17 ; dec # numbers to add
        BRNE Next_Num ; do for all the numbers
        ST Low_Byte, R18 ; store result
        ST High_Byte, R19
  ```
  
    *    Here X us used, which is the combination of R26 and R27.
    *   **Explanation:**
        *   This code adds four bytes of data located in data memory, starting at address `$160`.
        *   `LDI R17, 4`:  Loads the loop counter (4 bytes to add).
        *    LDI for R18 and R19, to initialize the sum.
        *   `LDI XL, 0x60` and `LDI XH, 0x01`: Initialize the X pointer (R27:R26) to 0x0160.
        *   `Next_Num` Loop:
            *   `LD R20, X+`: Loads the byte pointed to by X into R20 and *post-increments* X (important!).  This is the **Register Indirect with Post-increment** addressing mode.
            *   `ADD R18, R20`: Adds the byte to the low byte of the sum (R18).
            *   `BRCC No_Carry`: Branches if the Carry flag is *clear* (no carry occurred).
            *   `INC R19`: If there was a carry, increment the high byte of the sum (R19).
            *   `DEC R17` and `BRNE Next_Num`: Decrement the counter and loop if not zero.
        *   `ST Low_Byte, R18` and `ST High_Byte, R19`:  Store the low and high bytes of the sum to memory locations Low_Byte (0x170) and High_Byte (0x171).
  
  *   **Key Addressing Mode Concepts:**
    *    **Post-increment (`X+`, `Y+`, `Z+`):** The register pair (X, Y, or Z) is incremented *after* the memory access.
    *   **Pre-decrement (`-X`, `-Y`, `-Z`):** The register pair is decremented *before* the memory access.
    *   **Direct Addressing:** Uses a 16-bit address directly in the instruction (e.g., `LDS`, `STS`). Limited to the 64K data space.
    *   **Indirect Addressing:** Uses a register pair (X, Y, or Z) as a pointer to the memory location.
  
  ### III. Program Memory (Code Memory) Access
  
  *   **`LPM` (Load Program Memory):** Used to access data stored in the program memory (Flash ROM).
  
  *   **Handwritten Note:**
    ```assembly
    LPM R16, Z ; load value pointed to by
        ; Z from program memory
    LPM R16, Z+
    ; Used for accessing data in look-up tables
    ```
    *   The `Z` register (R31:R30) is used as a pointer into program memory.
    *   `LPM Rd, Z`: Loads a byte from the program memory location pointed to by Z into register `Rd`. Z remains unchanged.
    *   `LPM Rd, Z+`: Loads a byte from program memory and then *post-increments* Z. This is very useful for sequentially accessing data stored in tables.
  
  * **Handwritten Note and Figure 6-13b**: This section delves into the addressing of the program memory.
  
    *   **16-bit Code Memory:** Each location in program memory holds *two* bytes (a 16-bit word).  This is important!
    *   **Z Register Addressing:**  The Z register (R31:R30) is used as a pointer.
    *   **Byte Addressing:** The Z register actually addresses *bytes* within program memory, *not words*.
    *   **LSB of Z:** The Least Significant Bit (LSB) of the Z register determines whether you access the *low byte* (LSB = 0) or the *high byte* (LSB = 1) of a given word in program memory.
  
    *   **Handwritten Example (from the figure):**
        ```
        Low     High     Address
        0000     0000     0000 0000 0000  $0000   $0000
        0000     0000     0000 0001  $0001   $0000
        0000     0000     0000 0010  $0002   $0001
        0000     0000     0000 0011  $0003   $0001
        0000     0000     0000 0100  $0004   $0002
        ...
        0000     0000     0000 1010  $000A   $0005
        ...
        0000     0000     0011 1100  $003C   $001E
  
        1111     1111     1111 1100 $FFFC    $7FFE
        1111     1111     1111 1101 $FFFD    $7FFE
        1111     1111     1111 1110 $FFFE    $7FFF
        1111     1111     1111 1111 $FFFF    $7FFF
        ```
        *   Notice how the Low/High columns alternate.  When Z is even (LSB = 0), you're accessing the low byte of a word.  When Z is odd (LSB = 1), you're accessing the high byte.
        *   The "Address" column on the right shows the *word* address (what you'd logically think of). The "Low" and "High" columns show the actual *byte* addresses.
  
  *   **Handwritten Example with `.DB` and `LPM`:**
  
    ```assembly
    .ORG $000
    LDI R16,0xFF 
    OUT DDRC,R16 ; PORTC output port
    LDI R17,13 ; # characters to send to PORTC
    LDI ZL, LOW(Message<<1);
    LDI ZH, HIGH (Message<<1); Z=0110 0000 0000 0000
                               ;              16 8 4 2 0
                                            ;message location
  
    NOTDONE: LPM R16,Z+ ;load current character, increment Z
    OUT PORTC, R16 ;output current character to PORTC 
    DEC R17 ;decrement character count 
    BRNE NOTDONE ;get all characters output 
    ; to PORTC
  
    .ORG 0x300
    Message: .DB "Upcoming Test"
    ```
  
    *   `.ORG 0x300`:  This directive tells the assembler to place the following data starting at program memory address 0x300 (word address).
    *   `.DB "Upcoming Test"`: This directive defines bytes.  Each character in the string will be placed in a *byte* of program memory.
    *   The "Message<<1", multiplies the address of the label with 2. This is an important concept to grasp.
    *   The program uses `LPM R16, Z+` to fetch each character, then sends it to PORTC, effectively displaying the string.
  
  ### IV. Data Conversion: BCD and ASCII
  
  *   **BCD (Binary Coded Decimal):**  A way of representing decimal digits (0-9) using 4 bits.
    *   **Unpacked BCD:** One BCD digit per byte. The upper nibble is usually 0.  Example:  `0x04` represents the digit 4.
    *   **Packed BCD:** Two BCD digits per byte.  Example: `0x47` represents the digits 4 and 7.
  * **Handwritten Example (BCD):**
    ```
        Unpacked BCD
  - lower 4 bits represents the BCD Value
    0x04 => 0b0000 0100
               4
    packed BCD
    byte Upper Lower
        nibble nibble
        BCD BCD
    0x47=> 0100 0111
    ```
    *   **ASCII (American Standard Code for Information Interchange):**  A standard way of representing characters (letters, numbers, symbols) as numbers. See Appendix F in the textbook for a complete table.
    
    *   **Conversion:**
    *   **Packed BCD to ASCII:**
    1.  Separate the two BCD digits (unpack).
    2.  Add `0x30` to each unpacked BCD digit.  This is because the ASCII codes for digits '0' through '9' start at 0x30.
    *   **ASCII to Packed BCD:**
    1.  Subtract `0x30` from each ASCII digit to get the unpacked BCD.
    2.  Combine the two unpacked BCD digits into a single packed BCD byte.
    
    *	**Handwritten Note (BCD and ASCII conversion):**
    ```
    0	0000
    1	0001
    2	0010
    3	0011
    4	0100	
    5	0101
    6	0110
    7	0111
    8	1000
    9	1001
    ```
    
    These notes now provide a much more detailed and connected understanding of the concepts presented in Lecture 11. I've linked them to the specific textbook chapters and provided detailed explanations for the handwritten examples.  The combination of code snippets, explanations, and connections to the broader theory should make this a very useful study resource.