### **Stat 3113 - Practice Exam 2**
- **1. (8 points) Multiple Choice:** For the multiple choice questions, shade the correct answer.
  
  **(a) (2 points)** A Normal Distribution is uniquely defined by which pair of parameters?
    *   ◯ Mean and Median
    *   ◯ Mean and Standard Deviation
    *   ◯ Sample Size and Standard Deviation
    *   ◯ Variance and Range
  
  **(b) (2 points)** Which statement correctly describes the Central Limit Theorem?
    *   ◯ For a large sample size, the population distribution becomes approximately normal.
    *   ◯ For any sample size, the sampling distribution of the sample mean is exactly normal.
    *   ◯ For a large sample size, the sampling distribution of the sample mean becomes approximately normal, regardless of the population's shape.
    *   ◯ The mean of the sample is always equal to the mean of the population.
  
  **(c) (2 points)** A researcher calculates a 95% confidence interval and a 99% confidence interval from the same sample data. Which of the following is true?
    *   ◯ The 99% interval will be narrower.
    *   ◯ The 95% interval will be wider.
    *   ◯ Both intervals will have the same width.
    *   ◯ The 99% interval will be wider.
  
  **(d) (2 points)** A Type II Error in hypothesis testing is defined as:
    *   ◯ Rejecting a true null hypothesis.
    *   ◯ Failing to reject a true null hypothesis.
    *   ◯ Rejecting a false null hypothesis.
    *   ◯ Failing to reject a false null hypothesis.
- **2. (10 points) True/False:** Indicate whether the following statements are true or false.
  
  **(a)** ◯ True   ◯ False
  The t-distribution has thinner tails than the standard normal (Z) distribution.
  
  **(b)** ◯ True   ◯ False
  For a continuous random variable `X`, the probability `P(X = 10)` is equal to 0.
  
  **(c)** ◯ True   ◯ False
  The "memoryless" property means that for an exponential distribution, the waiting time for an event depends on how long you've already waited.
  
  **(d)** ◯ True   ◯ False
  If you fail to reject the null hypothesis, you have proven that the null hypothesis is true.
  
  **(e)** ◯ True   ◯ False
  Increasing the sample size `n` will typically decrease the standard error of the sample mean.
- ---
- **3. (16 points) Continuous Random Variable Analysis**
  The time `X` (in hours) required to complete a certain manufacturing task follows a probability density function (pdf) given by:
  \( f(x) =
  \begin{cases}
  \frac{1}{8}(4-x) & \text{for } 0 \le x \le 4 \\
  0 & \text{otherwise}
  \end{cases} \)
  
  **(a) (4 points)** Verify that this is a valid probability density function. (Show that the total area under the curve is 1).
  **(b) (4 points)** Calculate the expected value (mean) of the time required for the task, \(E[X]\).
  **(c) (4 points)** Calculate the variance of the time required, \(V(X)\).
  **(d) (4 points)** Find the probability that the task takes less than 1 hour to complete, \(P(X < 1)\).
- ---
- **4. (14 points) Exponential Distribution and Component Lifetime**
  The lifetime `X` (in years) of a satellite's power component follows an Exponential distribution. The mean lifetime is known to be 8 years.
  
  **(a) (4 points)** What is the rate parameter \(\lambda\) for this distribution?
  **(b) (5 points)** What is the probability that the component lasts for more than 10 years?
  **(c) (5 points)** Given that the component has already lasted for 6 years, what is the probability that it lasts for an additional 10 years (i.e., a total of at least 16 years)?
- **5. (10 points) Central Limit Theorem Application**
  The average time students at a large university spend studying per week is \(\mu = 20\) hours, with a population standard deviation of \(\sigma = 6\) hours. The shape of this distribution is known to be skewed to the right. A random sample of 36 students is selected.
  
  **(a) (2 points)** Why can we use the Central Limit Theorem in this scenario?
  **(b) (3 points)** What is the standard error of the sample mean?
  **(c) (5 points)** Calculate the probability that the sample mean study time is greater than 22 hours.
- ---
- **6. (32 points) Hypothesis Testing with JMP Output**
  A pharmaceutical company has developed a new drug designed to reduce blood pressure. They claim the new drug reduces systolic blood pressure by an average of more than 15 mmHg. To test this, a random sample of 25 patients is given the drug, and the reduction in their blood pressure is recorded. The company wants to test their claim at a significance level of `α = 0.05`.
  
  Below is the output from JMP for a hypothesis test.
  
  ```
  ▼ Test Mean
  Hypothesized Value      15
  Actual Estimate        17.8
  DF                     24
  Std Dev              5.2
  
  ▼ t Test
  Test Statistic        2.6923
  Prob > |t|            0.0128*
  Prob > t              0.0064*
  Prob < t              0.9936
  
  ▼ Confidence Intervals
  Parameter      Estimate   Lower CI   Upper CI   1-Alpha
  Mean             17.8     15.659     19.941      0.950
  Std Dev           5.2      4.011      7.215      0.950
  ```
  
  **(a) (4 points)** Which specific statistical test was performed?
    *   ◯ T-test for single mean
    *   ◯ Z-test for single mean
    *   ◯ Paired-data T-test
    *   ◯ Two-Sample Independent T-test
  
  **(b) (4 points)** Select the appropriate null and alternative hypotheses.
    *   ◯ \(H₀: \mu = 15, Hₐ: \mu < 15\)
    *   ◯ \(H₀: \mu = 15, Hₐ: \mu \ne 15\)
    *   ◯ \(H₀: \mu = 15, Hₐ: \mu > 15\)
  
  **(c) (4 points)** Obtain the test statistic from the output provided.
    Test Statistic = \_\_\_\_\_\_\_\_\_\_
  
  **(d) (4 points)** Identify the appropriate p-value from the output provided.
    P-value = \_\_\_\_\_\_\_\_\_\_
  
  **(e) (4 points)** Based on the p-value, should H₀ be rejected?
    *   ◯ Yes, reject H₀.
    *   ◯ No, do not reject H₀.
  
  **(f) (4 points)** What is the overall conclusion in the context of the problem?
    *   ◯ At a 5% significance level, the data does not support the claim that the drug reduces blood pressure by more than 15 mmHg.
    *   ◯ At a 5% significance level, the data provide enough evidence to conclude that the drug reduces blood pressure by more than 15 mmHg.
  
  **(g) (4 points)** Does the normality assumption need to be checked for this problem?
    *   ◯ Yes, because the sample size is small (`n < 30`).
    *   ◯ No, because the sample size is large enough for the CLT.
    *   ◯ No, the t-test does not require a normality assumption.
  
  **(h) (4 points)** Using the confidence interval provided, would you reach the same conclusion for a *two-sided* test (\(Hₐ: \mu \ne 15\))? Explain.
  
  ---
### **Solutions**
  
  **1. Multiple Choice:**
  (a) **Mean and Standard Deviation**.
  (b) **For a large sample size, the sampling distribution of the sample mean becomes approximately normal, regardless of the population's shape.**
  (c) **The 99% interval will be wider.** (Higher confidence requires a wider interval).
  (d) **Failing to reject a false null hypothesis.** (A "missed effect").
- **2. True/False:**
  (a) **False.** The t-distribution has *thicker/fatter* tails to account for more uncertainty.
  (b) **True.** The probability of a continuous variable taking any single exact value is zero.
  (c) **False.** The memoryless property means the waiting time is *unaffected* by how long you've already waited.
  (d) **False.** Failing to reject H₀ only means there is insufficient evidence to reject it; it does not prove H₀ is true.
  (e) **True.** The standard error formula is \\(\sigma/\sqrt{n}\\). As `n` increases, the denominator gets larger, so the overall value decreases.
- **3. Continuous RV:**
  (a) \(\int_0^4 \frac{1}{8}(4-x)dx = \frac{1}{8}[4x - \frac{x^2}{2}]_0^4 = \frac{1}{8}[(16 - \frac{16}{2}) - 0] = \frac{1}{8} = 1\). It is a valid pdf.
  (b) \(E[X] = \int_0^4 x \cdot \frac{1}{8}(4-x)dx = \frac{1}{8}\int_0^4 (4x-x^2)dx = \frac{1}{8}[2x^2 - \frac{x^3}{3}]_0^4 = \frac{1}{8}[32 - \frac{64}{3}] = \frac{1}{8}[\frac{32}{3}] = \frac{4}{3} \approx 1.333\).
  (c) First, find \(E[X^2] = \int_0^4 x^2 \cdot \frac{1}{8}(4-x)dx = \frac{1}{8}\int_0^4 (4x^2-x^3)dx = \frac{1}{8}[\frac{4x^3}{3} - \frac{x^4}{4}]_0^4 = \frac{1}{8}[\frac{256}{3} - 64] = \frac{1}{8}[\frac{64}{3}] = \frac{8}{3} \approx 2.667\).
  Then, \(V(X) = E[X^2] - (E[X])^2 = \frac{8}{3} - (\frac{4}{3})^2 = \frac{8}{3} - \frac{16}{9} = \frac{24-16}{9} = \frac{8}{9} \approx 0.889\).
  (d) \(P(X < 1) = \int_0^1 \frac{1}{8}(4-x)dx = \frac{1}{8}[4x - \frac{x^2}{2}]_0^1 = \frac{1}{8}[4 - \frac{1}{2}] = \frac{1}{8}[3.5] = \frac{3.5}{8} = 0.4375\).
- **4. Exponential:**
  (a) The mean is \(\mu = 1/\lambda\). So, \(\lambda = 1/\mu = 1/8 = \textbf{0.125}\).
  (b) \(P(X > 10) = e^{-\lambda x} = e^{-(0.125)(10)} = e^{-1.25} \approx \textbf{0.2865}\).
  (c) By the memoryless property, \(P(X > 16 | X > 6) = P(X > 10)\), which is the same answer as part (b), approximately **0.2865**.
- **5. CLT:**
  (a) Because the **sample size is large (n=36 ≥ 30)**, the CLT ensures the sampling distribution of \(\bar{X}\) is approximately normal.
  (b) \(SE(\bar{X}) = \sigma/\sqrt{n} = 6/\sqrt{36} = 6/6 = \textbf{1.0}\).
  (c) The sampling distribution is `X̄ ≈ N(20, 1)`. We need \(P(\bar{X} > 22)\). Standardize: \(Z = (22-20)/1 = 2.0\).
  \(P(Z > 2.0) = 1 - P(Z \le 2.0) = 1 - 0.9772 = \textbf{0.0228}\).
- **6. Hypothesis Testing:**
  (a) **T-test for single mean.** (Population \(\sigma\) is unknown).
  (b) \(H₀: \mu = 15, Hₐ: \mu > 15\). (The claim is that the reduction is "more than" 15).
  (c) Test Statistic = **2.6923**.
  (d) P-value = **0.0064**. (This corresponds to "Prob > t" for the right-tailed test).
  (e) **Yes, reject H₀.** (Because the p-value 0.0064 is less than α = 0.05).
  (f) **At a 5% significance level, the data provide enough evidence to conclude that the drug reduces blood pressure by more than 15 mmHg.** (Since we rejected H₀, we have evidence for Hₐ).
  (g) **Yes, because the sample size is small (n=25 < 30).** (The t-test relies on the assumption that the underlying population data is approximately normal, which is especially important for small samples).
  (h) **Yes, we would still reject H₀ for a two-sided test.** The 95% CI is `(15.659, 19.941)`. The null value of 15 is **not** inside this interval, so we would reject H₀.