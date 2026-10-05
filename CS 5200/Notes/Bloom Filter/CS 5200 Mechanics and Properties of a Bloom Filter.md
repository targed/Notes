### 1. The Underlying Structure (Slide 4)
A standard Hash Table uses an array of *pointers* or *objects*. A Bloom Filter uses a **Bit Array** (an array where every slot is literally just a `0` or a `1`).
*   $m$: The size of the bit array. (Initially, all $m$ bits are set to `0`).
*   $k$: The number of different, independent hash functions used.
- ### 2. Operation 1: Insertion (Slides 4 & 7)
  How do we add an element (let's say the string `"apple"`) to the set?
  1.  Take `"apple"` and run it through all $k$ hash functions.
  2.  Each hash function outputs an integer. We use modulo $m$ to get an index between $0$ and $m-1$.
  3.  Go to those $k$ indices in the bit array and change the bits from `0` to **`1`**.
    *   *Note:* If a bit is already a `1` (because another element flipped it earlier), just leave it as `1`.
  
  **Visual Example (Slide 7):**
  If $k=3$, adding the element $x$ might output indices 1, 5, and 13. We set `Array[1] = 1`, `Array[5] = 1`, and `Array[13] = 1`.
- ### 3. Operation 2: The Membership Query (Slide 8)
  How do we check if `"apple"` is in the set?
  1.  Run `"apple"` through the exact same $k$ hash functions.
  2.  Check the bit array at those $k$ indices.
  3.  **The "Definitely Not" Rule:** If **ANY** of those bits are `0`, `"apple"` was absolutely never inserted. (Because if it had been, all those bits would be `1`).
  4.  **The "Probably" Rule:** If **ALL** of those bits are `1`, the algorithm returns "True" (Possibly in set).
  
  **The Cause of False Positives:**
  Look at element $w$ in Slide 7. It hashes to three locations. Even though $w$ was never inserted, other elements ($x, y,$ and $z$) happened to flip those exact three bits to `1` during their own insertions. Thus, querying $w$ returns a False Positive.
- ### 4. The "No Deletion" Rule (Slide 9)
  This is a classic "Professor Deep Dive" exam question: **Can you remove an item from a standard Bloom Filter?**
  *   **The Answer:** NO.
  *   **The Reason:** Suppose you want to delete element $x$, which hashes to indices 1, 5, and 13. If you flip those bits back to `0`, you might accidentally corrupt element $y$, which also happened to use index 5. By deleting $x$, you would introduce a **False Negative** for $y$, which breaks the fundamental guarantee of a Bloom Filter.
  *   *(Note: There is a variation called a "Counting Bloom Filter" that allows deletion by using integer counters instead of single bits, but standard Bloom Filters cannot delete).*
- ### 5. Time Complexity & Hardware Advantage (Slide 10)
  The time complexity of a Bloom Filter is incredibly unique. 
  *   **Time Complexity:** $\mathbf{O(k)}$.
  *   Notice what variable is missing? **$N$**. The time it takes to add or query an item is *completely independent* of how many items are already in the set. Whether the filter holds 10 items or 10 billion items, it always takes exactly $k$ hash computations.
  *   **The Parallel Hardware Bonus:** Because the $k$ hash functions are completely independent of each other, hardware implementations (like inside a network router) can calculate all $k$ hashes **in parallel** at the exact same time.
  
  ---
- ### Part 2 Practice Questions (Mechanics)
  
  **Q1: The Bit Overlap**
  You have a Bloom Filter with $m=10$ bits and $k=2$ hash functions. 
  You insert "Cat" (hashes to 2 and 7).
  You insert "Dog" (hashes to 4 and 7).
  How many bits in the array are currently set to `1`?
  A) 4
  B) 3
  C) 2
  D) 1
  
  **Q2: Complexity Comparison**
  If a Hash Table uses chaining (Linked Lists) to resolve collisions, its worst-case lookup time is $O(N)$. What is the worst-case lookup time for a Bloom Filter?
  A) $O(N)$
  B) $O(1)$
  C) $O(k)$
  D) $O(\log N)$
  
  **Q3: Deletion Consequence**
  If a developer ignores the rules and forces a "deletion" feature by flipping bits back to `0`, what catastrophic property is introduced into the system?
  A) Infinite loops.
  B) False Positives.
  C) False Negatives.
  D) Determinacy Races.
  
  **Q4: The Full Array**
  What happens to a Bloom Filter if you keep adding elements to it indefinitely without increasing its size $m$?
  A) It throws an "Array Out of Bounds" exception.
  B) Every single bit eventually becomes `1`, and it returns "True" for every query.
  C) It automatically doubles in size and rehashes (like a HashMap).
  D) It begins to overwrite old elements.