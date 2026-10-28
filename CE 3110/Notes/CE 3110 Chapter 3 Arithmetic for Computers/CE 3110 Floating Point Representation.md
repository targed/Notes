#### **1. The Concept**
  Integers are great for exact counts, but bad for:
  1.  **Very small numbers** (e.g., $0.000000001$)
  2.  **Very large numbers** (e.g., $9 \times 10^{50}$)
  3.  **Fractions** (e.g., $0.5$)
  
  To solve this, we use **Scientific Notation** in binary.
  *   *Decimal:* $-2.34 \times 10^{56}$
  *   *Binary:* $\pm 1.\text{xxxx} \times 2^{\text{yyyy}}$
#### **2. The Standard**
  In the old days, every computer stored decimals differently. Now, everyone uses **IEEE 754**.
  
  **The Format (Memorize this):**
  We store a number in three fields: **Sign**, **Exponent**, and **Fraction**.
  
  $$ (-1)^S \times (1 + \text{Fraction}) \times 2^{(\text{Exponent} - \text{Bias})} $$
  
  **The Two Precisions:**
  | Feature | **Single Precision** (float) | **Double Precision** (double) |
  | :--- | :--- | :--- |
  | **Total Size** | 32 bits | 64 bits |
  | **Sign Bit (S)** | 1 bit | 1 bit |
  | **Exponent (E)** | 8 bits | 11 bits |
  | **Fraction (F)** | 23 bits | 52 bits |
  | **Bias** | **127** | **1023** |
#### **3. The Two "Tricks" of IEEE 754**
  To save space and make hardware faster, the standard uses two clever tricks:
  
  **Trick A: The "Hidden Bit" (Normalized Significand)**
  *   In scientific notation, the first digit is always non-zero (e.g., you write $1.5 \times 10^2$, not $0.15 \times 10^3$).
  *   In binary, the only non-zero digit is `1`.
  *   Therefore, *every* normalized number starts with `1.something`.
  *   **Optimization:** Since we *know* there is a `1` before the dot, **we don't store it**. We only store the "Fraction" (the part *after* the dot). When the hardware reads the number, it mentally adds the `1.` back.
  
  **Trick B: Biased Exponent**
  *   Exponents can be positive ($2^5$) or negative ($2^{-5}$).
  *   Usually, we use 2's Complement for negative numbers. However, sorting 2's Complement numbers is hard for hardware because negative numbers look like "large" numbers (leading 1s).
  *   **Optimization:** We use a **Bias**.
    *   **Single Precision Bias:** 127.
    *   If you want a real exponent of **0**, you store **127** ($0+127$).
    *   If you want **-1**, you store **126** ($-1+127$).
    *   If you want **+5**, you store **132** ($5+127$).
    *   *Result:* The stored exponent is always a positive unsigned integer (0 to 255), making it easy to sort.
#### **4. Conversion Examples**
  *Pay attention to this process. It is the most common exam problem.*
  
  **Example 1: Decimal to Binary Float**
  Convert **-0.75** to Single Precision.
  
  1.  **Determine Sign:** Negative, so $S = 1$.
  2.  **Convert to Binary Fixed Point:**
    $0.75 = 3/4 = 1/2 + 1/4 = 0.11_2$
  3.  **Normalize:** Move the dot until there is exactly one `1` to the left.
    $0.11_2 \rightarrow 1.1_2 \times 2^{-1}$
  4.  **Calculate Fraction:** Drop the leading `1`.
    Fraction = `.1` (padded with zeros: `1000...000`)
  5.  **Calculate Stored Exponent:**
    Real Exponent = $-1$.
    Stored = Real + Bias = $-1 + 127 = 126$.
    $126$ in binary = `0111 1110`.
  6.  **Assemble:**
    `1` (Sign) | `01111110` (Exp) | `10000000000000000000000` (Frac)
  
  **Example 2: Binary Float to Decimal (Slide 23)**
  Convert `11000000101000...` to Decimal.
  
  1.  **Sign:** `1` (Negative).
  2.  **Exponent:** `10000001`.
    $128 + 1 = 129$.
    Real Exponent = Stored - Bias = $129 - 127 = +2$.
  3.  **Significand:** Add the "Hidden 1" back to the fraction (`.01000...`).
    $1.01_2$
  4.  **Calculate:**
    $-1 \times 1.01_2 \times 2^2$
    Shift the dot 2 spots to the right: $-101_2$
    $101_2 = 5$.
    Result: **-5.0**
#### **5. Special Cases**
  Not every bit pattern represents a normal number. The hardware looks at the **Exponent** field to decide.
  
  | Exponent | Fraction | Meaning | Notes |
  | :--- | :--- | :--- | :--- |
  | **0** (00..00) | **0** | **Zero** | Can be +0 or -0. |
  | **0** (00..00) | **Non-Zero** | **Denormal** | Extremely small numbers. **No hidden bit** (Formula becomes $0.\text{Frac} \times 2^{1-\text{Bias}}$). |
  | **1 to 254** | **Any** | **Normal** | Standard numbers (Hidden bit is 1). |
  | **255** (11..11) | **0** | **Infinity** | Result of dividing by zero or overflow. Can be +Inf or -Inf. |
  | **255** (11..11) | **Non-Zero** | **NaN** | "Not a Number". Result of $0/0$ or $\sqrt{-1}$. |
  
  ---
### **Student "Check Your Understanding"**
  
  1.  **Bias Math:** If the stored exponent in a Single Precision number is `1000 0000` (128 decimal), what is the *actual* exponent ($2^x$)?
  2.  **Representation:** Why do we have a special representation for **Denormal** numbers (Exponent = 0)? What problem does it solve?
  3.  **Conversion:** Convert the number **1.5** into IEEE 754 Single Precision binary.
    *(Hint: $1.5 = 1 + 1/2$.)*