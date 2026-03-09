### 1. Linear Probing (Slides 17–19)
The simplest open addressing strategy. If your target index is full, just look at the very next spot. If that's full, look at the next, wrapping around to the beginning if necessary.

**The Formula:** 
$$ h(k, i) = (h(k) + i) \pmod m $$
(where $k$ is the key, $i$ is the attempt number starting at 0, and $m$ is the array size).

**Deep Dive Walkthrough (The Cascading Collision):**
Look at the example on Slide 18/19 with an array of size 100 and a compression function of `key % 100`.
1.  **Insert 14001:** `14001 % 100 = 1`. Index 1 is empty. (Goes to **01**).
2.  **Insert 53702:** `53702 % 100 = 2`. Index 2 is empty. (Goes to **02**).
3.  **Insert 43201:** `43201 % 100 = 1`. Index 1 is FULL.
  *   Probe $i=1$: `(1 + 1) % 100 = 2`. Index 2 is FULL.
  *   Probe $i=2$: `(1 + 2) % 100 = 3`. Index 3 is empty! (Goes to **03**).
4.  **Insert 70002:** `70002 % 100 = 2`. Index 2 is FULL.
  *   Probe $i=1$: `(2 + 1) % 100 = 3`. Index 3 is FULL (43201 just took it!).
  *   Probe $i=2$: `(2 + 2) % 100 = 4`. Index 4 is empty. (Goes to **04**).

**The Professor Deep Dive: "Primary Clustering"**
Look closely at step 4. `70002` collided with `53702`, but it *also* collided with `43201` during its probe. 
This is the fatal flaw of Linear Probing called **Primary Clustering**. Elements tend to clump together into massive, contiguous blocks. Once a cluster forms, any key that hashes *into* the cluster or *right after* the cluster will make the cluster even bigger, destroying your $O(1)$ lookup time and dragging it toward $O(N)$.

---
- ### 2. Double Hashing (Slides 31–40)
  To fix Primary Clustering, we need a probing sequence that doesn't just step by `+1` every time. We want the step size to be unique to the key itself. We achieve this by introducing a **second hash function**.
  
  **The Formula:**
  $$ h(k, i) = (h_1(k) + i \cdot h_2(k)) \pmod m $$
  *   $h_1(k)$ finds the starting index.
  *   $h_2(k)$ determines the **step size** (the jump).
  *   $i$ is the number of collisions we've had so far ($0, 1, 2 \dots$).
  
  **Deep Dive Walkthrough (Slides 36–40):**
  *   **Table Size ($m$):** 13
  *   **Hash 1:** $h_1(k) = k \pmod{13}$
  *   **Hash 2:** $h_2(k) = 1 + (k \pmod{11})$
  
  **Scenario A (Insert 98):**
  *   $h_1(98) = 98 \pmod{13} = 7$.
  *   *Uh oh, index 7 is already occupied by 72! Collision!*
  *   Calculate step size: $h_2(98) = 1 + (98 \pmod{11}) = 1 + 10 = \mathbf{11}$.
  *   Attempt $i=1$: $(7 + 1 \times 11) \pmod{13} = 18 \pmod{13} = \mathbf{5}$.
  *   Index 5 is empty. Insert 98 there.
  
  **Scenario B (Insert 14 - The Double Collision):**
  *   $h_1(14) = 14 \pmod{13} = 1$.
  *   *Collision with 79!*
  *   Calculate step size: $h_2(14) = 1 + (14 \pmod{11}) = 1 + 3 = \mathbf{4}$.
  *   Attempt $i=1$: $(1 + 1 \times 4) \pmod{13} = \mathbf{5}$. 
  *   *Collision with 98!* (We just put 98 there!).
  *   Attempt $i=2$: $(1 + 2 \times 4) \pmod{13} = 9 \pmod{13} = \mathbf{9}$.
  *   Index 9 is empty. Insert 14 there.
  
  **The Professor Deep Dive: Rules for $h_2(k)$**
  If a professor asks a trick question about Double Hashing, it usually revolves around $h_2(k)$. 
  1.  **Rule 1:** $h_2(k)$ **must never evaluate to 0**. If it did, your step size would be 0, and you would probe the exact same full slot in an infinite loop! Notice how your slide's formula is $1 + (k \pmod{11})$. The `1 +` guarantees it never equals 0.
  2.  **Rule 2:** The step size $h_2(k)$ must be **relatively prime** to the table size $m$. This ensures that the probing sequence eventually visits *every single slot* in the table instead of bouncing back and forth between just two slots. (This is why table sizes are usually prime numbers like 13).
  
  ---
- ### Part 3 Practice Questions
  
  **Q1: Linear Probing Trace**
  You have an empty hash table of size 7. The hash function is $h(k) = k \pmod 7$. You insert the following keys using Linear Probing: `[10, 24, 17]`.
  At what index does the key `17` ultimately get stored?
  *(Hint: Map them out one by one!)*
  
  **Q2: The Infinite Loop**
  You are designing a Double Hashing system. The table size is $m = 10$. 
  $h_1(k) = k \pmod{10}$
  $h_2(k) = k \pmod 5$
  If you try to insert the key `15` and index 5 is already full, what indices will the algorithm probe on attempts $i=1, 2,$ and $3$? Why is this a catastrophic failure?
  
  **Q3: Open Addressing vs. Load Factor**
  In Open Addressing (unlike Separate Chaining with Linked Lists), the Load Factor $\alpha$ (items / table size) can mathematically never exceed what number?