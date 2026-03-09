# PART 1: DYNAMIC TASK PARALLELISM

This chapter shifts from sequential execution to multi-core execution. You must be able to calculate theoretical limits and write pseudocode using `spawn` and `sync`.
- ### 1. The DAG Model & Core Metrics
  Every parallel program can be modeled as a Directed Acyclic Graph (DAG) where nodes are instructions (strands) and edges are dependencies.
  *   **Work ($T_1$):** The total time to run on 1 processor. (Count *all* the nodes in the graph).
  *   **Span ($T_\infty$):** The time to run on an *infinite* number of processors. (Count the nodes on the *longest/critical path*).
  *   **Parallelism:** $T_1 / T_\infty$. This is the maximum possible speedup and the limit of how many processors the algorithm can actually use efficiently.
- ### 2. The Three Laws of Parallelism (Crucial for Math Problems)
  *   **Work Law:** $T_p \ge T_1 / P$. (You can't finish faster than the total work divided evenly among your $P$ workers).
  *   **Span Law:** $T_p \ge T_\infty$. (You can never finish faster than the critical path, even with infinite workers).
  *   **Brent’s Theorem (Greedy Scheduler Bounds):** $T_p \le \frac{T_1 - T_\infty}{P} + T_\infty$.
    *   *How to use it on the exam:* If a question says "A research paper claims $P=10$ takes 42 seconds," you plug your max Work and max Span bounds into this equation. If the math says $T_{10} \le 41$, the paper is lying! (This was your exact HW problem).
- ### 3. Writing Parallel Algorithms (Pseudocode)
  You will be asked to write parallel code. **Rules:** Never mention "threads." Use `spawn` (fork/do not wait), `sync` (join/wait for children), and `parallel for`.
  *   **Divide and Conquer Template:**
    ```text
    func(low, high):
        if (base case) return
        mid = (low + high) / 2
        spawn func(low, mid)   // Child handles left half
        func(mid + 1, high)    // Parent handles right half
        sync                   // Parent MUST wait for child here
        combine_results()
    ```
  *   **Race Conditions:** A "Determinacy Race" happens when two parallel tasks access the same memory location, and *at least one writes to it*. (e.g., updating a global `count++` inside a `parallel for`). To fix this, use a **Parallel Reduction** (a tree of additions) instead of a shared variable.
- ### 4. Parallel Matrix Multiplication (MatMul)
  Be ready to compare the 3 versions:
  1.  **Simple Loops:** Outer loops $i$ and $j$ are `parallel for`, inner loop $k$ is serial. Span is bottlenecked by the $k$ loop: $\Theta(n)$.
  2.  **Recursive Block:** Split matrix into 8 blocks. Work = $\Theta(n^3)$, Span = $\Theta(\log^2 n)$.
  3.  **Optimal (Reduction):** `parallel for` for $i$ and $j$, but use a **Parallel Dot Product** for $k$. Work = $\Theta(n^3)$, Span = $\Theta(\log n)$. Parallelism = $\Theta(n^3 / \log n)$.
- ### 5. Parallel Merge Sort
  *   **Naive Version:** Parallelize the recursive sorts, but use a *serial* merge. Span = $\Theta(n)$ (bottlenecked by the $O(n)$ merge). Parallelism = $\Theta(\log n)$. (Very bad).
  *   **Optimal Version:** You must **parallelize the Merge step**.
    *   *How?* Find the **median** of the larger array. Use **Binary Search** to find where that median fits in the smaller array. Split both arrays at those points and recursively merge the left halves and right halves in parallel.
  
  ---
- # PART 2: HASHING & COLLISION RESOLUTION
  
  Hashing aims to achieve $O(1)$ add/search time by mapping data to an array index.
- ### 1. The 2-SUM Problem
  *   **Brute Force:** Nested loops $\to \Theta(n^2)$.
  *   **D&C (Sort + Binary Search):** $\Theta(n \log n)$.
  *   **Hashing:** Put all items in a HashSet. Iterate through array and check if $(Target - current\_item)$ exists in the set $\to \Theta(n)$ Expected Time.
- ### 2. Collision Resolution: Open Addressing
  When `Hash(k)` is already occupied, where do we go?
  *   **Linear Probing:** $h(k, i) = (h(k) + i) \pmod m$. 
    *   *Issue:* Causes **Primary Clustering** (blocks of data clump together, ruining $O(1)$ time).
  *   **Double Hashing:** $h(k, i) = (h_1(k) + i \cdot h_2(k)) \pmod m$.
    *   *How to use it on the exam:* If $h_1$ fails, you calculate a "step size" using $h_2$. If $h_1(98) = 7$ (full), and $h_2(98) = 11$, your next attempts are $(7 + 11) \pmod m$, then $(7 + 22) \pmod m$, etc. (This was on your slides!).
  *   **Cuckoo Hashing:** Uses **two** hash tables and **two** hash functions.
    *   *How it works:* If $T_1$ is full, kick out the existing item. The kicked-out item flies over to its hash spot in $T_2$. If that's full, it kicks *that* item out back to $T_1$.
    *   *Pros/Cons:* Guaranteed worst-case $O(1)$ lookup. But, if the load factor gets too high (> 50%), it triggers an infinite eviction loop and requires a total rehash.
  
  ---
- # PART 3: BLOOM FILTERS
  
  A space-efficient, probabilistic bit-array used to test membership. (e.g., Malicious URL checking, Bitcoin SPV wallets).
- ### 1. The Rules
  *   **NO False Negatives:** If it says "Not Present", it is 100% not there.
  *   **False Positives exist:** It might say "Present" when it isn't.
  *   **No Deletions:** You cannot flip a 1 back to a 0, or you will accidentally delete other items that share that bit.
- ### 2. The Core Equations (Memorize These!)
  Your professor loves the "Dartboard" math for this.
  *   **Variables:** $n$ = bits in array, $m$ = number of hash functions, $|S|$ = number of items, $b = n / |S|$ (bits per key).
  *   **Probability a given bit is 1:** $1 - e^{-m/b}$
  *   **False Positive Rate:** $(1 - e^{-m/b})^m$
  *   **Optimal number of hash functions:** $m = b \ln(2)$
  *   **Optimal False Positive Rate:** $(1/2)^{b \ln 2}$
    *   *Exam Trick:* If you double the space ($b \to 2b$), you mathematically **square** the false positive probability (e.g., $0.01 \to 0.0001$).
  
  ---
- # PART 4: LOCALITY SENSITIVE HASHING (LSH)
  *(Based on `ch03-lsh.pptx` and `Finding Similar Items.pdf` from your screenshot)*
  
  Standard hashing destroys similarity (changing "apple" to "appld" results in wildly different hash codes). **LSH** is designed so that similar items hash to the *same* buckets. Used for plagiarism detection, document similarity, etc.
- ### 1. The Pipeline
  1.  **Shingling:** Convert a document into a set of $k$-grams (substrings of length $k$). This turns a string into a mathematical Set.
  2.  **MinHashing:** Convert large sets into short "Signatures."
    *   *Jaccard Similarity:* The measure of similarity between two sets. $\text{Intersection} / \text{Union}$.
    *   *MinHash Property:* The probability that $h_{min}(A) == h_{min}(B)$ is exactly equal to the Jaccard Similarity of A and B.
  3.  **LSH:** Divide the MinHash signatures into "Bands" and "Rows." Hash each band. If two documents share even one band, they are considered a "Candidate Pair" for similarity.
  
  ---
- ### How to Practice Next
  We have a few days before the exam. I suggest we break our practice down by question types. Which of these would you like to do first?
  
  1.  **Write Parallel / D&C Algorithms:** I will give you a problem (like "Find the dot product of two arrays") and you write the pseudocode.
  2.  **Math / Recurrence Practice:** I will give you DAG node counts, Brent's law scenarios, and Bloom filter calculations to solve.
  3.  **Short Answer Theory:** I will ask you rapid-fire conceptual questions (e.g., "Why is Cuckoo hashing $O(1)$ worst case but Linear Probing isn't?").