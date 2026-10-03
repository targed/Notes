### **Stat 3113: Lesson 13 - The Poisson Probability Distribution**
  
  The Poisson distribution is another fundamental discrete probability distribution. It is particularly useful for modeling the number of times an event occurs over a specified interval of time, area, or volume, where the events happen randomly and independently at a constant average rate.
  
  ---
### **1. The Poisson Distribution**
  
  A random variable `X` follows a **Poisson distribution** if it describes the number of occurrences of an event in a fixed interval.
#### **A. Probability Mass Function (pmf)**
  The pmf of a Poisson random variable `X` with parameter \(\mu\) is given by:
  
  > \( p(x) = P(X = x) = \frac{e^{-\mu}\mu^x}{x!} \quad \text{for } x = 0, 1, 2, ... \)
  
  *   **Parameter \(\mu\):** The parameter \(\mu\) (mu) must be positive (\(\mu > 0\)). It represents the **average number of events** (or the rate) in the given interval.
  *   **Notation:** We write `X ~ Poisson(μ)` or `X ~ POI(μ)`.
#### **B. Expected Value and Variance**
  A unique and defining characteristic of the Poisson distribution is that its expected value (mean) and variance are equal to the same parameter, \\(\mu\\).
  
  *   **Expected Value (Mean):**
    > \( E(X) = \mu \)
  *   **Variance:**
    > \( V(X) = \mu \)
  *   **Standard Deviation:**
    > \( \sigma = \sqrt{V(X)} = \sqrt{\mu} \)
  
  ---
### **2. The Poisson Process**
  
  The Poisson distribution is the result of a **Poisson process**, which models events that are:
  1.  **Countable:** We are counting the number of occurrences.
  2.  **Random:** The events occur at random and are independent of one another.
  3.  **Occurring at a constant rate (\\(\lambda\\)):** While the exact timing is random, the long-run average rate of occurrence is constant.
  
  The parameter \(\mu\) is calculated as \(\mu = \lambda t\), where \(\lambda\) is the rate and `t` is the length of the specific interval.
  
  **Common Applications:**
  *   Number of car accidents at an intersection per month.
  *   Number of defects per square meter of sheet metal.
  *   Number of calls arriving at a call center per hour.
  *   Number of requests to a web server per minute.
  
  ---
### **3. Poisson Approximation to the Binomial**
  
  The Poisson distribution provides an excellent approximation for the Binomial distribution under specific conditions. This is useful because the Poisson pmf is often easier to compute than the Binomial pmf.
  
  *   **Conditions:** The approximation is accurate when `n` (the number of trials) is **large** and `p` (the probability of success) is **small**.
  *   **Rule:** If `X ~ Bin(n, p)` where `n` is large and `p` is small, then the distribution of `X` can be approximated by a Poisson distribution with parameter \\(\mu = np\\).
    > `Bin(n, p) ≈ Poisson(μ = np)`
  
  ---
### **4. Worked Examples**
#### **Example 1: Tornadoes**
  *   **Scenario:** The number of tornadoes `X` in a region follows a Poisson distribution with a mean of 8 per year. So, `X ~ Poisson(μ=8)`.
  *   **a) P(exactly 5 tornadoes) = P(X = 5):**
    \( P(X=5) = \frac{e^{-8}8^5}{5!} \approx 0.0916 \)
  *   **b) P(at most 5 tornadoes) = P(X ≤ 5):** This is a cdf calculation.
    \( P(X \le 5) = \sum_{x=0}^{5} \frac{e^{-8}8^x}{x!} = P(X=0) + ... + P(X=5) \approx 0.1912 \)
  *   **c) P(between 6 and 9 tornadoes, inclusive) = P(6 ≤ X ≤ 9):**
    This can be calculated as `P(X ≤ 9) - P(X ≤ 5)`.
    `F(9) - F(5) ≈ 0.7166 - 0.1912 = 0.5254`
  *   **d) P(more than 10 tornadoes) = P(X > 10):**
    Use the complement rule: `1 - P(X ≤ 10)`.
    `1 - F(10) = 1 - 0.8159 = 0.1841`
  *   **e) Mean and Standard Deviation:**
    *   Mean: `E(X) = μ = 8` tornadoes.
    *   Standard Deviation: `σ = \(sqrt{μ}\) = \sqrt{8} \approx 2.828` tornadoes.
#### **Example 2: Poisson Process (Drivers)**
  *   **Scenario:** The number of drivers follows a Poisson process with a mean rate of **10 per hour**. The time period is one hour, so `μ = 10`.
  *   **a) P(at most 3 drivers) = P(N(1) ≤ 3):**
    This is `F(3)` for `μ=10`, which is approximately `0.0103`.
  *   **b) P(exceeds 7 drivers) = P(N(1) > 7):**
    `1 - P(N(1) ≤ 7) = 1 - F(7) \approx 1 - 0.2202 = 0.7798`.
  *   **c) P(between 5 and 8 drivers, non-inclusive) = P(5 < N(1) < 8):**
    This means `P(X=6) + P(X=7)`. Alternatively, `P(X≤7) - P(X≤5)`.
    `F(7) - F(5) \approx 0.2202 - 0.0671 = 0.1531`.