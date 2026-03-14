## 1. The Limitation of Perceptron (Slide 3)
As we learned in Lecture 8, the Perceptron has a major flaw: it outputs a "Hard Decision" (1 or -1).
*   **The Problem:** It doesn't tell you **how sure** it is. A point barely crossing the line gets the same label as a point miles away from the line.
*   **The Solution:** Instead of predicting the class directly, we predict the **probability** that the class is 1: $P(y=1 | x)$.
- ## 2. The Sigmoid Function (Slide 4)
  To convert the raw linear score ($z = w^T x + b$)—which can range from $-\infty$ to $+\infty$—into a probability (0 to 1), we use the **Sigmoid (or Logistic) Function**:
  
  $$ \sigma(z) = \frac{1}{1 + e^{-z}} $$
  
  **Properties:**
  *   If $z \to \infty$, $\sigma(z) \to 1$.
  *   If $z \to -\infty$, $\sigma(z) \to 0$.
  *   If $z = 0$, $\sigma(z) = 0.5$ (The Decision Boundary).
  *   **Differentiability:** Unlike the "Step Function" of the Perceptron, the Sigmoid is smooth and differentiable everywhere, allowing us to use Calculus/Gradient Descent effectively.
- ## 3. The Loss Function: Cross-Entropy (Slides 5–6)
  We cannot use the Perceptron loss (distance of errors) or Mean Squared Error (MSE) effectively here. Instead, we use **Maximum Likelihood Estimation (MLE)**.
  
  **The Intuition:** We want to maximize the probability of the correct label.
  *   If $y=1$, we want $\hat{y}$ (prediction) to be close to 1.
  *   If $y=0$, we want $\hat{y}$ to be close to 0 (so $1-\hat{y}$ is close to 1).
  
  **The Log-Loss Formula (Binary Cross-Entropy):**
  $$ J(w) = - \sum_i [ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) ] $$
  *(Note: $\hat{y}_i$ is the predicted probability $\sigma(z)$)*.
  
  *   **Why Log?** It turns products of probabilities into sums, preventing arithmetic underflow and simplifying derivatives.
  *   **Why Negative?** Logarithms of probabilities (0 to 1) are negative. We want to *minimize* the loss, so we flip the sign to make it positive.
  *   **Convexity:** Crucially, this loss function is **Convex**. Gradient Descent is guaranteed to find the global minimum (unlike in Neural Networks).
- ## 4. The Gradient Update (Slides 7–10)
  If you take the derivative of the Log-Loss with respect to the weights (applying the Chain Rule), you get a beautiful, simple update rule.
  
  **The Update Rule:**
  $$ w \leftarrow w + \eta (y - \hat{y}) x $$
  *(Note: The slides use $\eta$ for learning rate and $(y - \sigma(z))$ as the error term).*
  
  **Comparison with Perceptron (Slide 9):**
  *   **Perceptron:** $w \leftarrow w + \eta (y - \text{HardPred}) x$
    *   Update happens *only* on error. The error term is always integer 1 or -1 (or 0).
  *   **Logistic:** $w \leftarrow w + \eta (y - \text{Probability}) x$
    *   Update happens *always* (unless probability is exactly 0 or 1).
    *   **Magnitude:** If the model is unsure (e.g., $y=1$, prob=0.6), the error is small (0.4), so the update is small. If the model is confidently wrong (e.g., $y=1$, prob=0.01), the error is huge (0.99), so the update is large.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Sigmoid Math:**
  If the raw linear score is $z = 0$, what is the output probability?
  *   *Calculation:* $\sigma(0) = \frac{1}{1 + e^{0}} = \frac{1}{1+1} = \mathbf{0.5}$.
  *   *Interpretation:* The point is exactly on the decision boundary. The model is 50/50 unsure.
  
  **2. Loss Function Logic:**
  Sample: $y=1$.
  *   Model A predicts $\hat{y} = 0.9$. Loss $\approx -\log(0.9) = 0.10$.
  *   Model B predicts $\hat{y} = 0.1$. Loss $\approx -\log(0.1) = 2.30$.
  *   *Conclusion:* Model B is penalized heavily for being confidently wrong.
  
  **3. Update Dynamics:**
  You have a data point $x=[2, 2]$. Label $y=1$.
  *   **Case 1:** Current prediction is 0.99.
    *   Error = $1 - 0.99 = 0.01$. The weight update is tiny. The model is already happy.
  *   **Case 2:** Current prediction is 0.20.
    *   Error = $1 - 0.20 = 0.80$. The weight update is large. The model aggressively moves the line to fix this.
  
  ***