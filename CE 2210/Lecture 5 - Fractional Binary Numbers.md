## Fractional Binary Numbers:
  
  **Representing Numbers Less Than 1:**
  
  * In decimal, we use powers of 10 (10<sup>-1</sup> = 0.1, 10<sup>-2</sup> = 0.01, etc.)
  * In binary, we use powers of 2 (2<sup>-1</sup> = 0.5, 2<sup>-2</sup> = 0.25, etc.)
  * Conversion: Multiply the fractional part by 2 repeatedly, placing a "1" for each whole number result and a "0" for each fractional result.
  * Example: Converting 0.6875 to binary:
  * 0.6875 * 2 = 1.375 -> 1 (2<sup>-1</sup>)
  * 0.375 * 2 = 0.75 -> 0 (2<sup>-2</sup>)
  * 0.75 * 2 = 1.5 -> 1 (2<sup>-3</sup>)
  * 0.5 * 2 = 1 -> 1 (2<sup>-4</sup>)
  * Result: 0.6875 = 0.1011_2
  
  
  **Round-Off Errors:**
  
  * Base 10 to Base 2 conversion accuracy depends on the number of bits used.
  * Example:
  * 0.1111_2 = 0.9375
  * 0.1110_2 = 0.8750
  * Rounding is necessary with limited bits, but adding more bits can improve accuracy.
  * Example: 0.90625 can be represented as 0.11101_2 with 5 bits.
## Negative Numbers in Binary:
  
  **Sign Bit:**
  
  * Most Significant Bit (MSB) is used to indicate the sign (0 for positive, 1 for negative).
  * Issue: This leads to wasted bits (e.g., 000 = -0) and doesn't work well for addition/subtraction.
  
  
  **Complement Representation:**
  
  **2's Complement:**
  
  * Most common representation for negative numbers.
  * MSB represents the coefficient of the 2<sup>n</sup> term (e.g., 100_2 = -1 * 2<sup>2</sup> + 0 * 2<sup>1</sup> + 0 * 2<sup>0</sup>).
  * Add 1 to the result after inverting all bits (flip 0s and 1s).
  * This ensures consistent arithmetic operations.
  * Example:
    * 100_2 = -4
    * 101_2 = -3
    * 110_2 = -2
    * 111_2 = -1
    * 000_2 = 0
    * 001_2 = 1
    * 010_2 = 2
    * 011_2 = 3
  
  **r and (r-1)'s Complement:**
  
  * r's Complement: r<sup>n</sup> - N (n = number of integer digits)
  * (r-1)'s Complement: r<sup>n</sup> - r<sup>-m</sup> - N (m = number of fractional digits)
  * Relationship: r's complement = (r-1)'s complement + r<sup>-m</sup>
  
  **4-bit Examples:**
  
  * 1's Complement of 7_10:
    * 7_10 = 0111_2
    * 1's complement: 1000_2
  * 1's Complement of 5_10:
    * 5_10 = 0101_2
    * 1's complement: 1010_2
  * 2's Complement of 7_10:
    * 1's complement of 7_10 = 1000_2
    * 2's complement: 1000_2 + 1_2 = 1001_2
    * This represents -7_10.
## Closing Remarks:
  
  We now understand how to represent fractional binary numbers and negative numbers using complement methods. Next, we'll examine how mathematical operations are performed using 2's complement.