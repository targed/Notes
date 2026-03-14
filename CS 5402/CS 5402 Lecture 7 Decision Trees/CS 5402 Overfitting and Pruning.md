## 1. The Phenomenon of Overfitting (Slides 37–40)
**Definition:** Overfitting occurs when a model learns the specific **noise** and random details in the training data rather than the underlying pattern.

*   **The Symptom (Slide 40):**
  *   **Training Accuracy:** Keeps increasing (approaches 100%) as the tree grows deeper.
  *   **Testing Accuracy:** Increases at first, but then **starts to drop** as the tree becomes too complex.
*   **The Cause (Slides 38–39):**
  *   Imagine a node where the outcome is random (e.g., flipping a coin). A decision tree will try to find a pattern anyway (e.g., "If Wind=Strong, it's Heads; if Wind=Weak, it's Tails").
  *   This split reduces entropy in the *training set* (pure luck), but it will fail completely on *new data* because the pattern doesn't actually exist.
- ## 2. Prevention Strategy A: Pre-Pruning (Early Stopping) [Slide 45]
  **Concept:** Stop the tree from growing *before* it becomes too complex. You halt the recursion if certain conditions are met.
  
  **Common Hyperparameters (Stopping Criteria):**
  1.  **Max Depth:** Do not split if the tree is already $D$ levels deep.
  2.  **Min Samples per Split:** Do not split a node if it contains fewer than $N$ examples (e.g., if a node has only 3 people, stop).
  3.  **Information Gain Threshold:** Do not split if the Gain is less than a small threshold $\epsilon$ (e.g., 0.01).
  
  *   **Pros:** Fast (you don't build unnecessary nodes).
  *   **Cons:** **The Horizon Effect.** A split might look useless now (Low Gain) but could unlock a massive Gain in the *next* level. Pre-pruning stops too early and misses these opportunities.
- ## 3. Prevention Strategy B: Post-Pruning (Slides 46–49)
  **Concept:** Grow the tree to its maximum extent (let it overfit), and then trim the branches that don't help. This is generally considered the **better approach** for accuracy.
  
  **The Algorithm:**
  1.  **Grow** the full tree until all leaves are pure.
  2.  **Evaluate** from the bottom up. For each subtree, ask: "If I replace this entire subtree with a single Leaf Node (predicted as the majority class), does the validation error increase?"
  3.  **Prune:** If the error stays the same or decreases on the validation set, **cut** the subtree and replace it with a leaf.
  4.  **Repeat** until no more pruning is possible.
  
  *   **Pros:** Avoids the Horizon Effect. Yields very compact, accurate trees.
  *   **Cons:** Slower training (you build a huge tree just to destroy half of it).
- ## 4. Other Solutions (Slide 43)
  *   **Ensemble Methods:** Instead of trying to perfect one tree, build 100 trees (Random Forest) or build trees sequentially to fix errors (Boosting). *This is the modern industry standard.*
  *   **Dimensionality Reduction:** Remove irrelevant features *before* training so the tree isn't tempted to split on noise.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Identifying the Strategy:**
  You set a rule in your code: `min_samples_leaf = 20`.
  *   Is this **Pre-pruning** or **Post-pruning**?
    *   *Answer:* **Pre-pruning**. You are setting a constraint to stop the growth *during* the build process.
  
  **2. The Horizon Effect:**
  *   Split A: Gain = 0.01 (looks bad).
  *   Split B (child of A): Gain = 0.99 (looks amazing).
  *   If you use **Pre-pruning** with a threshold of 0.05, what happens?
    *   *Answer:* The tree stops at Split A because $0.01 < 0.05$. You never discover the amazing Split B. This is why Pre-pruning is risky.
  
  **3. Visual Diagnosis (Slide 40):**
  You plot the accuracy of your tree.
  *   Depth 3: Train 85%, Test 82%.
  *   Depth 10: Train 95%, Test 84%.
  *   Depth 50: Train 99.9%, Test 75%.
  *   Which depth is the "Sweet Spot"?
    *   *Answer:* **Depth 10**. This is likely where the Test accuracy peaks before the model starts overfitting (memorizing noise).
  
  ***