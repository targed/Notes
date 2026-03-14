## 1. The Perceptron Convergence Theorem (Slide 46)
One of the most remarkable things about the Perceptron is that we can mathematical prove it will work—under specific conditions.

**The Theorem:**
If the training dataset is **linearly separable** (i.e., a perfect dividing line exists), the Perceptron algorithm is **guaranteed to converge** (stop) after a finite number of updates.

**The Bound on Steps:**
The maximum number of mistakes ($k$) the algorithm will make is bounded by:
$$ k \le \frac{R^2}{\gamma^2} $$

*   **$R$ (Radius):** The maximum magnitude of any input vector $x$. 
  *   *Intuition:* If your data points are very far from the origin (large numbers), $R$ is big, and the algorithm takes longer. This is why **Feature Scaling** is crucial.
*   **$\gamma$ (Gamma/Margin):** The distance from the decision boundary to the closest data point.
  *   *Intuition:* The "Gap" between the two classes.
  *   **Large Margin:** The classes are far apart. The algorithm finds the line quickly.
  *   **Small Margin:** The classes are almost touching. The algorithm struggles to fit the line in the tight space.
- ## 2. What if data is NOT Linearly Separable? (Slide 47)
  **Slide Context:** Question 1 asks what happens if no perfect line exists.
  *   **The Consequence:** The Perceptron will **never converge**. It will loop forever, updating weights endlessly, thrashing back and forth between errors.
  *   **The Fix:** In practice, we set a `max_epochs` (maximum number of passes) or use a "Pocket Algorithm" (keep the weights that had the fewest errors so far, even if not perfect).
- ## 3. The XOR Problem (Slide 49)
  This is the famous counter-example introduced by Minsky and Papert in 1969.
  *   **The Logic:**
    *   (0, 0) $\to$ Class 0
    *   (1, 1) $\to$ Class 0
    *   (0, 1) $\to$ Class 1
    *   (1, 0) $\to$ Class 1
  *   **The Geometry:** If you plot these points, the Class 0 points are diagonal from each other, and Class 1 points are diagonal from each other.
  *   **The Limit:** It is mathematically impossible to draw **one straight line** to separate them.
  *   **The Solution (Slide 50):** A single Perceptron fails. But if you chain multiple Perceptrons together in layers, you get a **Multi-Layer Perceptron (Neural Network)**, which creates non-linear boundaries.
- ## 4. Other Limitations (Slide 51)
  Even if the data is linear, the Perceptron has weaknesses compared to modern algorithms:
  1.  **No Probabilities:** It outputs raw scores ($w^T x$). It tells you "Positive," but not "99% sure Positive." (Solution: **Logistic Regression**).
  2.  **No Margin Maximization:** The Perceptron stops as soon as it finds *any* line that separates the data. It might pick a line barely touching the dots. A better model would pick the line exactly in the middle of the gap. (Solution: **SVM**).
  3.  **Scaling Sensitivity:** If one feature ranges from 0-1 and another from 0-1000, the Perceptron weights will be distorted.
  
  ***
- # Answers to In-Class Questions (Slides 47–48, 52)
  
  **Slide 47: Non-Linearly Separable Data**
  *   **Answer:** **B. The algorithm may keep updating indefinitely.** Since error never reaches 0, the `while(error > 0)` loop never breaks.
  
  **Slide 48: Large vs. Small Margin**
  *   **Answer:** **C. Perceptron converges faster on Dataset A (Large Margin).**
    *   *Reasoning:* Look at the bound formula $R^2 / \gamma^2$. Since $\gamma$ (margin) is in the denominator, a larger margin results in a smaller number of steps.
  
  **Slide 52: Conceptual Questions**
  *   **Why update only on misclassified samples?**
    *   If a point is correctly classified, the loss for that point is 0. The gradient is 0. There is no error signal to learn from. "If it ain't broke, don't fix it."
  *   **Why is the loss convex but non-smooth?**
    *   The loss function (Hinge Loss variant) looks like a "V" shape (Slide 23/30). It has a clear bottom (convex), but at the "kink" where the error hits zero, the derivative is undefined (non-smooth). This technically violates standard Gradient Descent rules but works with Sub-Gradient Descent.
  
  ***
- # Final Summary of Lecture 8
  1.  **The Model:** The Perceptron is a linear classifier ($w^T x + b$) that separates data with a hyperplane.
  2.  **The Learning:** It uses **Stochastic Gradient Descent**. It looks at one point, checks for error, and nudges the weights to fix *that specific error*.
  3.  **The Guarantee:** If a linear solution exists, the Perceptron *will* find it.
  4.  **The Catch:** If the data isn't linear (XOR) or is messy, the Perceptron fails. This limitation drove the invention of Neural Networks.