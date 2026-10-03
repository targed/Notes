### **Stat 3113: Lesson 21 - The T-Distribution**
  
  This lesson introduces the **t-distribution**, a probability distribution that is essential for making inferences about a population mean when the population standard deviation, \\(\sigma\\), is unknown.
  
  ---
### **1. The Problem: What if \\(\sigma\\) is Unknown?**
  
  In the previous lesson, we saw that if we know the population mean \(\mu\) and standard deviation \(\sigma\), we can standardize the sample mean \(\bar{X}\) to get a z-score:
  > \( Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \)
  
  This `Z` follows a Standard Normal distribution `N(0, 1)`.
  
  However, in nearly all real-world applications, the population standard deviation **\(\sigma\) is unknown**. The logical step is to replace \(\sigma\) with its estimate from the sample, the **sample standard deviation, `s`**.
  
  When we do this, we create a new statistic, called `T`:
  > \( T = \frac{\bar{X} - \mu}{s/\sqrt{n}} \)
  
  Because we've introduced a *new source of uncertainty* (the sample standard deviation `s`, which varies from sample to sample), this new statistic `T` no longer follows a perfect Normal distribution. Instead, it follows a **t-distribution**.
  
  ---
### **2. The T-Distribution**
  
  The t-distribution is a continuous, bell-shaped, and symmetric probability distribution that is used for inference about a mean when \(\sigma\) is unknown.
#### **A. Parameter: Degrees of Freedom (df)**
  
  Unlike the Normal distribution, which is defined by \(\mu\) and \(\sigma\), the t-distribution is defined by a single parameter called the **degrees of freedom (df)**.
  
  *   For problems involving a single sample mean, the degrees of freedom are calculated as:
    > **df = n - 1**
    where `n` is the sample size.
  *   **Notation:** The degrees of freedom are often denoted by the Greek letter `ν` (nu) or by `k`. We write `T ~ t_ν` to indicate that `T` follows a t-distribution with `ν` degrees of freedom.
#### **B. Properties of the T-Distribution**
  
  1.  **Symmetric and Centered at 0:** Like the standard normal (Z) distribution, all t-curves are bell-shaped and centered at 0.
  
  2.  **Thicker Tails:** The t-distribution has **more area in its tails** (it is more spread out) than the Z-distribution. This extra spread accounts for the added uncertainty of using `s` to estimate \(\sigma\).
  
  3.  **Approaches the Z-distribution as df increases:** As the sample size `n` (and thus the degrees of freedom `df = n-1`) increases, our estimate `s` becomes a more reliable estimate of \(\sigma\). Consequently, the t-distribution becomes less spread out and gets closer and closer to the Standard Normal (Z) distribution.
    *   As `df → ∞`, the t-curve converges to the Z-curve. In practice, for `df > 30` or `df > 100`, the two distributions are very similar.
  
  ---
### **3. Calculating Probabilities and Percentiles with the T-Distribution**
  
  Because the shape of the t-distribution depends on the degrees of freedom, we cannot use a single table like the Z-table. Instead, we rely on software or specialized t-tables.
#### **A. Excel Formulas**
  
  *   **CDF (Cumulative Probability):** To find the area to the left of a t-value `x`, `P(T ≤ x)`.
    > `=T.DIST(x, df, TRUE)`
  
  *   **Percentiles (Inverse CDF):** To find the t-value that has a certain area `α` to its left.
    > `=T.INV(α, df)`
#### **B. Critical Values of the T-Distribution**
  
  A **critical value**, denoted **`t_α,ν`**, is an upper percentile. It is the t-value (with `ν` degrees of freedom) that has an area of `α` to its **right**.
  
  > **`P(T ≥ t_α,ν) = α`**
  
  To find this using software that requires the area to the *left*, you would use the inverse function with a probability of `1-α`.
  
  This concept of critical values is fundamental for constructing confidence intervals and conducting hypothesis tests, which are key applications of the t-distribution.