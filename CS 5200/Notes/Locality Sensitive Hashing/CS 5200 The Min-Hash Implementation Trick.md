### 1. The Problem with Literal Shuffling (Slide 38)
Imagine our Boolean matrix has 10 million rows (all the unique shingles on the internet). 
To create a signature of 100 integers for a document, the theory says we must randomly shuffle an array of 10 million items **100 different times**.
*   **The Bottleneck:** Generating 100 permutations of 10 million items takes massive amounts of CPU time and RAM. It defeats the entire purpose of trying to be faster than $O(N^2)$ brute force!
- ### 2. The Solution: Universal Row Hashing
  Instead of physically shuffling the rows, we use **Hash Functions** to *simulate* a shuffle on the fly.
  
  **The Concept:**
  We want 100 random permutations. So, we simply pick **100 independent hash functions** ($h_1, h_2, \dots h_{100}$). 
  *   We use a simple Universal Hashing formula: $h(x) = (ax + b) \pmod p$
    *(Where $x$ is the row index, $a$ and $b$ are random integers, and $p$ is a prime number larger than the total number of rows $N$).*
  
  **How it Simulates a Shuffle:**
  Instead of moving Row 5 to a new position, we just plug `5` into the hash function.
  If $h_1(5) = 42$, it means in our "imaginary" shuffled matrix, Row 5 is now sitting at position 42.
- ### 3. The One-Pass Algorithm (The "Deep Dive" Mechanics)
  This is the exact algorithm your professor wants you to know. It computes the entire 100-integer signature for a document in a **single pass** over its data.
  
  **The Setup:**
  1.  We have a document $C$.
  2.  We initialize its Signature array `SIG(C)` of length 100 to all **Infinity ($\infty$)**.
  3.  We only care about the rows where the document actually has a `1` (the shingles it actually contains).
  
  **The One-Pass Scan:**
  For every row $R$ where Document $C$ has a `1`:
  1.  Run row index $R$ through all 100 hash functions: $h_1(R), h_2(R), \dots h_{100}(R)$.
  2.  For each hash function $i$ (from 1 to 100):
    *   If $h_i(R) < \text{SIG}(C)[i]$, update the signature: $\text{SIG}(C)[i] = h_i(R)$.
  
  **Why does this work?**
  Because we initialized everything to Infinity, we are constantly keeping the **minimum** hash value we've ever seen for that specific hash function. This perfectly mimics scanning down a randomly shuffled matrix and stopping at the very first `1` we hit! 
  
  We achieved Min-Hashing without ever building or shuffling the massive Boolean matrix. We just hashed the non-zero rows on the fly.
  
  ---
- ### Part 3 Practice Questions (Implementation Mechanics)
  
  **Q1: The Universal Hash Trap**
  You are choosing $p$ for your hash function $h(x) = (ax + b) \pmod p$. Your dataset has $N = 10,000$ unique shingles (rows). 
  Which of the following is a valid choice for $p$?
  A) $p = 5,000$
  B) $p = 10,000$
  C) $p = 10,009$ (A prime number $> N$)
  D) $p = 2$
  
  **Q2: Tracing the One-Pass Algorithm**
  Document $C$ contains shingles at row indices **3** and **7**.
  You are generating a signature using 2 hash functions:
  *   $h_1(x) = (2x + 1) \pmod 11$
  *   $h_2(x) = (3x + 2) \pmod 11$
  
  Your signature is initialized to $[\infty, \infty]$.
  1.  Process Row 3. What is the updated signature?
  2.  Process Row 7. What is the final signature for Document $C$?
  
  **Q3: The Efficiency Gain**
  If a document contains only 500 unique words out of a total possible dictionary of 1,000,000 words, how many times does the One-Pass algorithm evaluate a hash function to build a signature of length 100?
  A) 1,000,000 $\times$ 100
  B) 500 $\times$ 100
  C) 100
  D) 500
  
  ---
-