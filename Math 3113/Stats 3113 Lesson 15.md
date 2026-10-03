### **Stat 3113: Lesson 15 - The Cumulative Distribution Function (CDF) for Continuous Variables**
  
  This lesson delves into the Cumulative Distribution Function (CDF), a powerful tool for working with continuous random variables. The CDF provides a direct way to compute probabilities without needing to perform integration every time.
  
  ---
### **1. Definition of the CDF**
  
  The definition of the **Cumulative Distribution Function (CDF)**, denoted `F(x)`, is the same for both discrete and continuous random variables:
  > \( F(x) = P(X \le x) \)
  
  For a continuous random variable with a probability density function (pdf) `f(t)`, the CDF is found by integrating the pdf from negative infinity up to `x`:
  > \( F(x) = \int_{-\infty}^{x} f(t) \,dt \)
  
  **The Fundamental Relationship:**
  The relationship between the pdf and cdf is defined by calculus:
  *   The **CDF** is the **integral** of the **PDF**.
  *   The **PDF** is the **derivative** of the **CDF**.
    > \( f(x) = F'(x) \)
  
  ---
### **2. Using the CDF to Compute Probabilities**
  
  Once the CDF, `F(x)`, is known, calculating probabilities becomes a matter of simple arithmetic, avoiding the need for repeated integration.
  
  1.  **Probability of being less than a value `a`:**
    > \( P(X \le a) = F(a) \)
  
  2.  **Probability of being greater than a value `a`:** (Using the complement rule)
    > \( P(X > a) = 1 - P(X \le a) = 1 - F(a) \)
  
  3.  **Probability of being between values `a` and `b`:**
    > \( P(a < X \le b) = P(X \le b) - P(X \le a) = F(b) - F(a) \)
  
  This is a significant advantage, as integration can be complex, while evaluating a function is straightforward.
  
  ---
### **3. Deriving the CDF from the PDF**
  
  To find the CDF, you must integrate the pdf for all possible ranges of `x`. This typically results in a piecewise function.
#### **Example 1: Mail-Order Solicitation**
  Given the pdf: \( f(x) = \frac{2}{5}(x+2) \) for `0 ≤ x ≤ 1`, and 0 otherwise.
  
  We find the CDF, `F(x)`, by considering three cases:
  
  *   **Case 1: x < 0**
    \( F(x) = \int_{-\infty}^{x} 0 \,du = 0 \)
  *   **Case 2: 0 ≤ x ≤ 1**
    \( F(x) = \int_{-\infty}^{x} f(u) \,du = \int_{-\infty}^{0} 0 \,du + \int_{0}^{x} \frac{2}{5}(u+2) \,du \)
    \( = 0 + \frac{2}{5} [\frac{u^2}{2} + 2u]_{0}^{x} = \frac{2}{5} (\frac{x^2}{2} + 2x) = \frac{x^2 + 4x}{5} \)
  *   **Case 3: x > 1**
    \( F(x) = \int_{-\infty}^{x} f(u) \,du = \int_{0}^{1} \frac{2}{5}(u+2) \,du + \int_{1}^{x} 0 \,du = F(1) + 0 = \frac{1^2 + 4(1)}{5} = 1 \)
  
  The complete **CDF** is:
  \( F(x) =
  \begin{cases}
  0 & \text{if } x < 0 \\
  \frac{x^2 + 4x}{5} & \text{if } 0 \le x \le 1 \\
  1 & \text{if } x > 1
  \end{cases} \)
  
  **Using the CDF to calculate probabilities:**
  *   **P(0.5 < X ≤ 0.75)** = `F(0.75) - F(0.5)`
    = \(\frac{0.75^2 + 4(0.75)}{5} - \frac{0.5^2 + 4(0.5)}{5} = 0.7125 - 0.45 = 0.2625\)
  *   **P(X > 0.65)** = `1 - F(0.65)`
    = \(1 - \frac{0.65^2 + 4(0.65)}{5} = 1 - 0.6045 = 0.3955\)
  *   **P(X ≤ 0.2)** = `F(0.2)`
    = \(\frac{0.2^2 + 4(0.2)}{5} = \frac{0.84}{5} = 0.168\)
  
  ---
#### **Example 2: Finding the Median**
  Given the pdf: \( f(x) = \frac{3}{2x^2} \) for `1 ≤ x ≤ 3`.
  
  First, find the CDF for the interval of interest (`1 ≤ x ≤ 3`):
  \( F(x) = \int_{1}^{x} \frac{3}{2u^2} \,du = \frac{3}{2} [-\frac{1}{u}]_{1}^{x} = \frac{3}{2} (-\frac{1}{x} - (-\frac{1}{1})) = \frac{3}{2}(1 - \frac{1}{x}) \)
  
  To find the **median (\(x_m\))**, we set the CDF equal to 0.5 and solve for `x`:
  > \( F(x_m) = 0.5 \)
  > \( \frac{3}{2}(1 - \frac{1}{x_m}) = 0.5 \)
  > \( 1 - \frac{1}{x_m} = \frac{0.5 \times 2}{3} = \frac{1}{3} \)
  > \( \frac{1}{x_m} = 1 - \frac{1}{3} = \frac{2}{3} \)
  > \( x_m = \frac{3}{2} = 1.5 \)
  
  The median of this distribution is 1.5.