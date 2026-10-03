## Signed Multiplication:
  
  * **Review of Unsigned Multiplication:** Similar to decimal multiplication, but with binary digits.
  * **Signed Multiplication:** Find the product of the absolute values and then determine the sign based on the signs of the input numbers.
  * **Example: (-3) * (+4) = -12**
  * 3 = 11, 4 = 100 (binary)
  * 11 * 100 = 1100 = 12 (decimal)
  * Since signs are negative and positive, the product is negative.
  * Find the 2's complement of 1100 (using 5 bits to accommodate the range -16 to +15):
      * 01100 -> 10011 (1's comp) -> 10100 (2's comp) = -12
## Unsigned Division:
  
  * **Review of Unsigned Division:** Similar to decimal long division.
## 2's Complement (Signed) Division:
  
  * Similar to signed multiplication. Divide using absolute values, then determine the sign.
  * **Example: -10 / 3 = -3 with remainder -1**
    * 10 = 1010, 3 = 11 (binary)
    * Quotient = 11, remainder = 1 (binary)
    * Find the 2's complement of the quotient and remainder to get the signed results.
        * Quotient: 011 -> 100 (1's comp) -> 101 (2's comp) = -3
        * Remainder: 01 -> 10 (1's comp) -> 11 (2's comp) = -1
## Extending Unsigned and Signed Binary Integers:
  
  * **Unsigned Extension (Zero-Extension):** Pad with leading zeros when increasing the word size.
    * Example: 0101 (4 bits) -> 0000 0101 (8 bits)
  * **Signed Extension (Sign-Extension):** Replicate the sign bit when increasing the word size.
    * Example: 1010 (4 bits) -> 1111 1010 (8 bits)
## Closing Remarks:
  
  We've learned how to perform signed multiplication and division using 2's complement. Next, we'll explore 2's complement with fractions and discuss other binary encoding schemes.