### **Stat 3113: Lesson 11 - Discrete Random Variables**
  
  This lesson describes how to formally characterize the behavior of a discrete random variable by assigning probabilities to its possible outcomes. This is known as its **probability distribution**.
  
  ---
### **1. The Probability Mass Function (pmf)**
  
  The **probability mass function (pmf)**, denoted `p(x)`, gives the probability that a discrete random variable `X` is equal to a specific value `x`.
  
  > \( p(x) = P(X = x) \)
  
  The pmf is a complete description of the probability distribution. It is often represented as a table.
  
  **Properties of a pmf:**
  1.  **Non-negativity:** The probability for any value must be zero or positive.
    \( p(x) \ge 0 \) for all `x`.
  2.  **Sum to One:** The sum of the probabilities over all possible values of `x` must equal 1.
    \( \sum_{\text{all } x} p(x) = 1 \)
  
  **Example: Defects in Boxes**
  *   **Scenario:** 6 boxes are shipped. The number of defects in each box is: {0, 2, 0, 1, 2, 0}.
  *   **Random Variable (X):** Let X = # defects in a randomly chosen box.
  *   **Possible Values (Support) of X:** `{0, 1, 2}`.
  *   **Constructing the pmf:**
    *   P(X=0) = (Number of boxes with 0 defects) / (Total boxes) = 3/6 = 1/2
    *   P(X=1) = (Number of boxes with 1 defect) / (Total boxes) = 1/6
    *   P(X=2) = (Number of boxes with 2 defects) / (Total boxes) = 2/6 = 1/3
  *   **PMF Table:**
  | x | 0 | 1 | 2 | ow (otherwise) |
  | :--- | :-: | :-: | :-: | :--- |
  | **p(x)** | 1/2 | 1/6 | 1/3 | 0 |
  
  ---
### **2. The Cumulative Distribution Function (cdf)**
  
  The **cumulative distribution function (cdf)**, denoted `F(x)`, gives the probability that a random variable `X` takes on a value *less than or equal to* a specific value `x`.
  
  > \( F(x) = P(X \le x) = \sum_{y \le x} p(y) \)
  
  **Key Idea:** The cdf is the "cumulative sum" of the pmf. Its graph is a step function that "jumps" up at each possible value of X by the amount of the probability (the pmf) at that point.
  
  **Example: Defects in Boxes (continued)**
  *   `F(x) = 0` for `x < 0` (since X cannot be negative).
  *   For `0 ≤ x < 1`: `F(x) = P(X ≤ x) = P(X=0) = 1/2`.
  *   For `1 ≤ x < 2`: `F(x) = P(X ≤ x) = P(X=0) + P(X=1) = 1/2 + 1/6 = 4/6`.
  *   For `x ≥ 2`: `F(x) = P(X ≤ x) = P(X=0) + P(X=1) + P(X=2) = 1/2 + 1/6 + 1/3 = 1`.
  
  This is written as a piecewise function:
  \( F(x) =
  \begin{cases}
  0 & \text{if } x < 0 \\
  3/6 & \text{if } 0 \le x < 1 \\
  4/6 & \text{if } 1 \le x < 2 \\
  1 & \text{if } x \ge 2
  \end{cases} \)
  
  ---
### **3. Expected Value (Mean) of a Discrete RV**
  
  The **expected value** of a discrete random variable X, denoted `E(X)` or \(\mu_X\), is the theoretical long-run average value of the variable. It is the weighted average of all possible values, where the weights are their probabilities.
  
  > \( E(X) = \mu = \sum_{\text{all } x} x \cdot p(x) \)
  
  **Note:** The expected value of a random variable is the same concept as the **mean of its population**.
  
  **Example: Defects in Boxes (continued)**
  E(X) = (0 × 1/2) + (1 × 1/6) + (2 × 1/3) = 0 + 1/6 + 2/3 = **5/6 ≈ 0.833**
  *Interpretation:* If we were to repeat this experiment of randomly selecting a box many times, the average number of defects we would find per box would be approximately 0.833.
  
  ---
### **4. Variance and Standard Deviation of a Discrete RV**
  
  The **variance** and **standard deviation** measure the spread or variability of the random variable's distribution around its mean (expected value).
  
  *   **Variance (V(X) or \(\sigma^2\)):**
    > \( V(X) = \sigma^2 = E[(X-\mu)^2] = \sum (x-\mu)^2 p(x) \)
  
  *   **Calculation Formula for Variance:** A more convenient formula for calculations is:
    > \( V(X) = E(X^2) - [E(X)]^2 \)
    where \(E(X^2) = \sum x^2 p(x)\).
  
  *   **Standard Deviation (\(\sigma\)):**
    > \( \sigma = \sqrt{V(X)} \)
  
  **Example: Defects in Boxes (continued)**
  1.  First, find `E(X²)`.
    E(X²) = (0² × 1/2) + (1² × 1/6) + (2² × 1/3) = 0 + 1/6 + 4/3 = 9/6 = 1.5
  2.  Use the calculation formula for variance.
    V(X) = E(X²) - [E(X)]² = 1.5 - (5/6)² = 1.5 - 25/36 = **29/36 ≈ 0.806**
  3.  Find the standard deviation.
    \(\sigma = \sqrt{29/36} \approx 0.898\)
  
  ---
### **5. Rules for Expected Value and Variance**
  
  For a random variable `X` and constants `a` and `b`:
  
  *   **Expected Value of a Linear Transformation:**
    > \( E(aX + b) = aE(X) + b \)
  *   **Variance of a Linear Transformation:**
    > \( V(aX + b) = a^2V(X) \)
    *Note: Adding a constant `b` shifts the distribution but does not change its spread, so `b` does not affect the variance.*
  
  **Example: Defects in Boxes (continued)**
  *   E(3X + 2) = 3E(X) + 2 = 3(5/6) + 2 = 5/2 + 2 = **9/2**
  *   V(3X + 2) = 3²V(X) = 9(29/36) = **29/4**