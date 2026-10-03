## 1's Complement with Fractions:
  
  * Formula: r<sup>n</sup> - r<sup>-m</sup> - N (r = 2, n = number of integer digits, m = number of fractional digits)
  * Example: N = 4.375 = 0100.011 (n = 4, m = 3)
  * r<sup>n</sup> - r<sup>-m</sup> = 2<sup>4</sup> - 2<sup>-3</sup> = 16 - 0.125 = 15.875 = 1111.111 (binary)
  * 1's complement: 1111.111 - 0100.011 = 1011.100 (binary)
  * Rule: Flip the bits of the original number.
## 2's Complement with Fractions:
  
  * Formula: Same as 1's complement, but add 2<sup>-m</sup> instead of 1.
  * Example: N = 4.375 = 0100.011 (n = 4, m = 3)
    * 1's complement: 1011.100
    * 2's complement: 1011.100 + 0.001 = 1011.101 (binary) = -4.375 (decimal)
## Math with 2's Complement Fractions:
  
  * Example: 14.25 - 20.75 = -6.50 (n = 8, m = 2)
    * 14.25 = 00001110.01
    * 20.75 = 00010100.11
    * 1's(20.75) = 11101011.00
    * 2's(20.75) = 11101011.01
    * 00001110.01 + 11101011.01 = 11111001.10 = -6.50 (decimal)
## Other Encoding Schemes:
  
  **Binary Coded Decimal (BCD):**
  
  * Codes only numbers 0-9.
  * Used in applications where output needs to be in decimal form.
  * Addition: Overflow occurs when the sum is greater than 9. Add 6 to correct the result.
  
  **Gray Code:**
  
  * Useful for encoding rotating systems (e.g., sensor readings).
  * Only one bit changes between successive code words.
  * Construction:
    * 1-bit Gray Code: 0, 1
    * n-bit Gray Code: Append a 0 to the left of the (n-1)-bit Gray Code, then append a 1 to the left of the reversed (n-1)-bit Gray Code.
## Closing Remarks:
  
  We've explored 2's complement with fractions and learned about alternative encoding schemes like BCD and Gray Code. Next, we'll delve into the fundamentals of Boolean Logic and Algebra, forming the foundation of digital logic.