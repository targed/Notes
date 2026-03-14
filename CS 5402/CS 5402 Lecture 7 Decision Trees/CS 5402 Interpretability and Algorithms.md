## 1. The "White Box" Advantage (Slides 49–51)
**Concept:** In AI, models are often categorized as "Black Box" (accurate but unexplainable, like Deep Learning) or "White Box" (transparent). Decision Trees are the ultimate **White Box** model.

*   **The "Right to Explanation":** In industries like **Finance** (Loan Denial), **Healthcare** (Diagnosis), and **Law** (Sentencing), you cannot simply say " The AI said so." You must explain *why*.
*   **The Logic:** A decision tree allows you to trace a specific path:
  > "The loan was denied because Income < \$50k AND Credit Score < 600."
*   **Debugging:** If a tree makes a mistake, you can inspect the node where the wrong decision was made and understand exactly which feature caused the error.
- ## 2. Limitations of Decision Trees (Slide 52)
  Despite their transparency, single Decision Trees have significant mathematical flaws:
  1.  **Greedy Strategy:** Because they look for the best split *now* without looking ahead, they often miss the globally optimal tree structure.
  2.  **High Variance (Instability):** A tiny change in the training data (e.g., changing one data point) can cause the root node split to change, which completely rewrites the rest of the tree.
  3.  **Axis-Aligned Splits:** Trees can only draw vertical or horizontal lines ($x > 5$). They cannot draw diagonal lines ($x > y$). To approximate a diagonal boundary, a tree must create a jagged "staircase" structure, which is inefficient and prone to overfitting.
  
  ---
- ## 3. The Three Great Algorithms (Slide 56)
  Slide 56 asks you to explore **ID3, C4.5, and CART**. These are the specific software implementations of the Decision Tree logic. Here is the breakdown you need for exams:
- ### A. ID3 (Iterative Dichotomiser 3)
  *   **The Original:** Developed by Ross Quinlan (1986).
  *   **Metric:** Uses **Information Gain**.
  *   **Splits:** Creates a branch for *every* unique value of a categorical feature (Multi-way split).
  *   **Limitations:**
    *   Cannot handle **Continuous Attributes** (Slide 56 Q1: It has no mechanism to sort and find midpoints).
    *   Cannot handle **Missing Data**.
    *   Prone to the "High Cardinality" bias (splitting on ID).
- ### B. C4.5 (The Successor)
  *   **The Improvement:** Also by Quinlan (1993).
  *   **Metric:** Uses **Gain Ratio** (to fix the bias problem).
  *   **Continuous Data (Slide 56 Q2):** It handles numbers by sorting them and finding the best binary threshold (as discussed in Part 3).
  *   **Missing Data:** It handles missing values by ignoring them in the gain calculation or distributing them probabilistically to child nodes.
  *   **Pruning:** It includes built-in Post-Pruning.
- ### C. CART (Classification And Regression Trees)
  *   **The Modern Standard:** Used by Python's `scikit-learn`.
  *   **Metric:** Uses **Gini Index** (computationally faster than log-based Entropy).
  *   **Structure:** strictly **Binary Trees**.
  *   **Why Binary? (Slide 56 Q3):**
    *   *Efficiency:* Multi-way splits fragment the data too quickly (reducing sample size in leaves).
    *   *Simplicity:* Instead of splitting "Color" into {Red, Green, Blue}, CART splits it into {Red} vs {Green, Blue}. This prevents overfitting on categorical variables with many levels.
  *   **Regression:** CART is the only one of the three that can also predict continuous numbers (Regression Trees) by minimizing variance instead of impurity.
  
  ***
- # Answers to "After-Class Questions" (Slides 53–56)
  
  **1. What does "node impurity" mean?**
  It is a measure of how mixed the classes are in a node. A node with 50% Yes and 50% No has maximum impurity (High Entropy).
  
  **2. Give an example of a feature favored by Information Gain but not useful.**
  "Customer ID" or "Row Number." It creates pure leaf nodes (Gain is high) but has zero predictive power for new customers.
  
  **3. Why does ID3 not support continuous features?**
  ID3 relies on creating a branch for every unique value. For a continuous variable (like temperature), almost every value is unique, which would create a massive, wide, flat tree with 1 item per leaf immediately. ID3 lacks the algorithm to "bin" these values into ranges.
  
  **4. Why does CART restrict splits to be binary?**
  To avoid data fragmentation. Splitting a node into 10 children (for 10 categories) drastically reduces the data available for the next level of the tree, leading to statistically insignificant splits. Binary splits keep the data "thicker" as you go down the tree.
  
  ***
- # Final Summary of Lecture 7
  You have now mastered Decision Trees:
  1.  **Concept:** Using a flowchart of rules to classify data.
  2.  **Math:** Using **Entropy** and **Information Gain** to greedily choose the best questions to ask.
  3.  **Refinement:** Using **Gain Ratio** to avoid bias and **Binary Splits** to handle continuous numbers.
  4.  **Control:** Using **Pruning** (Post or Pre) to prevent the tree from memorizing noise (Overfitting).