### Latches and Flip-Flops
  
  *   **Latches:** Level-sensitive storage elements. The output follows the input when the enable (clock) signal is active (either HIGH or LOW, depending on the design).  When the enable is inactive, the output holds the last value.
  
  *   **D Latch (Clocked):**
      *   Inputs: D (data), φ (clock/enable)
      *   Outputs: Q, Q̅
      *   When φ = 1 (enable HIGH), Q follows D.
      *   When φ = 0 (enable LOW), Q holds the previous value.
  *Circuit Diagram (using NOR gates):*
  
       (Diagram of D Latch with NOR gates included, the same structure as in your notes).
  
      *   **Truth Table:**
  
          | D   | Q   |
          | :-: | :-: |
          | 0   | 0   |
          | 1   | 1   |
          | φ=0 | hold|
  
  *Example using NOR Gates:*
   A NOR gate is an or gate with the values inverted.
  
  *Nor gate truth table:*
  | A   | B   |  A+B  |
  | :-: | :-: | :-: |
  | 0   | 0   |  1   |
  | 0   | 1   |  0  |
  | 1   | 0   | 0  |
  | 1   | 1   |   0  |
  
      *Enable = 1*
      If D = 1, $Q = 0$ and $Q̅=1$
      If D = 0, $Q = 1$ and $Q̅=0$
      *Enable = 0*
      If $Q = 1$ then $Q̅= 0$ (0X) and if $Q = 0$ then $Q̅ = 1$ (X0)
  
  *Example using SR Latch:*
      If s = 1, $ Q = 1$, and if R = 1, $Q=0$, and if $S=R=1$, this is invalid.
  
      | S | R   | Q |
      | :-: | :-: | :-: |
      | 0 | 0 |hold  |
      | 0 | 1   | 0  |
      | 1 | 0 |  1  |
      | 1 | 1   |  Not Used  |
  
  *   **Flip-Flops:** Edge-triggered storage elements.  The output changes only on the rising (positive) or falling (negative) edge of the clock signal. This makes them synchronous.
  
  *   **D Flip-Flop (DFF):**
      *   Inputs: D (data), clk (clock)
      *   Outputs: Q, Q̅
      *   On the active clock edge (rising or falling, depending on design), Q takes the value of D.  Otherwise, Q holds its previous value.
  
  *Master-Slave D Flip-Flop:*
  
     A DFF is comprised of two D Latches.
  
      *Diagram as shown in handwritten notes, with Master (M) and Slave (S) stages.*
  
  *D Latch Level High, DFF rising edge, D Latch Level Low, DFF falling edge, and clocking with D latch high on clock, and then when low, and showing how the input D will be shown on each state.*
### Registers
  
  *   **Register:** A collection of flip-flops (usually D flip-flops) used to store a group of bits (a word).
  
    *   **4-bit Register:**
    This means that 4 bits (the smallest multiple of 8 between 1-8), each single binary value, will be stored.
        *Diagram of 4 D flip-flops connected, with a common clock signal.*
## Address Decoding
  
  **Address Decoding:** The process of generating chip select (CS) signals for memory chips and I/O devices using address and timing information from the processor.
  
  **Steps for Address Decoding:**
  
  1.  **Address Table:** Create a table showing the address range for each device to be decoded, relative to the processor.
  
  2.  **Address Lines:** Determine the address lines associated with each device.
  
  3.  **CS Enable Lines:** Determine the address lines that need to be connected to the CS (enable) lines of the devices.
  
  4.  **Logic:** Design the logic needed to generate the CS line signals.
  
  **Example (Using a Logic Gate):**
  Assume we are working with 16 address pins: A0 - A15
  We want to know the address range for RAM (Read/Write)
  
  *   **Address Table:**
  
    | Device | Start Address (Hex) | End Address (Hex) |
    | ------ | -------------------- | ------------------ |
    | RAM    | 3000                | 3FFF               |
  
  *   **Binary Representation:**
  
    *   Start Address (3000_H):
        A_15A_14A_13A_12A_11A_10A_9A_8A_7A_6A_5A_4A_3A_2A_1A_0
        = 0011 0000 0000 0000
    *   End Address (3FFF_H):
        A_15A_14A_13A_12A_11A_10A_9A_8A_7A_6A_5A_4A_3A_2A_1A_0
        = 0011 1111 1111 1111
  
  *   **Address Lines for CS:**  A_15, A_14, A_13, A_12 determine the range. A_15 and A_14 are always low, and A_13, A_12 are always high. The other bits change.
  
  *   **Logic:** Use a NAND gate of  A̅_15A̅_14A_13A_12. The NAND gate is connected to A_15, and A_14 through inverters, and A_13 and A_12 are given directly. The other bits are unconnected.
  
  *Circuit Diagram of the address decoder.*
  (Same as the diagram in the handwritten notes on the left).
  
  *   **Address Space:** The range of addresses for RAM is 3000_H - 3FFF_H.
  
  **Example (Using a 74LS138 Decoder):**
  
  *ROM*
  
  The ROM device has a chip enable: *CE*
  *   **Address Table:**
  
    | Device | Start Address (Hex) | End Address (Hex) |
    | ------ | -------------------- | ------------------ |
    | ROM    | 4000                | 4FFF               |
  
  *   **Binary Representation:**
  
    *   Start Address (4000_H):
        A_15A_14A_13A_12A_11A_10A_9A_8A_7A_6A_5A_4A_3A_2A_1A_0
        = 0100 0000 0000 0000
    *   End Address (4FFF_H):
        A_15A_14A_13A_12A_11A_10A_9A_8A_7A_6A_5A_4A_3A_2A_1A_0
        = 0100 1111 1111 1111
  
  *   **Address Lines for CS:** A_15, A_14, A_13, A_12 are used to determine the range.
  
  *   **Logic:** Use a 74LS138 decoder.  Connect A_15 to G2B(low), A_14 to G1 (high), A_13 to G2A(low), A_12 to input C, A_11 to input B, A_10 to input A.  Connect the appropriate output (Y0) of the 74LS138 to the CE pin of the ROM.
  
  *Circuit Diagram as the right handwritten image*
  
  *   **Address Range:**  The address range of the ROM is 4000_H to 4FFF_H.
  
  ---
  End of Lecture 2 Notes
  ---