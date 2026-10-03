# CpE 3150 - Lecture 1: Introduction and Review
## Overview of Microcontrollers and Embedded Systems
  
  This course focuses on the intersection of hardware and software in the design of embedded systems, with a particular emphasis on microcontrollers.
  
  **Key Definitions:**
  
  *   **Computer:** A device that stores and retrieves data, and sequentially executes a stored program without human intervention.
  *   **Microprocessor:** A computer contained on a single integrated circuit (IC).
  *   **Microcontroller:** A microprocessor with a number of integrated peripherals for control, instrumentation, and positioning applications.
  *   **Embedded System:** A computer or computer system that is part of a larger system. It interacts with its surrounding environment, though its presence might not be obvious.
  
  **Basic Microprocessor (µP) System:**
  
  A typical µP-based system includes:
  
  *   **Microprocessor (CPU):** The central processing unit, which executes instructions.
  *   **Memory (RAM/ROM):** Stores data and program instructions.
    *   RAM (Random Access Memory): Volatile memory for storing data and programs.
    *   ROM (Read-Only Memory): Non-volatile memory for storing permanent program instructions.
  *   **Input/Output (I/O) Ports:** Interface with external devices.
  *   **Address Bus:** Used to select memory locations or I/O devices.
  *   **Data Bus:** Used to transfer data between components.
  *   **Control Bus:** Used to synchronize operations and control flow.
  
  **The AVR Microcontroller Family:**
  
  This course will focus on the Atmel AVR (specifically the ATmega32) microcontroller, which is an 8-bit device. This means that its data values are typically multiples of 8 bits.
  
  **Review Topics: Number Systems**
  The number systems that we will work with are decimal (0-9), binary (0,1) and hexadecimal (0-9, A, B, C, D, E, F).
  Here is an example of base conversion, converting a hexadecimal to binary:
  Convert $A5F4_{(16)}$ to binary
  
  $$
  \begin{aligned}
  & A \rightarrow 1010 \\
  & 5 \rightarrow 0101 \\
  & F \rightarrow 1111 \\
  & 4 \rightarrow 0100
  \end{aligned}
  $$
  So:
  $A5F4_{(16)}$ = 1010010111110100
  
  ***
## Digital Logic Review
### Basic Logic Gates
  It is important to understand and interpret the various standard logic gates
  *   **AND Gate:** Output is HIGH (1) only if all inputs are HIGH (1).
  *   **OR Gate:** Output is HIGH (1) if at least one input is HIGH (1).
  *   **NOT Gate (Inverter):** Output is the inverse of the input.
  *   **NAND Gate:** Output is LOW (0) only if all inputs are HIGH (1).
  *   **NOR Gate:** Output is HIGH (1) only if all inputs are LOW (0).
  *   **XOR Gate (Exclusive OR):** Output is HIGH (1) if an odd number of inputs are HIGH (1).
  
  **Truth Tables**
  
  Truth tables provide a tabular representation of the output(s) for all possible input combinations. Here are some examples of truth tables:
  
  *   **NAND Gate:**
    | A   | B   |  AB  |
    | :-: | :-: | :-: |
    | 0   | 0   |  1   |
    | 0   | 1   |  1  |
    | 1   | 0   | 1  |
    | 1   | 1   |  0  |
  
  *Half Adder*
  
  $$\boxed{HA}$$
  
  Where:
  $a_n$ = first number
  $b_n$ = second number
  $S_n$ = sum
  $C_{n+1}$= carry-out
  
  | $a_n$ | $b_n$ | $S_n$ | $C_{n+1}$ |
  |-------|-------|-------|-------|
  |   0   |   0   |  0    |   0   |
  |   0   |   1   |   1    |   0   |
  |   1   |   0   |   1    |   0   |
  |   1   |   1   |   0    |   1   |
  
  Equations:
  $ S_n = a_n \oplus b_n$
  $C_{n+1} = a_n \cdot b_n$
  
  *Full Adder*
  
  $$\boxed{FA}$$
  
  Where:
  $a_n$ = first number
  $b_n$ = second number
  $S_n$ = sum
  $C_{n+1}$= carry-out
  
  | $a_n$ | $b_n$ | $C_n$ | $S_n$ | $C_{n+1}$ |
  |-------|-------|------|-----|--------|
  |   0   |   0   |  0  |  0  |   0   |
  |   0   |   1   |  0  |  1  |   0   |
  |   1   |   0   |  0  |  1  |   0   |
  |   1   |   1   |  0  |  0  |   1   |
  |   0   |   0   |  1  |  1  |   0   |
  |   0   |   1   |  1  |  0  |   1   |
  |   1   |   0   |  1  |  0  |   1   |
  |   1   |   1   |  1  |  1  |   1   |
  
  Equations:
  $S_n = a_n \oplus b_n \oplus C_n$
### Number Conversions
  
  *   **Decimal to Binary:** Successive division by 2 (integer part), and writing the remainders.
    For example:
    $$ 10_{(10)}= \frac{10}{2} = 5\, r0 $$
    $$ \frac{5}{2} = 2\, r1$$
    $$ \frac{2}{2} = 1\, r0 $$
    $$ \frac{1}{2} = 0\, r1 $$
  
    So, reading the remainders up we get 1010
  
  *   **Hexadecimal to Binary:** Convert each hexadecimal digit (0-9, A-F) to its 4-bit binary equivalent.
    For example:
    $$ A5F4_{(16)} = 1010\,0101\,1111\,0100 $$
  
  * Successive Multiplications.
    $$2.5 = .5 \times 2 = 1.0$$
    so:
    $$ 10.1_{(2)} $$
  
  *   **Signed Numbers (Two's Complement):**
    *   The most significant bit (MSB) represents the sign (0 for positive, 1 for negative).
    *   For an 8-bit representation, the range is -128 to +127.
    *   To find the two's complement of a number, invert all bits and add 1.
  For example, for 10010001 which is -111 in the 2's Complement system
  $$ 1 \times (-1) \times 2^7 + 0 \times 2^6 + 0 \times 2^5 + 1 \times 2^4 + 0 \times 2^3 + 0 \times 2^2 + 0 \times 2^1 + 1 \times 2^0 = -128 + 16 + 1 = -111$$
### Decoders and Multiplexers
  
  *   **Decoder:** A combinational circuit that converts a binary code (n input bits) into a single active output line (up to 2<sup>n</sup> output lines).
    For example:
    A 3-to-8 decoder (74138)
    Has 3 input lines (C, B, A) and 8 output lines.
    Each output line corresponds to a unique combination of the inputs.
    Enable lines: G1, G2A, G2B (active low)
    Active low outputs.
    Control word CBA: 000=11111110, 001=11111101, 010=11111011, 011=11110111, 100=11101111, 101=11011111, 110=10111111, and 111=01111111
  
  *   **Multiplexer (MUX):** A combinational circuit that selects one of several input lines and forwards it to a single output line.
    A 4-to-1 MUX has 4 input lines (D0, D1, D2, D3), 2 control lines (S1, S0), and 1 output line (Y).
    The control lines determine which input is selected.
    Logic expression:  $Y = \overline{S1} \cdot \overline{S0} \cdot D0 + \overline{S1} \cdot S0 \cdot D1 + S1 \cdot \overline{S0} \cdot D2 + S1 \cdot S0 \cdot D3$
### Full Adders
  
  Full adders are a type of adder that can take three input digits as opposed to the two input digits of a half adder.
  Here is the truth table for a Full Adder:
  
  | a_n | b_n | C_n | S_n | C_n+1 |
  |-----------|-----------|--------|------|--------|
  |     0    |     0    |    0   |  0   |    0  |
  |     0    |     0    |    1   |  1   |    0  |
  |     0    |     1    |    0   |  1   |    0  |
  |     0    |     1    |    1   |  0   |    1  |
  |     1    |     0    |    0   |  1   |    0  |
  |     1    |     0    |    1   |  0   |    1  |
  |     1    |     1    |    0   |  0   |    1  |
  |     1    |     1    |    1   |  1   |    1  |
  
  Where the letters a and b are the two numbers to be added together, c is the carry in from the previous addition and C is the carry out. The formula for the sum is the same as the half adder.
  
  $S_n =  a_n ⊕ b_n ⊕ C_n$