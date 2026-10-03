### **Stat 3113: Lesson 20 - The Central Limit Theorem (CLT)**
  
  This lesson introduces the concept of the **sampling distribution** and the powerful **Central Limit Theorem (CLT)**. The CLT is the theoretical foundation that makes most of statistical inference possible.
  
  ---
### **1. The Idea of a Sampling Distribution**
  
  First, let's distinguish between different types of distributions:
  
  *   **Population Distribution:** The probability distribution of the individual values for *every member* of the population. Its parameters (like \(\mu\) and \(\sigma\)) are constants and are usually unknown.
  *   **Sample Data Distribution:** This is just the distribution of the data in one specific sample that you've collected.
  *   **Sampling Distribution:** This is a *theoretical* probability distribution for a **statistic** (like the sample mean, \(\bar{X}\)). It answers the question: "If we were to take every possible sample of a given size `n` from a population, and calculate the mean for each sample, what would the distribution of all those sample means look like?"
  
  Because statistics (like \(\bar{X}\)) vary from sample to sample (**sampling variability**), they are themselves random variables and have their own probability distribution.
  
  ---
### **2. The Sampling Distribution of the Sample Mean**
#### **Result 1: When the Population Itself is Normal**
  
  If you take a random sample of size `n` from a population that is *already* known to be Normally distributed with mean \(\mu\) and standard deviation \(\sigma\), then the sampling distribution of the sample mean \(\bar{X}\) will also be **exactly** Normal.
  
  *   **Mean of the Sampling Distribution:** The mean of all possible sample means is equal to the population mean.
    > \( \mu_{\bar{X}} = \mu \)
  *   **Standard Deviation of the Sampling Distribution (Standard Error):** The standard deviation of all the sample means is the population standard deviation divided by the square root of the sample size.
    > \( \sigma_{\bar{X}} = \frac{\sigma}{\sqrt{n}} \)
    This quantity, \(\sigma_{\bar{X}}\), is so important it has its own name: the **Standard Error of the Mean (SEM)**.
  
  In summary, if `X ~ N(μ, σ)`, then `X̄ ~ N(μ, σ/√n)`.
  
  ---
### **3. The Central Limit Theorem (CLT)**
  
  Result 1 is great, but what if the population is *not* normal (e.g., it's skewed or uniform)? This is where the magic of the CLT comes in.
  
  > **The Central Limit Theorem states:**
  > For a random sample of size `n` drawn from **ANY** population (regardless of its shape) with a mean \(\mu\) and standard deviation \(\sigma\), as long as the sample size `n` is **sufficiently large**, the sampling distribution of the sample mean \(\bar{X}\) will be **approximately Normal**.
  
  *   **"Sufficiently Large":** A common rule of thumb is that **`n ≥ 30`** is large enough for the CLT to apply.
  *   **Resulting Distribution:**
    > If `n ≥ 30`, then `X̄ ≈ N(μ, σ/√n)`
  
  The amazing part is that even if the original population is heavily skewed, the distribution of the *sample means* will form a symmetric, bell-shaped curve.
  
  ---
### **4. Using the CLT for Calculations**
  
  The CLT allows us to calculate probabilities about sample means using the Normal distribution, even if we don't know the original population's shape.
  
  **The Standardization Formula for** \(\bar{X}\):
  To find probabilities, we convert the sample mean \(\bar{X}\) to a z-score using its specific mean (\(\mu\)) and standard deviation (the standard error, \(\sigma/\sqrt{n}\)).
  
  > \( Z = \frac{\bar{X} - \mu}{\sigma/\sqrt{n}} \)
  
  This `Z` will follow a Standard Normal Distribution, `N(0, 1)`.
  
  **Example: Rolla Residents' Income**
  *   Population: `μ = $30,000`, `σ = $10,000`. Shape is unknown.
  *   Sample: `n = 100`.
  *   Since `n = 100 ≥ 30`, the CLT applies. The sampling distribution of \(\bar{X}\) will be approximately Normal.
  *   Mean of \(\bar{X}\): \(\mu_{\bar{X}} = \mu = 30,000\)
  *   Standard Error of \(\bar{X}\): \(\sigma_{\bar{X}} = \frac{\sigma}{\sqrt{n}} = \frac{10,000}{\sqrt{100}} = \frac{10,000}{10} = 1,000\)
  *   So, `X̄ ≈ N(30000, 1000)`.
  
  **a) Probability the sample mean is less than $29,500:**
  1.  **Standardize:** \( Z = \frac{29,500 - 30,000}{1,000} = \frac{-500}{1,000} = -0.5 \)
  2.  **Find Probability:** \( P(\bar{X} < 29,500) = P(Z < -0.5) \approx 0.3085 \)
  
  **b) Probability the sample mean is greater than $31,333:**
  1.  **Standardize:** \( Z = \frac{31,333 - 30,000}{1,000} = \frac{1,333}{1,000} = 1.333 \)
  2.  **Find Probability:** \( P(\bar{X} > 31,333) = P(Z > 1.333) = 1 - P(Z \le 1.333) \approx 1 - 0.9087 = 0.0913 \)