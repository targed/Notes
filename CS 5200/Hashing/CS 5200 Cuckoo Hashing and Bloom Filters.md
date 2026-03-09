### 1. Cuckoo Hashing (Slides 41–54)
**The Problem it Solves:** In high-performance systems (like network routers looking up IP addresses in nanoseconds), you cannot afford the $O(n)$ worst-case lookup time that comes from Linear Probing or Double Hashing clusters. You need an absolute, ironclad guaranteed $O(1)$ worst-case lookup.

**The Logic:**
Instead of one array, we use **Two Hash Tables** (or one table logically split), each with its own completely different hash function ($h_1$ and $h_2$).
*   **Rule:** A key $k$ *must* reside in either `Table1[h1(k)]` or `Table2[h2(k)]`. It cannot be anywhere else.

**The Insertion Algorithm (The "Cuckoo" behavior):**
1.  Calculate $h_1(x)$. If `Table1` is empty, put $x$ there. Done.
2.  If `Table1` is full (let's say element $y$ is there), **evict** $y$ and put $x$ in its place.
3.  Now $y$ is homeless. Calculate $h_2(y)$ and put $y$ into `Table2`.
4.  If that spot in `Table2` is taken (let's say by $z$), evict $z$ and put $y$ there.
5.  Now $z$ is homeless. Calculate $h_1(z)$ and bounce it back to `Table1`.
6.  Repeat until everyone has a home.

**The Infinite Loop Problem (Slide 54):**
If the table gets too full, the elements will bounce back and forth between the two tables forever in an infinite cycle.
*   **The Fix:** Keep track of how many "bounces" happen. If it exceeds a `MAX` threshold, stop. This means the table is too congested.
*   You must then **Rehash** everything into a larger table with new hash functions.
*   To prevent this from happening frequently, Cuckoo Hashing requires a low **Load Factor**, keeping the tables strictly **below 50% full**.

**Complexity:**
*   **Lookup:** **$O(1)$ worst case.** You literally just check two memory addresses.
*   **Insert:** $O(1)$ expected/amortized, but $O(n)$ worst-case if it triggers a rehash.

---
- ### 2. Bloom Filters (Slides 3)
  **The Problem it Solves:** Sometimes your data is so massive it cannot even fit in a Hash Table in RAM, but you still need to quickly check if an item exists (e.g., checking a URL against a database of 1 billion malicious websites).
  
  **The Logic:**
  A Bloom Filter is a probabilistic data structure. It uses an array of **bits** (0s and 1s) instead of storing the actual keys.
  1.  Start with a bit array of size $m$, all set to 0.
  2.  Have $k$ different hash functions.
  3.  **Insert:** Run the key through all $k$ hash functions. If they output indices 3, 7, and 12, flip the bits at indices 3, 7, and 12 to **1**.
  4.  **Lookup:** Run the key through all $k$ hash functions. Check the bits at those indices.
    *   If *any* of those bits are 0 $\rightarrow$ The item is **DEFINITELY NOT** in the set.
    *   If *all* of those bits are 1 $\rightarrow$ The item is **PROBABLY** in the set.
  
  **The Tradeoff: False Positives**
  Because multiple different keys might happen to overlap and flip the same bits to 1, the filter might tell you a key exists when it actually doesn't. This is a **False Positive**.
  *   **Crucial Rule:** Bloom filters *never* have False Negatives. If it says the item is not there, it is mathematically guaranteed to not be there.
  
  **Why use it?**
  It saves massive amounts of space. Storing 1 billion URLs in a Hash Table might take 50 GB of RAM. A Bloom Filter with a 1% false-positive rate can represent that same set using only a few hundred Megabytes.
  
  **Applications (Slide 3):**
  *   **Web Browsers:** Quick check if a site is a known phishing site.
  *   **Bitcoin Wallets:** SPV nodes use them to request only relevant transactions without downloading the whole blockchain.
  *   **Databases:** Checking if a row exists before doing an expensive disk read.
  
  ---
- ### Practice Questions
  
  **Q1: Cuckoo Eviction Trace**
  You have two tables, $T_1$ and $T_2$.
  $h_1(x) = x \pmod 5$
  $h_2(x) = (x / 5) \pmod 5$
  Currently, $T_1[2]$ holds the value `12`.
  You insert the value `7`.
  1. What index in $T_1$ does `7` map to?
  2. A collision occurs. `12` is evicted. What index in $T_2$ is `12` moved to?
  
  **Q2: The Cuckoo Tradeoff**
  Cuckoo Hashing provides an absolute $O(1)$ worst-case lookup time, but it comes at a significant cost regarding memory efficiency. What is that cost?
  
  **Q3: Bloom Filter Certainty**
  You check a Bloom Filter to see if the username "Charlie" is taken. The filter returns "False". Can you safely allow the user to register the name "Charlie" without checking the main database? Why or why not?
  
  **Q4: Bloom Filter Deletion**
  Can you remove/delete an item from a standard Bloom Filter? Why or why not? *(Hint: Think about what happens if you flip a 1 back to a 0).*