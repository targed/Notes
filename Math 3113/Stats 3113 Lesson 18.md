### **Stat 3113: Lesson 18 - The Standard Normal Distribution**
  
  While any combination of mean (\(\mu\)) and standard deviation (\(\sigma\)) can define a Normal distribution, this would require a different calculation or table for every possible scenario. The **Standard Normal Distribution** is a special case that acts as a universal reference, allowing us to solve problems for *any* Normal distribution using a single framework.
  
  ---
### **1. What is the Standard Normal Distribution?**
  
  The Standard Normal Distribution is a Normal distribution with a **mean of 0** and a **standard deviation of 1**.
  
  *   **Parameters:** \(\mu = 0\) and \(\sigma = 1\).
  *   **Notation:** A random variable `Z` that follows this distribution is denoted as **`Z ~ N(0, 1)`**.
  *   **The Z-axis:** The horizontal axis of the Standard Normal curve is referred to as the "z-axis," and values on it are called **z-scores**.
### **2. Standardization and Z-scores**
  
  **Standardization** is the process of converting any normally distributed random variable `X ~ N(μ, σ)` into a standard normal random variable `Z ~ N(0, 1)`. This is done using the **z-score formula**:
  
  > \( Z = \frac{X - \mu}{\sigma} \)
  
  **Interpretation of a Z-score:**
  A z-score measures **how many standard deviations an observation `x` is away from its mean `μ`**.
  *   A **positive z-score** means the observation is *above* the mean.
  *   A **negative z-score** means the observation is *below* the mean.
  *   A z-score of 0 means the observation is exactly at the mean.
  
  **Example:**
  Suppose `X ~ N(17, 6)`. We want to find the z-score for an observation of `x=5`.
  *   \( z = \frac{5 - 17}{6} = \frac{-12}{6} = -2.0 \)
  *   **Interpretation:** The value `x=5` is exactly **2.0 standard deviations below** the mean of 17.
### **3. Finding Probabilities with the Standard Normal Distribution**
  
  Instead of using software like Excel with the specific \(\mu\) and \(\sigma\), we can standardize first and then use a **Standard Normal Cumulative Probability Table** (often called a "Z-table"). This table provides the cumulative probability for various z-scores.
  
  *   **Notation:** The cumulative probability for a z-score is denoted by the Greek letter Phi: **`Φ(z) = P(Z ≤ z)`**. The Z-table gives you the value of `Φ(z)`.
  
  The process is:
  1.  **Standardize:** Convert your `x`-value(s) to `z`-score(s) using the z-score formula.
  2.  **Look up:** Find the probability corresponding to your z-score(s) using the Z-table.
#### **Types of Calculations:**
  
  *   **P(X ≤ x) → P(Z ≤ z) = Φ(z)**
    *   Find the area directly from the Z-table.
    *   *Example:* `P(Z < 1.23) = Φ(1.23) ≈ 0.8907`
  
  *   **P(X > x) → P(Z > z) = 1 - Φ(z)**
    *   Look up `Φ(z)` in the table and subtract from 1.
    *   *Example:* `P(Z > -1.62) = 1 - P(Z ≤ -1.62) = 1 - Φ(-1.62) ≈ 1 - 0.0526 = 0.9474`
  
  *   **P(a < X < b) → P(z₁ < Z < z₂) = Φ(z₂) - Φ(z₁)**
    *   Convert both `a` and `b` to z-scores (`z₁` and `z₂`).
    *   Look up both cumulative probabilities in the table and subtract the smaller from the larger.
### **4. Finding Percentiles (Inverse Problems)**
  
  Percentile problems involve finding a **z-score** that corresponds to a given area (probability).
#### **A. Lower Percentiles**
  
  To find the value `z` that corresponds to the p-th percentile (i.e., the value with an area of `p` to its left).
  
  *   **Method:** Look for the probability `p` in the *body* of the Z-table and find the corresponding `z`-score from the row and column. Software can also be used.
  *   *Example:* Find the 60th percentile.
    *   We are looking for the z-score such that `P(Z ≤ z) = 0.60`.
    *   Using software: `=NORM.INV(0.6, 0, 1) ≈ 0.2533`.
    *   This means a z-score of approximately **0.2533** has 60% of the area to its left.
#### **B. Upper Percentiles (Critical Values)**
  
  An **upper percentile**, often called a **critical value** and denoted **`z_α`**, is the z-score with an area of `α` to its **right**.
  
  > **`P(Z ≥ z_α) = α`**
  
  *   **Method:** Since the Z-table gives the area to the *left*, to find `z_α`, you must look for the cumulative probability `1 - α` in the body of the table.
  *   *Example:* Find `z_0.05` (the z-score with 0.05 area to its right).
    *   We need to find the z-score with `1 - 0.05 = 0.95` area to its left.
    *   Looking for 0.9500 in the Z-table, we find it's halfway between z=1.64 and z=1.65. We use the more precise value **`z_0.05` ≈ 1.645**.