## AVR I/O Ports
  
  The ATmega32 has four 8-bit I/O ports: PORTA, PORTB, PORTC, and PORTD. These ports can be used for both input and output. Each pin on a port can be individually configured as either input or output.
  *Ports have alternate functions like, ADC, timers, interrupts, and serial communication.*
  
  *Refer to image of the 40-pin ATmega 32*
  *Refer to image of the number of pins and ports for different AVR devices*
  
  **Three Registers Control Each Port:**
  
  1.  **DDRx (Data Direction Register):**  Determines whether a pin is an input or an output.
  *   `DDRx = 1`:  Configures the corresponding pin as an output.
  *   `DDRx = 0`:  Configures the corresponding pin as an input.
  
  2.  **PORTx (Port Data Register):**
  *   If the pin is configured as an *output* (DDRx = 1), writing a '1' to PORTx drives the pin HIGH, and writing a '0' drives it LOW.
  *   If the pin is configured as an *input* (DDRx = 0), writing a '1' to PORTx enables the internal pull-up resistor, and writing a '0' disables the pull-up.
  
  3.  **PINx (Port Input Pins Register):** Reads the logic level (HIGH or LOW) present on the pin, *regardless* of whether it's configured as an input or output.
  
  **Example (Outputting to a Port):**
  
  ```assembly
  ; Configure PORTB as output and send 0x55
  LDI R16, 0xFF  ; Load 0xFF (all bits 1) into R16
  OUT DDRB, R16 ; Set all pins of PORTB as output
  LDI R16, 0x55  ; Load 0x55 into R16
  OUT PORTB, R16 ; Output 0x55 to PORTB
  ```
  
  **Example (Inputting from a Port):**
  
  ```assembly
  ; Configure PORTC as input and read its value into R17
  LDI R16, 0x00  ; Load 0x00 (all bits 0) into R16
  OUT DDRC, R16 ; Set all pins of PORTC as input
  IN  R17, PINC ; Read the value of PINC into R17
  ```
  
  *Refer to figure 4-2, Relations between Registers and Pins of AVR*
  
  *Refer to figure 4-3, The I/O Port in AVR, to get a view of how the data goes from Portx to the outside of the device*
  
  **Pull-Up Resistors:**
  
  *   AVR I/O pins have internal pull-up resistors (20-50 kΩ).
  *   Enabled by setting the corresponding PORTx bit to '1' *while* the pin is configured as an input (DDRx = 0).
  *   Used to provide a default HIGH logic level when no external device is driving the pin.
  
  *Refer to figure 4-4, The pull-up Resistor*
  
  **Synchronization Delay:**
  
  *   When reading from an input pin using the `IN` instruction, there is a small delay (one clock cycle) between when the value appears on the pin and when it is stable enough to be read.
  *   A `NOP` (no operation) instruction can be inserted before the `IN` instruction to ensure data stability.
  
  **Bit-Addressability:**
  
  *   AVR I/O ports (specifically, the lower 32 I/O registers, $00 - $1F in I/O space, or $20 - $3F in data memory space) are *bit-addressable*.
  *   This means you can set, clear, or test individual bits within these registers without affecting the other bits.
  
  **Bit Manipulation Instructions:**
  
  *   **SBI (Set Bit in I/O register):**  Sets a specific bit in an I/O register to '1'.
  *   `SBI  PORTB, 5` ; Sets bit 5 of PORTB to 1.
  
  *   **CBI (Clear Bit in I/O register):**  Clears a specific bit in an I/O register to '0'.
  *   `CBI  PORTB, 2` ; Clears bit 2 of PORTB to 0.
  
  *   **SBIS (Skip if Bit in I/O register is Set):**  Skips the next instruction if the specified bit in an I/O register is '1'.
  
  *   **SBIC (Skip if Bit in I/O register is Cleared):**  Skips the next instruction if the specified bit in an I/O register is '0'.
  
  *Refer to Table 4-7. Single-Bit (Bit-Oriented) Instructions for AVR*
  *Refer to Table 4-8. The Lower 32 I/O Registers*
  *Refer to Table 4-9. Single-Bit Addressability of Ports for ATmega32/16*
  
  **Example (Toggling a Bit Continuously):**
  
  ```assembly
  ; Toggle PB2 continuously
  SBI DDRB, 2   ; Make PB2 an output pin
  AGAIN:
  SBI PORTB, 2   ; Set PB2 HIGH
  CALL DELAY    ; Wait for some time
  CBI PORTB, 2   ; Clear PB2 LOW
  CALL DELAY    ; Wait for some time
  RJMP AGAIN    ; Repeat forever
  ```
  
  **Example (Checking an Input Pin):**
  *Refer to figure 4-8 and 4-9 for the formating of SBIS and SBIC respectively*
  
  ```assembly
  ; Monitor PB2.  If HIGH, send $45 to Port C
  CBI  DDRB, 2   ; Make PB2 an input
  OUT  DDRC, R16 ; Make Port C an output
  AGAIN:
  SBIS PINB, 2   ; Skip next if PB2 is HIGH
  RJMP AGAIN    ; Keep checking if LOW
  LDI  R16, 0x45 ; Load 0x45
  OUT  PORTC, R16 ; Output to Port C
  HERE:
  RJMP HERE     ; Stay here
  ```
  
  **Handwritten Notes (Incorporated and Expanded):**
  ```
  There are a total of 4 ports:
  Ports have 8 pins.
  There is a DDR register for setting inputs, and output.
  There is a pin register for accepting the inputs into the processor.
  There is a port register for outputting data onto the pins.
  
  ;Test Program:
  LDI R16, 0xFF
  OUT DDRB, R16  
  OUT DDRC, R16
  OUT DDRD, R16
  L3:
  OUT PORTB, R16
  OUT PORTC, R16
  OUT PORTD, R16
  COM R16 ;Complements R16
  RJMP L3
  
  ;Gets data from port C
  ;And sends it to B
  INCLUDE "M32DEF.INC"
  LDI R16, 0x00
  OUT DDRC, R16 ; Port c is input
  LDI R16, 0xFF
  OUT DDRB, R16 ; port b is output
  L2:
  IN  R16, PINC  ;read data from Port C
              ;and put in R16
  LDI R17, 5
  ADD R16, R17
  OUT PORTB, R16 ;send it to B
  RJMP L2        ;continuous forever
  
  ;Using the pullup resistors in port C
  ;By setting the PORTx bit to 1
  INCLUDE "M32DEF.INC"
  LDI R16, OxFF
  OUT DDRB, R16 ;Port b is an output
  OUT PORTC, R16 ;Make pullup resistors active
  LDI R16, 0x00
  OUT DDRC, R16 ;Port C input
  L2:
  IN R16, PINC    ;Read from port C
  ADD R17, 5
  OUT PORTB,R16  ;Send it to b
  RJMP L2        ;continue forever
  ```
  ***
### 1. Detailed Examples of Using Pull-up Resistors
  
  A common use of pull-up resistors is with switches or buttons.  Consider a simple switch connected between an AVR I/O pin and ground.
  
  **Circuit:**
  
  Imagine a connection where there is a switch. One end of the switch goes to GND, and the other is attached to the AVR pin.
  
  **Without Pull-Up:**  When the switch is open, the I/O pin is "floating" – its voltage is undefined. It might be HIGH, LOW, or somewhere in between, due to noise.  When the switch is closed, the pin is pulled LOW (to ground).
  
  **With Pull-Up:** When the switch is open, the internal pull-up resistor pulls the I/O pin HIGH.  When the switch is closed, the pin is pulled LOW (to ground), overriding the pull-up.  This provides a clear HIGH/LOW signal depending on the switch state.
  
  **Code Example (Reading a Switch with Pull-up):**
  
  ```assembly
  ; Assume a switch is connected to PB0, with the other end to ground.
  ; Read the switch state and output it to PORTC.
  
  .INCLUDE "M32DEF.INC"
  
  .EQU SWITCH_PIN = 0   ; Define the switch pin (PB0)
  
    LDI R16, (1<<SWITCH_PIN)  ; Set bit corresponding to SWITCH_PIN in R16
    OUT PORTB, R16 ; Enable pull-up on PB0
    LDI R16, 0x00
    OUT DDRB, R16          ; Configure PB0 as input
  
    LDI R16, 0xFF         ; Configure PORTC as output
    OUT DDRC, R16
  
  LOOP:
    SBIS PINB, SWITCH_PIN  ; Skip next instruction if PB0 is HIGH (switch open)
    RJMP SWITCH_PRESSED    ; Jump if switch is pressed (PB0 is LOW)
  
  SWITCH_OPEN:
    LDI R16, 0xFF         ; Turn on all LEDs on PORTC (switch open)
    OUT PORTC, R16
    RJMP LOOP
  
  SWITCH_PRESSED:
    LDI R16, 0x00         ; Turn off all LEDs on PORTC (switch pressed)
    OUT PORTC, R16
    RJMP LOOP
  ```
  
  **Explanation:**
  
  1.  `.EQU SWITCH_PIN = 0`: This is an assembler directive that defines a symbolic constant `SWITCH_PIN` and assigns it the value 0 (corresponding to PB0).  This makes the code more readable.
  
  2.  `LDI R16, (1<<SWITCH_PIN)` and `OUT PORTB, R16`:  This enables the pull-up resistor on PB0. `(1<<SWITCH_PIN)` is a bit shift operation.  `1<<0` is the same as `0b00000001`. This sets bit 0 of R16 to 1. Writing this to PORTB *while PB0 is configured as input* enables the pull-up.
  
  3.  `LDI R16, 0x00` and `OUT DDRB, R16`: This configures PB0 as an input. Writing all zeros to the DDR register makes all pins on that port inputs.
  
  4.  `SBIS PINB, SWITCH_PIN`:  This instruction checks the state of PB0. `SBIS` (Skip if Bit in I/O register is Set) skips the next instruction (`RJMP SWITCH_PRESSED`) if PB0 is HIGH (switch open).
  
  5.  `RJMP SWITCH_PRESSED`: If PB0 is LOW (switch pressed), this jump is taken.
  
  6. The rest of the code turns the LEDs on PORTC on or off based on the switch state.
### 2. Debouncing
  
  **Switch Bounce:** Mechanical switches don't make a clean, instantaneous transition between open and closed states.  When a switch is pressed or released, the contacts physically bounce, causing multiple rapid transitions between HIGH and LOW before settling. This is called "switch bounce".
  
  **Problem:**  If we read the switch state too quickly, we might interpret these bounces as multiple presses or releases.
  
  **Solutions:**
  
  *   **Hardware Debouncing:** Use a capacitor and/or resistor network (RC circuit) to filter out the rapid transitions.  A common approach is to connect a capacitor in parallel with the switch and a resistor in series.
  
  *   **Software Debouncing:**  Introduce a delay after detecting a switch state change, and then re-check the state. If the state is still the same after the delay, we assume it's a valid press/release.
  
  **Software Debouncing Example (Adding to the previous example):**
  
  ```assembly
  ; ... (Previous code to configure ports and enable pull-up) ...
  
  LOOP:
    SBIS PINB, SWITCH_PIN
    RJMP SWITCH_PRESSED
  
  SWITCH_OPEN:
    LDI R16, 0xFF
    OUT PORTC, R16
    RJMP LOOP
  
  SWITCH_PRESSED:
    ; Debounce the switch
    CALL DELAY_20MS   ; Wait for 20 milliseconds
  
    ; Check the switch state again AFTER the delay
    SBIS PINB, SWITCH_PIN   ;If it goes high means, the button has been released
    RJMP SWITCH_OPEN      ;if it is still pressed, we know it's not noise
  
    ; If we get here, the switch is still pressed after the delay
    LDI R16, 0x00
    OUT PORTC, R16
    RJMP LOOP
    
  ; --- Subroutine for a ~20ms delay ---
  ; (You'll need to create an appropriate delay subroutine)
  ; This is a VERY simplified example, and the delay
  ; will NOT be accurate. A proper delay routine uses timers.
  DELAY_20MS:
    LDI  R20, 250
    LDI  R21, 100
  DELAY_LOOP2:
    DEC  R21
    BRNE DELAY_LOOP2
    DEC  R20
    BRNE DELAY_LOOP2    ;or 1
    RET
  
  ```
  **Explanation:**
  
  1.  `CALL DELAY_20MS`:  After detecting a switch press, we call a subroutine to introduce a delay (approximately 20 milliseconds in this *simplified* example).  *Crucially, you would normally use a timer for accurate delays, not a simple loop like this. This loop's timing will vary greatly with clock speed and compiler optimizations.*
  
  2.  `SBIS PINB, SWITCH_PIN`:  *After* the delay, we check the switch state *again*.
  
  3.  `RJMP SWITCH_OPEN`: If the switch is now *open* (HIGH), we assume the initial press was just bounce, and we go back to the `SWITCH_OPEN` state.
  
  4. Only if the switch is *still pressed* after the delay do we consider it a valid press and turn off the LEDs.
### 3. Multiplexing I/O Pins
  
  Multiplexing allows you to control more devices than you have I/O pins by sharing pins and using a selection mechanism.  A common example is using a decoder.
  
  **Example: Controlling 8 LEDs with 3 I/O Pins and a 74LS138 Decoder:**
  
  *   **Components:**
    *   AVR microcontroller (e.g., ATmega32)
    *   74LS138 3-to-8 decoder
    *   8 LEDs
    *   Resistors (for current limiting)
  
  *   **Connection:**
    *   Connect three AVR I/O pins (e.g., PB0, PB1, PB2) to the decoder's input lines (A, B, C).
    *   Connect the decoder's enable lines (G1, G2A, G2B) appropriately (e.g., G1 to VCC, G2A and G2B to GND for always-enabled operation).
    *   Connect each of the decoder's output lines (Y0-Y7) to an LED through a current-limiting resistor.  The other end of each LED goes to ground.
  
  *   **Operation:**
    *   The AVR sets the three I/O pins (PB0-PB2) to represent a binary number from 0 to 7.
    *   The 74LS138 decoder activates the corresponding output line (Y0-Y7).  Only *one* output line is active (LOW) at a time.
    *   The active (LOW) output line allows current to flow through the connected LED, turning it on.
  
  *   **Code (Simplified):**
  
    ```assembly
    ; Control 8 LEDs using a 74LS138 decoder connected to PB0-PB2
  
    .INCLUDE "M32DEF.INC"
  
    .EQU LED_PORT = PORTB   ; Define the port connected to the decoder
    .EQU LED_DDR  = DDRB
  
    .ORG 0
        LDI R16, 0b00000111 ; PB0, PB1, and PB2 are outputs.
        OUT LED_DDR, R16   ; Set PB0-PB2 as outputs
  
    LOOP:
        ; Turn on LED 0
        LDI R16, 0b00000000  ; Binary 0 (Y0 will be active)
        OUT LED_PORT, R16
        CALL DELAY_1S        ; Wait
  
        ; Turn on LED 1
        LDI R16, 0b00000001  ; Binary 1 (Y1 will be active)
        OUT LED_PORT, R16
        CALL DELAY_1S
  
        ; ... (Continue for LEDs 2-7) ...
  
        ; Turn on LED 7
        LDI R16, 0b00000111  ; Binary 7 (Y7 will be active)
        OUT LED_PORT, R16
        CALL DELAY_1S
  
        RJMP LOOP            ; Repeat
    .ORG 0x100
    DELAY_1s:
    ;Insert delay of choice here
    RET
  
    ```
  
  This technique allows you to control 8 LEDs with only 3 I/O pins.  You can extend this principle to control even more devices by using larger decoders or multiple decoders.
  
  **Advantages:**
  * Conserves I/O pins.
  * Allows for expansion of output capabilities.
### 4. Port A as Input
  
  Port A can be used as an input port just like other ports by setting the corresponding bits in DDRA to '0'.  However, there are a few key differences and considerations when using Port A:
  
  *   **Alternate Functions:** Port A pins have several important alternate functions, primarily the Analog-to-Digital Converter (ADC) inputs (ADC0-ADC7). If you are using the ADC, those pins *cannot* be used as general-purpose I/O.
  
  *   **AVCC and AREF:**  Pins 30 and 32 on the ATmega32 (in the 40-pin DIP package) are AVCC (analog VCC) and AREF (analog reference).  These are *power supply* and *reference voltage* pins specifically for the ADC, and they are *not* part of PORTA.
  
  *    **Reading PINA:**  The `PINA` register always reflects the *voltage level* on the Port A pins, even if they are configured as outputs or are being used for their alternate ADC functions.  This is different from, say, reading `PORTB` when a pin is configured as output; reading `PORTB` will return what you *wrote* to `PORTB`, not necessarily the voltage on the pin.
  
  **Example: Using Port A as Input with Pull-ups:**
  
  ```assembly
  ; Configure Port A as input with pull-ups enabled.
  ; Read the state of Port A and output it to Port B.
  
  .INCLUDE "M32DEF.INC"
  
    LDI R16, 0xFF     ; Enable pull-ups on all Port A pins
    OUT PORTA, R16
  
    LDI R16, 0x00     ; Configure all of Port A as inputs
    OUT DDRA, R16
  
    LDI R16, 0xFF    ; Configure Port B as output
    OUT DDRB, R16
  
  LOOP:
    IN  R17, PINA   ; Read the state of Port A pins into R17
    OUT PORTB, R17  ; Output the value to Port B
    RJMP LOOP
  ```
  
  **Important Note:** If you have *external* pull-up or pull-down resistors connected to Port A pins, those will override the internal pull-ups. Be careful about conflicting configurations.
### 5. Dual Role of Ports A and B (and other ports)
  
  **Multiplexed Functions:**
  
  Many of the AVR's I/O pins serve multiple purposes.  This "multiplexing" allows a single pin to be used for:
  
  *   **General-Purpose I/O:**  Basic input or output, as controlled by DDRx, PORTx, and PINx.
  *   **Alternate Functions:**  Specialized functions controlled by other registers.
  
  **Examples of Alternate Functions:**
  
  *   **Port A (PA0-PA7):**  Analog-to-Digital Converter (ADC) inputs (ADC0-ADC7).  When the ADC is enabled, these pins are used for analog input *instead* of general-purpose I/O.
  
  *   **Port B (PB0-PB7):**
    *   PB0: Timer/Counter0 external clock input (T0), or SPI Slave Select (SS)
    *   PB1: Timer/Counter1 external clock input (T1)
    *   PB2: External Interrupt 2 input (INT2/AIN0)
    *   PB3: Output Compare Match output for Timer/Counter0 (OC0/AIN1)
    *   PB4: SPI Master Out, Slave In (MOSI)
    *   PB5: SPI Master In, Slave Out (MISO)
    *   PB6: SPI Serial Clock (SCK)
    *   PB7: Slave select pin for the SPI interface.
  
  *   **Port C (PC0-PC7):** Alternate functions include pins for the Two-wire Serial Interface (TWI/I2C)
  
  *   **Port D (PD0-PD7):**
    *   PD0, PD1:  USART (serial communication) RXD and TXD pins.
    *   PD2, PD3: External Interrupt inputs (INT0, INT1).
    *   Timer/Counter Output Compare pins (OCxA, OCxB).
    *   Input Capture pin (ICP).
  
  **Example (Conflicting Functions):**
  
  If you try to use PB5 as a general-purpose output *while* the SPI interface is enabled in master mode, the SPI module will override your output attempts, and you won't get the behavior you expect.  The SPI module will control the pin.
  
  **Best Practices:**
  
  *   **Consult the Datasheet:**  The ATmega32 datasheet (and the datasheets for other AVR microcontrollers) provides detailed tables and diagrams showing the alternate functions of each pin.  *Always* refer to the datasheet to avoid conflicts.
  *   **Disable Unused Peripherals:**  If you are not using a particular peripheral (e.g., the ADC, SPI, USART), disable it to free up the associated pins for general-purpose I/O.  Disabling is typically done by clearing bits in control registers.
  *   **Prioritize:** If you need to use a pin for both its general-purpose I/O function and an alternate function, you'll need to prioritize. You might need to switch between functions dynamically, or choose a different pin for one of the functions.
  
  **Example (Disabling the ADC):**
  
  ```assembly
  ; Disable the ADC to free up Port A pins for general-purpose I/O.
  ; (This is often done at the start of a program if the ADC isn't needed.)
  
    LDI R16, 0x00
    OUT ADCSRA, R16  ; Clear the ADEN bit (ADC Enable) in the ADCSRA register.
  ```
  
  By disabling unused peripherals, you minimize the chance of conflicts and maximize the number of I/O pins available for your application.
  
  ---