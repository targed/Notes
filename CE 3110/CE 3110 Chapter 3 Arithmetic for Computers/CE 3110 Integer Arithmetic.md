#### **1. Addition & Subtraction**
  You already know how to write `ADD` in assembly. This section explains what happens inside the ALU (Arithmetic Logic Unit).
  
  *   **The Hardware Reality:** Computers don't really "subtract." They only add.
  *   To do $7 - 6$, the computer performs $7 + (-6)$.
  *   It uses **2's Complement** to negate the second number (Invert bits + 1), then feeds it into the adder.
  *   **The Problem of Overflow:**
  *   We have a fixed number of bits (64 bits). If you add two massive numbers, the result might not fit.
  *   **The Rule of Signs:**
      *   Pos + Neg $\rightarrow$ **Never** overflows.
      *   Neg + Pos $\rightarrow$ **Never** overflows.
      *   Pos + Pos $\rightarrow$ **Can** overflow (Result looks Negative).
      *   Neg + Neg $\rightarrow$ **Can** overflow (Result looks Positive).
  *   *Note on Hardware:* The CPU detects overflow by checking if the Carry In to the MSB (Most Significant Bit) is different from the Carry Out.
#### **2. Multiplication**
  Multiplication is much harder than addition. In grade school, you learned "Long Multiplication." The CPU does the exact same thing, but in binary.
  
  *   **The Algorithm (Shift and Add):**
    1.  Check the last bit of the **Multiplier**.
    2.  If it is `1`, add the **Multiplicand** to the Product.
    3.  Shift the Multiplicand left (or shift the Product right).
    4.  Repeat for all bits.
  *   **Hardware Evolution:**
    *   *Slow Version:* Uses a 64-bit ALU and takes 64 clock cycles (one step per bit).
    *   *Fast Version (Parallel):* Uses many adders connected in a tree structure. It costs more silicon (money/space) but runs much faster.
  *   **Bit Expansion:**
    *   Multiplying two 64-bit numbers results in a **128-bit number**.
    *   *Example:* $99 \times 99 = 9801$ (2 digits $\times$ 2 digits = 4 digits).
  *   **LEGv8 Instructions:**
    *   `MUL`: Keeps the *lower* 64 bits (good for normal math where numbers aren't huge).
    *   `SMULH` (Signed Multiply High): Keeps the *upper* 64 bits.
    *   `UMULH` (Unsigned Multiply High): Keeps the *upper* 64 bits (unsigned).
    *   *Why separation?* If you need the full 128-bit result, you have to run two instructions (`MUL` then `SMULH`) and stitch them together.
#### **3. Division**
  Division is the most expensive operation in integer arithmetic (it is slow!).
  
  *   **The Algorithm (Restore Division):**
    *   It works like grade-school Long Division. It tries to subtract the divisor.
    *   If the result is positive, put a `1` in the quotient.
    *   If the result is negative (we subtracted too much), put a `0` in the quotient and **add the divisor back** (Restore).
  *   **LEGv8 Instructions:**
    *   `SDIV` (Signed) and `UDIV` (Unsigned).
    *   **Hardware Note:** Division in hardware handles divide-by-zero by... doing nothing. It doesn't crash the machine; it usually just returns `0`. It is up to the *software* (compiler) to check for zero before dividing.
  
  ---
### **Student "Check Your Understanding"**
  1.  **Overflow Logic:** If you add two negative numbers and the result comes out as `0x7FFFF...` (a positive number), has an overflow occurred?
  2.  **Multiplication Size:** If you multiply a 32-bit integer by a 32-bit integer, what is the maximum number of bits the result could require?
  3.  **Hardware:** Why is Division generally slower than Multiplication in hardware? (Hint: Think about "predicting" the answer).