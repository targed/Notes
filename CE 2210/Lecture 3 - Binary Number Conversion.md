## Useful Information:
  
  Memorize powers of 2: 1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096.
## Converting Binary to Octal and Hexadecimal:
  
  **Octal:**
  
  * Group binary digits in sets of three.
  * Example: 100010_2 -> 100 010 -> 42_8
  
  **Hexadecimal:**
  
  * Group binary digits in sets of four (pad with leading zeros if needed).
  * Use digits 0-9 and letters A-F.
  * Example: 100010_2 -> 0010 0010 -> 22_16
## Why Easy Conversion:
  
  Octal and hexadecimal represent all possible values of three and four binary digits respectively.  This allows for direct translation between the systems.
## Converting Decimal (Base 10) to Binary:
  
  **1. Polynomial Method:**
  
  * Convert a decimal number to its equivalent in another base using powers of the target base.
  * Example: Converting 156 to Binary:
    * 256 > 156, so place 0 in the 256 position.
    * 128 < 156, so place 1 in the 128 position (156 - 128 = 28).
    * Continue this process for each power of 2.
  * Result: 156 = 10011100_2
  
  **2. Divide by 2 Method:**
  
  * Divide the decimal number by 2 repeatedly.
  * The remainder form the binary representation in reverse order.
  * Example: Converting 156 to Binary:
    * 156 / 2 = 78 r 0
    * 78 / 2 = 39 r 0
    * 39 / 2 = 19 r 1
    * ...
  * Result: 156 = 10011100_2
## Converting Decimal to Hexadecimal:
  
  **Polynomial Method:**
  
  * Use powers of 16.
  * Example: Converting 156 to Hexadecimal:
    * 16<sup>1</sup> > 156, but 9 * 16 = 144. So, place 9 in the 16<sup>0</sup> position (156 - 144 = 12).
    * 12 = C_16, so place C in the 16<sup>1</sup> position.
  * Result: 156 = 9C_16
## Closing Remarks:
  
  We can now convert between various number systems and binary.  Next, we'll explore basic mathematical operations on binary numbers, including addition, subtraction, multiplication, and division.