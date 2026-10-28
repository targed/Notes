- **Lecture 14: AVR Timers - Waveform Generation and Input Capture**
  
  **I. Review of Timer/Counter Basics (Chapter 9)**
  
  *   **Timers vs. Counters:**
      *   **Timers:** Measure time intervals.  They are incremented by a predictable clock source (usually derived from the system clock).
      *   **Counters:** Count events. They are incremented by an external signal.
  
  *   **AVR Timers:** The ATmega32 has three timers:
      *   Timer0: 8-bit
      *   Timer1: 16-bit
      *   Timer2: 8-bit
  
  *   **Key Registers:**
      *   `TCCRn` (Timer/Counter Control Register): Configures the timer's operating mode, clock source (prescaler), and waveform generation behavior.  (e.g., `TCCR0`, `TCCR1A`, `TCCR1B`, `TCCR2`)
      *   `TCNTn` (Timer/Counter Register): Holds the current count value. (e.g., `TCNT0`, `TCNT1`, `TCNT2`).  For 16-bit timers, this is split into high and low bytes (e.g., `TCNT1H`, `TCNT1L`).
      *   `OCRn` (Output Compare Register): Used in CTC and PWM modes to define a compare match value. (e.g., `OCR0`, `OCR1A`, `OCR1B`, `OCR2`).
      *   `ICRn` (Input Capture Register): Used by Timer 1's Input Capture feature. (e.g. ICR1H, ICR1L)
      *    `TIFR` (Timer/Counter Interrupt Flag Register): the general register that has all of the interrupt flags.
      *   `TIMSK` (Timer/Counter Interrupt Mask Register): the general register used to enable the interrupt modes.
  
  *    The handwritten note emphasizes that Timer/Counter applications commonly involve either of two categories.
      1.  Detecting/counting external events.
      2.  Generating time periods or time delays.
  
  *   **Clock Sources:**
      *   **Internal Clock:**  Derived from the system crystal oscillator (Fosc). The frequency can be prescaled (divided).
      *   **External Clock:**  An external signal applied to a specific pin (e.g., T0 for Timer0, T1 for Timer1).
### Wave Generation (Textbook Chapters 9, 15, and 16)
  
  *   **Goal:** To produce square waves, pulses, or PWM (Pulse Width Modulation) signals.
  
  * **Handwritten note:** The goal is to create a 2KHz square wave using PORTA, bit 1. and Fosc of 16MHz.
  
  *	First, the time delays are needed.
  *T = 1/F.
  
    *   **Normal Mode (Chapter 9):**
        *   The timer counts up from 0 to its maximum value (0xFF for 8-bit, 0xFFFF for 16-bit).
        *   When it overflows (rolls over from the maximum value back to 0), the `TOVn` flag (Timer Overflow Flag) is set.
        *   Can be used for simple time delays, but the frequency is fixed unless the prescaler is changed.
  
    *   **CTC Mode (Clear Timer on Compare Match):**
        *   The timer counts up.
        *   When `TCNTn` matches the value in `OCRn`, the timer is cleared (reset to 0), and the `OCFn` flag (Output Compare Flag) is set.
        *   Allows for more flexible frequency control, as the "top" value is defined by `OCRn`.
  
    *   **PWM Modes (Fast PWM, Phase-Correct PWM):**  (Covered in more detail in Chapter 16, but introduced here).
        *   Used for generating variable-width pulses.
        *   Different PWM modes offer different characteristics (e.g., frequency, phase, resolution).
  
  *	**Handwritten Note**: The notes have some calculations and AVR Assembly instructions for generating a 2KHz square wave, which is solved in the next section.
### Solving the Handwritten Note's Example
  ```assembly
  ; Generate a 2KHz Square wave on
  ; PortA,bit1. Assume Fosc = 16MHz
  
  ; Main program
  .ORG 0
    LDI R16,0xff
    OUT DDRA, R16; making all bits PORTA
                    ; output (only need PA.1)
                    ; SBI DDRA, 1 (PA.1)
  ; *
  loop:   SBI PORTA, 1 ; PORTA.1 = 1
    RCALL Delay ; 
    NOP
    CBI PORTA, 1; PORTA.1 = 0
    RCALL Delay ;
    RJMP loop
  ```
  
  * In order to get the period for each cycle: T =1/F, then we get T = .5 ms.
  * Using 50% duty cycle for the pulse, we would need .25 ms for high and .25 ms for low.
  * If we were to use delay, we would write Delay = .25 ms.
  * Here in the assembly code we determine the clocks needed for a delay, in this case.
  * We set the MC time equal to 1/16,000,000.
  * Delay = .25 x 10^(-3) 
  * MC time = 1/ 16 x 10^6.
  * Then we want to calculate the amount of machine cycles. n = (.25 x 10^-3) / (1/16 x 10^6). n = 4000.
  * Next, the assembly code is to implement this delay, but in terms of clock cycles. The code is shown below.
  ```assembly
  ;Delay Routine
  ; .ORG 0x100
  ;Delay: LDI R18, 200
  ;Loop1: NOP
  ;	NOP
  ;	DEC R18
  ;	BRNE Loop1
  ;	RET
  ```
  
  * The assembly instruction above shows how to create a delay in which instructions such as NOP are used to delay for clock cycles.
### AVR Programming - Wave Generation and Input Capture
  
  *   **Key Registers:**
    *   `TCCRO`
  
  *   **Bit Manipulation (Handwritten Note):**
    *   `SBI PORTC, 2` ; Set bit 2 of PORTC (make it HIGH)
    *   `CBI PORTC, 2` ; Clear bit 2 of PORTC (make it LOW)
    *   `SBIS PINC, 2` ; Skip the next instruction if bit 2 of PINC is SET (HIGH)
    *   `SBIC PINC, 2` ; Skip the next instruction if bit 2 of PINC is CLEARED (LOW)
    *    These instructions (SBIS, SBIC) are crucial for checking pin status without altering other bits.
  
  *   **Status Register (SREG) - Handwritten Note:**
    *   Shows the order of flags: `I T H S V N Z C`
    *   `T`: Temporary storage bit (used with `BST` and `BLD` instructions).
  
  *   **`BST Rd, b` (Bit Store):** Copies bit `b` from register `Rd` to the `T` flag in SREG.
  *   **`BLD Rd, b` (Bit Load):** Copies the `T` flag to bit `b` of register `Rd`.
  *    The hand written note states the following:
  ```assembly
  Copy a bit;
  BST R16, 4 ; Store bit 4 from R16 in T
  BLD R16, 2; load bit T into bit z of 
  ; R16
  ```
  
  *   **Handwritten Note - Bitwise Operators in C (Ch. 6):**
  
  ```
  %       Modulo
  &       Bitwise AND
  |       Bitwise OR
  ^       Bitwise XOR
  ~       Bitwise NOT
  <<      Shift left (the expression by the number of places on the right)
  >>      Shift right
  ```
  
  *   **Example (Handwritten):**
  
    ```assembly
    LDI R16, 0b0111<<3    ; R16 = 0b0111000, 3 positions
    LDI R16, 0x48
    >> 4;                 ; R16 = 0x04
    ```
  
  *   **BCD (Binary Coded Decimal) and ASCII Conversion:** The handwritten note shows:
  
       *   **Unpacked BCD:** Each decimal digit is stored in a separate byte (lower nibble).
      *   **Packed BCD:** Two decimal digits are stored in a single byte (one in the upper nibble, one in the lower nibble).
  *    **SWAP Rd,** which swaps nibbles.
  
  *    **Chapter 9: Timer/Counter.**
  Microcontroller applications commonly involve, detecting/counting events and generating periods of time or time delays.
  Timers and Counters are very important when dealing with this.
  Timers increment at a predicable clock source, while counters are incremented by an external source.
  *    Example: We can use an ATmega32. It has Timer0, Timer1, and Timer2, where Timer0 and Timer2 are 8 bit, while timer1 is 16 bit.