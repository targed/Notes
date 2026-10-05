- (Reminder on Professor's variables for this section: $n$ = bit array size, $m$ = number of hash functions, $|S|$ = number of items inserted).
  
  ---
- ### 1. The "Bits per Key" Variable ($b$)
  To make the math much cleaner, your professor introduces a new, highly intuitive variable on Slide 26: $b$.
  *   $b = n / |S|$ 
  *   **What it means:** The number of bits allocated in the array for every one item you insert. 
  *   *Example:* If you have an array of 1000 bits ($n$) and you insert 100 items ($|S|$), then $b = 10$. You have 10 bits of space per item.
- ### 2. The Simplified Bit Probability (Slide 26)
  In Part 3, we found the probability that a specific bit is a `1` is: $1 - (1 - 1/n)^{m|S|}$.
  Using the calculus limit approximation for $e$ (Euler's number), this ugly algebraic formula simplifies beautifully:
  *   **Probability a given bit is `1`:** $\mathbf{1 - e^{-m/b}}$
- ### 3. The False Positive Formula (Slide 27)
  A false positive happens when we query a key that is *not* in the set, but by sheer bad luck, all $m$ of its hash functions land on bits that are already set to `1`.
  
  Since the probability of one bit being a `1` is $(1 - e^{-m/b})$, the probability that all $m$ bits are `1` is that probability multiplied by itself $m$ times:
  *   False Positive Rate ($P_{fp}$): $\mathbf{(1 - e^{-m/b})^m}$
  
  ---
- ### 4. The Deep Dive: The "Goldilocks" Number of Hash Functions (Slides 28-29)
  Look at the False Positive formula. We have a tradeoff involving $m$ (number of hash functions):
  *   If $m$ is **too small** (e.g., $m=1$), we only check 1 bit. It's very easy for a single bit to accidentally be `1`. High false positive rate.
  *   If $m$ is **too large** (e.g., $m=100$), we throw so many darts during insertion that the entire array quickly fills up with `1`s. High false positive rate.
  
  **The Calculus Solution:**
  By taking the derivative of the formula and setting it to zero, we find the absolute optimal number of hash functions to minimize false positives:
  *   Optimal $m$: $\mathbf{m = b \ln(2)}$
  *   *Note:* $\ln(2) \approx 0.693$. So the optimal number of hash functions is roughly 70% of your "bits per key" ratio.
- ### 5. The CLRS Textbook Problem (Slides 31-33)
  This is a direct application of the math, and highly likely to resemble a quiz question.
  
  Step A: The Base Case ($b=8$)
  If we allocate 8 bits per item:
  *   Optimal $m = 8 \ln(2) \approx 5.54$ (We'd use 5 or 6 hash functions).
  *   Plug it back into the False Positive formula, which simplifies to: $(1/2)^{b \ln 2}$.
  *   $(0.5)^{8 \ln 2} = (0.5)^{5.54} \approx \mathbf{0.023}$ (or about **2.3% error rate**).
  
  Step B: The Quiz Question ($b=16$)
  "Suppose we were willing to use twice as much space (16 bits per insertion)... What can you say about the corresponding false positive rate?"
  *   Math: $(0.5)^{16 \ln 2} \approx \mathbf{0.000488}$.
  *   Convert to percentage: **0.0488%**.
  *   **Answer:** Less than 0.1%.
  
  **The Professor Deep Dive Takeaway:**
  Look at what happened when we **doubled** the space (from 8 bits to 16 bits). The error rate didn't just get cut in half—it went from ~2% down to ~0.04%. **Doubling the space squares the false positive probability!**
  
  ---
- ### Part 4 Practice Questions: Hashing & Bloom Filters Final Set
  
  To ensure you are fully prepared, here are 10 practice questions covering the advanced Hashing and Bloom Filter concepts.
  
  **Q1. The Bloom Filter Tradeoff**
  Which of the following statements about a standard Bloom Filter is mathematically guaranteed?
  a) False Positives are impossible.
  b) False Negatives are impossible.
  c) The time complexity of a lookup grows as the array gets full.
  d) Elements can be deleted in $O(1)$ time.
  
  **Q2. Optimal Hash Functions**
  You design a Bloom Filter with an array of 10,000 bits ($n = 10000$). You plan to insert 1,000 items ($|S| = 1000$). Using the optimal formula $m = b \ln(2)$, approximately how many hash functions should you use? (Assume $\ln(2) \approx 0.7$).
  a) 3
  b) 7
  c) 10
  d) 70
  
  **Q3. The "Bits per Key" Ratio**
  In the formula $P_{fp} = (1 - e^{-m/b})^m$, what does the variable $b$ represent?
  a) The number of bits set to 1 in the array.
  b) The base of the natural logarithm.
  c) The total size of the bit array divided by the total number of items inserted.
  d) The number of hash functions.
  
  **Q4. The Cost of Space**
  A Bloom Filter with $b=10$ bits per key has a false positive rate of roughly $1\%$. If you increase the array size so that $b=20$ bits per key, what will the new approximate false positive rate be?
  a) $0.5\%$
  b) $0.1\%$
  c) $0.01\%$ (Because doubling $b$ squares the probability: $0.01 \times 0.01$)
  d) It remains $1\%$ unless you change the hash functions.
  
  **Q5. Cuckoo Hashing Lookup Time**
  Why does Cuckoo Hashing provide a strict $O(1)$ worst-case lookup time, while Open Addressing (Linear Probing) only provides $O(1)$ average-case lookup time?
  a) Cuckoo Hashing uses balanced binary trees for its buckets.
  b) Cuckoo Hashing limits the number of possible locations for any key to exactly two.
  c) Cuckoo Hashing never experiences collisions.
  d) Linear probing requires expensive mathematical modulo operations.
  
  **Q6. Cuckoo Hashing Infinite Loops**
  In Cuckoo Hashing, inserting a new element can cause a chain reaction of evictions. How do we prevent this chain reaction from becoming an infinite loop?
  a) We allow three items to share one nest.
  b) We switch to Linear Probing after 5 collisions.
  c) We track the number of evictions; if it hits a MAX threshold, we stop, resize the tables, and rehash everything with new functions.
  d) We delete the oldest item in the table.
  
  **Q7. Hardware Acceleration**
  Slide 10 notes an "unusual property" of Bloom Filters regarding hardware implementation. What is it?
  a) The bit array fits entirely in the L1 CPU cache.
  b) Because the $m$ hash functions are completely independent, they can all be calculated in parallel by the hardware.
  c) It does not require RAM.
  d) It uses bitwise XOR instead of modulo compression.
  
  **Q8. The Database Application**
  Why is a Bloom Filter useful as a "front-end" for a large disk-based database like Postgres or Cassandra?
  a) It compresses the actual database records to save disk space.
  b) It provides a fast way to definitively prove a record does **not** exist, saving a costly disk read.
  c) It automatically sorts the data as it enters the database.
  d) It guarantees that the database will never return a false positive to the end-user.
  
  **Q9. High Load Factors**
  What happens to a Cuckoo Hash table if the Load Factor exceeds 50%?
  a) The lookup time degrades to $O(N)$.
  b) The probability of eviction cycles (infinite loops) during insertion increases dramatically.
  c) It automatically converts into a Bloom Filter.
  d) The hash functions begin returning negative indices.
  
  **Q10. The Mathematical Limit**
  The derivation of the False Positive rate relies on the approximation $(1 - 1/x)^x \approx e^{-1}$ as $x$ gets very large. In the context of the Bloom Filter dartboard analogy, what variable acts as $x$ (the value approaching infinity)?
  a) The number of hash functions ($m$).
  b) The number of items ($|S|$).
  c) The size of the bit array ($n$).
  d) The false positive probability.
  
  ---
- ### **Solutions & Explanations**
  
  **Q1: b) False Negatives are impossible.** (If the bits aren't 1, the item was never inserted. Period.)
  **Q2: b) 7** ($b = 10000/1000 = 10$. Optimal $m = 10 \times 0.7 = 7$).
  **Q3: c) The total size of the bit array divided by the total number of items inserted.**
  **Q4: c) 0.01%** (As demonstrated in the CLRS problem on Slide 33, doubling $b$ mathematically squares the fraction $(1/2)^{b \ln 2}$).
  **Q5: b) Cuckoo Hashing limits the locations...** (You only ever check `T1[h1(x)]` and `T2[h2(x)]`. Exactly 2 checks, giving worst-case $O(1)$).
  **Q6: c) Track evictions, stop, resize, and rehash.**
  **Q7: b) Hash functions can be calculated in parallel.**
  **Q8: b) It prevents costly disk reads for non-existent records.** (If the filter says False, you don't touch the disk).
  **Q9: b) Probability of infinite loops increases.** (This is why Slide 54 explicitly says "Load factor kept below 50%").
  **Q10: c) The size of the bit array ($n$).** (As the array size $n$ gets massive, the approximation $1 - 1/n$ fits the limit definition of $e$).
  
  ---