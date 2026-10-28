#### **1. Floating Point Addition**
  Adding floating point numbers is much harder than adding integers.
  *   *Integer Math:* $5 + 100$. Just add the bits.
  *   *FP Math:* $1.5 \times 10^1 + 2.5 \times 10^3$. You cannot add $1.5 + 2.5$ directly because the exponents are different. It's like trying to add feet to inches without converting first.
  
  **The Algorithm (The 4 Steps):**
  1.  **Align Decimal Points:**
  *   Find the number with the *smaller* exponent.
  *   Shift its significand (fraction) to the **right** until its exponent matches the larger number.
  *   *Note:* This implies we lose precision. If you shift too far right, the bits "fall off" and disappear. (This is why $1.0 + 1.0 \times 10^{-50}$ just equals $1.0$ in a computer).
  2.  **Add Significands:** Now that exponents match, add the fractions normally.
  3.  **Normalize:**
  *   The result might not look like scientific notation anymore (e.g., $0.05 \times 10^5$ or $15.2 \times 10^5$).
  *   Shift the result left or right until there is exactly one `1` before the dot ($1.52 \times 10^6$).
  4.  **Round:** If the result has too many bits to fit in the fraction field (23 bits for float), round it off.
#### **2. Floating Point Multiplication**
  Multiplication is actually slightly simpler in logic than addition because you don't need to align the points first.
  
  **The Algorithm:**
  1.  **Add Exponents:**
    *   Rule of Exponents: $x^a \times x^b = x^{a+b}$.
    *   *Tricky Part:* Remember the **Bias**? If you simply add the stored exponents, you add the Bias twice.
    *   *Correction:* $\text{New Exponent} = (\text{Exp}_1 + \text{Exp}_2) - \text{Bias}$.
  2.  **Multiply Significands:** Multiply the fractions just like standard integer multiplication.
  3.  **Normalize:** Shift the result so it looks like $1.\text{xxxx}$.
  4.  **Sign:** The sign of the result is determined by XOR. (Pos $\times$ Neg = Neg).
#### **3. LEGv8 Floating Point Instructions**
  The CPU has a separate set of registers for floating point math. It does not use `X0`–`X30` for this.
  
  *   **Registers:**
    *   **32 Single-Precision (32-bit):** `S0` to `S31`.
    *   **32 Double-Precision (64-bit):** `D0` to `D31`.
    *   *Note:* `S0` is just the bottom half of `D0`. They overlap.
  *   **Instructions:** They start with **F**.
    *   `FADDS` / `FADDD` (Float Add Single / Double)
    *   `FMULS` / `FMULD` (Float Multiply Single / Double)
    *   `LDURS` / `STURS` (Load/Store Single)
  *   **Example:**
    Converting Fahrenheit to Celsius uses `FSUBS` (Subtract), `FMULS` (Multiply), and `FDIVS` (Divide). Notice the code loads constants (like `5.0` and `9.0`) from memory because you can't put a float immediate directly into an instruction easily.
#### **4. Subword Parallelism (SIMD)**
  *   **The Concept:**
    *   Graphics and Audio data usually use small numbers (8-bit colors, 16-bit audio samples).
    *   A 64-bit adder is overkill for adding two 8-bit numbers.
  *   **SIMD (Single Instruction, Multiple Data):**
    *   Take a huge 128-bit register.
    *   Slice it into **sixteen** 8-bit chunks.
    *   Run **one** `ADD` instruction that adds all 16 pairs simultaneously.
  *   **ARM Implementation (NEON):**
    *   Registers `V0` to `V31` (128 bits wide).
    *   *Example:* `ADD V1.16B, V2.16B, V3.16B`.
    *   Translation: "Take register V2 and V3, treat them as 16 bytes each, add them up, and store in V1."
    *   *Impact:* This makes operations like image processing or matrix multiplication (AI) 4x to 16x faster.
#### **5. Accuracy & Errors**
  *   **Precision is Finite:** There are infinite real numbers, but only $2^{32}$ patterns in a float register.
  *   **Approximation:** Numbers like $1/3$ or $0.1$ cannot be represented exactly in binary (just like $1/3$ is $0.3333...$ in decimal).
  *   **Consequence:**
    *   **Accumulation Error:** If you add $0.0001$ thousands of times, the tiny error in representation adds up.
    *   **The "Patriot Missile" failure:** A famous software bug where a tiny timing error multiplied by hours of operation caused a missile to miss its target.
    *   **Financial Advice:** Never use `float` for money. Use integers (count pennies) or Double Precision to minimize error.
  
  ---
### **Student "Check Your Understanding"**
  
  1.  **FP Addition:** Why does adding a very small number (e.g., $10^{-20}$) to a large number (e.g., $10^{20}$) often result in the large number changing by **zero**? (Hint: Think about Step 1 of the algorithm).
  2.  **FP Multiplication:** If you have two numbers with a Bias of 127. If you add their stored exponents, what value must you subtract to get the correct new stored exponent?
  3.  **SIMD:** You are programming a filter for a photo app that darkens every pixel by value 10. The pixels are 8-bit integers. Would you use `SUB` on 64-bit `X` registers or `SUB` on 128-bit `V` registers? Why?