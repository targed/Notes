## Introduction:
  
  Digital systems primarily use binary numbers.
  Binary numbers have only two possible values: 0 and 1.
  A single binary digit is called a bit.
  Binary is ideal for digital systems because:
  * Transistors, the basic digital components, operate using two voltage levels.
  * Storing and transmitting one of two values is simpler than handling more possibilities.
  * Binary is useful for encoding schemes, despite its limited values.
## Base 10 Encoding (Decimal System):
  
  * Uses ten symbols: 0, 1, 2, ..., 8, 9.
  * Each position represents a power of 10.
  * Familiar system due to our ten fingers.
## Base 2 Encoding (Binary System):
  
  * Uses two symbols: 0 and 1.
  * Each position represents a power of 2.
  * Ideal for computers based on transistors.
## Encoding Example:
  
  Binary numbers can be expressed as a sum of powers of 2:
  X = … + C5 * 2^5 + C4 * 2^4 + C3 * 2^3 + C2 * 2^2 + C1 * 2^1 + C0 * 2^0
  Cn represents either 0 or 1 in the binary number.
  
  To convert binary to decimal:
  * Multiply each power of two by its coefficient.
  * Sum the results.
  
  Example:
  Binary number: 101010
  Decimal value: X = 42
## Binary Encoding Symbols – ASCII:
  
  * ASCII (American Standard Code for Information Interchange) uses 7 or 8 bits to encode letters, numbers, and symbols.
  * Unicode is a more recent 16-bit encoding system.
  * Unicode supports characters from various languages.
## Where Do We Start with Math in Digital?
  
  Base conversion is essential in digital systems.
  Conversions are typically performed between Base 10 and Base 2.
  Other important base systems:
  * Octal (Base 8)
  * Hexadecimal (Base 16)
  
  Converting from Base 2, 8, or 16 to Base 10 is straightforward:
  * Multiply each coefficient with its respective base raised to the power.
  * Sum the terms.
## Closing Remarks:
  
  We can now represent numbers and symbols using binary.
  The next step is to learn about converting from other number systems to binary.