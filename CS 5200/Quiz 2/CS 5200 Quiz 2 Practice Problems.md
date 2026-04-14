### **Practice Quiz: Vector Search, Jaccard, and MinHashing**

**Question 1: Calculating Jaccard Similarity and Distance**
You are comparing the purchase history of two users to recommend products.
*   User A bought: `{Laptop, Mouse, Keyboard, Monitor}`
*   User B bought: `{Mouse, Monitor, Headphones}`
1. What is the exact Jaccard Similarity of User A and User B?
2. What is their Jaccard Distance?

**Question 2: Weighted Jaccard (Histograms)**
Instead of just boolean sets, you are comparing the frequency of words in two documents (Feature Vectors).
*   Doc 1 Vector: `[2, 0, 4, 1]`
*   Doc 2 Vector: `[1, 3, 2, 1]`
Calculate the **Weighted Jaccard Similarity** for these two vectors.

**Question 3: K-Shingling**
You are processing the word `"ALGORITHM"`. 
1. Write out the exact mathematical Set of **3-shingles** (character-level) for this word.
2. If you were analyzing a 1,000-page book and chose $k=1$ (1-shingles), why would Jaccard Similarity completely fail to find meaningful similarities between it and other books?

**Question 4: Manual Min-Hash Trace**
You have a boolean matrix representing 2 documents (Cols A and B) and 4 possible shingles (Rows 1-4).
```text
Row 1: A=1, B=0
Row 2: A=0, B=1
Row 3: A=1, B=1
Row 4: A=0, B=0
```
You apply a random permutation function $\pi$ that shuffles the rows into this order: **[Row 4, Row 2, Row 3, Row 1]**.
1. What is the Min-Hash value for Document A based on this permutation?
2. What is the Min-Hash value for Document B?
3. Do their signatures collide (match) for this specific hash function?

**Question 5: MinHash Probability**
You have two massive documents that share exactly 60% of their text (Jaccard Similarity = 0.60).
You compress them using MinHashing to generate signatures of length **200** (using 200 independent hash functions).
Exactly how many of the 200 integers in Document A's signature do you *expect* to perfectly match Document B's signature?

**Question 6: The "One-Pass" Implementation Trick**
In a production system, physically shuffling a matrix of 10 million rows is computationally impossible. 
Instead of shuffling, we simulate the process by hashing the row indices. Explain the logic of how we build a MinHash signature for a document in a **single pass**, assuming we initialize the signature array with `INFINITY`.

**Question 7: Calculating the S-Curve Threshold**
You are tuning your LSH pipeline. You decide to split your MinHash signatures into **$b = 20$ bands**, where each band has **$r = 5$ rows** (functions). 
Using the approximation formula, what is the similarity threshold ($s$) at which the S-curve steepens? 
*(Hint: You may leave the answer as a fraction/exponent if you don't have a calculator, but try to estimate it).*

**Question 8: Tuning LSH (False Negatives)**
Your current LSH system for finding plagiarized documents is returning a very clean list of candidates (almost no False Positives), but you discover that it is **missing** several documents that are genuinely plagiarized (High False Negatives). 
To catch these missing documents, how should you adjust your bands ($b$) and rows ($r$)?
A) Increase $b$ (bands) and decrease $r$ (rows).
B) Decrease $b$ (bands) and increase $r$ (rows).
C) Increase $r$ (rows) without changing $b$.

**Question 9: The LSH Probability Formula**
Two documents have a true Jaccard Similarity of $s = 0.5$. Your LSH system uses $b = 10$ bands and $r = 2$ rows.
Write out the mathematical equation to find the exact probability that these two documents will be flagged as a Candidate Pair. *(You do not need to solve the final arithmetic).*

**Question 10: The Mandatory Final Step**
Your LSH algorithm successfully hashes 5 million documents and flags 1,200 "Candidate Pairs" that hashed to the same buckets. 
What is the mandatory final step of the LSH pipeline that must be performed on these 1,200 pairs, and why is it necessary?

---
---
- ### **Solutions & Explanations**
  
  **Answer 1: Jaccard Similarity and Distance**
  1.  **Similarity:** Intersection size is 2 (`Mouse, Monitor`). Union size is 5 (`Laptop, Mouse, Keyboard, Monitor, Headphones`). Similarity = $\mathbf{2/5}$ (or 0.4).
  2.  **Distance:** $1 - \text{Similarity} = 1 - 0.4 = \mathbf{0.6}$.
  
  **Answer 2: Weighted Jaccard**
  *   **Numerator (Mins):** $\min(2,1) + \min(0,3) + \min(4,2) + \min(1,1) = 1 + 0 + 2 + 1 = \mathbf{4}$.
  *   **Denominator (Maxes):** $\max(2,1) + \max(0,3) + \max(4,2) + \max(1,1) = 2 + 3 + 4 + 1 = \mathbf{10}$.
  *   **Weighted Jaccard:** $\mathbf{4/10}$ (or 0.4).
  
  **Answer 3: K-Shingling**
  1.  **Set:** `{ALG, LGO, GOR, ORI, RIT, ITH, THM}`. (Note: A Set removes duplicates, though "ALGORITHM" happens to have no duplicate 3-shingles).
  2.  **Why $k=1$ fails:** If $k=1$, your shingles are just individual letters `{A, B, C...}`. Almost every book in the English language uses all 26 letters of the alphabet. Therefore, the Intersection and Union of any two books would be identical (all 26 letters), giving a Jaccard Similarity of 1.0 (100% identical) for completely different books.
  
  **Answer 4: Manual Min-Hash Trace**
  *   **Permutation Order:**[Row 4, Row 2, Row 3, Row 1]
  1.  **Doc A:** Read down the new order. Row 4? (A=0). Row 2? (A=0). Row 3? (A=1). **Hit!** The MinHash value for A is **Row 3**.
  2.  **Doc B:** Read down the new order. Row 4? (B=0). Row 2? (B=1). **Hit!** The MinHash value for B is **Row 2**.
  3.  **Collision?** No. (Row 3 $\neq$ Row 2).
  
  **Answer 5: MinHash Probability**
  *   The fundamental magic of MinHashing is that $Pr[h(A) == h(B)] = \text{Jaccard}(A, B)$. 
  *   If the similarity is 0.60, each hash function has exactly a 60% chance of colliding.
  *   Out of 200 functions, you expect $200 \times 0.60 = \mathbf{120}$ matches.
  
  **Answer 6: The "One-Pass" Implementation Trick**
  *   Instead of shuffling, we generate $M$ standard hash functions (like `h(x) = (ax+b)%p`). 
  *   We initialize a signature array of size $M$ to all `INFINITY`.
  *   We read the document. For every shingle that actually exists in the document, we hash it using all $M$ functions.
  *   If the resulting hash value is *less* than the current value in the signature array, we overwrite it. By keeping the **minimum** hash value seen, we perfectly simulate scanning down a randomly shuffled list to find the first "1".
  
  **Answer 7: Calculating the S-Curve Threshold**
  *   **Formula:** $s \approx (1/b)^{1/r}$
  *   **Calculation:** $s \approx (1/20)^{1/5}$.
  *   *(Estimation logic: $(0.5)^5 = 0.03125$, which is close to $1/20 = 0.05$. So the threshold is slightly higher than 0.5, roughly **0.55**).*
  
  **Answer 8: A) Increase $b$ (bands) and decrease $r$ (rows)**
  *   **Explanation:** If you are missing true duplicates (False Negatives), your filter is too strict. 
    *   To make it *easier* to match, you decrease $r$ (rows), because matching 2 out of 2 hashes is much easier than matching 5 out of 5 hashes. (This relaxes the strict **AND** condition).
    *   You increase $b$ (bands) to give the documents more chances to find a lucky bucket. (This strengthens the forgiving **OR** condition).
  
  **Answer 9: The LSH Probability Formula**
  *   Probability of matching one row: $s = 0.5$.
  *   Probability of matching a whole band (AND logic): $s^r \implies 0.5^2$.
  *   Probability of failing a band: $1 - 0.5^2$.
  *   Probability of failing ALL bands: $(1 - 0.5^2)^{10}$.
  *   Probability of matching AT LEAST ONE band (Candidate Pair): **$\mathbf{1 - (1 - 0.5^2)^{10}}$**.
  
  **Answer 10: Exact Matching / Verification**
  *   **Step:** You must perform an exact calculation (e.g., exact Jaccard Similarity or direct string comparison) on the 1,200 candidate pairs.
  *   **Why:** LSH is an *approximate* algorithm. It is guaranteed to produce **False Positives** (items that hashed to the same bucket by pure luck but aren't actually similar). You must verify the candidates to weed out the junk. The beauty is that calculating the exact similarity for 1,200 pairs takes milliseconds, whereas calculating it for 5 million items would take months.
- ---
- ---
- ---
- ### **Practice Exam: Locality Sensitive Hashing (LSH)**
  
  **Question 1: The $O(N^2)$ Bottleneck**
  Why did Uber Engineering need to use LSH for fraud detection even though they already had access to massive Big Data processing clusters (like Apache Spark)? 
  A) Big Data clusters cannot perform Jaccard Similarity math.
  B) Spark clusters run out of memory when storing strings.
  C) Without LSH, comparing every trip to every other trip takes $O(N^2)$ time, which scales terribly even on a supercomputer.
  D) LSH compresses the database so it fits on a single hard drive.
  
  **Question 2: Jaccard Similarity**
  You are comparing two Spotify users' playlists. 
  User A listens to: `{"Drake", "The Weeknd", "SZA", "Future"}`
  User B listens to: `{"SZA", "Future", "Rihanna"}`
  What is the exact Jaccard Similarity between User A and User B?
  
  **Question 3: The MinHash Guarantee**
  You generate MinHash signatures for Document X and Document Y using **200** independent hash functions. You calculate the exact Jaccard Similarity of the two documents to be **0.80** (80%). 
  Exactly how many integers in their 200-length signatures do you expect to perfectly match?
  
  **Question 4: Tracing the One-Pass MinHash**
  You are calculating the MinHash signature for a document that contains shingles at Row **2** and Row **4** of your global vocabulary.
  Your hash function is $h(x) = (3x + 1) \pmod 7$.
  Your signature array is initialized to `INFINITY`.
  1. What is the hash value for Row 2?
  2. What is the hash value for Row 4?
  3. What is the final MinHash value recorded in the signature for this document?
  
  **Question 5: LSH Banding Logic**
  You divide your 100-integer signatures into $b=20$ bands of $r=5$ rows. 
  Document A and Document B have identical integers in rows 1 through 5 (Band 1). However, they have completely different integers in rows 6 through 100 (Bands 2 through 20). 
  Will LSH flag Document A and Document B as a "Candidate Pair"? Why or why not?
  
  **Question 6: Tuning the S-Curve (Fixing False Negatives)**
  Your FBI fingerprint database is using LSH to find matches. Investigators complain that the system is missing obvious matches (i.e., **High False Negatives**). 
  To cast a "wider net" and catch these missing fingerprints, how should you adjust your LSH parameters?
  A) Increase the number of rows ($r$) and decrease the number of bands ($b$).
  B) Decrease the number of rows ($r$) and increase the number of bands ($b$).
  C) Increase both $b$ and $r$.
  D) Require the candidate pairs to match in at least two bands instead of one.
  
  **Question 7: The S-Curve Threshold Math**
  You configure your LSH system to use $b=16$ bands and $r=2$ rows. 
  Using the threshold approximation formula $s \approx (1/b)^{1/r}$, at what Jaccard Similarity score will documents jump to roughly a 50% chance of becoming a Candidate Pair?
  
  **Question 8: Probability Amplification Formula**
  Two documents have a true Jaccard Similarity of $s = 0.25$. Your LSH system uses $b = 10$ bands and $r = 3$ rows.
  Write the exact mathematical expression that represents the probability of these two documents becoming a Candidate Pair. *(No need to calculate the final decimal).*
  
  **Question 9: AND vs. OR Operations**
  In the LSH pipeline, the signature matrix is divided into bands and rows. 
  1. Which parameter ($b$ or $r$) represents the logical **AND** operation, and what does it do to the probability of a collision?
  2. Which parameter ($b$ or $r$) represents the logical **OR** operation, and what does it do to the probability of a collision?
  
  **Question 10: The Mandatory Final Step**
  LSH is an approximate algorithm. After the LSH system hashes 100 million fingerprints and successfully outputs a list of 500 Candidate Pairs, what must the system do next?
  A) Output the 500 pairs directly to the user as confirmed matches.
  B) Perform an exact $O(N^2)$ Jaccard Similarity comparison only on those 500 pairs to weed out False Positives.
  C) Re-hash the 500 pairs using a Bloom Filter.
  D) Merge the 500 pairs into a single document.
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: C) Without LSH, comparing every trip takes $O(N^2)$ time.**
  *   **Explanation:** Hardware and Big Data tools (Spark, Hadoop) just throw raw processing power at a problem. But $O(N^2)$ grows so violently that adding servers eventually stops helping. LSH fixes the *algorithmic* bottleneck by filtering the data into candidate pairs in near $O(N)$ time, giving the Big Data tools a manageable workload.
  
  **Answer 2: 2/5 (or 0.4)**
  *   **Explanation:** 
    *   Intersection (shared artists): `{"SZA", "Future"}` $\to$ Size 2.
    *   Union (all unique artists): `{"Drake", "The Weeknd", "SZA", "Future", "Rihanna"}` $\to$ Size 5.
    *   Similarity = $2 / 5 = 0.4$.
  
  **Answer 3: 160 integers**
  *   **Explanation:** The fundamental property of MinHashing is that $Pr[h_{min}(A) = h_{min}(B)] = \text{Jaccard}(A, B)$. Since their similarity is 0.80, every slot in the signature has an 80% chance of matching perfectly. Out of 200 slots, $200 \times 0.80 = \mathbf{160}$.
  
  **Answer 4: MinHash Trace**
  1.  **Row 2 Hash:** $h(2) = (3(2) + 1) \pmod 7 = 7 \pmod 7 = \mathbf{0}$.
  2.  **Row 4 Hash:** $h(4) = (3(4) + 1) \pmod 7 = 13 \pmod 7 = \mathbf{6}$.
  3.  **Final Signature:** The algorithm keeps the *minimum* hash value seen for the document. $\min(\infty, 0, 6) = \mathbf{0}$.
  
  **Answer 5: Yes!**
  *   **Explanation:** LSH only requires two signatures to hash to the same bucket in **AT LEAST ONE BAND** to become a candidate pair. Because they perfectly matched in Band 1, they are flagged as candidates. The fact that they failed Bands 2-20 does not matter. (This is the **OR** operation at work!).
  
  **Answer 6: B) Decrease the number of rows ($r$) and increase the number of bands ($b$).**
  *   **Explanation:** A False Negative means similar items are failing to match. You are being too strict.
    *   To make it *easier* to match, decrease $r$. (Matching 3 rows is much easier than matching 10 rows).
    *   To give them *more chances* to match, increase $b$. 
    *   This shifts the S-Curve to the **left**, lowering the similarity threshold.
  
  **Answer 7: $s \approx 0.25$**
  *   **Explanation:** Using the threshold formula $s \approx (1/b)^{1/r}$:
    *   $s \approx (1/16)^{1/2}$
    *   Raising to the $1/2$ power is a square root. $\sqrt{1/16} = 1 / \sqrt{16} = \mathbf{1/4}$ or $\mathbf{0.25}$.
  
  **Answer 8: $1 - (1 - 0.25^3)^{10}$**
  *   **Explanation:**
    *   Probability of matching 1 row: $0.25$
    *   Matching all $r=3$ rows in a band (AND): $0.25^3$
    *   Failing to match a band (NOT): $1 - 0.25^3$
    *   Failing to match ALL $b=10$ bands (AND): $(1 - 0.25^3)^{10}$
    *   Matching at least one band (NOT): $1 - (1 - 0.25^3)^{10}$
  
  **Answer 9: AND/OR Operations**
  *   **1. Rows ($r$) = AND:** To match a band, Row 1 AND Row 2 AND Row 3 must match. This *decreases* the probability of a collision, punishing dissimilar items and lowering False Positives.
  *   **2. Bands ($b$) = OR:** To become a candidate, you must match Band 1 OR Band 2 OR Band 3. This *increases* the probability of a collision, giving similar items multiple chances to match and lowering False Negatives.
  
  **Answer 10: B) Perform an exact comparison only on those 500 pairs.**
  *   **Explanation:** LSH is an approximate filter. Because we used hash functions, there is a chance that two completely different fingerprints accidentally hashed to the same bucket (a False Positive). The mandatory final step is to run the exact (slow) algorithm on the candidate pairs to weed out the false positives before showing the results to the FBI investigator.
- ---
- ---
- ---
- ### **Practice Exam: Dynamic Programming (Theory & Fundamentals)**
  
  **Question 1: The Core Difference (DP vs. D&C)**
  You are deciding whether to solve a new algorithm using Divide & Conquer (like Merge Sort) or Dynamic Programming. What is the defining characteristic of the problem's subproblems that mandates you must use Dynamic Programming?
  A) The subproblems must be strictly disjoint (non-overlapping).
  B) The subproblems must heavily overlap, causing redundant calculations if solved naively.
  C) The problem must be divisible by exactly 2 at every step.
  D) The subproblems must require sorting before they can be merged.
  
  **Question 2: The DP Time Complexity Formula**
  According to the textbook, the total running time of a Dynamic Programming algorithm can be bounded by a specific formula. 
  If your algorithm evaluates exactly $f(n)$ subproblems, takes $g(n)$ time to compute each subproblem, and takes $h(n)$ time to reconstruct the final answer, what is the tightest upper bound on the total time complexity?
  A) $O(f(n) + g(n) + h(n))$
  B) $O(f(n) \cdot g(n) \cdot h(n))$
  C) $O(f(n) \cdot g(n) + h(n))$
  D) $O(f(n)^{g(n)} + h(n))$
  
  **Question 3: The "Silver Platter" Thought Experiment**
  When designing a DP algorithm, the first step is to establish **Optimal Substructure**. To do this, we often pretend someone handed us the perfect optimal solution $S$ on a silver platter. 
  What is the primary goal of looking at the *last* element (or decision) of this optimal solution $S$?
  A) To find the base case of the recursion tree.
  B) To narrow down the infinite possibilities into a small, finite number of mutually exclusive cases (e.g., "Item $n$ is either in $S$ or not in $S$").
  C) To sort the remaining items in the array.
  D) To calculate the space complexity of the cache.
  
  **Question 4: Top-Down vs. Bottom-Up Space Complexity**
  You write two DP solutions for the Fibonacci sequence: 
  1. **Memoization (Top-Down):** A recursive function that passes a `cache` array of size $N$.
  2. **Tabulation (Bottom-Up):** A `for` loop from 2 to $N$ that fills an array `A` of size $N$.
  
  While both have a Space Complexity of $O(N)$, why is the **Bottom-Up** approach strictly more memory-efficient in real-world hardware execution?
  
  **Question 5: The Reconstruction Trade-off**
  You optimize a 1D Dynamic Programming algorithm to only keep the "previous two" values in memory (e.g., `prev1` and `prev2`), completely dropping the array `A` of size $N$. 
  Your Space Complexity drops to $O(1)$. Which of the following is a direct consequence of this optimization?
  A) The Time Complexity degrades to $O(N^2)$.
  B) You can no longer find the maximum optimal *value*.
  C) You can no longer trace backward to reconstruct the *actual set of items* that made up the optimal value.
  D) The algorithm becomes a Greedy algorithm.
  
  **Question 6: Tracing the Redundancy**
  If you run the naive (un-memoized) recursive Fibonacci algorithm for `fib(5)`, it creates a massive tree. Exactly how many times will the function `fib(2)` be calculated from scratch?
  A) 1
  B) 2
  C) 3
  D) 5
  
  **Question 7: The DP 3-Step Recipe**
  According to Chapter 16, which of the following represents the correct sequence for the 3-step Dynamic Programming design recipe?
  A) (1) Write a `for` loop, (2) Create an array, (3) Return the last element.
  B) (1) Identify a small collection of subproblems, (2) Show how to solve larger subproblems using smaller ones (the recurrence), (3) Show how to infer the final solution.
  C) (1) Divide the array in half, (2) Conquer the halves, (3) Merge the results.
  D) (1) Sort the items by weight, (2) Pick the largest item, (3) Subtract its capacity.
  
  **Question 8: Memoization Implementation**
  When implementing Top-Down Memoization, you initialize your cache array to a specific "dummy" value (often `-1` or `infinity`). 
  What is the exact purpose of this dummy value?
  A) It serves as the base case for $N=0$.
  B) It acts as a flag to tell the program whether a subproblem has already been solved or if it needs to do the math.
  C) It prevents the stack from overflowing.
  D) It tracks which items are included in the final set.
  
  **Question 9: Why "Dynamic Programming"?**
  According to the historical excerpt from Richard Bellman in your textbook, why is the paradigm called "Dynamic Programming"?
  A) Because the size of the array dynamically changes during execution.
  B) Because it uses dynamic memory allocation (pointers) in C++.
  C) It was a 1950s marketing gimmick to hide the fact that he was doing mathematical research from a hostile Secretary of Defense.
  D) Because it dynamically updates the optimal solution during a recursive trace.
  
  **Question 10: Polynomial vs. Exponential Goals**
  While Divide & Conquer algorithms are generally used to make already-polynomial algorithms faster (e.g., $O(n^2) \to O(n \log n)$), what is the "killer application" of Dynamic Programming?
  A) It takes algorithms that require $O(n \log n)$ time and reduces them to $O(\log n)$.
  B) It provides $O(1)$ lookup times for unsorted databases.
  C) It takes optimization problems that would normally require exhaustive $O(2^n)$ exponential search and solves them in polynomial time (e.g., $O(n)$ or $O(n^2)$).
  D) It completely eliminates the need for RAM.
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **A1: B) The subproblems must heavily overlap...**
  *   **Explanation:** This is the foundational distinction. If you split a problem and the two halves never interact or share data (like Merge Sort), D&C is perfect. If the subproblems overlap (e.g., calculating `fib(4)` and `fib(3)` both require calculating `fib(2)`), D&C will do the work multiple times, resulting in exponential $O(2^n)$ time. DP fixes this by caching overlapping results.
  
  **A2: C) $O(f(n) \cdot g(n) + h(n))$**
  *   **Explanation:** This formula is explicitly highlighted on Page 119 of your textbook. 
    *   You have $f(n)$ total subproblems to solve (e.g., $n$ items).
    *   Each subproblem takes $g(n)$ time to compute (e.g., an $O(1)$ `max` comparison).
    *   This gives $O(f(n) \cdot g(n))$ time to fill the table.
    *   Finally, you add $h(n)$, which is the time to reconstruct the answer at the end (usually $O(n)$). 
    *   Total time is the table-filling time PLUS the reconstruction time.
  
  **A3: B) To narrow down the infinite possibilities into a small, finite number of mutually exclusive cases.**
  *   **Explanation:** By assuming an optimal solution exists and looking at the last element, we force a tautology: the element is either IN the set, or OUT of the set. This immediately gives us our two recursive cases and eliminates the need to guess blindly among millions of combinations.
  
  **A4: The Call Stack Overhead**
  *   **Explanation:** Even though both use $O(N)$ space theoretically, Top-Down Memoization uses recursion. Every recursive call adds a frame to the computer's Call Stack (storing variables, return addresses, etc.). A Bottom-Up `for` loop does not use the Call Stack. It only uses the physical $O(N)$ array in heap memory. Therefore, Bottom-Up avoids stack overflow errors and has much lower constant-factor overhead.
  
  **A5: C) You can no longer trace backward to reconstruct the actual set...**
  *   **Explanation:** To do the reconstruction step, you must start at the end and ask, "Did Case 1 or Case 2 win?" To answer that, you need to check the values at `A[i-1]` and `A[i-2]`. If you only kept the last two values in memory, you have permanently deleted the "tracks" for everything before them. You can output the final max score, but you can't output the items that created it.
  
  **A6: C) 3**
  *   **Explanation:** Look at the recursion tree for `fib(5)`:
    *   `fib(5)` calls `fib(4)` and `fib(3)`.
    *   `fib(4)` calls `fib(3)` and **`fib(2)`** (First time).
    *   That `fib(3)` calls **`fib(2)`** (Second time) and `fib(1)`.
    *   The original right-hand `fib(3)` calls **`fib(2)`** (Third time) and `fib(1)`.
    *   Total = 3 times. (This absurd redundancy is exactly what DP eliminates).
  
  **A7: B) (1) Identify subproblems, (2) Show how to solve larger using smaller (recurrence), (3) Infer final solution.**
  *   **Explanation:** This is the exact 3-step paradigm listed in the gray box on Page 118 of the textbook. It applies to WIS, Knapsack, Sequence Alignment, and virtually all DP problems.
  
  **A8: B) It acts as a flag to tell the program whether a subproblem has already been solved...**
  *   **Explanation:** In Memoization, the very first line of the function is `if (cache[i] != -1) return cache[i];`. The dummy value `-1` means "I haven't done this math yet." If it's anything *other* than `-1`, the program knows it can safely skip the math and return the cached answer instantly.
  
  **A9: C) It was a 1950s marketing gimmick...**
  *   **Explanation:** Richard Bellman invented it while working at RAND. The Secretary of Defense hated the word "research" and "mathematics." Bellman used "Dynamic" because it was a positive word that couldn't be used in a pejorative sense, and "Programming" meant "planning" (like TV programming). It was a cover-up to secure funding!
  
  **A10: C) It takes optimization problems that would normally require exhaustive $O(2^n)$ search and solves them in polynomial time.**
  *   **Explanation:** Page 121 of your textbook highlights this. D&C takes fast algorithms and makes them slightly faster. DP is a "killer app" because it takes completely impossible, exponential problems (like Knapsack or checking billions of independent sets) and solves them in linear or quadratic time. It crosses the boundary from Intractable to Tractable.
- ---
- ---
- ---
- ### **Practice Exam: DP - WIS & Combinations**
  
  **Question 1: WIS Array Tracing**
  You are given a path graph with the following node weights: `w =[4, 2, 6, 8, 3]`.
  Assume 1-based indexing where $w_1 = 4$. You are building the DP array `A` bottom-up.
  What is the exact value stored in `A[4]`?
  A) 10
  B) 12
  C) 14
  D) 18
  
  **Question 2: WIS Reconstruction (Backward Trace)**
  You have run the WIS algorithm on a path graph of 5 nodes and generated the following DP array: 
  `A =[0, 5, 5, 12, 14, 14]` *(Note: A[0] is 0, A[1] corresponds to node 1, etc.)*
  The weights of the nodes were `w = [5, 2, 7, 9, 1]`.
  Trace the reconstruction algorithm backward starting from $i = 5$. Which specific nodes are included in the Maximum Weight Independent Set?
  A) {v1, v3, v5}
  B) {v2, v4}
  C) {v1, v4}
  D) {v1, v3}
  
  **Question 3: Greedy vs. DP (Finding the Flaw)**
  Consider a path graph with weights `w = [10, 22, 10]`.
  1. What total weight will a naive Greedy algorithm (picking the heaviest available node first) return?
  2. What total weight will the Dynamic Programming algorithm return?
  
  **Question 4: Combinations ($nCr$) Logic**
  In the recursive formula for combinations: $C(n, r) = C(n-1, r-1) + C(n-1, r)$.
  In the "thought experiment" used to derive this, we focus on a single specific person (e.g., "Alice") from the group of $n$ people. 
  Which part of the recurrence represents the scenario where **Alice IS selected** to be in the subgroup of $r$ members?
  A) $C(n-1, r)$
  B) $C(n-1, r-1)$
  C) $C(n, r)$
  D) $C(n-2, r-1)$
  
  **Question 5: Combinations Time Complexity**
  If you implement the combinations algorithm `combinations(n, r)` using pure top-down recursion with NO memoization cache, the time complexity is roughly $O(2^n)$. 
  If you implement it *with* a 2D memoization cache, what is the new tightest upper bound on the time complexity?
  A) $O(n \log r)$
  B) $O(n^2)$
  C) $O(n \cdot r)$
  D) $O(n + r)$
  
  **Question 6: WIS Subproblem Definition**
  In the WIS algorithm, what exactly does the value stored at `A[i]` represent?
  A) The maximum weight of an independent set that *must* include vertex $i$.
  weight of an independent set for the entire graph.
  C) The maximum weight of an independent set considering *only* the prefix of the graph from vertex 1 up to vertex $i$.
  D) The weight of vertex $i$ plus the weight of vertex $i-2$.
  
  **Question 7: Distinct Subproblems**
  One of the core reasons DP is faster than Divide & Conquer for these problems is the number of *distinct* subproblems. 
  For a path graph of $N$ vertices, exactly how many distinct subproblems are evaluated in the WIS algorithm?
  A) $O(2^N)$
  B) $O(N^2)$
  C) Exactly $N + 1$
  D) $O(\log N)$
  
  **Question 8: Combinations Base Cases**
  When writing the recursive `combinations(group, members)` function, you need base cases to stop the recursion. According to your homework slides, what should the function return if `members == group` (i.e., you are picking $n$ people from a group of $n$ people)?
  A) 0
  B) 1
  C) `group`
  D) `members - 1`
  
  **Question 9: The $O(1)$ Space Optimization**
  You are running the WIS algorithm on a path graph of 1 million nodes. You only need to know the *maximum total weight*, and you do NOT need to know which specific nodes make up the set. 
  To optimize RAM, you decide not to allocate an array `A` of size 1,000,000. Exactly how many integer variables do you need to keep in memory at any given time to successfully run the `for` loop?
  A) 1
  B) 2
  C) 3
  D) $\log N$
  
  **Question 10: Reconstruction Time Complexity**
  After building the $O(N)$ size array `A` for the WIS problem, how much time does the reconstruction step take to trace backward and find the exact vertices in the optimal set?
  A) $O(1)$
  B) $O(N)$
  C) $O(N \log N)$
  D) $O(N^2)$
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: B) 12**
  *   **Trace the Array:**
    *   `w = [4, 2, 6, 8, 3]` (Indices 1 to 5)
    *   `A[0] = 0`
    *   `A[1] = 4` (Base case)
    *   `A[2] = max(A[1], A[0] + w[2]) = max(4, 0 + 2) = 4`
    *   `A[3] = max(A[2], A[1] + w[3]) = max(4, 4 + 6) = 10`
    *   `A[4] = max(A[3], A[2] + w[4]) = max(10, 4 + 8) = 12`
  *   **Result:** `A[4]` is exactly 12.
  
  **Answer 2: C) {v1, v4}**
  *   **Trace Backward:**
    *   Start at `i=5`. `A[5] = 14`. Look at `A[4]`. `A[4]` is also 14. 
    *   Because `A[5] == A[4]`, Case 1 won. Vertex 5 did not improve the score. **Exclude v5**. Move to `i=4`.
    *   At `i=4`. `A[4] = 14`. Compare to `A[3]` (12). Since $14 > 12$, Case 2 won. **Include v4**. Because we included v4, we must skip v3. Move to `i=2`.
    *   At `i=2`. `A[2] = 5`. Compare to `A[1]` (5). They are equal. **Exclude v2**. Move to `i=1`.
    *   At `i=1`. Base case. **Include v1**.
  *   **Result:** The set is `{v1, v4}`. (Check: weights $5 + 9 = 14$).
  
  **Answer 3: Greedy = 22, DP = 20**
  *   **Greedy Logic:** The heaviest node is 22. The algorithm picks it. It must then cross out 22's neighbors (the two 10s). The only node chosen is 22. Total = 22.
  *   **DP Logic:** DP checks all valid combinations. It sees that taking the two 10s gives 20, but taking the 22 gives 22. Wait! In this specific graph, **Greedy actually wins/finds the optimal answer**, and DP will *also* return 22! 
  *   *Professor Trap Alert:* Just because greedy *usually* fails doesn't mean it fails 100% of the time. However, DP is guaranteed to find the true mathematical maximum every single time. (If the weights were `[15, 22, 15]`, Greedy gets 22, DP gets 30).
  
  **Answer 4: B) $C(n-1, r-1)$**
  *   **Explanation:** If Alice is selected, she takes up 1 of the $r$ available slots in our subgroup. Therefore, we only need to find $r-1$ more members. Because Alice is removed from the total pool of candidates, we pick them from the remaining $n-1$ people. 
  
  **Answer 5: C) $O(n \cdot r)$**
  *   **Explanation:** The time complexity of a memoized DP algorithm is bounded by the number of unique subproblems. Because the function takes two parameters (`n` and `r`), we need a 2D matrix of size $n \times r$ to cache the answers. We calculate each cell exactly once, so the time complexity drops from exponential to $O(n \cdot r)$.
  
  **Answer 6: C) The maximum weight of an independent set considering only the prefix of the graph from vertex 1 up to vertex $i$.**
  *   **Explanation:** The core of DP is solving smaller "prefixes" of the problem. `A[i]` doesn't care about nodes $i+1$ or $n$. It assumes the universe ends at node $i$ and finds the optimal answer for that specific restricted universe.
  
  **Answer 7: C) Exactly $N + 1$**
  *   **Explanation:** The naive recursive algorithm makes $O(2^N)$ calls. But it is just evaluating the prefixes $G_0, G_1, G_2 \dots G_N$ over and over again. There are exactly $N+1$ unique prefixes, meaning there are only $N+1$ distinct subproblems.
  
  **Answer 8: B) 1**
  *   **Explanation:** If you have a group of 10 people, and you need to form a committee of exactly 10 people, there is only **1** possible way to do it: pick everyone.
  
  **Answer 9: B) 2**
  *   **Explanation:** The recurrence is `A[i] = max(A[i-1], A[i-2] + w[i])`. 
    To calculate the current step, you only need to know the answer from 1 step ago, and 2 steps ago. You can literally just use two integer variables (e.g., `prev1` and `prev2`) and update them in the loop. You do not need the full array.
  
  **Answer 10: B) $O(N)$**
  *   **Explanation:** The reconstruction `while` loop starts at index $N$ and moves backward either 1 step or 2 steps at a time until it hits 0. It does an $O(1)$ arithmetic comparison at each step. Therefore, it takes exactly $O(N)$ time to retrace the path.
- ---
- ---
- ---
- ### **Practice Exam: 2D Dynamic Programming (Knapsack & Sequence Alignment)**
  
  **Question 1: The Pseudo-Polynomial Trap (Knapsack)**
  You design a 0/1 Knapsack algorithm that builds an $n \times C$ matrix, yielding a time complexity of $O(nC)$. 
  If your input consists of $n = 50$ items, but the knapsack capacity $C$ is $10^{12}$ (1 Trillion), your program will likely crash or run for hours. 
  Why is $O(nC)$ technically **not** considered a true polynomial-time algorithm in theoretical computer science?
  A) Because it requires a 2D array instead of a 1D array.
  B) Because $C$ represents the *numeric magnitude* of the capacity, not the *size of the input data* (it only takes a few bits to type the number 1 Trillion).
  C) Because the items are not sorted beforehand.
  D) Because it is impossible to reconstruct the items.
  
  **Question 2: Manual Matrix Trace (Knapsack Calculation)**
  You are filling out the Knapsack DP matrix `A[i][c]`. 
  You are currently evaluating **Item 3** (Value $v_3 = 5$, Size $s_3 = 3$). 
  The current column capacity you are checking is **$c = 7$**. 
  Looking at the previous row (`i=2`), you see the following values:
  *   `A[2][7] = 8`
  *   `A[2][4] = 6`
  *   `A[2][3] = 4`
  
  Using the Knapsack recurrence relation, what is the exact integer value that will be placed in `A[3][7]`?
  A) 8
  B) 11
  C) 13
  D) 9
  
  **Question 3: The "Too Heavy" Edge Case (Knapsack)**
  You are evaluating **Item 4** (Value $v_4 = 100$, Size $s_4 = 10$). The current column capacity is **$c = 5$**.
  According to the DP recurrence, what value will be placed in `A[4][5]`?
  A) 100
  B) `A[3][5]`
  C) `A[3][0] + 100`
  D) 0
  
  **Question 4: Manual Trace (Knapsack Reconstruction)**
  You have finished building the matrix `A` for 4 items and capacity $C=5$. The final value at `A[4][5]` is **20**.
  You are tracing backward to find which items were stolen.
  *   `A[4][5] = 20`. Size of Item 4 is $s_4 = 2$.
  *   `A[3][5] = 20`. Size of Item 3 is $s_3 = 3$.
  *   `A[3][3] = 15`. 
  According to the reconstruction logic (where ties `A[i-1][c-si]+vi >= A[i-1][c]` favor inclusion), is **Item 4** included in the optimal set? Why or why not?
  
  **Question 5: Fractional vs. 0/1 Knapsack**
  A friend suggests that instead of $O(nC)$ Dynamic Programming, you should just calculate the "Value-to-Weight" ratio of each item, sort them, and greedily put the highest ratio items into the bag first. 
  For which variation of the Knapsack problem does this Greedy algorithm actually yield the mathematically perfect optimal answer?
  A) The 0/1 Knapsack Problem.
  B) The Fractional Knapsack Problem (where you can take 50% of an item).
  C) The Unbounded Knapsack Problem (where you have infinite copies of each item).
  D) It never yields the optimal answer.
  
  **Question 6: Sequence Alignment Base Cases**
  You are using the Needleman-Wunsch algorithm to align String X of length $m=5$ and String Y of length $n=4$. The penalty for inserting a Gap is $\alpha_{gap} = 2$.
  When you initialize the 2D matrix `P[i][j]`, what exact value will be placed in `P[3][0]` (Row 3, Column 0), and what does it represent?
  A) 0 (Because Column 0 means the string is empty).
  B) 6 (Representing the penalty of matching 3 characters of X against 3 gaps).
  C) 2 (Just the standard gap penalty).
  D) Infinity.
  
  **Question 7: Sequence Alignment Recurrence (The 3 Cases)**
  In the Sequence Alignment DP matrix, cell `P[i][j]` takes the **minimum** of three options. Which of the following correctly pairs the geometric direction you look in the matrix with its logical meaning?
  A) Look Diagonal = Match/Mismatch; Look Up/Left = Insert a Gap.
  B) Look Left = Match; Look Up = Mismatch; Look Diagonal = Gap.
  C) Look Up = Match; Look Diagonal = Insert a Gap.
  D) Look Diagonal = Gap; Look Up/Left = Match/Mismatch.
  
  **Question 8: Sequence Alignment Manual Trace**
  You are aligning X="CAT" and Y="CAR".
  You are calculating the cell for the final letters: 'T' vs 'R'. (`i=3, j=3`).
  *   Mismatch penalty = 3. 
  *   Gap penalty = 2.
  *   `P[2][2]` (CA vs CA) = 0
  *   `P[2][3]` (CA vs CAR) = 2
  *   `P[3][2]` (CAT vs CA) = 2
  
  What is the final minimum penalty score placed in `P[3][3]`?
  
  **Question 9: Space Optimization (Knapsack & Sequence Alignment)**
  In both Knapsack and Sequence Alignment, to calculate the values for row `i`, you only ever need to look at row `i-1`. 
  If you optimize the algorithm to only store 2 rows in memory (reducing Space Complexity from $O(nC)$ to $O(C)$), what capability do you permanently lose?
  A) The ability to find the maximum possible value / minimum penalty score.
  B) The ability to process strings/items of different lengths.
  C) The ability to reconstruct the actual items stolen / the actual string alignment.
  D) You lose no capabilities; it is a strict upgrade.
  
  **Question 10: 2D State Space Dependencies**
  In a 2D Dynamic Programming matrix (like Knapsack), you can safely write a program that fills the matrix out row-by-row (top to bottom) OR column-by-column (left to right). 
  Why is this true?
  A) Because the recursive base cases are all 0.
  B) Because cell `[i][c]` only depends on data from `[i-1]` (above it) and `[c - s_i]` (to the left of it), meaning as long as you move forward and down, the data you need is always already computed.
  C) Because the matrix is symmetrical.
  D) Because matrix multiplication is commutative.
  
  ---
  ---
- ### **Solutions & "Deep Dive" Explanations**
  
  **Answer 1: B) Because $C$ represents the numeric magnitude...**
  *   **Explanation:** In theoretical computer science, polynomial time $O(n^k)$ must scale with the *number of bits* required to represent the input. Writing the number `1,000,000,000,000` takes only about 40 bits of data, but it forces your `for` loop to run 1 Trillion times! Because the runtime explodes exponentially relative to the input's bit-size, it is "Pseudo-Polynomial." (This is why 0/1 Knapsack remains NP-Hard).
  
  **Answer 2: B) 11**
  *   **Explanation:** The recurrence is $\max(A[i-1][c], A[i-1][c - s_i] + v_i)$. 
    *   Case 1 (Exclude): Look straight up to `A[2][7]`, which is **8**.
    *   Case 2 (Include): Look up and left by size 3. Go to `A[2][7 - 3]` $\to$ `A[2][4]`. The value there is **6**. Add the current item's value ($v_3 = 5$). $6 + 5 = \mathbf{11}$.
    *   $\max(8, 11) = \mathbf{11}$. 
  
  **Answer 3: B) `A[3][5]`**
  *   **Explanation:** The item has a size of 10, but the current column capacity is only 5. The item is physically too heavy to fit in the bag. Therefore, you are *forced* into Case 1 (Exclude). You simply inherit the optimal value from the row directly above it at the exact same capacity: `A[3][5]`.
  
  **Answer 4: No, Item 4 is EXCLUDED.**
  *   **Explanation:** Look at the values. `A[4][5]` is 20. Look at the row above it: `A[3][5]` is *also* 20. 
    *   Did Item 4 improve our score? No. 
    *   Mathematically, $A[i-1][c - s_i] + v_i$ evaluates to $A[3][3] + v_4 \to 15 + (\text{unknown value, but it didn't beat 20})$.
    *   Because the value didn't change from the row above it, the algorithm knows Item 4 was bypassed. We skip it, capacity remains 5, and we move up to $i=3$. 
  
  **Answer 5: B) The Fractional Knapsack Problem.**
  *   **Explanation:** If you are allowed to take "half a television," the greedy Value/Weight ratio method works perfectly! You just grind the items into dust and pour them in until the bag is exactly full to the brim. It is the restriction of taking *whole* items (0/1) that breaks the Greedy algorithm by leaving empty gaps, mandating Dynamic Programming.
  
  **Answer 6: B) 6**
  *   **Explanation:** Row 3, Column 0 means aligning the first 3 characters of String X against an *empty* String Y (0 characters). The only way to align 3 characters against nothing is to insert 3 gaps in Y. Since each gap costs 2, $3 \times 2 = \mathbf{6}$. 
  
  **Answer 7: A) Look Diagonal = Match/Mismatch; Look Up/Left = Insert a Gap.**
  *   **Explanation:** 
    *   Looking **Diagonal** (`[i-1][j-1]`) means you consumed a character from *both* strings. This implies you either matched them or swallowed the mismatch penalty.
    *   Looking **Up** (`[i-1][j]`) means you consumed a character from X, but Y stayed the same. You inserted a gap in Y.
    *   Looking **Left** (`[i][j-1]`) means you consumed a character from Y, but X stayed the same. You inserted a gap in X.
  
  **Answer 8: 3**
  *   **Explanation:** We calculate all 3 cases and take the `min()`.
    *   **Case 1 (Diagonal/Mismatch):** 'T' vs 'R' is a mismatch. $A[2][2] + \alpha_{mismatch} = 0 + 3 = \mathbf{3}$.
    *   **Case 2 (Up/Gap in Y):** $A[2][3] + \alpha_{gap} = 2 + 2 = \mathbf{4}$.
    *   **Case 3 (Left/Gap in X):** $A[3][2] + \alpha_{gap} = 2 + 2 = \mathbf{4}$.
    *   $\min(3, 4, 4) = \mathbf{3}$.
  
  **Answer 9: C) The ability to reconstruct the actual items stolen / the actual string alignment.**
  *   **Explanation:** If you only keep the last two rows, you have permanently deleted the history of the matrix. When you get to the bottom-right corner and find your max value of 100, you have no way to trace your steps backward to row 0 to see *which decisions* led to that 100. 
  
  **Answer 10: B) Because cell `[i][c]` only depends on data from `[i-1]` (above) and `[c - s_i]` (left).**
  *   **Explanation:** Dynamic Programming is all about topological sorting of dependencies. You cannot calculate a cell until its "prerequisites" are calculated. Since a cell only looks Up and Left, as long as your `for` loops generally progress downward and rightward, the prerequisite cells will magically always be waiting for you, fully calculated!