### **Stat 3113: Lesson 12 - The Binomial Probability Distribution**
  
  The Binomial distribution is a specific type of discrete probability distribution that is extremely useful in engineering and quality control. It is used to model the number of "successes" in a fixed number of independent trials.
  
  ---
### **1. The Binomial Random Variable**
  
  An experiment is a **Binomial experiment** if it satisfies the following four conditions:
  
  1.  **Fixed Number of Trials (n):** The experiment consists of a fixed number of identical trials, `n`.
  2.  **Independent Trials:** The outcome of one trial does not affect the outcome of any other trial.
  3.  **Two Outcomes:** Each trial results in one of two mutually exclusive outcomes, typically labeled **Success (S)** or **Failure (F)**.
  4.  **Constant Probability (p):** The probability of a "Success," denoted by `p`, remains the same for every trial. The probability of a "Failure" is therefore `1 - p`, often denoted as `q`.
  
  If these conditions are met, the random variable `X`, defined as the **total number of successes in `n` trials**, is called a **Binomial Random Variable**.
  
  *   **Notation:** We write **`X ~ Bin(n, p)`** to indicate that X is a binomial random variable with `n` trials and a success probability of `p`.
  
  ---
### **2. The Binomial Distribution Formulas**
#### **A. Probability Mass Function (pmf)**
  
  The pmf of a binomial distribution calculates the probability of getting *exactly `x` successes* in `n` trials.
  
  > \( P(X=x) = \binom{n}{x} p^x (1-p)^{n-x} \quad \text{for } x = 0, 1, 2, ..., n \)
  
  Where:
  *   \(\binom{n}{x} = \frac{n!}{x!(n-x)!}\) is the binomial coefficient, representing the number of ways to choose `x` successes from `n` trials.
  *   \(p^x\) is the probability of getting `x` successes.
  *   \((1-p)^{n-x}\) is the probability of getting `n-x` failures.
#### **B. Cumulative Distribution Function (cdf)**
  
  The cdf calculates the probability of getting *`x` or fewer successes* in `n` trials.
  
  > \( F(x) = P(X \le x) = \sum_{y=0}^{x} \binom{n}{y} p^y (1-p)^{n-y} \)
  
  **Using Software (like Excel):**
  *   **pmf (P(X=x)):** `=BINOM.DIST(x, n, p, FALSE)`
  *   **cdf (P(X≤x)):** `=BINOM.DIST(x, n, p, TRUE)`
  
  ---
### **3. Mean, Variance, and Standard Deviation**
  
  For a binomial random variable `X ~ Bin(n, p)`, there are simple shortcut formulas for the expected value, variance, and standard deviation.
  
  *   **Expected Value (Mean):**
    > \( E(X) = \mu = np \)
    *Interpretation: The average number of successes we expect in `n` trials.*
  
  *   **Variance:**
    > \( V(X) = \sigma^2 = np(1-p) \)
  
  *   **Standard Deviation:**
    > \( \sigma = \sqrt{np(1-p)} \)
  
  **Note:** A Binomial random variable can be thought of as the **sum of `n` independent Bernoulli random variables**.
  
  ---
### **4. Worked Examples**
#### **Example 1 & 2: `X ~ Bin(20, 0.15)`**
  *   **a) P(X = 8):**
    \(P(X=8) = \binom{20}{8}(0.15)^8(0.85)^{12} \approx 0.0046\)
  *   **b) P(X ≤ 2):**
    This is a cdf calculation: `F(2)`.
    \(P(X \le 2) = P(X=0) + P(X=1) + P(X=2) \approx 0.4049\)
  *   **c) P(X ≥ 1):** Use the complement rule. The opposite of "at least one" is "none".
    \(P(X \ge 1) = 1 - P(X=0) \approx 1 - 0.0388 = 0.9612\)
  *   **d) Expected Value, Variance, and Standard Deviation:**
    *   `E(X) = np = 20 \times 0.15 = 3`
    *   `V(X) = np(1-p) = 20 \times 0.15 \times 0.85 = 2.55`
    *   `σ = \sqrt{2.55} \approx 1.5969`
#### **Example 3: Crystal Goblets**
  *   **Scenario:** 10% of goblets have flaws. Sample size `n=6`.
  *   **Define RV:** Let `X` = # of goblets with flaws ("seconds").
  *   **Distribution:** `X ~ Bin(n=6, p=0.10)`.
  *   **a) P(exactly one is a second) = P(X = 1):**
    \(P(X=1) = \binom{6}{1}(0.10)^1(0.90)^5 \approx 0.3543\) (or 35.4%)
  *   **b) P(at least two are seconds) = P(X ≥ 2):**
    Use the complement rule: `1 - P(X ≤ 1)`.
    `P(X ≥ 2) = 1 - (P(X=0) + P(X=1)) \approx 1 - (0.5314 + 0.3543) = 0.1143`
  *   **c) Average number of seconds:** This is the expected value.
    `E(X) = np = 6 \times 0.10 = 0.6`
    *On average, we would expect to find 0.6 flawed goblets in a sample of six.*
  
  ---
### **5. Estimating the Success Probability `p`**
  
  In real-world applications, the true probability of success `p` is often unknown. We use sample data to estimate it.
  
  *   **Sample Proportion (\\(\hat{p}\\)):** The best estimate of the population proportion `p` is the sample proportion, \\(\hat{p}\\). It is calculated as the number of successes (`X`) divided by the number of trials (`n`).
    > \( \hat{p} = \frac{X}{n} \)
  *   \(\hat{p}\) is an **unbiased estimator** of `p`, meaning its expected value is equal to the true `p`.
  *   **Uncertainty of \(\hat{p}\):** Since \(\hat{p}\) is a statistic calculated from a sample, it has its own variability, measured by its standard deviation (often called the **standard error**).
    > \( \sigma_{\hat{p}} = \sqrt{\frac{p(1-p)}{n}} \approx \sqrt{\frac{\hat{p}(1-\hat{p})}{n}} \)
#### **Example 4: Ice Cream Containers**
  *   **Data:** `n = 20` containers, `X = 3` are underfilled.
  *   **Estimate `p`:**
    \(\hat{p} = \frac{X}{n} = \frac{3}{20} = 0.15\)
    *Our best estimate for the probability that the machine underfills a container is 15%.*
  *   **Estimate the standard deviation of \\(\hat{p}\\):**
    \(\sigma_{\hat{p}} \approx \sqrt{\frac{0.15(1-0.15)}{20}} \approx 0.0794\)