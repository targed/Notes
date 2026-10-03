### **Stat 3113: Lesson 16 - The Exponential Distribution**
  
  This lesson introduces the **Exponential distribution**, a continuous probability distribution that is fundamentally linked to the Poisson process. While the Poisson distribution counts the *number of events* in an interval, the Exponential distribution models the *time between* those events.
  
  ---
### **1. The Exponential Distribution**
  
  The Exponential distribution is a continuous distribution used to model the waiting time until the next event occurs or the lifetime of a component.
#### **A. Probability Density Function (pdf)**
  
  A continuous random variable `X` has an **Exponential distribution** with parameter \\(\lambda\\) if its pdf is:
  > f(x) =
  \begin{cases}
  \lambda e^{-\lambda x} & \text{for } x \ge 0 \\
  0 & \text{otherwise}
  \end{cases}
  
  *   **Parameter \\(\lambda\\):** The parameter \\(\lambda\\) (lambda) is the **rate parameter**. It represents the average number of events per unit of time (the same rate as in the Poisson process).
  *   **Notation:** We write `X ~ Exp(λ)`.
#### **B. Key Formulas and Results**
  
  For an exponentially distributed random variable `X ~ Exp(λ)`:
  
  *   **Cumulative Distribution Function (cdf):**
    > \( F(x) = P(X \le x) = 1 - e^{-\lambda x} \quad \text{for } x \ge 0 \)
  
  *   **Survival Function (Probability of "lasting longer than x"):**
    > \( P(X > x) = 1 - F(x) = e^{-\lambda x} \)
  
  *   **Expected Value (Mean):** The average waiting time until the next event.
    > \( E(X) = \mu = \frac{1}{\lambda} \)
    *This is a crucial inverse relationship: if the rate \(\lambda\) is high (many events per hour), the average waiting time \(1/\lambda\) is low (few hours per event).*
  
  *   **Variance:**
    > \( V(X) = \sigma^2 = \frac{1}{\lambda^2} \)
  
  *   **Standard Deviation:**
    > \( \sigma = \frac{1}{\lambda} \)
    *Note: For the exponential distribution, the mean and the standard deviation are always equal.*
  
  ---
### **2. The "Memoryless" Property**
  
  The Exponential distribution has a unique and important characteristic called the **memoryless property**.
  
  > **P(X > x + t | X > x) = P(X > t)**
  
  **Interpretation:** This means that the probability of waiting an *additional `t` seconds* does not depend on how long you have *already been waiting*. The process "forgets" its past. If a component has already lasted for `x` hours, the probability that it lasts for another `t` hours is the same as the probability that a brand-new component would last for `t` hours.
  
  **Example:**
  If the response time of a server is exponential and it has already been 3 seconds, the probability that you'll have to wait another 12 seconds is the same as the initial probability of having to wait more than 12 seconds.
  \(P(X > 15 | X > 3) = P(X > 12)\)
  
  ---
### **3. Relationship with the Poisson Process**
  
  The Poisson and Exponential distributions are two sides of the same coin.
  
  *   A **Poisson Process** models the number of events occurring in an interval at a constant average rate \\(\lambda\\).
    *   The **number of events** `X` in an interval of length `t` follows a **Poisson distribution** with parameter `μ = λt`.
  
  *   If events follow a Poisson process with rate \\(\lambda\\), then:
    *   The **waiting time `T` until the next event** follows an **Exponential distribution** with the same rate parameter \\(\lambda\\).
  
  **Example:**
  If customer arrivals at a bank follow a Poisson process at a rate of \\(\lambda = 10\\) customers per hour, then the waiting time `T` between customer arrivals follows an Exponential distribution with \\(\lambda = 10\\). The average waiting time would be `E[T] = 1/λ = 1/10` of an hour (6 minutes).
  
  ---
### **4. Worked Examples**
#### **Example 1: Computer Response Time**
  
  *   **Scenario:** The response time `X` is exponential with an *expected (mean)* time of 5 seconds.
  *   **Find \\(\lambda\\):** We are given `E[X] = 5`. We know `E[X] = 1/λ`.
    So, \(5 = 1/\lambda \implies \lambda = 1/5 = 0.2\).
  *   **a) P(at most 10 seconds) = P(X ≤ 10):**
    This is the cdf, `F(10)`.
    \(F(10) = 1 - e^{-\lambda \cdot 10} = 1 - e^{-0.2 \cdot 10} = 1 - e^{-2} \approx 0.8647\)
  *   **b) P(between 5 and 10 seconds) = P(5 ≤ X ≤ 10):**
    `P(5 ≤ X ≤ 10) = F(10) - F(5) = (1 - e^{-0.2 \cdot 10}) - (1 - e^{-0.2 \cdot 5})`
    `= (1 - e^{-2}) - (1 - e^{-1}) = e^{-1} - e^{-2} \approx 0.3679 - 0.1353 = 0.2326`
  *   **d) P(longer than 15s | already longer than 3s) = P(X > 15 | X > 3):**
    Using the memoryless property:
    `P(X > 15 | X > 3) = P(X > 12)`
    `= e^{-\lambda \cdot 12} = e^{-0.2 \cdot 12} = e^{-2.4} \approx 0.0907`
#### **Example 2: Radioactive Emissions**
  
  *   **Scenario:** Emissions follow a Poisson process with a mean rate of 15 particles per minute.
  *   **Identify \(\lambda\):** The rate is given, but we need to be careful with units. The question asks about seconds.
    *   Rate in minutes: \(\lambda = 15\) particles/minute.
    *   Rate in seconds: \(\lambda = \frac{15}{60} = 0.25\) particles/second.
  *   **Distribution of waiting time T:** `T ~ Exp(λ=0.25)`.
  *   **a) P(more than 5 seconds elapse) = P(T > 5):**
    `P(T > 5) = e^{-\lambda \cdot 5} = e^{-0.25 \cdot 5} = e^{-1.25} \approx 0.2865`
  *   **b) Mean waiting time:**
    `E[T] = 1/λ = 1/0.25 = 4` seconds.