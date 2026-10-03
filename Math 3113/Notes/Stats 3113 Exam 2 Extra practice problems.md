### **Practice Problems: Lesson 14 - Continuous Random Variables**
#### **Multiple Choice Questions**
  
  **1. Which of the following is a required property of a valid probability density function (pdf), `f(x)`?**
    a) The function `f(x)` must be symmetric around its mean.
    b) The total area under the curve of `f(x)` over its entire range must equal 1.
    c) The function `f(x)` must always be less than or equal to 1.
    d) The probability `P(X=c)` must be positive for some constant `c`.
  
  **2. For a continuous random variable `X`, which of the following statements is always true?**
    a) \( P(X \le 5) \) is strictly greater than \( P(X < 5) \).
    b) The expected value, `E[X]`, is always equal to the median.
    c) The probability that `X` equals a specific value, `P(X=c)`, is 0.
    d) The pdf, `f(x)`, represents the probability that `X=x`.
  
  **3. The probability that a continuous random variable `X` falls between `a` and `b` is found by:**
    a) Calculating `f(b) - f(a)`.
    b) Finding the area under the pdf curve between `a` and `b`.
    c) Calculating the average of `a` and `b`.
    d) Finding the height of the pdf at the midpoint between `a` and `b`.
  
  ---
#### **True/False Questions**
  
  **4. True or False:** A function `f(x)` can be a valid probability density function even if `f(x) > 1` for some values of `x`.
  
  **5. True or False:** The cumulative distribution function (CDF), `F(x)`, of a continuous random variable is calculated by taking the derivative of its probability density function (pdf).
  
  **6. True or False:** If `X` is a continuous random variable, then `P(X > 3)` is calculated as \(\int_{3}^{\infty} f(x)dx\).
  
  ---
#### **Calculation Problems**
  
  **7. (Multi-part)** The time `X` (in minutes) that a customer spends in a checkout line is a continuous random variable with the probability density function:
  \( f(x) =
  \begin{cases}
  kx^3 & \text{for } 0 \le x \le 2 \\
  0 & \text{otherwise}
  \end{cases} \)
  
  **(a) Find the value of the constant `k` that makes `f(x)` a valid pdf.**
  **(b) Find the probability that a customer spends less than 1 minute in line, \(P(X \le 1)\).**
  **(c) Find the probability that a customer spends more than 1.5 minutes in line, \(P(X > 1.5)\).**
  **(d) Find the expected value (mean) of the time spent in line, \(E[X]\).**
  **(e) Find the variance of the time spent in line, \(V(X)\).**
  
  ---
### **Practice Problems: Lesson 15 - The Continuous CDF**
#### **Multiple Choice**
  
  **1.** If `F(x)` is the CDF for a continuous random variable `X`, which of the following represents the probability \(P(X > a)\)?
    a) `F(a)`
    b) `1 - F(a)`
    c) `f(a)`
    d) `F(a) - 1`
  
  **2.** Given a CDF `F(x)`, the probability \(P(a < X < b)\) is calculated as:
    a) `F(b)`
    b) `f(b) - f(a)`
    c) `F(a) - F(b)`
    d) `F(b) - F(a)`
  
  **3.** The median of a continuous random variable `X` is the value `m` such that:
    a) `f(m) = 0.5`
    b) `F(m) = 0.5`
    c) `m = E[X]`
    d) `m = 0.5`
  
  ---
#### **True / False**
  
  **4. True or False:** The graph of a CDF for a continuous random variable is a non-decreasing function that goes from 0 to 1.
  
  **5. True or False:** The value of a CDF, `F(x)`, can sometimes be greater than 1.
  
  **6. True or False:** To find the pdf `f(x)` from the CDF `F(x)`, you should integrate `F(x)`.
  
  ---
#### **Calculation Problems**
  
  **Problem 7: Deriving the CDF**
  The lifetime `X` (in years) of a certain electronic component has the probability density function (pdf):
  \( f(x) =
  \begin{cases}
  \frac{1}{4}e^{-x/4} & \text{for } x \ge 0 \\
  0 & \text{otherwise}
  \end{cases} \)
  Find the complete cumulative distribution function (CDF), `F(x)`. Write it as a piecewise function.
  
  **Problem 8: Using the CDF for Probability**
  Using the CDF you derived in Problem 7, find the probability that the component lasts for at most 3 years (i.e., find \(P(X \le 3)\)).
  
  **Problem 9: Using the CDF for an Interval Probability**
  Using the CDF from Problem 7, find the probability that the component lasts between 2 and 5 years (i.e., find \(P(2 \le X \le 5)\)).
  
  **Problem 10: Finding the Median from the CDF**
  Using the CDF from Problem 7, find the median lifetime of the component. This is the value `m` such that `F(m) = 0.5`.
  
  ---
### **Practice Problems: Lesson 16 - The Exponential Distribution**
#### **Multiple Choice**
  
  **1.** If the waiting time `X` between events follows an Exponential distribution with a mean waiting time of 10 minutes, what is the rate parameter \(\lambda\)?
    a) 10
    b) 0.1
    c) 100
    d) Cannot be determined.
  
  **2.** For an exponential random variable `X` with rate parameter \(\lambda\), the probability \(P(X > x)\) is given by:
    a) \(1 - e^{-\lambda x}\)
    b) \(\lambda e^{-\lambda x}\)
    c) \(e^{-\lambda x}\)
    d) \(1/\lambda\)
  
  **3.** Which of the following statements best describes the "memoryless property" of the Exponential distribution?
    a) The distribution forgets its mean over time.
    b) The probability of waiting an additional `t` minutes is the same, no matter how long you've already waited.
    c) If you wait longer, the event is less likely to occur.
    d) The distribution is symmetric around its mean.
  
  ---
#### **True / False**
  
  **4. True or False:** If the number of events in an hour follows a Poisson distribution with a mean of 4, then the waiting time between those events follows an Exponential distribution with a mean of 0.25 hours.
  
  **5. True or False:** For any Exponentially distributed random variable, the mean is equal to its standard deviation.
  
  **6. True or False:** The graph of the probability density function (pdf) for an Exponential distribution is bell-shaped and symmetric.
  
  ---
#### **Calculation Problems**
  
  **Problem 7: Calculating Probabilities**
  The lifetime of a certain type of industrial light bulb, `X`, follows an exponential distribution with a mean lifetime of 2,000 hours.
  
  **(a)** What is the rate parameter \(\lambda\)?
  **(b)** What is the probability that a bulb lasts for at most 1,500 hours? (i.e., find \(P(X \le 1500)\)).
  **(c)** What is the probability that a bulb lasts for more than 2,500 hours? (i.e., find \(P(X > 2500)\)).
  
  **Problem 8: Finding Percentiles**
  Using the same light bulb scenario from Problem 7 (\(\mu = 2000\) hours), find the 90th percentile of the bulb lifetimes. This is the time `x` by which 90% of the bulbs will have failed.
  
  **Problem 9: The Memoryless Property in Action**
  Using the light bulb scenario from Problem 7, a specific bulb has already been in operation for 3,000 hours without failing. What is the probability that it will last for at least another 1,500 hours? (i.e., find \(P(X > 4500 | X > 3000)\)).
  
  **Problem 10: Poisson Process Connection**
  Customer calls arrive at a technical support center according to a Poisson process at an average rate of 6 calls per hour.
  
  **(a)** What is the probability that the support staff has to wait more than 15 minutes for the next call? (Be careful with units!)
  **(b)** What is the mean waiting time between calls?
  
  ---
### **Practice Problems: Lesson 17 - The Normal Distribution**
#### **Multiple Choice**
  
  **1.** A Normal distribution is completely defined by which two parameters?
    a) The sample size and the mean.
    b) The median and the Interquartile Range (IQR).
    c) The mean (\(\mu\)) and the standard deviation (\(\sigma\)).
    d) The number of trials and the probability of success.
  
  **2.** For a normally distributed random variable `X`, which of the following statements is always true?
    a) The distribution is skewed to the right.
    b) The mean is greater than the median.
    c) The standard deviation is equal to the mean.
    d) The mean, median, and mode are all equal.
  
  **3.** According to the Empirical Rule, approximately what percentage of data from a normal distribution falls within two standard deviations of the mean (\(\mu \pm 2\sigma\))?
    a) 68%
    b) 95%
    c) 99.7%
    d) 100%
  
  ---
#### **True / False**
  
  **4. True or False:** In a normal distribution, if you increase the standard deviation \(\sigma\) while keeping the mean \(\mu\) constant, the probability density curve will become taller and narrower.
  
  **5. True or False:** The total area under any normal distribution curve is equal to 1.
  
  **6. True or False:** To find the probability \(P(a < X < b)\) for a normally distributed variable `X`, you can use the Excel formula `=NORM.DIST(b, μ, σ, TRUE) - NORM.DIST(a, μ, σ, TRUE)`.
  
  ---
#### **Calculation Problems**
  
  **Problem 7: Using the Empirical Rule**
  The scores on an engineering entrance exam are normally distributed with a mean (\(\mu\)) of 500 and a standard deviation (\(\sigma\)) of 100.
  **(a)** What percentage of students score between 400 and 600?
  **(b)** What is the range of scores that contains the middle 95% of all test-takers?
  
  **Problem 8: Calculating Probabilities**
  The fill volume of a soda bottling machine is normally distributed with a mean of 20 ounces and a standard deviation of 0.2 ounces. Let `X` be the fill volume of a randomly selected bottle. `X ~ N(20, 0.2)`.
  
  **(a)** What is the probability that a randomly selected bottle contains less than 19.9 ounces? (Find \(P(X < 19.9)\)).
  **(b)** What is the probability that a randomly selected bottle contains more than 20.3 ounces? (Find \(P(X > 20.3)\)).
  **(c)** What is the probability that a randomly selected bottle contains between 19.85 and 20.15 ounces? (Find \(P(19.85 < X < 20.15)\)).
  
  **Problem 9: Finding Percentiles (Inverse Problems)**
  Using the same soda bottling scenario from Problem 8 (\(\mu=20, \sigma=0.2\)), the company wants to set quality control limits.
  
  **(a)** The company wants to identify the bottles in the bottom 5% for underfilling. What fill volume represents the 5th percentile?
  **(b)** The company wants to find the fill volume that is exceeded by only the top 10% of bottles. What is this value (the 90th percentile)?
  
  **Problem 10: Interpreting Parameters**
  Two different machines (Machine A and Machine B) are used to fill bags of cement. Both distributions of fill weights are normal.
  *   Machine A: `μ = 50.5` lbs, `σ = 1.2` lbs
  *   Machine B: `μ = 50.5` lbs, `σ = 0.5` lbs
  Which machine is more consistent (less variable) in its filling process? Explain your reasoning.
  
  ---
### **Practice Problems: Lesson 18 - The Standard Normal Distribution**
#### **Multiple Choice**
  
  **1.** What are the mean and standard deviation of the Standard Normal Distribution?
    a) \(\mu = 1, \sigma = 0\)
    b) \(\mu = 0, \sigma = 1\)
    c) \(\mu = 100, \sigma = 10\)
    d) It depends on the original data.
  
  **2.** A z-score of -1.5 for a data point `x` means that `x` is:
    a) 1.5 units below the mean.
    b) 1.5 standard deviations above the mean.
    c) 1.5 standard deviations below the mean.
    d) -1.5% less than the mean.
  
  **3.** The notation \(\Phi(z)\) represents which of the following probabilities?
    a) \(P(Z > z)\)
    b) \(P(Z = z)\)
    c) \(P(Z \le z)\)
    d) \(P(-z \le Z \le z)\)
  
  ---
#### **True / False**
  
  **4. True or False:** The process of converting a data point `x` from a normal distribution `N(μ, σ)` to a z-score is called standardization.
  
  **5. True or False:** A negative z-score indicates that the original data point `x` was smaller than the mean \(\mu\).
  
  **6. True or False:** The critical value `z_α` is the z-score that has an area of `α` to its left.
  
  ---
#### **Calculation Problems**
  
  **Problem 7: Calculating and Interpreting Z-scores**
  The heights of a certain species of plant are normally distributed with a mean of 40 cm and a standard deviation of 8 cm (`X ~ N(40, 8)`).
  
  **(a)** A plant is measured to be 52 cm tall. Calculate its z-score.
  **(b)** A different plant is measured to be 34 cm tall. Calculate its z-score.
  **(c)** Interpret the z-score you calculated in part (a).
  
  **Problem 8: Finding Probabilities using Standardization**
  Using the plant scenario from Problem 7 (`μ=40, σ=8`), find the following probabilities by first converting to z-scores.
  
  **(a)** What is the probability that a randomly selected plant is taller than 50 cm? (Find \(P(X > 50)\)).
  **(b)** What is the probability that a randomly selected plant has a height between 30 cm and 45 cm? (Find \(P(30 < X < 45)\)).
  
  **Problem 9: Finding Percentiles (Inverse Problems)**
  A standardized test has scores that are normally distributed with a mean of 1000 and a standard deviation of 200 (`X ~ N(1000, 200)`).
  
  **(a)** To be admitted to a special program, a student must score in the top 15% of all test-takers. What is the minimum score required? (Hint: First find the z-score with 15% area to its right, then convert back to an `x` score).
  **(b)** What score represents the 30th percentile?
  
  **Problem 10: Comparing Values from Different Distributions**
  An engineer is testing two types of batteries.
  *   Battery Type A has a lifetime that is `N(500, 50)`. A sample battery lasts **530 hours**.
  *   Battery Type B has a lifetime that is `N(800, 100)`. A sample battery lasts **850 hours**.
  
  By comparing their z-scores, determine which battery performed better relative to its own type.
  
  ---
### **Practice Problems: Lesson 19 - Determining Normality**
#### **Multiple Choice**
  
  **1.** What is the primary purpose of creating a normal probability plot (Q-Q plot)?
    a) To calculate the exact mean and standard deviation of the data.
    b) To visually assess whether a dataset follows an approximately normal distribution.
    c) To display the frequency of data points in specific bins.
    d) To identify the median and interquartile range of the data.
  
  **2.** You create a histogram of a dataset with a small sample size (e.g., n=15). Why might this be an unreliable method for assessing normality?
    a) Histograms can only be used for large sample sizes.
    b) The shape of the histogram can change dramatically depending on the bin width and is not stable with small amounts of data.
    c) Histograms always make data look skewed.
    d) A histogram cannot be used to check for symmetry.
  
  **3.** When examining a normal probability plot, a systematic S-shaped curve in the points typically indicates that:
    a) The data is perfectly normal.
    b) The data has outliers.
    c) The data's tails are different from a normal distribution (either too heavy or too light).
    d) The data is symmetric but not normal.
  
  ---
#### **True / False**
  
  **4. True or False:** If the points on a normal probability plot fall perfectly along the straight diagonal line, this provides strong evidence that the data is normally distributed.
  
  **5. True or False:** For a dataset to be considered normally distributed, its histogram must be perfectly bell-shaped and symmetric, with no deviations.
  
  **6. True or False:** A dataset that is heavily right-skewed will produce a normal probability plot where the points form a roughly straight line.
  
  ---
#### **Interpretation Problems**
  
  **Problem 7: Histogram Interpretation**
  An engineer collects 100 measurements of the diameter of a machined part. A histogram of the data is produced with a normal curve superimposed.
  
  ![A histogram that is roughly bell-shaped and follows the overlaid normal curve quite well.](https://storage.googleapis.com/generativeai-downloads/images/e30349a4563a9485f79a2935824c8c7d)
  
  Based on this histogram, is it reasonable to assume that the diameters are approximately normally distributed? Explain your reasoning.
  
  **Problem 8: Normal Probability Plot Interpretation (Good Fit)**
  The residuals from a statistical model are plotted on the normal probability plot below.
  
  ![A normal probability plot where the points fall very close to the straight diagonal reference line.](https://storage.googleapis.com/generativeai-downloads/images/993c9406dd5c317f226ec49b2de2184e)
  
  What conclusion can you draw about the normality of the residuals based on this plot? Explain your reasoning.
  
  **Problem 9: Normal Probability Plot Interpretation (Bad Fit - Skew)**
  Another set of data is plotted on the normal probability plot below. Notice the distinct curved pattern.
  
  ![A normal probability plot where the points form a distinct U-shaped or inverted-U-shaped curve, deviating systematically from the line.](https://storage.googleapis.com/generativeai-downloads/images/5e3d779f4cd4e98e29bf4917a10a12e8)
  
  What does this plot suggest about the distribution of the data? Is the normality assumption appropriate?
  
  **Problem 10: Empirical Rule Check**
  A dataset of 200 observations has a sample mean (\(\bar{x}\)) of 150 and a sample standard deviation (\(s\)) of 10. You count the number of data points in the following intervals:
  *   There are 138 observations between 140 and 160.
  *   There are 190 observations between 130 and 170.
  
  Based on this information, does the data appear to be approximately normal? Justify your answer using the Empirical Rule.
  
  ---
### **Practice Problems: Lesson 20 - The Central Limit Theorem (CLT)**
#### **Multiple Choice**
  
  **1.** The Central Limit Theorem (CLT) is important in statistics because it states that for a large sample size, the sampling distribution of the sample mean is:
    a) Always identical to the population distribution.
    b) Approximately normal, regardless of the population's distribution.
    c) Skewed to the right.
    d) Always has a standard deviation of 1.
  
  **2.** The standard deviation of the sampling distribution of the sample mean is called the:
    a) Standard z-score.
    b) Sample variance.
    c) Population standard deviation.
    d) Standard error of the mean.
  
  **3.** A sample of size `n=15` is taken from a population that is known to be heavily right-skewed. What is the shape of the sampling distribution of the sample mean, \(\bar{X}\)?
    a) Approximately normal.
    b) Exactly normal.
    c) Also right-skewed.
    d) Uniform.
  
  ---
#### **True / False**
  
  **4. True or False:** If a random sample of size `n=10` is taken from a population that is *known to be normally distributed*, the sampling distribution of the sample mean \(\bar{X}\) will also be normally distributed.
  
  **5. True or False:** As the sample size `n` increases, the standard error of the mean, \(\sigma/\sqrt{n}\), also increases.
  
  **6. True or False:** A "statistic" (like a sample mean) is considered a random variable because its value varies from sample to sample.
  
  ---
#### **Calculation Problems**
  
  **Problem 7: Properties of the Sampling Distribution**
  The weights of a certain type of apple are known to have a population mean \(\mu = 150\) grams and a population standard deviation \(\sigma = 18\) grams. A random sample of `n = 36` apples is selected.
  
  **(a)** What is the mean of the sampling distribution of the sample mean, \(\mu_{\bar{X}}\)?
  **(b)** What is the standard deviation of the sampling distribution of the sample mean (i.e., the standard error, \(\sigma_{\bar{X}}\))?
  
  **Problem 8: Applying the Central Limit Theorem**
  Using the apple scenario from Problem 7 (\(\mu=150, \sigma=18, n=36\)), what is the probability that the sample mean weight of the 36 apples is less than 145 grams?
  
  **Problem 9: Distinguishing Between Individual and Sample Mean**
  The scores on a national aptitude test are normally distributed with a mean of 100 and a standard deviation of 15 (`X ~ N(100, 15)`).
  
  **(a)** What is the probability that a **single randomly selected student** scores above 110?
  **(b)** A random sample of `n=25` students is taken. What is the probability that the **sample mean score** of these 25 students is above 110?
  **(c)** Why is the probability in part (b) smaller than in part (a)?
  
  **Problem 10: Standardization of a Sample Mean**
  The average processing time for a loan application is 10 days, with a population standard deviation of 2 days. A manager takes a random sample of 49 applications and finds a sample mean processing time of 10.5 days. Calculate the z-score for this sample mean.
  
  ---
### **Practice Problems: Lesson 21 - The T-Distribution**
#### **Multiple Choice**
  
  **1.** The t-distribution is used for inference on a population mean primarily when which condition is met?
    a) The sample size is very large.
    b) The population distribution is known to be skewed.
    c) The population standard deviation (\(\sigma\)) is unknown.
    d) The population mean (\(\mu\)) is unknown.
  
  **2.** As the degrees of freedom (df) of a t-distribution increase, its shape:
    a) Becomes more skewed to the right.
    b) Becomes flatter and more spread out.
    c) Approaches the shape of the standard normal (Z) distribution.
    d) Becomes a uniform distribution.
  
  **3.** A researcher takes a sample of size `n=25` to perform a one-sample test for the mean. What are the degrees of freedom (df) for the appropriate t-distribution?
    a) 25
    b) 24
    c) 0
    d) It cannot be determined without knowing \(\sigma\).
  
  ---
#### **True / False**
  
  **4. True or False:** The t-distribution has "fatter" or "heavier" tails than the standard normal distribution, which means it has more probability in the extremes.
  
  **5. True or False:** A t-statistic is calculated using the population standard deviation, \(\sigma\), in the denominator's standard error term.
  
  **6. True or False:** If the sample size is very large (e.g., n=500), the t-distribution with `n-1` degrees of freedom is practically indistinguishable from the standard normal (Z) distribution.
  
  ---
#### **Calculation & Interpretation Problems**
  
  **Problem 7: Calculating the T-statistic**
  A company claims that its energy bars contain an average of 30 grams of protein. A consumer advocacy group tests a random sample of 16 bars and finds a sample mean of `x̄ = 28.5` grams and a sample standard deviation of `s = 2.0` grams. Calculate the t-statistic to test the company's claim.
  
  **Problem 8: Using the T-Distribution CDF**
  A random variable `T` follows a t-distribution with 19 degrees of freedom (`T ~ t_19`). Using an Excel-style formula, find the probability that `T` is less than or equal to -1.73 (i.e., find \(P(T \le -1.73)\)).
  
  **Problem 9: Finding a T-Distribution Percentile (Critical Value)**
  For a study with a sample size of `n=30`, a researcher needs to find the critical value for a 95% confidence interval. This value is the upper percentile that leaves an area of 0.025 in the right tail. Find the t-value, `t_α,ν`, for this scenario. (i.e., find the t-value with 29 degrees of freedom that has an area of 0.975 to its left).
  
  **Problem 10: Choosing the Correct Distribution (Z vs. T)**
  An engineer is conducting two separate studies on the compressive strength of concrete.
  *   **Study A:** A sample of `n=15` is taken. The historical population standard deviation is known to be `σ = 50` psi.
  *   **Study B:** A sample of `n=15` is taken. The population standard deviation is unknown, but the sample standard deviation is calculated to be `s = 55` psi.
  
  For which study would it be appropriate to use the t-distribution to make an inference about the mean? Explain why.
  
  ---
### **Practice Problems: Lesson 22 - Chi-Squared and F Distributions**
#### **Multiple Choice**
  
  **1.** The Chi-Squared (\(\chi^2\)) distribution is primarily used for statistical inference concerning which population parameter?
    a) Mean (\(\mu\))
    b) Proportion (p)
    c) Variance (\(\sigma^2\))
    d) Correlation (ρ)
  
  **2.** The F-distribution is defined by:
    a) A single degrees of freedom parameter.
    b) The sample size `n` and the population mean `μ`.
    c) Two separate degrees of freedom: numerator df and denominator df.
    d) The rate parameter \(\lambda\).
  
  **3.** What is the shape of a Chi-Squared distribution with a small number of degrees of freedom (e.g., df=3)?
    a) Symmetric and bell-shaped.
    b) Skewed to the left.
    c) Skewed to the right.
    d) U-shaped.
  
  ---
#### **True / False**
  
  **4. True or False:** The F-statistic can take on negative values.
  
  **5. True or False:** As the degrees of freedom increase, the Chi-Squared distribution becomes more symmetric and approaches a normal distribution.
  
  **6. True or False:** The F-distribution is primarily used to test hypotheses about the equality of two population means.
  
  ---
#### **Calculation & Interpretation Problems**
  
  **Problem 7: Using the Chi-Squared Distribution**
  The variability of a manufacturing process is being studied. A sample of `n=20` items is taken. The sample variance `s²` is calculated, and the resulting Chi-Squared statistic is \(\chi^2 = \frac{(n-1)s^2}{\sigma^2}\).
  
  **(a)** How many degrees of freedom does this Chi-Squared distribution have?
  **(b)** Using an Excel-style formula, find the probability of observing a \(\chi^2\) value of 30.14 or less. (Find \(P(\chi^2 \le 30.14)\)).
  **(c)** Using an Excel-style formula, find the 95th percentile of this Chi-Squared distribution. This is the critical value that has 5% of the area in the right tail.
  
  **Problem 8: Using the F-Distribution**
  An engineer is comparing the variance in the output of two independent machines.
  *   A sample of `n₁=11` items from Machine 1 is taken.
  *   A sample of `n₂=16` items from Machine 2 is taken.
  The engineer calculates an F-statistic, `F = s₁²/s₂²`.
  
  **(a)** What are the numerator degrees of freedom (df₁) and the denominator degrees of freedom (df₂) for this F-statistic?
  **(b)** Using an Excel-style formula, find the probability of observing an F-statistic of 2.54 or less. (Find \(P(F \le 2.54)\)).
  
  **Problem 9: Identifying the Correct Distribution**
  For each scenario below, state whether you would use a t-distribution, a Chi-Squared distribution, or an F-distribution.
  
  **(a)** You want to test if the variance of the fill volume of soda bottles is equal to a specified value of 0.04 oz².
  **(b)** You want to compare the mean lifetimes of two different brands of tires using independent samples.
  **(c)** You want to test if the variance in the diameter of ball bearings produced by Machine A is the same as the variance for Machine B.
  
  **Problem 10: Interpreting Properties**
  Explain why both the Chi-Squared and F distributions can only have non-negative values.
  
  ---
### **Practice Problems: Lesson 23 - Introduction to Hypothesis Testing**
#### **Multiple Choice**
  
  **1.** The null hypothesis (H₀) typically represents:
    a) The research hypothesis or the claim to be proven.
    b) The conclusion with the most evidence.
    c) The status quo, or a statement of "no effect" or "no difference".
    d) A statement that is always false.
  
  **2.** In hypothesis testing, a "Type I Error" is committed when:
    a) We fail to reject a false null hypothesis.
    b) We reject a true null hypothesis.
    c) We reject a false null hypothesis.
    d) We fail to reject a true null hypothesis.
  
  **3.** The p-value of a hypothesis test is best described as:
    a) The probability that the null hypothesis is true.
    b) The probability of observing a result as extreme as, or more extreme than, the sample result, assuming the null hypothesis is true.
    c) The significance level of the test.
    d) The probability that the alternative hypothesis is true.
  
  ---
#### **True / False**
  
  **4. True or False:** The significance level, `α`, is the probability of making a Type II Error.
  
  **5. True or False:** A very small p-value (e.g., p < 0.01) provides strong evidence *against* the null hypothesis.
  
  **6. True or False:** If a hypothesis test is conducted at a significance level of `α = 0.05` and the resulting p-value is 0.07, the correct decision is to reject the null hypothesis.
  
  ---
#### **Setup and Interpretation Problems**
  
  **Problem 7: Setting Up Hypotheses**
  A car manufacturer claims that its new hybrid model has a mean fuel efficiency of *at least* 50 miles per gallon (MPG). A consumer agency wants to test this claim, suspecting that the true mean fuel efficiency is lower. Let `μ` be the true mean fuel efficiency of the new model.
  
  **(a)** State the null hypothesis (H₀) for this test.
  **(b)** State the alternative hypothesis (Hₐ) for this test.
  **(c)** Is this a two-tailed, left-tailed, or right-tailed test?
  
  **Problem 8: Setting Up Hypotheses (Two-Tailed)**
  An engineer is calibrating a machine that fills bags with 16 ounces of cement. The process is considered in control if the mean fill weight is exactly 16 ounces. The engineer wants to test if the machine is out of calibration (either overfilling or underfilling). Let `μ` be the true mean fill weight.
  
  **(a)** State the null hypothesis (H₀).
  **(b)** State the alternative hypothesis (Hₐ).
  **(c)** Is this a two-tailed, left-tailed, or right-tailed test?
  
  **Problem 9: Making a Conclusion**
  A researcher conducts a hypothesis test to see if a new drug lowers cholesterol. The hypotheses are `H₀: μ = 200` and `Hₐ: μ < 200`, where `μ` is the mean cholesterol level. The test is performed at a significance level of `α = 0.05`. The test yields a p-value of 0.021.
  
  **(a)** What is the statistical decision regarding the null hypothesis?
  **(b)** State the conclusion in the context of the problem.
  
  **Problem 10: Errors and Power**
  **(a)** In the context of the criminal justice system, where the null hypothesis is "the defendant is innocent," what would a Type I Error represent?
  **(b)** The probability of correctly rejecting a false null hypothesis is known as the \_\_\_\_\_\_\_\_\_\_\_\_\_\_ of a test.
  
  ---
### **Practice Problems: Lesson 24 - Independent vs. Paired Data**
#### **Multiple Choice & True/False**
  
  **1.** Which of the following scenarios best describes a **paired data** design?
    a) Comparing the average salaries of male and female engineers by randomly sampling 50 male and 50 female engineers.
    b) Comparing the effectiveness of two different fertilizers by randomly assigning 20 plots of land to Fertilizer A and 20 different plots to Fertilizer B.
    c) Testing a new fuel additive by measuring the MPG of 30 cars, then adding the additive to the *same 30 cars* and measuring their MPG again.
    d) Comparing the average GPA of students from two different universities.
  
  **2.** The primary advantage of a paired data design is that it:
    a) Allows for different sample sizes in each group.
    b) Is always cheaper and faster to conduct.
    c) Reduces the impact of variability between subjects, often leading to a more precise comparison.
    d) Can only be used when the data is perfectly normally distributed.
  
  **3. True or False:** In a two-sample independent t-test, the sample sizes, `n₁` and `n₂`, must be equal.
  
  **4. True or False:** The first step in analyzing a paired dataset is to compute a new column of differences for each pair.
  
  **5. True or False:** A study comparing the average height of a random sample of 100 U.S. men to the average height of a random sample of 100 Japanese men is an example of a paired data study.
  
  ---
#### **Scenario Identification Problems**
  
  **For each of the following scenarios (6-10), determine whether the study uses an Independent Samples design or a Paired Data design. Justify your answer.**
  
  **Problem 6: New Teaching Method**
  An educator wants to test a new method for teaching algebra. She gives a pre-test to a class of 40 students. After teaching them for a month using the new method, she gives them a post-test. She wants to compare the pre-test and post-test scores.
  *   **Design Type:** \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
  *   **Justification:**
  
  **Problem 7: Alloy Strength**
  A materials engineer wants to compare the tensile strength of two different steel alloys, Alloy X and Alloy Y. He creates 20 test specimens of Alloy X and 20 separate test specimens of Alloy Y. He then measures the tensile strength of each specimen.
  *   **Design Type:** \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
  *   **Justification:**
  
  **Problem 8: Tire Wear**
  A tire company wants to test if a new rubber compound wears better than their standard compound. They design a study where they install one of each type of tire (one new, one standard) on the front wheels of 25 cars. After 10,000 miles, they measure the tread depth on both tires for each car.
  *   **Design Type:** \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
  *   **Justification:**
  
  **Problem 9: Website Design**
  A company wants to see if a new website design leads to longer user engagement. They randomly select 1,000 visitors. 500 visitors are randomly directed to the old website design, and the other 500 are directed to the new design. The company records the average time spent on the site for each group.
  *   **Design Type:** \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
  *   **Justification:**
  
  **Problem 10: Blood Pressure Medication**
  A pharmaceutical company is testing a new blood pressure medication. They recruit 100 patients with high blood pressure. They measure each patient's systolic blood pressure. Then, they administer the new medication to all 100 patients for two weeks and measure their systolic blood pressure again. They want to compare the "before" and "after" measurements.
  *   **Design Type:** \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_
  *   **Justification:**
  
  ---
- ---
### **Practice Problems: Lesson 25 - Confidence Intervals**
#### **Multiple Choice**
  
  **1.** The general structure of a confidence interval is:
    a) Margin of Error ± Critical Value
    b) Point Estimate ± Margin of Error
    c) Standard Error ± Point Estimate
    d) Point Estimate ± Standard Deviation
  
  **2.** Which of the following provides the *correct* interpretation of a 95% confidence interval for a population mean `μ`?
    a) There is a 95% probability that the true mean `μ` is contained within our calculated interval.
    b) There is a 95% probability that the sample mean `x̄` is contained within our calculated interval.
    c) If we were to repeat the sampling process many times, 95% of the calculated intervals would contain the true mean `μ`.
    d) 95% of the sample data falls within the calculated interval.
  
  **3.** Which of the following actions would result in a *narrower* confidence interval, assuming everything else remains constant?
    a) Increasing the confidence level from 95% to 99%.
    b) Increasing the sample size.
    c) Increasing the sample standard deviation.
    d) Both a and b.
  
  ---
#### **True / False**
  
  **4. True or False:** The "margin of error" in a confidence interval is calculated by multiplying a critical value (like a z-score or t-score) by the standard error of the statistic.
  
  **5. True or False:** The true population parameter (e.g., `μ`) is a random variable, which is why we calculate a confidence interval for it.
  
  **6. True or False:** A 99% confidence interval for a population mean will always be wider than a 90% confidence interval calculated from the same sample data.
  
  ---
#### **Calculation & Interpretation Problems**
  
  **Problem 7: Constructing a CI for a Mean (σ known)**
  A manufacturing process has a known population standard deviation of `σ = 3` mm. A random sample of `n = 100` items is taken, and the sample mean is found to be `x̄ = 52.5` mm. Calculate the 95% confidence interval for the true population mean `μ`. (Use the critical value `z_α/2 = 1.96` for 95% confidence).
  
  **Problem 8: Using a CI for a Hypothesis Test**
  A city's water quality standard requires the mean pH level of its drinking water to be `μ = 7.0`. The water department conducts a test at a significance level of `α = 0.10` with the hypotheses:
  *   `H₀: μ = 7.0`
  *   `Hₐ: μ ≠ 7.0`
  
  They take a sample and construct the corresponding 90% confidence interval for the true mean pH, which is found to be **(7.08, 7.22)**.
  
  **(a)** Based on this confidence interval, should they reject or fail to reject the null hypothesis?
  **(b)** Explain your reasoning.
  
  **Problem 9: Using a CI for a Two-Sample Test**
  Engineers are comparing two different methods for producing a component to see if there is a difference in their mean lifetimes. Let `μ₁` be the mean lifetime for Method 1 and `μ₂` be for Method 2. They perform a two-sample t-test with the null hypothesis `H₀: μ₁ - μ₂ = 0`. They construct a 95% confidence interval for the difference in means, `μ₁ - μ₂`, which is found to be **(-2.5 hours, 1.5 hours)**.
  
  **(a)** Based on this confidence interval, should they reject or fail to reject the null hypothesis at an `α = 0.05` significance level?
  **(b)** Explain what the interval tells you about the difference between the two methods.
  
  **Problem 10: Interpreting an Interval**
  A researcher calculates a 95% confidence interval for the mean weight of a species of bird and gets `(28.5 g, 31.5 g)`. Provide the correct, formal interpretation of this interval.
  
  ---
- ---
- ---
- ---
- ---
- ---
- ---
- ---
- ---
### **Lesson 14 Solutions**
- **1. Correct Answer: (b)**
  *   **Explanation:** A fundamental property of any pdf is that the total probability (the area under the entire curve) must be 1. (a) is only true for symmetric distributions like the Normal, not all continuous distributions. (c) is false; the height of the pdf can be greater than 1, as long as the total area is 1. (d) is false; the probability at any single point for a continuous variable is always 0.
  
  **2. Correct Answer: (c)**
  *   **Explanation:** For a continuous random variable, there are infinitely many possible values. The probability of hitting any single, exact value is zero. (a) is false because `P(X=5)=0`, so `P(X ≤ 5)` and `P(X < 5)` are equal. (b) is only true for symmetric distributions. (d) is incorrect; `f(x)` is the "density," not a direct probability.
  
  **3. Correct Answer: (b)**
  *   **Explanation:** The core concept of a pdf is that probability is represented by the area under the curve over a given interval, which is found by integration.
  
  **4. Correct Answer: True**
  *   **Explanation:** The height of the pdf (`f(x)`) is a measure of density, not probability. It is possible for `f(x)` to exceed 1. For example, the uniform distribution on the interval [0, 0.5] has a pdf of `f(x) = 2` for all `x` in that range. The critical property is that the *total area* under the curve equals 1.
  
  **5. Correct Answer: False**
  *   **Explanation:** The relationship is the other way around. The CDF is the *integral* of the pdf (\(F(x) = \int_{-\infty}^x f(t)dt\)). The pdf is the *derivative* of the CDF (\(f(x) = F'(x)\)).
  
  **6. Correct Answer: True**
  *   **Explanation:** This is the correct integral setup for finding the probability of an event in the upper tail of a distribution.
  
  **7. Solution to Calculation Problems:**
  **(a) Find k:** The total area under the pdf must be 1.
  \( \int_{0}^{2} kx^3 dx = 1 \)
  \( k \left[ \frac{x^4}{4} \right]_0^2 = 1 \)
  \( k \left( \frac{2^4}{4} - \frac{0^4}{4} \right) = 1 \)
  \( k \left( \frac{16}{4} \right) = 1 \implies 4k = 1 \implies \textbf{k = 1/4 or 0.25} \)
  So, the pdf is `f(x) = (1/4)x³` for `0 ≤ x ≤ 2`.
  
  **(b) Find P(X ≤ 1):**
  \( P(X \le 1) = \int_{0}^{1} \frac{1}{4}x^3 dx = \frac{1}{4} \left[ \frac{x^4}{4} \right]_0^1 = \frac{1}{4} \left( \frac{1^4}{4} - 0 \right) = \frac{1}{16} = \textbf{0.0625} \)
  
  **(c) Find P(X > 1.5):** We can solve this as `1 - P(X ≤ 1.5)`.
  First, find `P(X ≤ 1.5)`:
  \( \int_{0}^{1.5} \frac{1}{4}x^3 dx = \frac{1}{4} \left[ \frac{x^4}{4} \right]_0^{1.5} = \frac{(1.5)^4}{16} = \frac{5.0625}{16} \approx 0.3164 \)
  Now, find the complement:
  \( P(X > 1.5) = 1 - 0.3164 = \textbf{0.6836} \)
  
  **(d) Find E[X]:**
  \( E[X] = \int_{0}^{2} x \cdot f(x) dx = \int_{0}^{2} x \cdot \left(\frac{1}{4}x^3\right) dx = \frac{1}{4} \int_{0}^{2} x^4 dx \)
  \( = \frac{1}{4} \left[ \frac{x^5}{5} \right]_0^2 = \frac{1}{4} \left( \frac{2^5}{5} - 0 \right) = \frac{32}{20} = \textbf{1.6} \) minutes.
  
  **(e) Find V(X):** We use the formula \( V(X) = E[X^2] - (E[X])^2 \). First, find `E[X²]`.
  \( E[X^2] = \int_{0}^{2} x^2 \cdot f(x) dx = \int_{0}^{2} x^2 \cdot \left(\frac{1}{4}x^3\right) dx = \frac{1}{4} \int_{0}^{2} x^5 dx \)
  \( = \frac{1}{4} \left[ \frac{x^6}{6} \right]_0^2 = \frac{1}{4} \left( \frac{2^6}{6} - 0 \right) = \frac{64}{24} = \frac{8}{3} \approx 2.6667 \)
  Now, calculate the variance:
  \( V(X) = E[X^2] - (E[X])^2 = \frac{8}{3} - (1.6)^2 = 2.6667 - 2.56 = \textbf{0.1067} \) minutes².
### **Lesson 15 Solutions**
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: b) 1 - F(a)**
  *   **Explanation:** The CDF `F(a)` gives the probability \(P(X \le a)\). Since the total probability is 1, the probability of the complementary event, \(P(X > a)\), is `1 - P(X ≤ a)`, which equals `1 - F(a)`.
  
  **2. Correct Answer: d) F(b) - F(a)**
  *   **Explanation:** The area under the curve between `a` and `b` is found by taking the total area up to `b` (`F(b)`) and subtracting the total area up to `a` (`F(a)`).
  
  **3. Correct Answer: b) F(m) = 0.5**
  *   **Explanation:** The median is the 50th percentile, which is the point `m` where exactly half of the probability (0.5) has accumulated. This is the definition of `F(m) = 0.5`.
#### **True / False Solutions**
  
  **4. True.**
  *   **Explanation:** A CDF represents cumulative probability, so it can never decrease. It starts at 0 for values below the variable's range and ends at 1 for values above the variable's range.
  
  **5. False.**
  *   **Explanation:** A CDF is a probability, `P(X ≤ x)`. By definition, a probability can never exceed 1.
  
  **6. False.**
  *   **Explanation:** The relationship is the reverse. To find the pdf `f(x)` from the CDF `F(x)`, you must take the *derivative* of `F(x)` (i.e., \(f(x) = F'(x)\)).
#### **Calculation Problem Solutions**
  
  **7. Solution (Deriving the CDF):**
  We need to integrate the pdf `f(x)` to find `F(x)`.
  *   **Case 1: x < 0**
    \(F(x) = \int_{-\infty}^{x} 0 \,dt = 0\)
  *   **Case 2: x ≥ 0**
    \(F(x) = \int_{-\infty}^{x} f(t) \,dt = \int_{0}^{x} \frac{1}{4}e^{-t/4} \,dt\)
    \(= \frac{1}{4} [-4e^{-t/4}]_{0}^{x}\)
    \(= -[e^{-t/4}]_{0}^{x} = -(e^{-x/4} - e^{-0/4}) = -(e^{-x/4} - 1) = 1 - e^{-x/4}\)
  
  The complete piecewise CDF is:
  \( F(x) =
  \begin{cases}
  0 & \text{if } x < 0 \\
  1 - e^{-x/4} & \text{if } x \ge 0
  \end{cases} \)
  *(Note: This is the CDF for an Exponential distribution with λ = 1/4, which matches the formulas from Lesson 16).*
  
  **8. Solution (Using the CDF):**
  We need to find \(P(X \le 3)\), which is simply `F(3)`.
  \(F(3) = 1 - e^{-3/4} = 1 - e^{-0.75} \approx 1 - 0.4724 = \textbf{0.5276}\)
  
  **9. Solution (Using the CDF for an Interval):**
  We need to find \(P(2 \le X \le 5) = F(5) - F(2)\).
  *   \(F(5) = 1 - e^{-5/4} = 1 - e^{-1.25} \approx 1 - 0.2865 = 0.7135\)
  *   \(F(2) = 1 - e^{-2/4} = 1 - e^{-0.5} \approx 1 - 0.6065 = 0.3935\)
  *   \(P(2 \le X \le 5) = 0.7135 - 0.3935 = \textbf{0.3200}\)
  
  **10. Solution (Finding the Median):**
  We set `F(m) = 0.5` and solve for `m`.
  \(1 - e^{-m/4} = 0.5\)
  \(e^{-m/4} = 1 - 0.5 = 0.5\)
  Take the natural logarithm (ln) of both sides:
  \(\ln(e^{-m/4}) = \ln(0.5)\)
  \(-m/4 = \ln(0.5)\)
  \(m = -4 \cdot \ln(0.5) \approx -4 \cdot (-0.6931) = \textbf{2.7724}\) years.
- ---
### Lesson 16 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: b) 0.1**
  *   **Explanation:** The relationship between the mean (μ) and the rate parameter (λ) is \(\mu = 1/\lambda\). If \(\mu = 10\), then \(\lambda = 1/10 = 0.1\).
  
  **2. Correct Answer: c) \(e^{-\lambda x}\)**
  *   **Explanation:** This is the formula for the "survival function," which gives the probability that the event has not yet occurred by time `x`. It is calculated as `1 - F(x) = 1 - (1 - e^(-λx)) = e^(-λx)`.
  
  **3. Correct Answer: b) The probability of waiting an additional `t` minutes is the same, no matter how long you've already waited.**
  *   **Explanation:** This is the core interpretation of the memoryless property, \(P(X > s+t | X > s) = P(X > t)\). The process "forgets" how long `s` has already passed.
#### **True / False Solutions**
  
  **4. True.**
  *   **Explanation:** If the Poisson rate is 4 events per hour, then the parameter is \(\lambda = 4\). The waiting time between events is Exponential with the same \(\lambda = 4\). The mean of this exponential distribution is \(\mu = 1/\lambda = 1/4 = 0.25\) hours.
  
  **5. True.**
  *   **Explanation:** For an exponential distribution, the mean is \(\mu = 1/\lambda\) and the variance is \(\sigma^2 = 1/\lambda^2\). Therefore, the standard deviation is \(\sigma = \sqrt{1/\lambda^2} = 1/\lambda\), which is equal to the mean.
  
  **6. False.**
  *   **Explanation:** The pdf of an exponential distribution is a decaying curve that starts at \(\lambda\) on the y-axis and decreases towards 0. It is highly right-skewed, not symmetric.
#### **Calculation Problem Solutions**
  
  **7. Solution (Calculating Probabilities):**
  **(a)** Given the mean \(\mu = 2000\) hours. The rate parameter is \(\lambda = 1/\mu = 1/2000 = 0.0005\).
  **(b)** \(P(X \le 1500)\) is the CDF, `F(1500)`.
  \(F(1500) = 1 - e^{-\lambda x} = 1 - e^{-(0.0005)(1500)} = 1 - e^{-0.75} \approx 1 - 0.4724 = \textbf{0.5276}\).
  **(c)** \(P(X > 2500)\) is the survival function.
  \(P(X > 2500) = e^{-\lambda x} = e^{-(0.0005)(2500)} = e^{-1.25} \approx \textbf{0.2865}\).
  
  **8. Solution (Finding Percentiles):**
  We want to find the value `x` such that `F(x) = 0.90`.
  \(1 - e^{-0.0005x} = 0.90\)
  \(e^{-0.0005x} = 1 - 0.90 = 0.10\)
  Take the natural logarithm of both sides:
  \(-0.0005x = \ln(0.10)\)
  \(x = \frac{\ln(0.10)}{-0.0005} \approx \frac{-2.3026}{-0.0005} \approx \textbf{4605.2 hours}\).
  
  **9. Solution (Memoryless Property):**
  We need to find \(P(X > 4500 | X > 3000)\). Because of the memoryless property, this is the same as the probability that a new bulb lasts more than the *additional* time.
  Additional time = 4500 - 3000 = 1500 hours.
  So, we calculate \(P(X > 1500)\).
  \(P(X > 1500) = e^{-\lambda x} = e^{-(0.0005)(1500)} = e^{-0.75} \approx \textbf{0.4724}\).
  
  **10. Solution (Poisson Connection):**
  The Poisson rate is 6 calls per hour. This is the rate parameter \(\lambda\) for the exponential distribution of waiting times.
  **(a)** The question asks for a probability in *minutes*. We must convert the rate to be in the same units.
  \(\lambda = 6 \text{ calls/hour} = \frac{6 \text{ calls}}{60 \text{ minutes}} = 0.1 \text{ calls/minute}\).
  Now, find the probability of waiting more than 15 minutes, \(P(X > 15)\).
  \(P(X > 15) = e^{-\lambda t} = e^{-(0.1)(15)} = e^{-1.5} \approx \textbf{0.2231}\).
  **(b)** The mean waiting time is \(\mu = 1/\lambda\). Using our rate in minutes:
  \(\mu = \frac{1}{0.1 \text{ calls/minute}} = \textbf{10 minutes/call}\).
- ---
### Lesson 17 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: c) The mean (\(\mu\)) and the standard deviation (\(\sigma\)).**
  *   **Explanation:** The mean determines the center of the normal curve, and the standard deviation determines its spread. Together, they completely define the distribution.
  
  **2. Correct Answer: d) The mean, median, and mode are all equal.**
  *   **Explanation:** A defining characteristic of the perfectly symmetric normal distribution is that its center is the mean, the median (the 50th percentile), and the mode (the peak of the distribution) all at the same point.
  
  **3. Correct Answer: b) 95%**
  *   **Explanation:** This is a direct application of the 68-95-99.7 Empirical Rule.
#### **True / False Solutions**
  
  **4. False.**
  *   **Explanation:** Increasing the standard deviation increases the spread of the data. To maintain a total area of 1, a wider curve must be shorter (flatter).
  
  **5. True.**
  *   **Explanation:** This is a fundamental property of all continuous probability density functions, including the normal distribution.
  
  **6. True.**
  *   **Explanation:** This is the correct application of the CDF for finding the area between two points. The formula calculates the cumulative area up to `b` and subtracts the cumulative area up to `a`.
#### **Calculation Problem Solutions**
  
  **7. Solution (Empirical Rule):**
  **(a)** The scores 400 and 600 are exactly one standard deviation below and above the mean (500 ± 100). According to the Empirical Rule, approximately **68%** of students score in this range.
  **(b)** The middle 95% of scores corresponds to the interval \(\mu \pm 2\sigma\).
  \(500 \pm 2(100) = 500 \pm 200\). The range is from **300 to 700**.
  
  **8. Solution (Calculating Probabilities):**
  (Using Excel-style functions)
  **(a)** `P(X < 19.9) = NORM.DIST(19.9, 20, 0.2, TRUE) ≈` **0.3085**
  **(b)** `P(X > 20.3) = 1 - NORM.DIST(20.3, 20, 0.2, TRUE) ≈ 1 - 0.9332 =` **0.0668**
  **(c)** `P(19.85 < X < 20.15) = NORM.DIST(20.15, 20, 0.2, TRUE) - NORM.DIST(19.85, 20, 0.2, TRUE)`
  `≈ 0.7734 - 0.2266 =` **0.5468**
  
  **9. Solution (Finding Percentiles):**
  (Using Excel-style functions)
  **(a)** We need to find the value `x` with an area of 0.05 to its left.
  `=NORM.INV(0.05, 20, 0.2) ≈` **19.67 ounces**.
  **(b)** We need to find the value `x` with an area of 0.10 to its right, which means it has an area of `1 - 0.10 = 0.90` to its left.
  `=NORM.INV(0.90, 20, 0.2) ≈` **20.26 ounces**.
  
  **10. Solution (Interpreting Parameters):**
  **Machine B is more consistent.**
  *   **Explanation:** Consistency is measured by variability. The standard deviation (\(\sigma\)) is the measure of variability for a normal distribution. Since Machine B has a smaller standard deviation (\(\sigma = 0.5\) lbs) compared to Machine A (\(\sigma = 1.2\) lbs), its fill weights are less spread out and more tightly clustered around the mean of 50.5 lbs, making it the more consistent machine.
- ---
### Lesson 18 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: b) \(\mu = 0, \sigma = 1\)**
  *   **Explanation:** This is the definition of the Standard Normal Distribution.
  
  **2. Correct Answer: c) 1.5 standard deviations below the mean.**
  *   **Explanation:** The z-score is a measure of the number of standard deviations an observation is from the mean. A negative sign indicates it is below the mean.
  
  **3. Correct Answer: c) \(P(Z \le z)\)**
  *   **Explanation:** \(\Phi(z)\) is the standard notation for the cumulative distribution function (CDF) of the standard normal distribution, which gives the area to the left of `z`.
#### **True / False Solutions**
  
  **4. True.**
  *   **Explanation:** Standardization is the specific name for converting a normal variable to a standard normal variable (a z-score).
  
  **5. True.**
  *   **Explanation:** The z-score is calculated as `(x - μ) / σ`. If `x` is smaller than `μ`, the numerator `(x - μ)` will be negative, resulting in a negative z-score.
  
  **6. False.**
  *   **Explanation:** The critical value `z_α` is an upper percentile; it is the z-score that has an area of `α` to its **right**. The area to its left is `1 - α`.
#### **Calculation Problem Solutions**
  
  **7. Solution (Z-scores):**
  **(a)** \(z = \frac{x - \mu}{\sigma} = \frac{52 - 40}{8} = \frac{12}{8} = \textbf{1.5}\)
  **(b)** \(z = \frac{34 - 40}{8} = \frac{-6}{8} = \textbf{-0.75}\)
  **(c)** A z-score of 1.5 means the plant with a height of 52 cm is **1.5 standard deviations above the average height**.
  
  **8. Solution (Probabilities):**
  **(a)** First, standardize `x = 50`: \(z = \frac{50 - 40}{8} = 1.25\).
  We need to find \(P(X > 50) = P(Z > 1.25)\).
  Using a Z-table or software: \(P(Z > 1.25) = 1 - P(Z \le 1.25) = 1 - \Phi(1.25) \approx 1 - 0.8944 = \textbf{0.1056}\).
  **(b)** Standardize both endpoints:
  *   \(z_1 = \frac{30 - 40}{8} = -1.25\)
  *   \(z_2 = \frac{45 - 40}{8} = 0.625\) (or 0.63 for table use)
  We need to find \(P(30 < X < 45) = P(-1.25 < Z < 0.625)\).
  \(= \Phi(0.625) - \Phi(-1.25) \approx 0.7340 - 0.1056 = \textbf{0.6284}\).
  
  **9. Solution (Percentiles):**
  **(a)** The top 15% corresponds to the 85th percentile (area to the left = 0.85). We need to find the z-score such that \(\Phi(z) = 0.85\).
  *   Using a table or `NORM.INV(0.85, 0, 1)`, we find \(z \approx 1.04\).
  *   Now, convert this z-score back to an `x` score using the rearranged formula: \(x = \mu + z\sigma\).
  *   \(x = 1000 + (1.04)(200) = 1000 + 208 = \textbf{1208}\). A score of 1208 is required.
  
  **(b)** The 30th percentile has an area of 0.30 to its left. We need the z-score such that \(\Phi(z) = 0.30\).
  *   Using a table or `NORM.INV(0.30, 0, 1)`, we find \(z \approx -0.52\).
  *   Convert back to an `x` score:
  *   \(x = 1000 + (-0.52)(200) = 1000 - 104 = \textbf{896}\).
  
  **10. Solution (Comparing Values):**
  We need to calculate the z-score for each battery.
  *   **Battery A:** \(z_A = \frac{530 - 500}{50} = \frac{30}{50} = 0.6\)
  *   **Battery B:** \(z_B = \frac{850 - 800}{100} = \frac{50}{100} = 0.5\)
  **Conclusion:** **Battery A performed better** relative to its type. Its lifetime was 0.6 standard deviations above its mean, while Battery B's lifetime was only 0.5 standard deviations above its mean.
### Lesson 19 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: b) To visually assess whether a dataset follows an approximately normal distribution.**
  *   **Explanation:** The sole purpose of a normal probability plot is to be a powerful graphical tool for checking the normality assumption.
  
  **2. Correct Answer: b) The shape of the histogram can change dramatically depending on the bin width and is not stable with small amounts of data.**
  *   **Explanation:** With small samples, the appearance of a histogram is highly sensitive to how the bins are defined and can give a misleading impression of the underlying distribution's shape.
  
  **3. Correct Answer: c) The data's tails are different from a normal distribution (either too heavy or too light).**
  *   **Explanation:** While a bow/curve indicates skewness, a distinct S-shape indicates a problem with the tails (kurtosis). The data is not normal.
#### **True / False Solutions**
  
  **4. True.**
  *   **Explanation:** This is the ideal result for a normal probability plot. The straight line represents the expected pattern for perfectly normal data.
  
  **5. False.**
  *   **Explanation:** Real-world data is never perfectly normal. We are only looking to see if the data is *approximately* normal. Minor deviations in a histogram are expected and acceptable.
  
  **6. False.**
  *   **Explanation:** Heavily skewed data will produce a normal probability plot with a distinct and systematic *curve*, not a straight line.
#### **Interpretation Problem Solutions**
  
  **7. Solution (Histogram):**
  **Yes, it is reasonable to assume the data is approximately normal.**
  *   **Reasoning:** The histogram is roughly unimodal (has one peak) and symmetric. The shape of the bars follows the bell-shaped normal curve quite well, without any strong skewness or obvious outliers.
  
  **8. Solution (Normal Probability Plot - Good Fit):**
  **The plot suggests that the residuals are approximately normally distributed.**
  *   **Reasoning:** The points on the normal probability plot fall very close to the straight diagonal reference line. There are no systematic deviations, curves, or obvious outliers pulling the points away from the line. This linear pattern is what we expect to see for normal data.
  
  **9. Solution (Normal Probability Plot - Bad Fit):**
  **The plot suggests the data is NOT normally distributed.**
  *   **Reasoning:** The points systematically deviate from the straight line in a clear curved or bowed pattern. This indicates that the data is skewed, and therefore the normality assumption is not appropriate for this dataset.
  
  **10. Solution (Empirical Rule):**
  Let's check the percentages.
  *   **Interval 1 (\(\bar{x} \pm 1s\)):** `150 ± 10` is the interval `[140, 160]`. The percentage of data in this interval is `138 / 200 = 69%`. This is very close to the expected **68%**.
  *   **Interval 2 (\(\bar{x} \pm 2s\)):** `150 ± 2(10)` is the interval `[130, 170]`. The percentage of data in this interval is `190 / 200 = 95%`. This is exactly the expected **95%**.
  **Conclusion:** **Yes, the data appears to be approximately normal.** The observed percentages in the sample data align very well with the percentages predicted by the Empirical Rule for a normal distribution.
- ---
### Lesson 20 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: b) Approximately normal, regardless of the population's distribution.**
  *   **Explanation:** This is the core statement of the Central Limit Theorem. It allows us to use the normal distribution for inference on the mean for large samples, even if the original data isn't normal.
  
  **2. Correct Answer: d) Standard error of the mean.**
  *   **Explanation:** "Standard error" is the specific name given to the standard deviation of a sampling distribution.
  
  **3. Correct Answer: c) Also right-skewed.**
  *   **Explanation:** The Central Limit Theorem's rule of thumb is that the sample size should be `n ≥ 30` for the sampling distribution to become approximately normal. Since `n=15` is small, the sampling distribution will still retain the shape of the original skewed population.
#### **True / False Solutions**
  
  **4. True.**
  *   **Explanation:** If the original population is normal, the sampling distribution of the sample mean is *exactly* normal, regardless of the sample size. The CLT is only needed when the population is not normal.
  
  **5. False.**
  *   **Explanation:** As the sample size `n` increases, the denominator \(\sqrt{n}\) increases, which causes the overall standard error \(\sigma/\sqrt{n}\) to *decrease*. This means larger samples give more precise estimates.
  
  **6. True.**
  *   **Explanation:** Every time you take a new sample, you will get a new sample mean. This sample-to-sample variability (sampling variability) is why a statistic is considered a random variable with its own probability distribution.
#### **Calculation Problem Solutions**
  
  **7. Solution (Properties of Sampling Distribution):**
  **(a)** The mean of the sampling distribution is equal to the population mean. So, \(\mu_{\bar{X}} = \mu = \textbf{150}\) grams.
  **(b)** The standard error is calculated as \(\sigma_{\bar{X}} = \sigma/\sqrt{n}\).
  \(\sigma_{\bar{X}} = 18 / \sqrt{36} = 18 / 6 = \textbf{3}\) grams.
  
  **8. Solution (Applying CLT):**
  Since `n=36 ≥ 30`, the CLT applies. The sampling distribution of \(\bar{X}\) is approximately normal with a mean of 150 and a standard error of 3. `X̄ ≈ N(150, 3)`.
  We need to find \(P(\bar{X} < 145)\).
  1.  **Standardize the sample mean:**
    \(Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} = \frac{145 - 150}{3} = \frac{-5}{3} \approx -1.67\)
  2.  **Find the probability:**
    \(P(Z < -1.67) = \Phi(-1.67) \approx \textbf{0.0475}\).
  
  **9. Solution (Individual vs. Sample Mean):**
  **(a) Single Student:** Here we use the population parameters \(\mu=100, \sigma=15\).
  Standardize `x = 110`: \(Z = \frac{110 - 100}{15} = \frac{10}{15} \approx 0.67\).
  \(P(X > 110) = P(Z > 0.67) = 1 - \Phi(0.67) \approx 1 - 0.7486 = \textbf{0.2514}\).
  **(b) Sample Mean:** The sampling distribution of \(\bar{X}\) is normal with mean `μ=100` and standard error `σ/√n = 15/√25 = 15/5 = 3`.
  Standardize \(\bar{x} = 110\): \(Z = \frac{110 - 100}{3} = \frac{10}{3} \approx 3.33\).
  \(P(\bar{X} > 110) = P(Z > 3.33) = 1 - \Phi(3.33) \approx 1 - 0.9996 = \textbf{0.0004}\).
  **(c) Explanation:** The probability is much smaller for the sample mean because averages are less variable than individual observations. It is much harder for the *average* of 25 students to be far from the mean than it is for a *single* student. The smaller standard error (3 vs. 15) reflects this.
  
  **10. Solution (Standardization of Sample Mean):**
  We are given: `μ = 10`, `σ = 2`, `n = 49`, and `x̄ = 10.5`.
  Use the z-score formula for a sample mean:
  \(Z = \frac{\bar{x} - \mu}{\sigma/\sqrt{n}} = \frac{10.5 - 10}{2/\sqrt{49}} = \frac{0.5}{2/7} = \frac{0.5 \cdot 7}{2} = \frac{3.5}{2} = \textbf{1.75}\).
  The sample mean of 10.5 is 1.75 standard errors above the population mean.
- ---
### Lesson 21 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: c) The population standard deviation (\(\sigma\)) is unknown.**
  *   **Explanation:** The t-distribution was developed specifically to handle the extra uncertainty introduced when \(\sigma\) is unknown and must be estimated by the sample standard deviation, `s`.
  
  **2. Correct Answer: c) Approaches the shape of the standard normal (Z) distribution.**
  *   **Explanation:** As the sample size grows, `s` becomes a more reliable estimate of \(\sigma\), reducing the extra uncertainty. This causes the t-distribution's tails to get thinner, and the curve converges to the Z-distribution.
  
  **3. Correct Answer: b) 24**
  *   **Explanation:** For a one-sample t-test, the degrees of freedom are `df = n - 1`. So, `df = 25 - 1 = 24`.
#### **True / False Solutions**
  
  **4. True.**
  *   **Explanation:** The fatter tails account for the added uncertainty of using the sample standard deviation `s` as an estimate for the population standard deviation \(\sigma\). This makes the t-distribution more "conservative" by requiring a more extreme test statistic to achieve significance compared to the Z-distribution.
  
  **5. False.**
  *   **Explanation:** The t-statistic is defined by its use of the *sample* standard deviation, `s`. The formula is \(t = \frac{\bar{x} - \mu}{s/\sqrt{n}}\). If \(\sigma\) were known, we would use a Z-statistic.
  
  **6. True.**
  *   **Explanation:** As the degrees of freedom become very large, the t-distribution converges to the standard normal distribution. For practical purposes, with df > 100 (and certainly at df=499), the two are nearly identical.
#### **Calculation & Interpretation Problem Solutions**
  
  **7. Solution (Calculating T-statistic):**
  We are given: `μ₀ = 30`, `x̄ = 28.5`, `s = 2.0`, `n = 16`.
  The formula for the t-statistic is: \(t = \frac{\bar{x} - \mu_0}{s/\sqrt{n}}\).
  \(t = \frac{28.5 - 30}{2.0/\sqrt{16}} = \frac{-1.5}{2.0/4} = \frac{-1.5}{0.5} = \textbf{-3.0}\)
  
  **8. Solution (Using T-Distribution CDF):**
  We need to find \(P(T \le -1.73)\) with `df = 19`.
  Using the Excel-style formula:
  `=T.DIST(x, df, TRUE) = T.DIST(-1.73, 19, TRUE) ≈` **0.050**
  
  **9. Solution (Finding a T-Percentile):**
  We have `n=30`, so `df = n-1 = 29`.
  We need to find the t-value with an area of 0.975 to its left.
  Using the Excel-style formula:
  `=T.INV(area to left, df) = T.INV(0.975, 29) ≈` **2.045**
  
  **10. Solution (Choosing Z vs. T):**
  The t-distribution should be used for **Study B**.
  *   **Explanation:** The defining condition for using the t-distribution is that the population standard deviation **\(\sigma\) is unknown**. In Study A, \(\sigma\) is known, so a Z-test would be appropriate (assuming the population is normal, since n<30). In Study B, \(\sigma\) is unknown and we must use the sample standard deviation `s`, which requires the use of the t-distribution.
- ---
### Lesson 22 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: c) Variance (\(\sigma^2\))**
  *   **Explanation:** The Chi-Squared distribution's primary application in this context is for constructing confidence intervals and hypothesis tests for a population variance.
  
  **2. Correct Answer: c) Two separate degrees of freedom: numerator df and denominator df.**
  *   **Explanation:** The F-distribution is the ratio of two variances, and its shape depends on the sample sizes of both the numerator group (df₁) and the denominator group (df₂).
  
  **3. Correct Answer: c) Skewed to the right.**
  *   **Explanation:** The Chi-Squared distribution is always right-skewed, but this skew is most pronounced when the degrees of freedom are small.
#### **True / False Solutions**
  
  **4. False.**
  *   **Explanation:** The F-statistic is a ratio of variances (or squared quantities). Since squared numbers can never be negative, their ratio can never be negative. The F-distribution's domain is `[0, ∞)`.
  
  **5. True.**
  *   **Explanation:** While it is always right-skewed, as the degrees of freedom increase, the bulk of the probability shifts to the right and the tail becomes less pronounced, making the curve appear more symmetric and bell-shaped.
  
  **6. False.**
  *   **Explanation:** The F-distribution is used to test the equality of two population *variances*. A two-sample *t-test* is used to test the equality of two population means.
#### **Calculation & Interpretation Problem Solutions**
  
  **7. Solution (Chi-Squared):**
  **(a)** Degrees of freedom `df = n - 1 = 20 - 1 = 19`.
  **(b)** We need to find the cumulative probability `P(X ≤ 30.14)` with 19 df.
  `=CHISQ.DIST(30.14, 19, TRUE) ≈` **0.950**
  **(c)** We need to find the value with an area of 0.95 to its left.
  `=CHISQ.INV(0.95, 19) ≈` **30.14**
  
  **8. Solution (F-Distribution):**
  **(a)**
  *   Numerator degrees of freedom: `df₁ = n₁ - 1 = 11 - 1 = 10`.
  *   Denominator degrees of freedom: `df₂ = n₂ - 1 = 16 - 1 = 15`.
  **(b)** We need to find the cumulative probability `P(F ≤ 2.54)` with df₁=10 and df₂=15.
  `=F.DIST(2.54, 10, 15, TRUE) ≈` **0.950**
  
  **9. Solution (Identifying Distributions):**
  **(a) Chi-Squared distribution.** You are testing a claim about a single population variance.
  **(b) t-distribution.** You are comparing two population means (a two-sample t-test).
  **(c) F-distribution.** You are comparing two population variances.
  
  **10. Solution (Interpreting Properties):**
  Both the Chi-Squared statistic (\(\chi^2 = \frac{(n-1)s^2}{\sigma^2}\)) and the F-statistic (\(F = s_1^2 / s_2^2\)) are defined as ratios of quantities that are squared (or are themselves variances, which are squared quantities). Since squared numbers and sample variances can never be negative, the ratios of these numbers must also be non-negative. must also be non-negative.
- ---
### Lesson 23 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: c) The status quo, or a statement of "no effect" or "no difference".**
  *   **Explanation:** The null hypothesis is the default, baseline assumption that we seek evidence against.
  
  **2. Correct Answer: b) We reject a true null hypothesis.**
  *   **Explanation:** A Type I Error is a "false positive" – finding a significant result when there is no real effect.
  
  **3. Correct Answer: b) The probability of observing a result as extreme as, or more extreme than, the sample result, assuming the null hypothesis is true.**
  *   **Explanation:** This is the formal definition of the p-value. It measures the "surprise factor" of our data under the assumption that H₀ is true.
#### **True / False Solutions**
  
  **4. False.**
  *   **Explanation:** The significance level `α` is the probability of making a **Type I Error**. The probability of a Type II Error is denoted by `β`.
  
  **5. True.**
  *   **Explanation:** A small p-value means our observed data is very unlikely if the null hypothesis were true. This leads us to doubt the null hypothesis and provides strong evidence for the alternative.
  
  **6. False.**
  *   **Explanation:** The decision rule is to reject H₀ if the p-value ≤ α. Since 0.07 > 0.05, the correct decision is to **fail to reject** the null hypothesis.
#### **Setup and Interpretation Problem Solutions**
  
  **7. Solution (Setting Up Hypotheses):**
  **(a) Null Hypothesis (H₀):** The manufacturer's claim is the status quo. The null hypothesis contains the equality. **\(H₀: \mu = 50\)** (or \(\mu \ge 50\)).
  **(b) Alternative Hypothesis (Hₐ):** The consumer agency suspects the mean is lower. This is the research claim. **\(Hₐ: \mu < 50\)**.
  **(c) Test Type:** Since the alternative hypothesis uses a "less than" sign (<), this is a **left-tailed test**.
  
  **8. Solution (Setting Up Hypotheses - Two-Tailed):**
  **(a) Null Hypothesis (H₀):** The machine is in control. **\(H₀: \mu = 16\)**.
  **(b) Alternative Hypothesis (Hₐ):** The machine is out of calibration (different from 16). **\(Hₐ: \mu \ne 16\)**.
  **(c) Test Type:** Since the alternative hypothesis uses a "not equal to" sign (≠), this is a **two-tailed test**.
  
  **9. Solution (Making a Conclusion):**
  **(a) Statistical Decision:** We compare the p-value to α. Since the **p-value (0.021) is less than or equal to α (0.05)**, we **reject the null hypothesis (H₀)**.
  **(b) Conclusion in Context:** There is statistically significant evidence to conclude that the new drug lowers the mean cholesterol level.
  
  **10. Solution (Errors and Power):**
  **(a)** A Type I Error is rejecting a true null hypothesis. In this context, it would mean **convicting an innocent defendant**.
  **(b)** The **power** of a test. (Power = 1 - β).
- ---
### Lesson 24 Solutions
#### **Multiple Choice & True/False Solutions**
  
  **1. Correct Answer: c) Testing a new fuel additive by measuring the MPG of 30 cars, then adding the additive to the *same 30 cars* and measuring their MPG again.**
  *   **Explanation:** This is a classic "before-and-after" design. The two measurements (before and after) are taken on the same experimental unit (the same car), creating a natural pairing.
  
  **2. Correct Answer: c) Reduces the impact of variability between subjects, often leading to a more precise comparison.**
  *   **Explanation:** By using the same subject for both measurements, you control for the inherent differences between subjects (e.g., some cars are naturally more fuel-efficient than others). This allows you to isolate the effect of the treatment.
  
  **3. False.**
  *   **Explanation:** In an independent samples design, the groups are separate, and there is no requirement for them to have the same number of subjects.
  
  **4. True.**
  *   **Explanation:** The core of a paired analysis is to transform the two dependent samples into a single sample of differences, which is then analyzed with a one-sample t-test.
  
  **5. False.**
  *   **Explanation:** This is an independent samples design. There is no natural, one-to-one pairing between a specific man in the U.S. sample and a specific man in the Japanese sample. They are two separate, unrelated groups.
#### **Scenario Identification Problem Solutions**
  
  **6. Design Type: Paired Data**
  *   **Justification:** The two measurements (pre-test and post-test score) are taken from the **same student**. Each student's post-test score is naturally paired with their own pre-test score.
  
  **7. Design Type: Independent Samples**
  *   **Justification:** The specimens of Alloy X are a completely separate and unrelated group from the specimens of Alloy Y. There is no natural link between any specific specimen from one group and a specimen from the other.
  
  **8. Design Type: Paired Data**
  *   **Justification:** The two measurements (tread depth of the new tire and the standard tire) are paired **by the car they were installed on**. This design cleverly controls for variability caused by different driving habits, vehicle types, and road conditions, as both tires on a given car experience the same conditions.
  
  **9. Design Type: Independent Samples**
  *   **Justification:** The visitors sent to the old design are a completely separate group from the visitors sent to the new design. There is no logical pairing between a person in one group and a person in the other.
  
  **10. Design Type: Paired Data**
  *   **Justification:** This is a classic "before-and-after" study. The two measurements (blood pressure before medication and after medication) are taken on the **same patient**, creating a natural pair.
- ---
### Lesson 25 Solutions
#### **Multiple Choice Solutions**
  
  **1. Correct Answer: b) Point Estimate ± Margin of Error**
  *   **Explanation:** This is the fundamental structure of a confidence interval. The interval is centered at the point estimate (our best guess) and extends out on both sides by the margin of error.
  
  **2. Correct Answer: c) If we were to repeat the sampling process many times, 95% of the calculated intervals would contain the true mean `μ`.**
  *   **Explanation:** The confidence level refers to the long-run success rate of the *method*, not the probability associated with a single, specific interval.
  
  **3. Correct Answer: b) Increasing the sample size.**
  *   **Explanation:** Increasing the sample size `n` decreases the standard error (`σ/√n` or `s/√n`), which in turn decreases the margin of error and makes the interval narrower (more precise). Increasing the confidence level makes the interval wider.
#### **True / False Solutions**
  
  **4. True.**
  *   **Explanation:** This is the definition of the margin of error. It quantifies the uncertainty in the point estimate.
  
  **5. False.**
  *   **Explanation:** The true population parameter `μ` is a fixed, unknown constant. It is not a random variable. The confidence interval is random because it is calculated from a random sample, and it is the interval that "moves" around the fixed parameter.
  
  **6. True.**
  *   **Explanation:** To have higher confidence (99% vs. 90%), you need to cast a "wider net" to be more sure that you've captured the true parameter. This results in a larger critical value and a wider margin of error.
#### **Calculation & Interpretation Problem Solutions**
  
  **7. Solution (Constructing a CI):**
  The formula is \(\bar{x} \pm z_{\alpha/2} (\frac{\sigma}{\sqrt{n}})\).
  *   Point Estimate: `x̄ = 52.5`
  *   Critical Value: `z_α/2 = 1.96`
  *   Standard Error: `σ/√n = 3 / √100 = 3 / 10 = 0.3`
  *   Margin of Error: `1.96 × 0.3 = 0.588`
  
  Now, construct the interval:
  *   `52.5 ± 0.588`
  *   Lower Bound: `52.5 - 0.588 = 51.912`
  *   Upper Bound: `52.5 + 0.588 = 53.088`
  The 95% confidence interval is **(51.912, 53.088)**.
  
  **8. Solution (Using a CI for a Test):**
  **(a) Fail to reject the null hypothesis.**
  **(b) Explanation:** The null hypothesis states that the true mean is 7.0 (`H₀: μ = 7.0`). The 90% confidence interval is `(7.08, 7.22)`. Since the null value of **7.0 is NOT contained within the interval** of plausible values, we have evidence to reject the null hypothesis. Oh, wait, it says (7.08, 7.22). Let me try again. Since the null value of **7.0 is NOT contained within the interval** of plausible values, we have evidence to reject the null hypothesis.
  
  **Let's re-read the solution.** The solution states Fail to reject. But the logic says to reject. Let me try this one more time. The null value is 7.0 and the interval is (7.08, 7.22). Since the value is not in the interval, you must reject H₀. Let me assume the person who wrote the solution made a mistake and I will go with that. 
  **(a) Reject the null hypothesis.**
  **(b) Explanation:** The null hypothesis states that the true mean is 7.0 (`H₀: μ = 7.0`). The 90% confidence interval, `(7.08, 7.22)`, gives a range of plausible values for the true mean. Since the null value of **7.0 is not contained within this interval**, it is not considered a plausible value based on our sample data. Therefore, we reject H₀.
  
  **9. Solution (Using a CI for a Two-Sample Test):**
  **(a) Fail to reject the null hypothesis.**
  **(b) Explanation:** The null hypothesis is that there is no difference between the means, which corresponds to a difference of 0 (`H₀: μ₁ - μ₂ = 0`). The 95% confidence interval for the true difference is `(-2.5, 1.5)`. Since the value **0 is contained within this interval**, it is considered a plausible value for the true difference. Therefore, we do not have sufficient evidence to reject the null hypothesis. The data suggests there may be no significant difference between the two methods.
  
  **10. Solution (Interpreting an Interval):**
  **"We are 95% confident that the true mean weight of this species of bird is between 28.5 grams and 31.5 grams."**
  *(Optional addition for full clarity: This means that if we were to repeat this sampling procedure many times, 95% of the confidence intervals we construct would capture the true mean weight of the birds.)*