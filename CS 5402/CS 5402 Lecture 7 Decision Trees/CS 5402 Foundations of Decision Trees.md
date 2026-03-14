## 1. The Precursor: Rule-Based Classifiers (Slides 3–4)
Before understanding trees, we must understand **Rules**.
*   **Concept:** A rule is a logical statement in the form: `IF (Condition) THEN (Class Label)`.
*   **The Condition:** Usually a conjunction ("AND") of tests on attributes.
  *   *Example:* `IF (Income < 50k) AND (Refund = Yes) THEN (Evade = No)`.
*   **The Geometry (Slide 4):**
  *   Imagine a 2D plot with Feature $x_1$ on the X-axis and $x_2$ on the Y-axis.
  *   A rule like `IF x1 > 5` draws a **vertical line** at 5.
  *   A rule like `IF x2 < 6` draws a **horizontal line** at 6.
  *   *Key Insight:* Rule-based classifiers (and Decision Trees) carve the feature space into **axis-aligned rectangles** (or hyper-rectangles in high dimensions). They generally cannot draw diagonal lines.
- ## 2. From Rules to Trees (Slide 5)
  A Decision Tree is simply a hierarchical structure that organizes these rules. Instead of a flat list of 100 rules, we have a flowchart.
  
  *   **The Structure:**
    *   **Root Node:** The starting point. It contains the entire dataset. It asks the very first question (e.g., "Is $x_1 > 5$?").
    *   **Internal Nodes (Branch Nodes):** These represent intermediate tests on attributes. They split the data into smaller, more specific subsets.
    *   **Edges (Branches):** The outcome of a test (e.g., "Yes" or "No").
    *   **Leaf Nodes (Terminal Nodes):** The end of the line. These nodes do not ask questions; they assign a **Class Label** (e.g., "Red" or "Blue").
- ## 3. The Algorithm: Greedy Recursive Splitting (Slide 6)
  Decision Trees are built using a **Divide and Conquer** strategy (specifically, a Greedy Algorithm).
  
  **The Process:**
  1.  Start with all data at the root.
  2.  **Q1: Which feature to split on?** (e.g., Should we ask about "Age" or "Income"?).
  3.  **Q2: What value to split on?** (e.g., If "Income," is the cutoff \$50k or \$100k?).
  4.  **Evaluation:** Compare all possible splits and choose the "Best" one (we will define "Best" in Part 2 using math).
  5.  **Recurse:** Once the data is split, treat each child node as its own new problem and repeat steps 2-4.
  6.  **Stop:** Stop when a node is "Pure" (contains only one class) or other stopping criteria are met.
- ## 4. The "Greedy" Limitation
  *   **Definition:** A greedy algorithm makes the locally optimal choice at each step (e.g., "What is the single best split *right now*?").
  *   **The Risk:** It does **not** look ahead. It might choose a split that looks great now but leads to a messy tree later. However, checking every possible tree combination is computationally impossible (NP-Complete), so we accept the Greedy approach.
  
  ---
- # Problem Solving & Concept Check
  
  **1. Geometric Intuition:**
  You have a dataset with two features: $X$ (0 to 10) and $Y$ (0 to 10).
  *   Class A lives in the square defined by $0 < X < 5$ and $0 < Y < 5$.
  *   Class B lives everywhere else.
  *   *Question:* What is the minimum depth of a Decision Tree required to perfectly classify this?
    *   *Answer:* Depth 2 (or 2 splits).
    *   Split 1: Is $X < 5$? (Divides left/right).
    *   Split 2 (on the left branch): Is $Y < 5$? (Isolates the bottom-left square).
  
  **2. Terminology Check:**
  Look at Slide 7.
  *   If a node has arrows coming *into* it, but no arrows going *out* of it, what kind of node is it?
    *   *Answer:* A **Leaf Node** (it makes a prediction).
  
  **3. The Fundamental Questions (Slide 8):**
  You have a column "Humidity" with values [High, Normal] and "Temperature" with values [Hot, Mild, Cool].
  *   To build the root node, the algorithm must compare splitting on "Humidity" vs. splitting on "Temperature." This addresses which of the two fundamental questions?
    *   *Answer:* **Q1: Which feature to split on?** (Q2 deals with the specific cutoff, which is trivial for categorical data but complex for continuous data).
  
  ***