## 1. What is a Perceptron? (Slide 2)
The Perceptron is the simplest form of a biological neuron simulated mathematically.
*   **Biological Analogy:**
  *   **Inputs ($x$):** Like dendrites receiving signals.
  *   **Weights ($w$):** Like the strength of the synaptic connection.
  *   **Summation:** Like the cell body aggregating signals.
  *   **Activation/Threshold:** Like the neuron firing an electrical spike if the signal is strong enough.
*   **Machine Learning Definition:** It is a **Linear Binary Classifier**. It draws a straight line (or hyperplane) to separate data into two classes (e.g., Yes/No, +1/-1).
- ## 2. The Mathematical Model (Slide 3)
  The decision function is a linear combination of inputs and weights.
  
  **The Formula:**
  $$ f(x) = w^T x + b $$
  *   $x$: The input vector (features).
  *   $w$: The weight vector (parameters the model learns).
  *   $b$: The bias term (allows the line to shift away from the origin).
  
  **The Decision Rule:**
  *   If $w^T x + b > 0 \rightarrow$ Predict Class **+1**.
  *   If $w^T x + b < 0 \rightarrow$ Predict Class **-1**.
  *   If $w^T x + b = 0 \rightarrow$ You are exactly on the decision boundary.
- ## 3. The Geometry of the Decision Boundary (Slides 4–6)
  This is the most critical concept for "Advanced Algorithms" exams. You must understand what $w$ and $b$ represent geometrically.
- ### A. The Hyperplane
  In 2D, $w^T x + b = 0$ creates a **Line**. In 3D, it creates a **Plane**. In $n$-dimensions, it is a **Hyperplane**.
  This hyperplane splits the universe into two half-spaces: the positive side and the negative side.
- ### B. The Normal Vector (Slide 6)
  *   **Claim 1:** The weight vector $w$ is always **orthogonal (perpendicular)** to the decision boundary.
    *   *Why?* If you move along the decision boundary, the prediction score ($w^T x$) doesn't change. This implies the dot product is zero, which geometrically means perpendicularity.
    *   *Intuition:* $w$ points in the direction of the "Positive" class.
- ### C. Distance from Point to Plane (Slide 6)
  To know how "confident" a prediction is, we measure how far a point $x_i$ is from the dividing line.
  **The Distance Formula:**
  $$ d = \frac{|w^T x_i + b|}{||w||} $$
  *   The numerator ($w^T x + b$) is the "raw score."
  *   The denominator ($||w||$) normalizes it so the scale of weights doesn't distort the distance.
  *   *Note:* This concept is the foundation of **Support Vector Machines (SVM)**, which try to maximize this distance (margin).
- ## 4. Linearly Separable Data (Slide 7)
  *   **Definition:** A dataset is "Linearly Separable" if there exists *at least one* line (hyperplane) that can perfectly separate the two classes with zero errors.
  *   **Significance:** The Perceptron algorithm is **guaranteed to converge** (stop and find a solution) *if and only if* the data is linearly separable. If the data is messy (overlapping), the basic Perceptron will loop forever.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Geometric Intuition:**
  You have a 2D Perceptron with weights $w = [2, 0]$ and bias $b = 0$.
  *   What is the equation of the line?
    *   $2x_1 + 0x_2 + 0 = 0 \rightarrow 2x_1 = 0 \rightarrow x_1 = 0$.
  *   **Visual:** This is a vertical line along the Y-axis.
  *   **Direction:** Since $w=[2,0]$, the "Positive" side is to the right ($x_1 > 0$).
  
  **2. The Role of Bias ($b$):**
  Suppose you set $b = 0$. Can the Perceptron classify a dataset where all the Positive points are at $(10, 10)$ and all Negative points are at $(20, 20)$?
  *   *Answer:* **No.** If $b=0$, the decision boundary *must* pass through the origin $(0,0)$. A line through the origin cannot separate two clusters that are both far away in the positive quadrant. The bias $b$ is required to "shift" the line away from the origin.
  
  **3. Vector Math:**
  If point $x$ is on the positive side of the line, what is the sign of the angle between vector $w$ and vector $x$ (assuming $b=0$)?
  *   *Answer:* The angle is less than 90 degrees (Acute), because the dot product $w^T x$ must be positive.
  
  ***