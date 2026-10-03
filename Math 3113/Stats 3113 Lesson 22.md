### **Stat 3113: Lesson 22 - Chi-Squared and F Distributions**
  
  This lesson introduces two more continuous probability distributions that are derived from the Normal distribution. They are not used for making inferences about means, but rather for making inferences about **variances and standard deviations**. They are the foundation for statistical methods like Analysis of Variance (ANOVA) and tests for the equality of variances.
  
  ---
### **1. The Chi-Squared** (\(\chi^2\)) **Distribution**
  
  The Chi-Squared distribution is primarily used for making inferences about a single population's variance, \(\sigma^2\).
#### **A. Definition**
  
  If you take a random sample of size `n` from a **Normal** population with variance \(\sigma^2\), and you calculate the sample variance \(s^2\), then the following statistic has a **Chi-Squared** (\(\chi^2\)) **distribution**:
  
  > \( \chi^2 = \frac{(n-1)s^2}{\sigma^2} \)
  
  *   **Parameter: Degrees of Freedom (df):** Like the t-distribution, the Chi-Squared distribution is defined by a single parameter, the **degrees of freedom**, which is calculated as:
    > **df = n - 1**
  *   **Notation:** We write `X ~ χ²_ν`, where `ν` (nu) represents the degrees of freedom.
#### **B. Properties of the Chi-Squared Distribution**
  
  1.  **Non-negative:** The Chi-Squared statistic is a ratio of squared quantities, so its values are always **greater than or equal to 0**.
  2.  **Positively Skewed (Right-Skewed):** The Chi-Squared distribution is **not symmetric**. It has a long tail to the right.
  3.  **Shape Depends on df:** The shape of the curve depends on its degrees of freedom.
    *   For small df, the skew is very pronounced.
    *   As the degrees of freedom increase, the curve becomes less skewed and more symmetric, eventually approaching a Normal distribution.
  
  ---
### **2. The F-Distribution**
  
  The F-distribution is most commonly used to **compare the variances of two different populations**. It is the key distribution used in Analysis of Variance (ANOVA).
#### **A. Definition**
  
  If you have two independent random samples from two **Normal** populations, and you calculate the two sample variances (\(s_1^2\) and \(s_2^2\)), the **ratio of these variances** (adjusted for their population variances) follows an **F-distribution**:
  
  > \( F = \frac{s_1^2 / \sigma_1^2}{s_2^2 / \sigma_2^2} \)
  
  When testing if the two population variances are equal (i.e., \(\sigma_1^2 = \sigma_2^2\)), the formula simplifies to the ratio of the sample variances:
  > \( F = \frac{s_1^2}{s_2^2} \)
  
  *   **Parameters: Two Degrees of Freedom:** The F-distribution is unique in that it has **two** different degrees of freedom:
    1.  **Numerator Degrees of Freedom (df₁ or v₁):** `df₁ = n₁ - 1`, where `n₁` is the sample size of the group in the numerator.
    2.  **Denominator Degrees of Freedom (df₂ or v₂):** `df₂ = n₂ - 1`, where `n₂` is the sample size of the group in the denominator.
  *   **Notation:** We write `F ~ F_{v₁, v₂}`. The order matters!
#### **B. Properties of the F-Distribution**
  
  1.  **Non-negative:** Like the Chi-Squared distribution, the F-statistic is a ratio of variances, so its values are always **greater than or equal to 0**.
  2.  **Positively Skewed (Right-Skewed):** The F-distribution is also **not symmetric**.
  3.  **Shape depends on both df₁ and df₂:** The specific shape of the F-curve is determined by the pair of degrees of freedom.
  
  ---
### **3. Excel Formulas**
  
  Both distributions are continuous and require software for probability and percentile calculations.
#### **Chi-Squared Formulas:**
  *   **CDF (Cumulative Probability):** To find the area to the left of a \(\chi^2\)-value `x`, `P(X ≤ x)`.
    > `=CHISQ.DIST(x, df, TRUE)`
  *   **Percentiles (Inverse CDF):** To find the \\(\chi^2\\)-value that has a certain area `α` to its left.
    > `=CHISQ.INV(α, df)`
#### **F-Distribution Formulas:**
  *   **CDF (Cumulative Probability):** To find the area to the left of an F-value `x`, `P(X ≤ x)`.
    > `=F.DIST(x, df1, df2, TRUE)`
  *   **Percentiles (Inverse CDF):** To find the F-value that has a certain area `α` to its left.
    > `=F.INV(α, df1, df2)`