### **Stat 3113: Lesson 25 - Confidence Intervals Intro**
  
  While a **point estimate** (like the sample mean, \(\bar{x}\)) provides a single best guess for an unknown population parameter, it is almost certainly not the exact true value. A **confidence interval (CI)** is a more useful tool that provides a *range of plausible values* for the parameter, thereby quantifying the uncertainty in our estimate.
  
  ---
### **1. The Structure of a Confidence Interval**
  
  A confidence interval is built around a point estimate and has a standard structure:
  
  > **Point Estimate ± Margin of Error**
  
  *   **Point Estimate:** The statistic calculated from your sample (e.g., \(\bar{x}\)). This is the center of the interval.
  *   **Margin of Error:** The "plus or minus" part that determines the width of the interval. It reflects the uncertainty in our estimate. A smaller margin of error means a more precise estimate.
  
  The final result is an interval `(L, U)` where `L` is the Lower bound and `U` is the Upper bound.
  
  ---
### **2. Deriving a Confidence Interval for a Mean (when \(\sigma\) is known)**
  
  We can derive the formula for a CI using the Central Limit Theorem (CLT). The logic starts with a probability statement about the standardized sample mean `Z`.
  
  1.  We know that for a sample mean \\(\bar{X}\\), the statistic \( Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \) follows a Standard Normal distribution.
  2.  We can find two z-scores, \(-z_{\alpha/2}\) and \(+z_{\alpha/2}\), that capture a certain central probability, `1-α`. The value `1-α` is the **confidence level**.
  3.  Through algebraic rearrangement of the inequality \( -z_{\alpha/2} \le \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \le z_{\alpha/2} \), we isolate the unknown parameter \(\mu\) in the center.
  
  This process yields the formula for a **(1-α)100% confidence interval for \(\mu\)**:
  
  > \( \bar{x} \pm z_{\alpha/2} \left( \frac{\sigma}{\sqrt{n}} \right) \)
  
  Where:
  *   \(\bar{x}\) is the **point estimate**.
  *   \( z_{\alpha/2} \) is the **critical value** from the Z-distribution for the desired confidence level.
  *   \( \frac{\sigma}{\sqrt{n}} \) is the **standard error** of the mean.
  *   The entire second term, \( z_{\alpha/2} \left( \frac{\sigma}{\sqrt{n}} \right) \), is the **margin of error**.
  
  ---
### **3. The Meaning of "Confidence"**
  
  This is a crucial and often misunderstood concept.
  
  *   **INCORRECT Interpretation:** "There is a 95% probability that the true population parameter \(\mu\) is in our calculated interval (L, U)."
    *   This is wrong because the true parameter \(\mu\) is a fixed, constant number. It is either in our specific interval or it is not. The probability is either 1 or 0, we just don't know which.
  
  *   **CORRECT Interpretation:** "We are 95% confident that the true population parameter \(\mu\) is between L and U."
    *   The "95% confidence" refers to the **reliability of the method**, not a specific interval. It means that if we were to repeat our sampling process many times and calculate a 95% confidence interval for each sample, **95% of those constructed intervals would successfully capture the true population parameter \(\mu\)**.
  
  ---
### **4. The Trade-off: Confidence vs. Precision**
  
  There is a natural trade-off between the level of confidence and the precision (width) of the interval.
  
  *   **Higher Confidence Level → Wider Interval:** To be more confident that you have captured the true parameter, you need a wider "net." A 99% CI will always be wider (less precise) than a 95% CI for the same data.
  *   **Larger Sample Size (n) → Narrower Interval:** As the sample size increases, the standard error (\(\sigma/\sqrt{n}\)) decreases. This reduces the margin of error and results in a narrower, more precise interval for the same confidence level. This is the primary way to improve the precision of an estimate.
  
  ---
### **5. Using Confidence Intervals for Hypothesis Testing**
  
  A two-sided confidence interval can be used as a shortcut to perform a two-tailed hypothesis test.
  
  *   **The Rule:** A (1-α)100% CI contains all the values for \(\mu_0\) for which the null hypothesis `H₀: μ = μ₀` would *not* be rejected at a significance level of `α`.
  
  *   **How to use it:**
    1.  State your null hypothesis `H₀: μ = μ₀` (e.g., `H₀: μ = 30`).
    2.  Calculate the corresponding confidence interval (e.g., a 95% CI for `α = 0.05`).
    3.  Check if the null value `μ₀` is inside the interval.
        *   If the null value **IS IN** the interval: **Fail to reject H₀**. The null value is a plausible value.
        *   If the null value **IS NOT IN** the interval: **Reject H₀**. The null value is not a plausible value based on our sample.
  
  **Example:**
  *   `H₀: μ = 30`, `Hₐ: μ ≠ 30`, `α = 0.05`.
  *   The calculated 95% CI is `(23.45, 27.95)`.
  *   **Conclusion:** The null value of 30 is **not** in the interval. Therefore, we **reject H₀**.