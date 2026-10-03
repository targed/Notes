### **Descriptive Stats**
  
  This section covers the fundamental numerical summaries for a *sample* of data.
  
  *   **Mean:** \(\bar{x} = \frac{1}{n}\sum x_i\)
  *   **What it is:** The arithmetic average of your sample data.
  *   **How/When to use it:** Use this to find the "center" or "typical" value of your dataset. It's best for data that is symmetric and does not have extreme outliers.
  
  *   **Variance & Std Dev:** \(s^2 = \frac{1}{n-1}\sum(x_i - \bar{x})^2\) and \(s = \sqrt{s^2}\)
  *   **What they are:** Measures of the average spread or variability of the data points around the sample mean (\(\bar{x}\)). The variance is in squared units, while the standard deviation is in the original units of the data.
  *   **How/When to use them:** Use these to describe how consistent or spread out your data is. A small `s` means the data is tightly clustered around the mean; a large `s` means it's more spread out.
  
  *   **IQR & Outliers:** \(IQR = Q_3 - Q_1\); Outliers are `< Q_1 - 1.5 \cdot IQR` or `> Q_3 + 1.5 \cdot IQR`
  *   **What they are:** The IQR is the range of the middle 50% of your data. The outlier rule is a common guideline for identifying data points that are unusually far from the rest of the data.
  *   **How/When to use them:** The IQR is a measure of spread that is resistant to outliers. Use it, along with the outlier rule, to describe the spread of skewed data or data with extreme values.
  
  *   **Skew & Resistant Measures:**
  *   **What it is:** A description of the shape of your data's distribution. "Resistant" means a measure is not strongly affected by outliers.
  *   **How/When to use it:** Compare the mean and median to describe the shape. If `Mean < Median`, it's likely skewed left. If `Mean > Median`, it's likely skewed right. **Always use resistant measures (Median, IQR) to describe the center and spread of skewed data.**
  
  ---
### **Probability Rules**
  
  These are the fundamental rules for calculating and manipulating probabilities of events.
  
  *   **Complement:** \(P(A^c) = 1 - P(A)\)
    *   **What it is:** The probability that event A does *not* happen.
    *   **How/When to use it:** Use this when it's easier to calculate the probability of an event happening and then subtract from 1. It is especially useful for "at least one" problems (e.g., P(at least one) = 1 - P(none)).
  
  *   **General Addition Rule:** \(P(A \cup B) = P(A) + P(B) - P(A \cap B)\)
    *   **What it is:** The probability that event A *or* event B (or both) will occur.
    *   **How/When to use it:** Use this to find the probability of a union of two events. The \(- P(A \cap B)\) term corrects for double-counting the outcomes that are in both events. If the events are mutually exclusive, \(P(A \cap B) = 0\), and the formula simplifies to \\(P(A) + P(B)\\).
  
  *   **Conditional Probability:** \\(P(A|B) = \frac{P(A \cap B)}{P(B)}\\)
    *   **What it is:** The probability of event A happening, *given that* you know event B has already happened.
    *   **How/When to use it:** Use this when you need to update a probability based on new information. The formula shows that you are restricting your sample space to only the outcomes in event B.
  
  *   **General Multiplication Rule:** \\(P(A \cap B) = P(A|B)P(B)\\)
    *   **What it is:** The probability that both event A *and* event B happen.
    *   **How/When to use it:** This is a rearrangement of the conditional probability formula. Use it to find the probability of a sequence of events, especially in tree diagrams where you multiply probabilities along a branch.
  
  *   **Independence:** If \\(P(A|B) = P(A)\\), then \\(P(A \cap B) = P(A)P(B)\\)
    *   **What it is:** Two events are independent if the occurrence of one does not change the probability of the other.
    *   **How/When to use it:** If you know (or can assume) events are independent (like coin flips or sampling with replacement), you can use this simplified multiplication rule to find the probability of their intersection.
  
  *   **Total Probability & Bayes' Theorem:**
    *   **What they are:** Tools for solving complex conditional probability problems, often with tree diagrams. Total Probability finds the overall probability of a final outcome. Bayes' Theorem "reverses" the conditioning, allowing you to find the probability of an earlier event given a later outcome.
    *   **How/When to use them:** Use Total Probability to find the probability of an event in the second (or later) stage of a tree. Use Bayes' Theorem when you are asked for a probability of a first-stage event, given that a second-stage event has occurred (e.g., "Given the product was defective, what is the probability it came from Machine A?").
  
  ---
### **Random Variables (RV)**
  
  This section covers the formulas for probability distributions, which are mathematical models describing random phenomena.
#### **Discrete RV**
  *   **pmf, cdf, Mean, Variance:** These are the theoretical counterparts to the descriptive stats. Instead of averaging data points, you are calculating the weighted average over all possible outcomes, where the weights are the probabilities `p(x)`.
  *   **How/When to use them:** Use these when you have a probability model (like a table of `x` and `P(X=x)`) for a discrete random variable and need to find its theoretical center (mean) and spread (variance).
#### **Continuous RV**
  *   **pdf, cdf, Mean, Variance:** These are the continuous versions of the above, where summations (Σ) are replaced by integrals (∫). The pdf `f(x)` gives the "density," and probability is the *area* under the curve.
  *   **How/When to use them:** Use these when you have a probability density function `f(x)` for a continuous random variable. To find probabilities, you integrate the pdf over an interval. To find the mean or variance, you integrate `x*f(x)` or `x²*f(x)`.
  
  *   **Linear Transformations:** `E[aX+b]` and `Var[aX+b]`
    *   **What they are:** Rules for how the mean and variance change when you scale (`a`) and shift (`b`) a random variable.
    *   **How/When to use them:** Use these when you perform a linear unit conversion (e.g., from Fahrenheit to Celsius) and need to find the new mean and variance without recalculating from scratch. Notice that shifting by `b` does not change the variance.
  
  ---
### **Named Distributions**
  
  These are specific, common probability models. You use them when a problem's setup matches their defined conditions.
  
  *   **Binomial:** Models the *number of successes* in a fixed number (`n`) of independent trials.
    *   **How/When to use it:** Use when you see a scenario with a fixed number of trials, each trial is independent, there are only two outcomes (success/failure), and the probability of success (`p`) is constant.
  
  *   **Poisson:** Models the *number of events* occurring in a fixed interval of time or space.
    *   **How/When to use it:** Use when counting random, independent events over an interval (e.g., defects per meter, calls per hour). Key feature: `Mean = Variance = μ`. Also use it to approximate the Binomial when `n` is large and `p` is small, with `μ = np`.
  
  *   **Normal:** The "bell curve," used for continuous data.
    *   **How/When to use it:** This is the foundational distribution for continuous variables and, importantly, for the sampling distributions used in inference (thanks to the CLT). The Standard Normal `Z ~ N(0,1)` is used to find probabilities for any Normal distribution after converting with `Z = (X-μ)/σ`.
  
  *   **Uniform:** All outcomes in an interval `[a, b]` are equally likely. The pdf is a flat line.
    *   **How/When to use it:** Use when there is no reason to think any value in an interval is more likely than another (e.g., a random angle of imperfection on a circle).
  
  *   **Exponential:** Models the *waiting time* between events in a Poisson process.
    *   **How/When to use it:** Use when a problem asks for the probability related to the time until the *next* event occurs (e.g., time until the next customer arrives). Its key feature is the "memoryless property."
  
  ---
### **Sampling Distributions**
  
  This section describes the probability distributions of *statistics* themselves (like \\(\bar{x}\\) or \\(\hat{p}\\)). This is the bridge between probability and inference.
  
  *   **Sample Mean (\\(\bar{X}\\)) & Sample Proportion (\\(\hat{p}\\)) formulas:**
    *   **What they are:** Formulas for the theoretical mean, variance, and standard error (SE) of the sampling distribution of the sample mean and sample proportion.
    *   **How/When to use them:** These are the foundational formulas for all confidence intervals and hypothesis tests involving means and proportions. They tell you the center and spread of all possible sample results you could get.
  
  *   **Central Limit Theorem (CLT):**
    *   **What it is:** The most important theorem in statistics. It states that for a large sample size (`n`), the sampling distribution of the sample mean (or proportion) will be approximately Normal, *regardless of the original population's shape*.
    *   **How/When to use it:** The CLT is the reason we can use Normal distribution (Z-scores) or t-distributions to perform inference (CIs and tests) on means and proportions, as long as the sample size is large enough.
  
  ---
### **Confidence Intervals (CIs)**
  
  *   **Formulas for Mean (σ known/unknown) and Proportion:**
    *   **What they are:** A recipe for creating a range of plausible values for an unknown population parameter (μ or p). The general form is `Point Estimate ± (Critical Value) × (Standard Error)`.
    *   **How/When to use them:** Use these when asked to *estimate* a population mean or proportion.
        *   Use the **Z** interval for a mean only if the population standard deviation **σ is known** (rare).
        *   Use the **t** interval for a mean if **σ is unknown** and you are using the sample standard deviation `s` (common).
        *   Use the **proportion** formula when estimating a population proportion `p`.
  
  ---
### **Test Statistics**
  
  *   **Formulas for One/Two-sample means and props:**
    *   **What they are:** A standardized score that measures how many standard errors your sample result is from the null hypothesis value. The general form is `(Sample Statistic - Null Value) / (Standard Error)`.
    *   **How/When to use them:** Use these to conduct a hypothesis test.
        *   **One-sample mean:** Testing a claim about a single population mean μ.
        *   **One-sample prop:** Testing a claim about a single population proportion p.
        *   **Two-sample means:** Comparing the means of two independent groups.
        *   **Two-sample props:** Comparing the proportions of two independent groups.
        *   Note the `s_p^2` is the "pooled variance" used in the two-sample t-test when assuming equal variances. The `p-hat` in the two-sample prop test is the "pooled proportion."
  
  ---
### **Errors & Power**
  
  *   **Type I & Type II Errors, Power:**
    *   **What they are:** Definitions for the possible mistakes made in hypothesis testing and the performance of a test.
    *   **How/When to use them:** These are conceptual definitions. You use them to understand the risks and trade-offs of hypothesis testing. **Power (1-β)** is the probability of correctly detecting a real effect, and it is a key measure of how good a test is.