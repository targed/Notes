### 1. Band and Hash (Pages 17–19)
We don't want to require the *entire* 100-integer signature to match perfectly, because that would mean the documents have to be 100% identical. But we also don't want to check every single pair of signatures one by one.

**The Solution: Banding**
1.  We split our 100-integer signature into **$b$ sub-vectors** (called bands).
2.  If $b = 20$, then each band contains exactly **$r = 5$** integers ($20 \times 5 = 100$).
3.  Instead of passing the whole signature through a hash function, we pass *each band* through a hash function into a set of hash buckets.

**The Candidate Rule:**
If Document A and Document B have the *exact same 5 integers* in Band 1, they will hash to the exact same bucket. If they hash to the same bucket in **any** of the 20 bands, we pull them out and label them as a **Candidate Pair**.
- ### 2. The Python Implementation (Pages 19–21)
  The Pinecone guide shows exactly how this looks in code:
  ```python
  def split_vector(signature, b):
    r = int(len(signature) / b)
    subvecs =[]
    for i in range(0, len(signature), r):
        subvecs.append(signature[i : i+r])
    return subvecs
  ```
  *   **The Dictionary Trick (Page 28):** In Python, we don't even need a complex hash function for this step. We can literally convert the sub-vector into a string or tuple and use it as a `key` in a Python Dictionary. If two documents produce the exact same sub-vector string, they get appended to the exact same dictionary key!
- ### 3. The S-Curve and Optimizing Bands (Pages 30–32)
  Once we have our candidate pairs, we calculate their *exact* similarity. But how do we guarantee we catch the right pairs? We use the probability formula:
  
  **Probability of becoming a candidate:** $\mathbf{P = 1 - (1 - s^r)^b}$
  *(Where $s$ is the true Jaccard similarity of the two documents).*
  
  **Visualizing the Math (Page 30):**
  If you graph this formula, it creates an **S-Curve**. 
  *   Documents with a low similarity (e.g., $s = 0.2$) have almost a 0% chance of becoming a candidate.
  *   Documents with high similarity (e.g., $s = 0.8$) have almost a 100% chance of becoming a candidate.
  *   The vertical "cliff" of the S-curve is the **Similarity Threshold**.
- ### 4. The "Deep Dive" Trade-off: Shifting the Curve (Pages 31–32)
  Your professor will absolutely test you on this dynamic. What happens if we change the number of bands ($b$)?
  
  *   **Increasing $b$ (e.g., $b=20 \to b=25$):**
    *   If $b$ goes up, $r$ must go down (since $b \times r = 100$). 
    *   Because $r$ is smaller, it is *easier* for a band to match perfectly (matching 4 integers is easier than matching 5).
    *   Because $b$ is larger, documents have *more chances* to find a match.
    *   **The Graph Effect:** The S-curve shifts to the **LEFT**. 
    *   **The Trade-off:** You catch more documents, meaning fewer **False Negatives** (FN). However, because the test is easier, you accidentally catch more "junk" pairs, increasing your **False Positives** (FP).
  
  *   **Decreasing $b$ (e.g., $b=20 \to b=10$):**
    *   $r$ goes up to 10. The test becomes much stricter.
    *   **The Graph Effect:** The S-curve shifts to the **RIGHT**.
    *   **The Trade-off:** You get very few False Positives (your candidate pool is pure), but your False Negatives skyrocket (you miss genuinely similar documents).
  
  ---
- ### Part 4 Practice Questions (Banding & Optimization)
  
  **Q1: The Signature Split**
  Your MinHash algorithm outputs a dense signature of exactly **120 integers**. You want to use $b = 15$ bands for your LSH system. How many integers ($r$) will be in each band?
  A) 15
  B) 10
  C) 8
  D) 120
  
  **Q2: The LSH Python Implementation**
  In the Pinecone Python example, after splitting the signature into bands, what data structure is used to physically group the candidate pairs together?
  A) A Binary Search Tree
  B) A Python Dictionary (where the band values are the keys)
  C) A NumPy multidimensional array
  D) A Linked List
  
  **Q3: Modifying the S-Curve**
  Your current LSH system has a similarity threshold that is too low. It is returning thousands of candidate pairs that only have a 40% similarity, overloading your exact-matching servers with False Positives.
  To fix this, you need to shift the S-curve to the **RIGHT**. How should you adjust your bands ($b$) and rows ($r$)?
  A) Increase $b$, decrease $r$.
  B) Decrease $b$, increase $r$.
  C) Increase both $b$ and $r$.
  D) Decrease both $b$ and $r$.
  
  **Q4: The False Positive Definition (Page 32)**
  Looking at the graph on Page 32 of the Pinecone guide, what exactly constitutes a **False Positive (FP)** in the context of LSH?
  A) A pair of vectors that are highly similar, but failed to hash to the same bucket.
  B) A pair of vectors that hashed to the same bucket (became candidates), but their actual calculated similarity was below your desired threshold.
  C) A pair of vectors that crashed the Python dictionary.
  D) A pair of vectors that share the exact same 100-integer signature.
  
  ---
- ### **Solutions**
  
  **A1: C) 8**
  *   **Math:** The total signature length $L = b \times r$. Therefore, $120 = 15 \times r \implies r = 120 / 15 = 8$.
  
  **A2: B) A Python Dictionary**
  *   **Logic:** As shown on Page 28, they use a dictionary (`lsh.buckets`) where the key is the stringified sub-vector (e.g., `'[65, 438, 534]'`) and the value is a list of Document IDs that produced that sub-vector.
  
  **A3: B) Decrease $b$, increase $r$.**
  *   **Logic:** If you decrease the number of bands (fewer chances to match) and increase the rows per band (harder to match a specific band), the criteria to become a candidate becomes much stricter. This shifts the curve to the right, meaning only highly similar documents will survive the filter, eliminating your False Positives.
  
  **A4: B) A pair of vectors that hashed to the same bucket, but their actual similarity was below your threshold.**
  *   **Logic:** The LSH step is a *filter*. A False Positive means the filter said "Yes, these look similar!" (by putting them in the same bucket), but when you did the expensive, exact math later, you realized the filter was wrong. 
  
  ---
-