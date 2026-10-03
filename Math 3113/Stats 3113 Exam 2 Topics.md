### **Topic Breakdown for Exam 2 (Lessons 14-25)**
#### **Continuous Random Variables (Lessons 14 & 15)**
  *   **Lesson 14: Introduction to Continuous RVs**
    *   **Concept:** Understanding the difference between a discrete Probability Mass Function (pmf) and a continuous **Probability Density Function (pdf)**.
    *   **Key Idea:** For continuous variables, probability is the **area under a curve**, not the height of a point.
    *   **Crucial Property:** The probability of a continuous random variable being equal to any single value is zero (\(P(X=c)=0\)). This means \(P(X \le a) = P(X < a)\).
    *   **Properties of a pdf, f(x):**
        1.  It must be non-negative (\(f(x) \ge 0\)).
        2.  The total area under the entire curve must be 1 (\(\int_{-\infty}^{\infty} f(x)dx = 1\)).
    *   **Calculations:**
        *   Finding probabilities by integrating the pdf over an interval: \(P(a \le X \le b) = \int_a^b f(x)dx\).
        *   Calculating **Expected Value (Mean)**: \(\mu = E[X] = \int_{-\infty}^{\infty} x \cdot f(x)dx\).
        *   Calculating **Variance**: \(\sigma^2 = V[X] = E[X^2] - \mu^2\), where \(E[X^2] = \int_{-\infty}^{\infty} x^2 \cdot f(x)dx\).
  
  *   **Lesson 15: The Continuous CDF**
    *   **Concept:** The **Cumulative Distribution Function (CDF)**, \(F(x)\), is a function that gives the total probability up to a value `x`.
    *   **Definition:** \(F(x) = P(X \le x) = \int_{-\infty}^x f(t)dt\).
    *   **Key Relationship:** The pdf is the derivative of the CDF (\(f(x) = F'(x)\)).
    *   **Skills:**
        *   Deriving the complete, piecewise CDF from a given pdf.
        *   Using the CDF as a shortcut to calculate probabilities:
            *   \(P(X \le a) = F(a)\)
            *   \(P(X > a) = 1 - F(a)\)
            *   \(P(a < X < b) = F(b) - F(a)\)
#### **Named Continuous Distributions (Lessons 16, 17, 18)**
  *   **Lesson 16: The Exponential Distribution**
    *   **Application:** Models the waiting time between events in a Poisson process or the lifetime of a component.
    *   **Parameter:** The rate parameter, \\(\lambda\\).
    *   **Formulas:**
        *   Mean: \(\mu = 1/\lambda\)
        *   Standard Deviation: \(\sigma = 1/\lambda\)
        *   CDF: \(F(x) = 1 - e^{-\lambda x}\)
        *   Probability of lasting longer than x: \(P(X > x) = e^{-\lambda x}\)
    *   **Key Property:** The **Memoryless Property** (\(P(X > s+t | X > s) = P(X > t)\)).
  
  *   **Lessons 17 & 18: The Normal & Standard Normal Distributions**
    *   **Concept:** The properties of the bell-shaped Normal distribution, defined by its mean \(\mu\) and standard deviation \(\sigma\).
    *   **The Empirical Rule:** Know the 68-95-99.7% rule for 1, 2, and 3 standard deviations from the mean.
    *   **Standardization:** The process of converting any Normal variable `X` into a Standard Normal variable `Z` (with \(\mu=0, \sigma=1\)).
    *   **Z-Score Formula:** \(Z = \frac{X - \mu}{\sigma}\). You must know how to calculate and interpret a z-score.
    *   **Skills (using a Z-table or software):**
        1.  **"Forward" Problems:** Given an `x` or `z` value, find a probability/area.
            *   Find \(P(X \le a)\), \(P(X > a)\), and \(P(a < X < b)\).
        2.  **"Inverse" Problems:** Given a probability/area, find the corresponding `x` or `z` value.
            *   Find **percentiles** (e.g., find the value of `x` for the 90th percentile).
            *   Find **critical values** (e.g., find the `z`-score that has an area of `α` to its right).
#### **Sampling Distributions (Lessons 19, 20, 21, 22)**
  *   **Lesson 19: Determining Normality**
    *   **Concept:** Knowing how to check if a dataset is approximately normal is a required assumption for many statistical tests.
    *   **Methods:**
        *   Interpreting a **histogram** with a normal curve overlay.
        *   Interpreting a **Normal Probability Plot** (or Q-Q Plot). (Key: if data is normal, points form a straight line).
  
  *   **Lesson 20: The Central Limit Theorem (CLT)**
    *   **Concept:** The single most important theorem in statistics.
    *   **What it says:** For a large sample size (\(n \ge 30\)), the **sampling distribution of the sample mean (\(\bar{X}\))** will be approximately Normal, even if the original population was not.
    *   **Properties of the Sampling Distribution of \(\bar{X}\):**
        *   Mean: \(\mu_{\bar{X}} = \mu\)
        *   Standard Deviation (called the **Standard Error**): \(\sigma_{\bar{X}} = \sigma/\sqrt{n}\)
    *   **Skill:** Calculating probabilities for a *sample mean* using the standardization formula: \(Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}}\).
  
  *   **Lesson 21: The t-Distribution**
    *   **When to use it:** For inference on a mean when the population standard deviation **\(\sigma\) is unknown** (which is almost always the case).
    *   **The t-statistic:** \(t = \frac{\bar{X} - \mu}{s/\sqrt{n}}\), where `s` is the sample standard deviation.
    *   **Parameter:** The **degrees of freedom (df)**, which for a one-sample test is **`df = n-1`**.
    *   **Properties:** Bell-shaped and symmetric like the Normal distribution, but with "fatter tails" to account for the extra uncertainty of using `s` instead of \(\sigma\).
  
  *   **Lesson 22: Chi-Squared & F Distributions**
    *   **Concept:** Know what these distributions are used for.
    *   **Chi-Squared (\(\chi^2\)):** Used for inference on a single population variance (\(\sigma^2\)).
    *   **F-Distribution:** Used for comparing two population variances (\(\sigma_1^2 / \sigma_2^2\)). This is the basis for ANOVA.
    *   **Properties:** Both are right-skewed and can only take non-negative values.
#### **Statistical Inference (Lessons 23, 24, 25)**
  *   **Lesson 23: Introduction to Hypothesis Testing**
    *   **The Logic:** The framework of assuming a "no effect" claim is true and looking for evidence against it.
    *   **Key Components:**
        *   **Null Hypothesis (H₀):** The status quo, claim of no difference (e.g., \(\mu = 50\)).
        *   **Alternative Hypothesis (Hₐ):** The research claim you want to prove (e.g., \(\mu \ne 50\), \(\mu > 50\), or \(\mu < 50\)).
        *   **Test Statistic:** A standardized score measuring how far the sample result is from the null claim.
        *   **P-value:** The probability of getting your sample result (or something more extreme), assuming the null hypothesis is true.
        *   **Significance Level (α):** The cutoff for making a decision.
    *   **The Decision Rule:** If **p-value ≤ α**, you **Reject H₀**. If **p-value > α**, you **Fail to Reject H₀**.
    *   **Errors:** Know the definitions of **Type I Error** (rejecting a true null) and **Type II Error** (failing to reject a false null).
  
  *   **Lesson 24: Comparing Two Means**
    *   **The Key Distinction:** Knowing whether a study design uses **independent samples** or **paired data**.
        *   **Independent:** Two separate, unrelated groups are being compared.
        *   **Paired:** Two measurements are taken on the same subject (e.g., before/after) or on matched pairs. The analysis is done on the *differences*.
    *   **Skill:** Be able to read a scenario and determine if it's a paired or independent design.
  
  *   **Lesson 25: Confidence Intervals**
    *   **Purpose:** To provide a range of plausible values for an unknown population parameter.
    *   **General Formula:** Point Estimate ± Margin of Error
    *   **Interpretation:** Be able to correctly interpret a confidence level. "We are 95% confident..." refers to the reliability of the method, not the probability that a specific interval contains the parameter.
    *   **Relationship to Hypothesis Testing:** For a two-tailed test, if the null value is *not* inside the confidence interval, you reject H₀.