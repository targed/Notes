### 1. The Problem Statement (Slide 22)
**Input:** An unsorted array $A$ of $n$ integers, and a target integer $t$.
**Goal:** Determine if there are two numbers $x$ and $y$ in $A$ such that $x + y = t$.

*Example:* `A =[4, 1, 9, 3, 5]`, Target `t = 12`. 
*Answer:* "Yes" (because 9 + 3 = 12).

---
- ### 2. The Evolution of the Solution (Slides 23–27)
  
  To solve this, we need to find if the "complement" ($y = t - x$) exists in the array for any given $x$. Let's look at three ways to do this.
- #### Attempt 1: Brute Force (Slide 24)
  For every number $x$, use a **Linear Search** to scan the rest of the array for $y$.
  *   **Time Complexity:** $\mathbf{\Theta(n^2)}$. 
  *   **Why:** For each of the $n$ elements, you scan up to $n$ elements.
  *   **Space Complexity:** $O(1)$.
- #### Attempt 2: Sorting + Binary Search (Slide 25 & 26)
  We can use our Divide and Conquer knowledge to speed this up.
  1.  **Sort** the array $A$. (Takes $O(n \log n)$ time).
  2.  Loop through each element $x$ in the sorted array.
  3.  Instead of a linear search, use **Binary Search** to look for $y = t - x$. (Takes $O(\log n)$ time per element).
  *   **Total Time:** $O(n \log n) + n \times O(\log n) = \mathbf{\Theta(n \log n)}$. 
  *   **Space Complexity:** $O(1)$ or $O(n)$ depending on the sorting algorithm used.
- #### Attempt 3: The Hash Table Solution (Slide 27)
  We can break the $\log n$ barrier using Hashing.
  1.  Create an empty Hash Table $H$.
  2.  **Phase 1:** Loop through $A$ and insert every element into $H$. (Takes $n \times O(1) = O(n)$ time).
  3.  **Phase 2:** Loop through $A$ again. For each $x$, check if $y = t - x$ is in $H$ using a Hash Lookup. (Takes $n \times O(1) = O(n)$ time).
  *   **Total Time:** $O(n) + O(n) = \mathbf{\Theta(n)}$.
  *   **Space Complexity:** $\mathbf{O(n)}$ (Because we must store all $n$ elements in the Hash Table).
  
  ---
- ### 3. Professor Deep Dive: "Expected" Time & Load Factor (Slide 28)
  
  Slide 28 contains a massive "Deep Dive" caveat: **Hashing wins, BUT it is $O(n)$ time *on average* (in expectation).**
  
  Why doesn't the professor say it's an absolute guarantee?
  *   **The Worst-Case Trap:** If you have a terrible hash function, *every single number* might collide and map to the exact same index. If this happens, your Hash Table degenerates into a Linked List.
  *   If that happens, an insert takes $O(1)$, but a lookup takes $O(n)$. 
  *   The total worst-case time for 2-SUM using a Hash Table is technically $\mathbf{O(n^2)}$. 
  
  **How do we prevent the worst-case? (Load Factor & Amortization)**
  *   **Load Factor ($\alpha$):** Defined as `Number of Objects / Array Size`. It measures how "full" your hash table is.
  *   **Amortization:** To keep lookups strictly $O(1)$ on average, the system monitors the Load Factor. When it gets too high (e.g., > 0.75), the system creates a brand new, double-sized array and rehashes everything. Just like the `ArrayList` example from your earlier chapters, this resizing is expensive but happens rarely enough that the **Amortized Time** remains $O(1)$ per operation.
  
  ---
- ### 4. Homework Extension: The 3-SUM Problem (Slide 29)
  The slides end with a prompt for the 3-SUM problem: Find if $x + y + z = t$.
  
  **How to solve it using 2-SUM logic:**
  1.  Sort the array (optional, but helps avoid duplicates).
  2.  Use a `for` loop to fix the first number $x$.
  3.  Now, you just need to find if $y + z = t - x$.
  4.  This is literally just the **2-SUM problem** on the remainder of the array!
  5.  **Complexity:** You run an $O(n)$ 2-SUM algorithm $n$ times (once for each $x$). Therefore, **3-SUM takes $O(n^2)$ time** using hashing.
  
  ---
- ### Part 2 Practice Questions
  
  **Q1: The Time-Space Tradeoff**
  You are writing software for a router with extremely strict memory limits (very small RAM). You need to solve the 2-SUM problem. Which approach should you choose and why?
  1.  The Hash Table approach.
  2.  The Sort + Binary Search approach.
  
  **Q2: Best Case vs. Worst Case**
  In the Hash Table solution for 2-SUM, what is the *Best Case* time complexity, and what is the *Absolute Worst Case* time complexity if the hash function is completely broken?
  
  **Q3: 4-SUM Extension**
  Using the logic we applied to 3-SUM, what would be the expected time complexity of a **4-SUM** algorithm ($w + x + y + z = t$) if you use nested loops combined with a Hash Table?