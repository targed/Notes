### **Stat 3113: Lesson 14 - Continuous Random Variables**
  
  This lesson transitions from discrete random variables, which have countable outcomes, to continuous random variables, which can take any value within a given interval.
  
  ---
### **1. PMF (Discrete) vs. PDF (Continuous)**
  
  *   **Discrete (pmf - probability mass function):** For a discrete random variable, the pmf gives the probability at specific points. The graph consists of "spikes," and the **height** of each spike represents the probability at that point.
  *   **Continuous (pdf - probability density function):** For a continuous random variable, we use a pdf, denoted `f(x)`. The graph is a smooth curve. Probabilities are not given by the height of the curve but are represented by the **area under the curve** over an interval.
  
  ---
### **2. The Probability Density Function (pdf)**
  
  The pdf, `f(x)`, of a continuous random variable `X` must satisfy two key properties:
  1.  **Non-negativity:** The function must always be on or above the x-axis.
    \( f(x) \ge 0 \) for all x.
  2.  **Total Area is One:** The total area under the entire curve must equal 1.
    \( \int_{-\infty}^{\infty} f(x) \,dx = 1 \)
#### **Calculating Probabilities with the pdf**
  The probability that `X` falls within an interval `[a, b]` is the integral of the pdf over that interval.
  > \( P(a \le X \le b) = \int_{a}^{b} f(x) \,dx \)
  
  **Crucial Point for Continuous Variables:**
  Because the probability of a continuous random variable taking on any *single exact value* is zero, the inclusion or exclusion of endpoints does not change the probability.
  > \( P(X = a) = \int_{a}^{a} f(x) \,dx = 0 \)
  
  This means:
  \( P(a \le X \le b) = P(a < X \le b) = P(a \le X < b) = P(a < X < b) \)
  
  ---
### **3. Expected Value and Variance**
  
  The concepts of expected value (mean), variance, and standard deviation extend to continuous random variables, with summation being replaced by integration.
  
  *   **Expected Value (Mean, \\(\mu\\)):**
    > \( E(X) = \mu = \int_{-\infty}^{\infty} x \cdot f(x) \,dx \)
  
  *   **Variance (\(\sigma^2\)):** The shortcut formula is still the most convenient:
    > \( V(X) = \sigma^2 = E(X^2) - \mu^2 \)
    where \( E(X^2) = \int_{-\infty}^{\infty} x^2 \cdot f(x) \,dx \).
  
  *   **Standard Deviation (\(\sigma\)):**
    > \( \sigma = \sqrt{V(X)} \)
  
  ---
### **4. Median and Percentiles**
  
  *   **p-th Percentile (\(x_p\)):** The p-th percentile is the value \(x_p\) such that `p` percent of the area under the pdf is to its left.
    > \( P(X \le x_p) = \int_{-\infty}^{x_p} f(x) \,dx = \frac{p}{100} \)
  *   **Median (\(x_m\)):** The median is the **50th percentile**. It is the value that splits the area under the pdf into two equal halves.
    > \( P(X \le x_m) = \int_{-\infty}^{x_m} f(x) \,dx = 0.5 \)
  
  ---
### **5. Worked Example: Uniform Distribution**
  
  This example features the **Continuous Uniform Distribution**, where the probability is constant over a defined interval `[A, B]`. The pdf is `f(x) = 1/(B-A)` for `A ≤ x ≤ B`.
  
  **Scenario:** The angle `X` of an imperfection on a tire is uniformly distributed between 0° and 360°.
  
  *   **1. Find the pdf.**
    The total area must be 1. `f(x) = k`. So \( \int_{0}^{360} k \,dx = 1 \implies k[x]_{0}^{360} = 1 \implies k(360) = 1 \implies k = 1/360 \).
    The pdf is **`f(x) = 1/360`** for `0 ≤ x ≤ 360`.
  
  *   **2. Find P(90° < X < 180°).**
    \( P(90 < X < 180) = \int_{90}^{180} \frac{1}{360} \,dx = \frac{1}{360}[x]_{90}^{180} = \frac{180 - 90}{360} = \frac{90}{360} = 0.25 \)
  
  *   **3. Find P(X > 90°).**
    \( P(X > 90) = \int_{90}^{360} \frac{1}{360} \,dx = \frac{360 - 90}{360} = \frac{270}{360} = 0.75 \)
  
  *   **4. Find the mean and standard deviation.**
    *   **Mean (E(X)):**
        \( E(X) = \int_{0}^{360} x \cdot \frac{1}{360} \,dx = \frac{1}{360} [\frac{x^2}{2}]_{0}^{360} = \frac{360^2}{2 \cdot 360} = \frac{360}{2} = 180 )
    *   **Variance (V(X)):**
        First find `E(X²)`. \( E(X^2) = \int_{0}^{360} x^2 \cdot \frac{1}{360} \,dx = \frac{1}{360} [\frac{x^3}{3}]_{0}^{360} = \frac{360^3}{3 \cdot 360} = \frac{360^2}{3} = 43200 \)
        \( V(X) = E(X^2) - \mu^2 = 43200 - 180^2 = 43200 - 32400 = 10800 \)
    *   **Standard Deviation (σ):**
        \( \sigma = \sqrt{10800} \approx 103.92 \)
  
  *   **5. Find the median and first quartile.**
    *   **Median (\(x_m\)):** We need to solve \( \int_{0}^{x_m} \frac{1}{360} \,dx = 0.5 \).
        \( \frac{x_m}{360} = 0.5 \implies x_m = 360 \times 0.5 = 180 \). The median is 180°.
    *   **First Quartile (\(x_{25}\)):** We need to solve \( \int_{0}^{x_{25}} \frac{1}{360} \,dx = 0.25 \).
        \( \frac{x_{25}}{360} = 0.25 \implies x_{25} = 360 \times 0.25 = 90 \). The first quartile is 90°.