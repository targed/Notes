### 1. The "Bands and Rows" Technique (Slides 43–45)
Instead of comparing whole signatures, we divide each signature into **$b$ bands** consisting of **$r$ rows**.
*   *Example:* A signature of 100 integers can be divided into $b=20$ bands, where each band has $r=5$ integers.

**The Hashing Rule:**
For each band, we take those $r$ integers and hash them into a bucket. 
*   If Document A and Document B have the *exact same $r$ integers* in Band 1, they will hash to the exact same bucket.
*   If two documents hash to the same bucket in **AT LEAST ONE BAND**, they are flagged as a **Candidate Pair**.
- ### 2. The Probability Math (The "Deep Dive" Derivation)
  This is guaranteed to be on your exam. You must understand how to derive the probability that two documents become a candidate pair. (Slides 53–54).
  
  Let **$t$** be the actual Jaccard Similarity of two documents.
  1.  **Match in one specific row:** The probability their Min-Hash values match is exactly **$t$**.
  2.  **Match an entire band (AND logic):** All $r$ rows in the band must match. Probability = **$t^r$**.
  3.  **Fail to match a band (NOT logic):** Probability they disagree in at least one row = **$1 - t^r$**.
  4.  **Fail to match ALL bands (AND logic):** They fail Band 1, AND fail Band 2... up to $b$ bands. Probability = **$(1 - t^r)^b$**.
  5.  **Match AT LEAST ONE band (NOT logic):** The probability they become a candidate pair is the opposite of failing all bands. 
    *   **Final Formula:** $\mathbf{1 - (1 - t^r)^b}$
- ### 3. Tracing the Math (Slides 48–49)
  Your professor loves this specific example ($b=20, r=5$). 
  
  **Scenario A: High Similarity ($t = 0.8$)**
  *   We *want* these to be candidates.
  *   Prob of matching 1 band: $0.8^5 = 0.328$.
  *   Prob of failing all 20 bands: $(1 - 0.328)^{20} \approx 0.00035$.
  *   Prob of becoming candidates: $1 - 0.00035 = \mathbf{0.99965}$.
  *   *Result:* 99.96% of 80%-similar documents will correctly be caught! (Only ~0.035% are **False Negatives**).
  
  **Scenario B: Low Similarity ($t = 0.3$)**
  *   We *do not* want these to be candidates.
  *   Prob of matching 1 band: $0.3^5 = 0.00243$.
  *   Prob of failing all 20 bands: $(1 - 0.00243)^{20} \approx 0.9526$.
  *   Prob of becoming candidates: $1 - 0.9526 = \mathbf{0.0474}$.
  *   *Result:* Only 4.74% of 30%-similar documents will accidentally slip through as **False Positives**. We successfully filtered out over 95% of the "junk" comparisons!
- ### 4. The Threshold & The S-Curve (Slides 55–56)
  If you graph the formula $1 - (1 - t^r)^b$, it forms an **S-Curve**. 
  *   It stays near 0% for low similarities, then shoots almost straight up to 100% at a specific threshold, acting like a binary "Yes/No" filter.
  
  **The Magic Threshold Formula:**
  The exact similarity score where the curve shoots upward (the threshold $s$) can be approximated by:
  $$ s \approx (1/b)^{1/r} $$
  
  *   *Example:* If $b=20, r=5 \implies s \approx (1/20)^{1/5} \approx \mathbf{0.55}$. 
  *   Any documents with $> 55\%$ similarity will likely be caught. Anything $< 55\%$ will likely be ignored.
  
  ---
- ### Part 4 Practice Questions
  
  **Q1: The Threshold Approximation**
  You are building an LSH system. You have signatures of length 100. You divide them into $b = 16$ bands of $r = 2$ rows. (Note: $16 \times 2 = 32$. We only use the first 32 hash values for this test).
  Using the approximation formula $s \approx (1/b)^{1/r}$, what is the similarity threshold?
  A) $0.25$
  B) $0.50$
  C) $0.75$
  D) $0.125$
  
  **Q2: Tuning for False Positives (Slide 50)**
  Your current LSH setup ($b=20, r=5$) is catching too many unrelated documents (False Positives), wasting your CPU time in the final verification step.
  To fix this, you decide to change to **$b=15, r=5$**. 
  According to Slide 50, what is the trade-off of this decision?
  A) False Positives go down, but False Negatives go up.
  B) False Positives go down, and False Negatives go down.
  C) False Positives go up, but False Negatives go down.
  D) The threshold $s$ moves to the left.
  
  **Q3: The Final Step (Slide 58)**
  LSH successfully reduced your 10 million documents down to just 5,000 "Candidate Pairs". What is the mandatory final step in the algorithm?
  A) Output the 5,000 pairs to the user immediately.
  B) Hash the candidate pairs one more time into a Bloom Filter.
  C) Check the actual signatures (or actual text) of those 5,000 pairs in main memory to weed out the False Positives.
  D) Run the 5,000 pairs through a B-Tree.
  
  **Q4: The Formula Construction**
  In the formula $1 - (1 - t^r)^b$, which part specifically represents the logical **AND** condition (requiring multiple hash functions to agree simultaneously)?
  A) $(1 - t^r)$
  B) Raising to the power of $b$
  C) Raising to the power of $r$
  D) The outer $1 - (\dots)$
  
  ---
- ### **Solutions & Explanations**
  
  **A1: A) 0.25**
  *   **Math:** $s \approx (1/b)^{1/r} = (1/16)^{1/2}$. 
  *   Raising something to the $1/2$ power is the same as taking the square root. 
  *   $\sqrt{1/16} = 1 / \sqrt{16} = \mathbf{1/4} = \mathbf{0.25}$.
  
  **A2: A) False Positives go down, but False Negatives go up.**
  *   **Logic:** By decreasing the number of bands ($b$), you are giving the documents *fewer chances* (fewer "OR" conditions) to find a match. This makes the test much stricter. The junk documents will fail (False Positives drop), but some genuinely similar documents might get unlucky and fail all 15 bands (False Negatives rise).
  
  **A3: C) Check the actual signatures in main memory...**
  *   **Logic:** LSH is an *approximate* filter. It guarantees False Positives exist! You must perform an exact comparison (either comparing the 100-integer signatures or the original text documents) on the candidates to find the true similarities. Because you are only doing 5,000 comparisons instead of 50 trillion, this step is extremely fast.
  
  **A4: C) Raising to the power of $r$**
  *   **Logic:** Inside a single band, you have $r$ rows. For the band to match, Row 1 AND Row 2 AND ... Row $r$ must match. Since they are independent probabilities, you multiply $t \times t \times \dots \times t = t^r$. The power of $b$ represents the "OR" condition across the different bands.
  
  ---
-