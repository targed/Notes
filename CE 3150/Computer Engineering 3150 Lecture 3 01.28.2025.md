## Review of Address Decoding
  
  From the previous lecture, we learned how to decode addresses to generate chip select (CS) signals for memory and I/O devices.
  
  **Example: (Address Decoding Continued)**
  
  Target Address: 0000h - 1FFFh
  
  Address Table:
  
  | A_15 | A_14 | A_13 | A_12 | A_11 | A_10 | A_9 | A_8 | A_7 | A_6 | A_5 | A_4 | A_3 | A_2 | A_1 | A_0 |
  | :------------: | :------------: | :------------: | :------------: | :------------: | :-------------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: | :-----------: |
  |       0       |       0       |       0       |       0       |       0       |        0        |      0       |      0       |      0       |      0       |      0       |      0       |      0       |      0       |      0       |      0       |
  |       0       |       0       |       0       |       1       |       1       |        1        |      1       |      1       |      1       |      1       |      1       |      1       |      1       |      1       |      1       |      1       |
  
  Size of memory:
  
  A_12 - A_0 = 13 Address Lines.
  
  $2^{13} = (2^{10})^3 = 1K \cdot 8 = 8K$ Words
  
  Here is a diagram of how to hook up the 74138 for this example.
  
  ![image of 74138 wired for an address space of 0000h - 1FFFh.](https://i.imgur.com/qHh5Y33.png)
  
  **Example (Memory Capacity)**
  
  Given a 4M x 4 DRAM, find the number of address lines and data lines.
  
  *   **Capacity:** 4M x 4 = 16M bits.
  *   **Address Lines:** 4M = $2^{22}$, so there are 22 address lines (pins).
  *   **Data Lines:** 4 data lines (bits/pins).
  
  **Example (More Address Decoding)**
  What is the address range of ROM device with respect to the processor?
  
  |      CE       |                      ROM                      |
  |:-------------:|:---------------------------------------------:|
  | A_15 | 0 1 0 0 \| 0 0 0 0 0 0 0 0 0 0 0 0 |
  | start        | 0 1 0 0 \| 1 1 1 1 1 1 1 1 1 1 1 1 |
  |   stop     |                                               |
  
  *Start 0x4000*
  *stop 0x4FFF*
  
  Address range = 0x4000-0x4FFF
  
  **Review of 2's complement**
  
  What is the decimal value for the following binary bit pattern:
  D = 1101
  $$ -D => 1101 $$
  $$ 0010 $$
  $$ \frac{+0001} {0011} $$
  
  $$ 2FFFF0h $$
  $$ FFFFFFh \Rightarrow +0000\,0000\,0000\,0000\,0001 $$
  $$ + 0011\, 0000\,0000\,0000\,0000 $$
  $$ 0000\,0000\,0000\,0000\,0000\,0001 = 30000h $$
  
  ***
## AVR Microcontroller Overview
  
  *   **Origin:** Developed by Alf Bogen and Vegard Wollen at the Norwegian Institute of Technology (NTH). Now produced by Atmel (acquired by Microchip).
  *   **Architecture:**  RISC (Reduced Instruction Set Computer). This contrasts with CISC (Complex Instruction Set Computer).
  *   **Introduction:** Introduced in 1996.
  
  *   **Characteristics:**
    *   8-bit microcontroller (with AVR32 being a notable 32-bit exception).
    *   Not 100% software compatible across family members (may require register location adjustments when porting code).
    *   Commonly used types: ATmega32, ATmega324pb.
  
  *   **AVR Family Categories:**
    1.  **Mega:** Larger memory, more features.
    2.  **Tiny:** Smaller size, less power consumption, limited features.
    3.  **Classic:** Older, less commonly used in new designs.
    4.  **Special Purpose:**  Specialized functionalities like USB, CAN, LCD controllers.
  
  *   **Simplified View:**  (Simplified block diagram of an AVR microcontroller, similar to Figure 1-2 in the textbook)
  
  *   **Block Diagram:** (Detailed block diagram of the ATmega32, similar to Figure 1-4 in the textbook. )
  
  *   **Harvard vs. von Neumann Architecture:**
    *   **von Neumann:** Single address space for both code (instructions) and data.
    *   **Harvard:** Separate address spaces for code and data. AVR uses Harvard architecture, improving speed and efficiency.
    *   *Figure 0-20 von Neumann vs. Harvard Architecture: A visual comparing the two architectures. The key point being shared vs separate data and instruction paths.*
  
  *   **Comparison with 8051 and PIC18:** (Refer to Tables 1-3, 1-6 and 1-7 in the textbook.)
  
  *   **Choosing a Microcontroller:**
  
    1.  **Resources Available:**  Availability of development tools, libraries, community support.
    2.  **Software Development Tools:**  Assemblers, compilers (especially C compilers), debuggers, simulators, emulators (e.g., Atmel Studio 7.0, xSimulator, monitor, emulator, Keil).
    3.  **Cost-Effectiveness and Meeting Application Specifications:**  Considering factors like speed (e.g., 40 MHz for ASIC, 20 MHz for ATmega32), power requirements, memory (RAM/ROM), and data packaging.
  
  **AVR Studio Simulator Example**
  There is a simulator that is included with AtmelStudio:
  
  ![image of AVR studio simulator](https://i.imgur.com/Qj9vY7S.png)
- Okay, I'll expand on the AVR instruction set and register usage, adding to the existing Lecture 3 notes. I'll also make sure to include suggestions for further elaboration at the end of future note sets.
## AVR Instruction Set Architecture (ISA)
  
  The AVR instruction set is designed to be efficient and relatively easy to use for assembly language programming. It's a RISC architecture, meaning most instructions execute in a single clock cycle.
  
  **Key Features of the AVR Instruction Set:**
  
  *   **Load/Store Architecture:** Most arithmetic and logic operations are performed on registers. Data must be loaded from memory into registers before being processed, and results are stored back into memory from registers.
  *   **Mostly 16-bit Instructions:** The majority of AVR instructions are 16 bits (2 bytes) wide. There are a few 32-bit (4-byte) instructions (like `LDS` and `STS` for direct memory access and `JMP` and `CALL` for long jumps/calls).
  *   **Harvard Architecture:** Separate memory spaces for instructions (program memory, Flash) and data (SRAM, EEPROM, registers). This allows simultaneous instruction fetch and data access.
  *   **Fixed Instruction Length:** Almost all instructions have a consistent format, making decoding simple and fast.
  *   **Orthogonality:** Instructions are relatively orthogonal, meaning that you can generally use any register with any instruction that operates on registers.  There are *some* restrictions, but it's more consistent than, say, the 8051.
  
  **Instruction Categories:**
  
  *   **Data Transfer:** Move data between registers, memory, and I/O ports.
    *   `MOV Rd, Rr`: Move data between registers.
    *   `LDI Rd, K`: Load immediate value (constant) into a register.
    *   `LDS Rd, k`: Load direct from SRAM.
    *   `STS k, Rr`: Store direct to SRAM.
    *   `LD Rd, X/Y/Z`: Load indirect using X, Y, or Z register as a pointer.
    *    `LD Rd, X+/Y+/Z+`: Load indirect with post-increment.
    *    `LD Rd, -X/-Y/-Z`: Load indirect with pre-decrement.
    *   `IN Rd, A`: Load from I/O port.
    *   `OUT A, Rr`: Output to I/O port.
    *   `PUSH Rr`: Push register onto the stack.
    *   `POP Rd`: Pop value from stack into register.
    *   `LPM Rd, Z`: Load program memory (used for accessing data stored in the Flash memory).
  
  *   **Arithmetic and Logic:** Perform calculations and bitwise operations.
    *   `ADD Rd, Rr`: Add two registers.
    *   `ADC Rd, Rr`: Add with carry.
    *   `SUB Rd, Rr`: Subtract two registers.
    *   `SBC Rd, Rr`: Subtract with carry.
    *   `SUBI Rd, K`: Subtract immediate.
    *   `SBCI Rd, K`: Subtract immediate with carry.
    *   `AND Rd, Rr`: Logical AND.
    *   `OR Rd, Rr`: Logical OR.
    *   `EOR Rd, Rr`: Exclusive OR.
    *   `COM Rd`: One's complement (invert bits).
    *   `NEG Rd`: Two's complement (negate).
    *   `INC Rd`: Increment.
    *   `DEC Rd`: Decrement.
    *   `MUL Rd, Rr`: Multiply (unsigned).
    *  `MULS Rd, Rr`: Multiply (signed)
    *   `LSL Rd`: Logical Shift Left.
    *   `LSR Rd`: Logical Shift Right.
    *   `ASR Rd`: Arithmetic Shift Right.
    *   `ROL Rd`: Rotate Left through Carry.
    *   `ROR Rd`: Rotate Right through Carry.
    *  `SWAP Rd`: swap nibbles (4 bits)
  
  *   **Branch and Control:** Change the flow of program execution.
    *   `RJMP k`: Relative jump.
    *   `IJMP`: Indirect jump (using Z register).
    *   `CALL k`: Call subroutine.
    *   `RCALL k`: Relative call.
    *   `RET`: Return from subroutine.
    *   `RETI`: Return from interrupt.
    *   `CP Rd, Rr`: Compare.
    *   `CPI Rd, K`: Compare with immediate.
    *   `SBRS Rd, b`: Skip if bit in register set.
    *   `SBRC Rd, b`: Skip if bit in register cleared.
    *   `SBIS P, b`: Skip if bit in I/O register set.
    *   `SBIC P, b`: Skip if bit in I/O register cleared.
    *   Conditional Branch Instructions: `BREQ`, `BRNE`, `BRSH`, `BRLO`, `BRGE`, `BRLT`, etc. (branch based on flags in the status register).
  
  *   **Bit Manipulation:** Work with individual bits within registers and I/O ports.
    *   `SBI P, b`: Set bit in I/O register.
    *   `CBI P, b`: Clear bit in I/O register.
    *   `SBR Rd, k`: Set bits in register (using a mask).  (This is actually a synonym for ORI with a specific mask.)
    *   `CBR Rd, k`: Clear bits in register (using a mask). (This is a synonym for ANDI with an inverted mask)
    *   `BST Rd, b`: Bit store from register to T flag.
    *   `BLD Rd, b`: Bit load from T flag to register.
    *   `BSET s`: Set a flag in the SREG (status register).  (e.g., `BSET 6` sets the I flag for global interrupts).
    *   `BCLR s`: Clear a flag in the SREG.
  
  *   **MCU Control:** Instructions related to the microcontroller's operation.
    *   `NOP`: No operation (used for timing delays).
    *   `SLEEP`: Enter sleep mode.
    *   `WDR`: Watchdog timer reset.
    *   `BREAK`: For on-chip debugging.
## AVR Register Usage
  
  The AVR architecture has 32 general-purpose 8-bit registers, named R0 through R31. These registers are directly connected to the ALU (Arithmetic Logic Unit), allowing for fast operations.
  
  *   **General-Purpose Registers (R0-R31):**
    *   All registers can be used for most arithmetic and logic operations.
    *   Registers R16-R31 are typically preferred for `LDI` (load immediate) instructions.
    *   Registers R0 and R1 are used as the destination for multiplication results.
    * Registers are divided up into different groupings to serve as the 16 bit registers X, Y, Z.
          X-register: R26 (XL), and R27 (XH)
          Y-register: R28(YL), and R29(YH)
          Z-register: R30(ZL), and R31(ZH)
  
  *   **Special Function Registers (SFRs):** These registers are located in the I/O space and control various aspects of the microcontroller's peripherals and operation (timers, interrupts, serial communication, etc.).  Examples include:
    *   **SREG (Status Register):** Contains flags that reflect the result of arithmetic and logic operations (Zero flag, Carry flag, Negative flag, Overflow flag, Half-carry flag, etc.).
    *   **SP (Stack Pointer):**  A 16-bit register that points to the top of the stack (in SRAM).
    *   **PORTx (Data Output Registers):** Used to write data to I/O pins.
    *   **DDRx (Data Direction Registers):** Control the direction (input/output) of I/O pins.
    *   **PINx (Port Input Registers):** Used to read data from I/O pins.
    *   **TCNTx (Timer/Counter Registers):** Hold the current count value for timers.
    *   **TCCRx (Timer/Counter Control Registers):** Control the operation of timers.
    *   **OCRnx (Output Compare Registers):** Used for PWM and timer compare operations.
    *   **UBRR (USART Baud Rate Register):**  Used to configure the baud rate for serial communication.
    *   **UCSR (USART Control and Status Register):**  Controls and monitors the USART (serial port).
    *   **SPDR (SPI Data Register):**  Used for SPI (Serial Peripheral Interface) communication.
    *   **SPSR (SPI Status Register):**  Provides status information for SPI.
    *   **SPCR (SPI Control Register):**  Configures the SPI interface.
    *  **TWBR (Two-Wire Bit Rate Register):** Sets the frequency for I2C.
    * **TWSR(Two-Wire Status Register):** Provides the status of the I2C module.
    * **TWCR(Two-Wire Control Register):** Enable bits for I2C.
    * **TWDR(Two-Wire Data Register):** Contains the address and data for I2C.
    * **TWAR(Two-Wire Address Register):** Contains the address of the slave I2C device.
  
  
  *   **Register Pairs (16-bit Registers):** Some operations require 16-bit values. The AVR uses register pairs to represent 16-bit data.  Common pairs include:
    *   **X register (R27:R26):** Often used as a pointer for indirect addressing in data memory.
    *   **Y register (R29:R28):**  Another pointer for indirect addressing.
    *   **Z register (R31:R30):** Used as a pointer for indirect addressing, especially for accessing program memory (LPM instruction). Also used for indirect jumps and calls.
  
  **Example (using registers):**
  
  ```assembly
  ; Add two 16-bit numbers
  LDI R16, 0x12  ; Load low byte of first number into R16
  LDI R17, 0x34  ; Load high byte of first number into R17
  LDI R18, 0x56  ; Load low byte of second number into R18
  LDI R19, 0x78  ; Load high byte of second number into R19
  
  ADD R16, R18  ; Add low bytes
  ADC R17, R19  ; Add high bytes with carry
  ```
  
  This example adds two 16-bit numbers stored in R17:R16 and R19:R18. The result is stored in R17:R16.
  
  **Stack Pointer (SP):**
  
  The stack pointer (SP) is a 16-bit register that points to the top of the stack. The stack is a region of SRAM used for temporary data storage (e.g., for subroutine calls, interrupt handling, and saving registers).
  
  *   **PUSH Rr:** Decrements the SP and then stores the value of Rr onto the stack.
  *   **POP Rd:** Loads the value from the top of the stack into Rd and then increments the SP.
  
  The AVR stack grows *downward* in memory. This is important to remember when initializing the stack pointer.
  
  **AVR I/O Space:**
  Access to ports, and register settings