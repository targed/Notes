### **Part 7: B+-Tree Insertion**

The goal of insertion is to add a record while ensuring no node overflows (exceeds $2d$ entries).

**1. The Algorithm**
1.  **Search:** Find the correct leaf node $L$ where the entry belongs.
2.  **Insert:** Put the entry into $L$.
3.  **Check Capacity:**
  *   If $L$ has space ($< 2d$ entries), **Done**.
  *   If $L$ is full (Overflow), we must **Split** the node.

**2. Splitting a Leaf Node (Copy Up)**
*   Split the full leaf $L$ into two nodes: $L$ and $L_{new}$.
*   Redistribute entries evenly.
*   **Copy Up:** Take the **middle key** and copy it up to the parent index node to serve as the separator/pointer to the new node.
  *   *Important:* The key remains in the leaf node because leaves must contain *all* data keys.

**3. Splitting an Index Node (Push Up)**
*   If the parent index node becomes full after copying up, it must also split.
*   **Push Up:** Take the **middle key** and move it *entirely* up to the parent's parent.
  *   *Important:* The key is **removed** from the current index node because it is now serving as the separator at the level above. Index keys are just signposts, not data.

**4. Tree Growth**
*   Splits propagate upwards.
*   If the **Root** splits, a new root is created above it. This is how the tree grows in height.
*   **Key Concept:** B+-Trees grow from the **bottom up**.

---
- ### **Part 8: B+-Tree Deletion**
  
  The goal of deletion is to remove a record while ensuring no node underflows (drops below 50% occupancy or $d$ entries).
  
  **1. The Algorithm**
  1.  **Search:** Find the leaf $L$ containing the entry.
  2.  **Delete:** Remove the entry.
  3.  **Check Capacity:**
    *   If $L$ is at least half-full ($\ge d$ entries), **Done**.
    *   If $L$ is under-full, we must fix it using **Redistribution** or **Merging**.
  
  **2. Option A: Redistribution (Borrowing)**
  *   Check if a **sibling** (an adjacent node with the same parent) has extra entries.
  *   If yes, move an entry from the sibling to the under-full node.
  *   **Adjust Parent:** Update the separator key in the parent to reflect the shift.
  *   *Pros:* Preferred over merging because it keeps the tree structure stable.
  
  **3. Option B: Merging**
  *   If the sibling is also barely full (has exactly $d$ entries), we cannot borrow.
  *   **Merge:** Combine the under-full node and the sibling into a single node.
  *   **Pull Down:** Since the two nodes are now one, the separator key in the parent is no longer needed. We "pull down" (delete) that key from the parent.
  *   *Effect:* This causes a deletion in the parent node, which might trigger an underflow in the parent (recursion).
  
  **4. Tree Shrinkage**
  *   If merging propagates all the way to the root, and the root ends up with only one child, the root is deleted and the child becomes the new root.
  *   **Key Concept:** B+-Trees shrink from the **top down**.
  
  ---
- ### **Summary of B+-Tree Operations**
  
  | Operation | Condition | Action | Key Movement |
  | :--- | :--- | :--- | :--- |
  | **Insertion** | Node Full | Split | **Copy Up** (Leaf) or **Push Up** (Index) |
  | **Deletion** | Node Under-full (Sibling rich) | Redistribute | Move key sideways, update Parent |
  | **Deletion** | Node Under-full (Sibling poor) | Merge | Combine nodes, **Pull Down** key from Parent |