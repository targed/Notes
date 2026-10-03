### **Stat 3113: Lesson 17 - The Normal Distribution**
  
  This lesson introduces the **Normal distribution**, a continuous probability distribution that is fundamental to statistical inference. Its symmetric, bell-shaped curve appears naturally in countless real-world scenarios.
  
  ---
### **1. Properties of the Normal Distribution**
  
  The Normal distribution, also called the Gaussian distribution, is a continuous distribution defined by two parameters: its mean (\(\mu\)) and its standard deviation (\(\sigma\)).
  
  *   **Shape:** It is **bell-shaped and perfectly symmetric** around its mean, \(\mu\).
  *   **Center:** The mean \(\mu\) is the center of the distribution. Due to symmetry, the **mean = median**.
  *   **Spread:** The standard deviation \(\sigma\) controls the spread of the curve.
    *   A **larger \(\sigma\)** results in a flatter, wider curve.
    *   A **smaller \(\sigma\)** results in a taller, narrower curve.
  *   **Notation:** We write **`X ~ N(μ, σ)`** to indicate that the random variable `X` follows a Normal distribution with mean \(\mu\) and standard deviation \(\sigma\).
  *   **Total Area:** The total area under the entire curve is always 1.
#### **A. Probability Density Function (pdf)**
  
  The pdf for the Normal distribution is given by the complex formula:
  > \( f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2} \)
  
  **The Integration Problem:** To find probabilities (areas under the curve), one would need to integrate this function. However, this integral **cannot be solved analytically** using standard integration techniques. Therefore, we must rely on numerical methods, which are built into statistical software and calculators.
  
  ---
### **2. The Empirical Rule (68-95-99.7 Rule)**
  
  For any Normal distribution, a predictable percentage of the data falls within a certain number of standard deviations from the mean.
  
  *   Approximately **68%** of the data lies within **1 standard deviation** of the mean (in the interval \(\mu \pm \sigma\)).
  *   Approximately **95%** of the data lies within **2 standard deviations** of the mean (in the interval \(\mu \pm 2\sigma\)).
  *   Approximately **99.7%** of the data lies within **3 standard deviations** of the mean (in the interval \(\mu \pm 3\sigma\)).
  
  **Example:**
  If the lengths of herring follow a Normal distribution with `μ = 54.0 mm` and `σ = 4.5 mm`:
  *   We expect about **95%** of the fish to be between:
    *   `μ ± 2σ = 54.0 ± 2(4.5) = 54.0 ± 9.0`
    *   The interval is **(45.0 mm, 63.0 mm)**.
  
  ---
### **3. Calculating Probabilities with Software (Excel)**
  
  Since we cannot integrate the pdf by hand, we use software functions that compute the cumulative distribution function (CDF), `F(x) = P(X ≤ x)`.
  
  *   **P(X < x) or P(X ≤ x):** (Area to the left of `x`)
    > `=NORM.DIST(x, μ, σ, TRUE)`
  
  *   **P(X > x) or P(X ≥ x):** (Area to the right of `x`)
    > `=1 - NORM.DIST(x, μ, σ, TRUE)`
  
  *   **P(a < X < b):** (Area between `a` and `b`)
    > `=NORM.DIST(b, μ, σ, TRUE) - NORM.DIST(a, μ, σ, TRUE)`
  
  **Example (Herring: `μ=54.0, σ=4.5`)**
  *   **a) Percentage less than 60 mm long (P(X < 60)):**
    `=NORM.DIST(60, 54.0, 4.5, TRUE) ≈ 0.9088` or **90.88%**
  *   **b) Probability of length more than 51 mm (P(X > 51)):**
    `=1 - NORM.DIST(51, 54.0, 4.5, TRUE) ≈ 1 - 0.2525 = 0.7475` or **74.75%**
  *   **c) Proportion between 51 and 60 mm (P(51 < X < 60)):**
    `=NORM.DIST(60, ...) - NORM.DIST(51, ...)`
    `≈ 0.9088 - 0.2525 = 0.6563` or **65.63%**
  
  ---
### **4. Percentiles of a Normal Distribution (Inverse Problems)**
  
  Percentile problems are the reverse of probability problems. Here, you are given a probability (an area) and you need to find the corresponding `x`-value.
  
  *   The **p-th percentile** is the value `x` such that `p%` of the data is less than or equal to `x`.
  
  We use an inverse function in software to solve these problems.
  
  *   **Finding a Lower (or Bottom) percentile:** To find the value `x` that has an area `α` to its left.
    > `=NORM.INV(α, μ, σ)`
  
  *   **Finding an Upper (or Top) percentile:** To find the value `x` that has an area `α` to its right, you must find the value with `1-α` to its left.
    > `=NORM.INV(1 - α, μ, σ)`
  
  **Example (Herring: `μ=54.0, σ=4.5`)**
  *   **a) Find the 70th percentile (70% of fish are shorter than what value?):**
    This is a lower percentile with `α = 0.70`.
    `=NORM.INV(0.70, 54.0, 4.5) ≈ 56.36 mm`
  *   **b) Find the value such that 80% of fish are longer:**
    This is an upper 80th percentile. If 80% are *greater*, then 20% must be *less*. We need to find the lower 20th percentile with `α = 0.20`.
    `=NORM.INV(0.20, 54.0, 4.5) ≈ 50.21 mm`