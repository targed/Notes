- ***NOTATION ALERT ***
  In the previous slides, $m$ was the array size and $k$ was the number of hash functions. In this math section (starting on Slide 20), your professor switches the variables!*
  *   $n$ = Size of the bit array (the dartboard).
  *   $m$ = Number of hash functions (the darts).
  *   $|S|$ = Number of elements inserted into the set.
  *Be very careful to read how the variables are defined on your quiz! We will use the professor's Slide 20 notation for this section.*
  
  ---
- ### 1. Probability Basics (Slides 14–18)
  To build the formula, we need two fundamental rules of probability:
  1.  **The "NOT" Rule:** If the probability of an event happening is $P$, the probability of it *not* happening is $1 - P$.
    *   *Example:* Probability of rolling a 4 is $1/6$. Probability of *not* rolling a 4 is $1 - 1/6 = 5/6$.
  2.  **The "AND" Rule (Independent Events):** To find the probability of multiple independent events happening together, you multiply their individual probabilities.
    *   *Example:* Rolling a 4 AND then an odd number = $(1/6) \times (1/2) = 1/12$.
- ### 2. The Dart Analogy (Slides 21–24)
  Imagine the bit array (of size $n$) is a dartboard divided into $n$ equal slices. 
  Every time a hash function outputs an index, it's like throwing a dart at the board.
  
  **Step A: One Dart**
  *   What is the probability that a single dart hits a specific slice (e.g., Index 0)? $1/n$.
  *   What is the probability that a single dart **misses** Index 0? $1 - 1/n$.
  
  **Step B: All Darts for One Item**
  *   When we insert one item into the Bloom Filter, we run it through $m$ hash functions. That means we throw $m$ darts.
  *   What is the probability that all $m$ darts miss Index 0? 
    *   (Miss 1) AND (Miss 2) AND ... (Miss $m$)
    *   $(1 - 1/n) \times (1 - 1/n) \dots = \mathbf{(1 - 1/n)^m}$.
  
  **Step C: All Darts for All Items**
  *   We don't just insert one item; we insert $|S|$ items into the filter.
  *   Total darts thrown = (Darts per item) $\times$ (Total items) = $m|S|$.
  *   What is the probability that Index 0 survives the entire process and **remains a `0`**?
    *   It must be missed by *every single dart* thrown.
    *   **Probability a bit is 0:** $\mathbf{(1 - 1/n)^{m|S|}}$.
  
  **Step D: The Probability of a `1`**
  *   If the probability of a bit remaining `0` is $(1 - 1/n)^{m|S|}$, we use the "NOT" rule to find the probability that it gets hit at least once and becomes a `1`.
  *   **Probability a bit is 1:** $\mathbf{1 - (1 - 1/n)^{m|S|}}$.
  
  ---
- ### 3. The Calculus Shortcut (Slide 25)
  The formula $1 - (1 - 1/n)^{m|S|}$ is mathematically clunky. Your professor uses a famous calculus approximation involving Euler's number ($e \approx 2.718$).
  
  There is a fundamental limit in calculus:
  $$ \lim_{x \to \infty} \left(1 - \frac{1}{x}\right)^x = \frac{1}{e} = e^{-1} $$
  
  By applying this limit to our dart formula (because $n$, the array size, is usually very large), the ugly algebraic formula beautifully simplifies to an exponential equation involving $e$. We will see the exact result of this simplification in Part 4!
  
  ---
- ### Part 3 Practice Questions
  
  **Q1: The Total Darts**
  You have a Bloom Filter with a bit array of size 1000. You use 4 hash functions. You insert 50 different words into the filter.
  How many total "darts" have been thrown at the bit array?
  A) 1000
  B) 200
  C) 50
  D) 4
  
  **Q2: The "Miss" Probability**
  Using the numbers from Q1 (Array size $n=1000$, 4 hash functions, 50 items), what is the exact mathematical probability that the very last bit in the array (Index 999) is still a `0`?
  A) $(1/1000)^{200}$
  B) $1 - (1/1000)^{200}$
  C) $(1 - 1/1000)^{200}$
  D) $(1 - 1/1000)^{50}$
  
  **Q3: The Independence Assumption**
  Slide 19 states an assumption: "Uniform distribution of keys in the bit array." What does this mean in the context of the Dart Analogy?
  A) Every dart throw is perfectly aimed at the center.
  B) A dart is equally likely to hit any of the $n$ slices, and previous throws do not affect future throws.
  C) You cannot hit the same slice twice.
  D) The number of darts equals the number of slices.
  
  **Q4: The "NOT" Rule Application**
  If the probability that a specific bit remains `0` after all insertions is $0.25$ (or 25%), what is the probability that the bit is a `1`?