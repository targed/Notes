## 1. Pop Quiz Solutions
- ### Quiz A: The Mixed Bounds
  **Function:** $T(n) = \frac{1}{2}n^2 + 3n$.
  Which statements are true?
  *   a) $T(n) = O(n)$ $\to$ **FALSE.** ($n^2$ grows faster than $n$. You can't squeeze $n^2$ under $n$).
  *   b) $T(n) = \Omega(n)$ $\to$ **TRUE.** ($n^2$ grows "at least" as fast as $n$. This is a "loose" lower bound, but mathematically valid).
  *   c) $T(n) = \Theta(n^2)$ $\to$ **TRUE.** (The dominant term is $n^2$. It is tight).
  *   d) $T(n) = O(n^3)$ $\to$ **TRUE.** ($n^3$ is a valid upper bound. $n^2$ will always be smaller than $n^3$).
- ### Quiz B: The Summation
  **Algorithm A steps:** $\frac{n(n-1)}{2}$.
  *   **Simplify:** $\frac{1}{2}n^2 - \frac{1}{2}n$.
  *   **Dominant Term:** $n^2$.
  *   **Result:** It is $\Theta(n^2)$.
  *   **Correct Option:** Because it is $\Theta(n^2)$, it is automatically **also** $O(n^2)$ and $\Omega(n^2)$. All three options are technically correct descriptions, but $\Theta(n^2)$ is the most precise.
  
  ---
- ## 2. finding the "Crossover Point"
  We often say $O(N \log N)$ is faster than $O(N^2)$. But is it *always* faster?
  **No.**
  For small inputs, the **constants** (which Big O ignores) actually matter.
- ### Scenario 1: Insertion Sort vs. Merge Sort
  *   **Insertion Sort:** $8n^2$ (High growth rate, but low constant overhead).
  *   **Merge Sort:** $64n \log n$ (Low growth rate, but high constant overhead).
  *   **Question:** For which $n$ is Insertion Sort faster?
    $$ 8n^2 < 64n \log_2 n $$
  *   **Math:**
    1.  Divide by $8n$:
        $$ n < 8 \log_2 n $$
    2.  Substitute values (Trial & Error):
        *   If $n=32$: $32 < 8(5) \to 32 < 40$ (True).
        *   If $n=64$: $64 < 8(6) \to 64 < 48$ (False).
    3.  **Answer:** The "crossover point" is around **43**.
  *   **Takeaway:** If you are sorting fewer than 43 items, Insertion Sort is actually faster! This is why real-world sorting libraries (like Python's Timsort) use Insertion Sort for small chunks and Merge Sort for the rest.
- ### Scenario 2: Polynomial vs. Exponential
  *   **Algorithm A:** $100n^2$ (Polynomial).
  *   **Algorithm B:** $2^n$ (Exponential).
  *   **Question:** When does the Polynomial algorithm become faster (i.e., when does $2^n$ explode past $100n^2$)?
    $$ 100n^2 < 2^n $$
  *   **Math (Trial & Error):**
    *   $n=5$: $2500$ vs $32$ ($2^n$ is way faster/smaller).
    *   $n=14$: $19,600$ vs $16,384$ ($2^n$ is still faster/smaller).
    *   $n=15$: $22,500$ vs $32,768$ ($2^n$ just exploded).
  *   **Answer:** At $n=15$, the Exponential algorithm becomes uselessly slow compared to the Polynomial one.
  
  ---
- ## 3. Amortized Time
  This is a special topic. Sometimes an operation is usually fast ($O(1)$), but occasionally very slow ($O(N)$). How do we categorize it?
  
  **The Metaphor:** Rent vs. Buying.
  *   Paying \$300,000 for a house one day is expensive ($O(N)$).
  *   But if you live there for 10 years, the *amortized* (spread out) cost is just monthly rent ($O(1)$).
  
  **The Example: Dynamic Arrays (ArrayList / Vector)**
  *   **Normal Insert:** You just put the item in the next empty slot. Cost: $O(1)$.
  *   **The Problem:** The array is full!
  *   **Expansion:**
    1.  Create a new array double the size.
    2.  Copy all $N$ items over. Cost: $O(N)$.
    3.  Insert the new item.
  *   **Amortized Analysis:**
    *   It happens so rarely (only when the array powers of 2 fill up) that if we average the work over all insertions, the cost is still $O(1)$.
  
  ---
- #### **1. Amortized Analysis: The Three Methods**
  *   **The Nuance:** Depending on how "deep" your professor goes, they might mention there are three ways to prove Amortized Time.
  *   **The Addition:**
    1.  **Aggregate Method:** Total cost of $N$ operations divided by $N$. (This is the ArrayList doubling example).
    2.  **Accounting (Banker's) Method:** We "overcharge" cheap operations (save credits) to pay for the one expensive operation later.
    3.  **Potential (Physicist's) Method:** We represent the "stored energy" of the data structure as a mathematical function.
- #### **2. "Expected" vs. "Amortized"**
  *   **The Nuance:** These are often confused but mean different things for a quiz.
  *   **The Addition:**
    *   **Amortized:** A **guarantee** over a sequence. The expensive operation *will* happen, but it's rare.
    *   **Expected:** A **probability** based on random input (like Quicksort). The expensive operation *might* happen if you're unlucky, but on average, it won't.
- # Summary of Course Introduction
  You have now completed the comprehensive notes for the Introduction slide decks. Here is what you should master before moving to the next module:
  
  1.  **Definitions:** Know the formal math for $O$ (upper), $\Omega$ (lower), and $\Theta$ (tight).
  2.  **Hierarchy:** Know that $O(1) < O(\log N) < O(N) < O(N \log N) < O(N^2) < O(2^N)$.
  3.  **Calculation:** Be able to look at a loop and determine its complexity.
    *   `j = j * 2` means Logarithmic.
    *   Nested loops mean Multiply.
    *   Sequential loops mean Add.
  4.  **Nuance:** Understand that for small $N$, a "worse" algorithm might be faster due to constants (the Crossover Point).