- **Lecture 10: AVR Assembly - I/O Ports, Addressing, and Timers**
## **I. I/O Port Structure (Chapter 4 and Appendix C)**
  
  *   **AVR I/O Ports:** The ATmega32 (and other AVRs) provides multiple I/O ports (PORTA, PORTB, PORTC, PORTD) for interacting with external devices.
  
  *	All I/O ports are bidirectional.
  
  *   **Three Registers per Port:** Each port (e.g., PORTA) has three associated 8-bit registers:
      1.  **DDRx (Data Direction Register):** Configures the direction of each pin (input or output).
          *   `DDRx = 0`: Configures the corresponding port pin as an **input**.
          *   `DDRx = 1`: Configures the corresponding port pin as an **output**.
          *   The default value upon the reset is that all the bits are going to have a 0.
      2.  **PORTx (Port Register):**
          *   **When used as output:** Writing a value to PORTx sets the output level of the corresponding pin (HIGH or LOW).
          *   **When used as input:** Setting a bit in PORTx to '1' enables the internal pull-up resistor for the corresponding pin.
      3.  **PINx (Pin Input Register):** Reads the actual voltage level present on the corresponding port pin.
  
  *   **Bit Addressability:** AVR ports can be accessed both byte-wise (all 8 bits at once) and bit-wise (individual bits).
  
  *	The I/O registers are mapped in the memory. The memory location is between $00 and $1F.
  *	All these register are 8 bits.
  
  *   **Pull-Up Resistors:** AVR I/O pins have internal pull-up resistors. These can be enabled by setting the corresponding bit in the PORTx register *when the pin is configured as input*.
  
  * **Important concept:** An I/O port is made up of: data direction, a switch, the pin, and a pull-up resistor.
  
  * **Reading from a Pin (Input)**
     1.  Make the pin an input: `DDRx.n = 0` (where `n` is the pin number).
          *This will set the corresponding pin to high impedance, because the switch between the I/O pin and the system is set to off.*
      2.  Read the value from the `PINx` register.
  
  *   **Writing to a Pin (Output)**
      1.  Make the pin an output: `DDRx.n = 1`.
          *   This will allow the output to go to the pin.
      2.  Write the desired value (0 or 1) to the corresponding bit in the `PORTx` register.
  
  *   **Handwritten Note Elaboration:** The handwritten note emphasizes the crucial step of setting the `DDRx` register to configure the port direction *before* attempting to read from or write to the port. Failing to do so can result in damage or incorrect behavior. Also, the `PORTx` controls the internal pull-up resistor, so when all of the I/O register is configured as an input, it must have 1 bits to turn on the internal pull-up resistor, and 0 bits will disable the pull-up resistor.
##  **II. Addressing Modes (Chapter 6)**
  :LOGBOOK:
  CLOCK: [2025-03-08 Sat 16:14:24]
  :END:
  
  *   **Register Direct:** Uses General Purpose Registers (GPRs, R0-R31).
  *Example: "ADD R16, R17", R16 acts as the destination and R17 is where it is getting added from.*
  *	There are some special instruction that use this mode:
  	*	"LDI"- Load Immidiate.
  	*	"ANDI"- And imidiate.
  	*	"SUBI"- Substract imidiate.
  * **Indirect:** Uses X, Y, or Z registers as pointers to data memory locations.
      *   **X Register:** R26 (XL) and R27 (XH)
      *   **Y Register:** R28 (YL) and R29 (YH)
      *   **Z Register:** R30 (ZL) and R31 (ZH)
  
  *	Example:
  ```assembly
  LDI XL, 0x30	;load R26 (the low byte of x) with 0x30
  LDI XH, 0x01	;load R27 (the high byte of X) with 0x1
  LD R18, X 	;copy the contents of location 0x130 to R18
  ```
  
  *   **Immediate:** The operand is a constant value.
  	*Example: "LDI R29, 0x20"*
  
  *   **Auto-increment/decrement:**  Available with indirect addressing using X, Y, Z.
      *   `LD Rd, X+` (Post-increment)
      *   `LD Rd, -X` (Pre-decrement)
      *   `LDD Rd, Y+q` (Indirect with displacement)
  
  * **Example (Auto-increment):**
     ```assembly
      LDI    R16, 0x5        ;R16 = 5 (R16 for counter)
      LDI    R20,0x55      ;load R20 with value 0x55 (value to be copied)
      LDI    YL, 0x40       ;load YL with value 0x40
      LDI    YH, 0x1		 ;load YH with value 0x1
    L1:   ST     Y+,R20       ;copy R20 to memory pointed to by Y
      DEC    R16         ;decrement the counter
      BRNE   L1          ;loop while counter is not zero
    ```
  ## **III. Timers (Chapter 9)**
  
  *   **Timer/Counter Registers:**
      *   `TCNTx`: Timer/Counter register (e.g., TCNT0, TCNT1, TCNT2). This is where the count is stored.
      *   `TCCRx`: Timer/Counter Control Register.  Configures the timer's mode of operation, clock source, and prescaler.
      *   `OCRx`: Output Compare Register.  Used in CTC (Clear Timer on Compare Match) mode.
  *   **Timer Modes:**
      *   **Normal Mode:**  Counts up to the maximum value (0xFF for 8-bit timers, 0xFFFF for 16-bit timers) and then overflows.
      *   **CTC (Clear Timer on Compare Match) Mode:** Counts up to a value specified in the `OCRx` register, then resets.
  * **Using Timers for Delays**
      
      *   **Concept:**  We can create precise time delays by configuring the timer to count at a known frequency (determined by the system clock and the prescaler) and then waiting for a specific number of counts.
      *   **Calculation:**
  
          1.  **Timer Clock Period:**
             $T_{clock} = \frac{1}{F_{timer}}$
  
             where $F_{timer}$ is the frequency of the clock.
  
          2.  **Total Delay:**
             $T_{delay} = T_{clock} \times Number of Counts$
          
      *   **Example (Handwritten Note):** Create a 2 kHz square wave on PORTA, bit 1. Assume $F_{osc} = 16 MHz$.
          1.  **Desired Period (T):**
             $T = \frac{1}{F} = \frac{1}{2\text{kHz}} = 0.5 \text{ms}$
  
          2.  **Half Period (for 50% duty cycle):**
          $T = 0.5 / 2 = 250 \mu s$
  
          3.  *Set DDR and enable pull-up resistor:*
  
          ```assembly
          LDI R16, 0xFF
          OUT DDRA, R16 ; making all bits PORTA output (only need PA.1)
          ;SBI DDRA, 1 (PA.1)
          ```
      4.  
           ```assembly
           main program 
           .ORG 0
           LDI R16, 0XFF 
           OUT DDRA, R16 ; making all bits PORTA output (only need PA.1) 
           ; SBI DDRA, 1(PA.1) 
           ;* MCS loop: SBI PORTA, 1 ; PORTA.I = 1 
           2 RCALL Delay ; a 
           NOP 
           CBI PORTA, 1 ; PORTA.1 = 0 
           RCALL Delay i> 
           RJMP loop
           ```
  
          *   **Clock Cycles Calculation:**  The handwritten note provides an assembly code snippet, and then derives the required clock cycles for the delay routine.  Key instructions and their cycle counts are:
              *   SBI (Set Bit in I/O register): 2 cycles
              *   CBI (Clear Bit in I/O register): 2 cycles
              *   RCALL (Relative Call): 3 cycles
              *   NOP (No Operation): 1 cycle
              *   RJMP (Relative Jump): 2 cycles
          *   Based on the code, the total cycles *outside* the delay loop would be $2 + 3 + 1 + 2 + 3 + 2 = 13$
          
          * The delay loop formula is (3 x X) - 1 + 4 = 12604
              *  LDI takes 1 cycle
              *  DEC takes 1 cycle.
              *  BRNE (if not zero) takes 2 cycles, (if zero) takes 1 cycle.
              *  RET takes 4 cycles.
  
          * The author did not make the equation completely, and did not solve for X.
  
  **IV. Key Takeaways**
  
  *   **AVR I/O is Memory-Mapped:** The I/O registers are accessed as if they were memory locations.
  *   **Bit Manipulation is Key:**  The ability to set, clear, and test individual bits is crucial for efficient I/O control.
  *   **Timers Provide Flexibility:** AVR timers can be used for time delays, event counting, and waveform generation (including PWM).
  *   **Understanding Clock Sources:**  The system clock frequency and prescaler settings are fundamental to calculating timer delays and frequencies.
  *   **Interrupts for Efficiency:** Interrupts allow the AVR to respond to events without constantly polling, freeing up the CPU for other tasks.
  *   **Context Saving:** When working with interrupts or subroutines, be mindful of saving and restoring register values (including the SREG - Status Register) to avoid unexpected behavior.
  * Always enable a bit when wanting to write, and then clear that bit.