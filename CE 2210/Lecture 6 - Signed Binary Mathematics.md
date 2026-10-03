## 2's Complement Math:
  
  * **Word Length:** Specifies the number of bits used to represent a number.
  * **Example (n=4):**
  * 3 = 0011_2
  * 6 = 0110_2
  * 1's(6) = 1001_2
  * 2's(6) = 1010_2
  * 1101_2 = -1 * 2<sup>3</sup> + 1 * 2<sup>2</sup> + 0 * 2<sup>1</sup> + 1 * 2<sup>0</sup> = -8 + 4 + 0 + 1 = -3
  * **Addition:** To subtract a number, add its 2's complement.
  * **Example: 3 - 6 = -3**
  * 3 = 0011_2
  * 2's(6) = 1010_2
  * 0011_2 + 1010_2 = 1101_2 = -3
## Overflow:
  
  * **Issue:** When the result of a signed operation exceeds the range of the word length, overflow occurs.
  * **Example: 7 + 1 = 8 (using 4-bit words)**
    * 7 = 0111_2
    * 1 = 0001_2
    * 0111_2 + 0001_2 = 1000_2 = -8 (in 2's complement)
  * **Word Length Limitation:** 4-bit words can represent numbers from -8 to +7.
  * **Solution:** Use a larger word length to accommodate a wider range of numbers.
## 8-bit Examples:
  
  * **Example 1: 61 - 38 = 23**
    * 61 = 00111101
    * 38 = 00100110
    * 2's(38) = 11011010
    * 00111101 + 11011010 = 100010111 (drop the 9th bit) = 00010111 = 23
  * **Example 2: 47 - 56 = -9**
    * 47 = 00101111
    * 56 = 00111000
    * 2's(56) = 11001000
    * 00101111 + 11001000 = 11110111 = -9
  * **Example 3: 19 - 120 = -101**
    * 19 = 00010011
    * 120 = 01111000
    * 2's(120) = 10001000
    * 00010011 + 10001000 = 10011011 = -101
## Another Pitfall:
  
  * **Example: -1 - 128 = -129**
    * -1 = 11111111
    * 128 = 10000000 (Note: 128 cannot be represented in 8-bit signed 2's complement.  It would overflow)
    * Assuming you meant -1-127
    * 127 = 01111111
    * 2's complement of 127 = 10000001
    * 11111111 + 10000001 = 101111110 (Discarding the carry bit which signals an overflow) = 01111110 = 126 (incorrect due to overflow)
  
  * **Issue:** Overflow occurs when the sign bit changes during the operation, even if the result *appears* to be within the range of the word length.
## Closing Remarks:
  
  We've explored signed addition and subtraction using 2's complement. Next, we'll cover signed multiplication and division with 2's complement and discuss other binary encoding schemes.