### **6.2 Error-Detection and Correction Techniques**

When bits are transmitted over a physical medium, they are subject to noise and interference, which can cause some bits to flip (a 0 becomes a 1, or a 1 becomes a 0). The primary goal of these techniques is to allow the receiver to detect, and in some cases correct, these errors.

**The Basic Approach: Redundancy**

All error detection and correction schemes work by adding extra bits to the data. These extra bits are called **Error Detection and Correction (EDC)** bits.

*   **D:** The data bits we want to protect (e.g., the datagram).
*   **EDC:** The redundant bits calculated from D.
*   The sender transmits both D and EDC.
*   The receiver gets D' and EDC' (which may have errors). It performs a check. If the check fails, an error is detected.

It's important to note that **no error detection scheme is 100% perfect**. There is always a small probability that bit errors can occur in such a way that they go undetected. However, more powerful schemes can make this probability vanishingly small.

---
- ### **1. Parity Checking**
  
  This is the simplest form of error detection.
- #### **Single Bit Parity**
  *   **Mechanism:** The sender adds a single **parity bit** to the end of a block of data (e.g., a byte).
  *   **Even Parity:** The sender sets the parity bit to either 0 or 1 to ensure that the total number of '1's in the data plus the parity bit is **even**.
  *   **Odd Parity:** The sender ensures the total number of '1's is **odd**.
  *   **Receiver Check:** The receiver counts the number of '1's in the received data plus the parity bit. If it's using even parity and the count is odd, it knows an error has occurred.
  *   **Limitation:** Single bit parity can only detect an **odd number of bit errors**. If two bits flip, the parity check will still pass, and the error will go undetected.
- #### **Two-Dimensional Parity**
  This is a more powerful scheme that can not only detect but also **correct single bit errors**.
  
  *   **Mechanism:**
    1.  The data bits are arranged in a 2D grid (e.g., 7 bytes of 7 bits each).
    2.  A single parity bit is computed for each **row**.
    3.  A single parity bit is also computed for each **column**.
    4.  The sender transmits the data along with both the row and column parity bits.
  *   **Receiver Check:**
    1.  The receiver re-computes the parity for each row and column.
    2.  If it finds a row and a column whose computed parity does not match the received parity bit, the intersection of that row and column is exactly where the single bit error occurred.
    3.  Knowing the location of the error, the receiver can simply **flip the bit** to correct it, without needing to request a retransmission.
  
  ---
- ### **2. Internet Checksum**
  
  This is the checksum algorithm used by the transport layer (UDP and TCP) and the network layer (IP header). It is also a form of error detection.
  
  *   **Mechanism (Recap):**
    1.  The data is treated as a sequence of 16-bit integers.
    2.  These integers are added together using **1's complement arithmetic**.
    3.  The checksum is the 1's complement (the bitwise NOT) of the final sum.
  *   **Primary Purpose:** It is designed to be simple and fast to compute in software.
  *   **Limitation:** Like parity, it is a relatively weak error-detection scheme and can miss certain types of errors.
  
  ---
- ### **3. Cyclic Redundancy Check (CRC)**
  
  CRC is a much more powerful and robust error-detection technique. It is the standard used in many modern link-layer technologies, including **Ethernet** and **Wi-Fi**.
  
  *   **Core Idea:** Treats the data bits as a representation of a polynomial and uses polynomial division. The math is done using binary arithmetic (modulo 2).
  
  *   **Mechanism:**
    1.  Both the sender and receiver agree on a fixed, `r+1`-bit pattern called the **Generator (G)**.
    2.  The sender wants to send `d` bits of data, which we'll call **D**.
    3.  The sender's goal is to compute an `r`-bit set of CRC bits, which we'll call **R**, such that the combined `d+r` bits (`<D,R>`) are exactly divisible by G (using modulo-2 arithmetic).
    4.  **Calculation of R:** The value of R is the **remainder** of the division:
        $ R = \text{remainder} \left( \frac{D \cdot 2^r}{G} \right) $
        This is done using long division with XOR operations instead of subtraction.
    5.  **Transmission:** The sender appends the calculated `r`-bit remainder `R` to the original data `D` and transmits the `<D,R>` frame.
    6.  **Receiver Check:** The receiver gets the `<D,R>` frame. It divides the entire received frame by the same generator G.
        *   If the remainder is **zero**, the receiver assumes the frame is error-free.
        *   If the remainder is **non-zero**, an error is detected, and the frame is typically dropped.
  
  *   **Power of CRC:** The properties of the generator polynomial G determine the power of the CRC. A well-chosen generator can detect:
    *   All single-bit errors.
    *   All double-bit errors.
    *   Any odd number of bit errors.
    *   All "burst" errors (a sequence of consecutive flipped bits) of length less than or equal to `r`.