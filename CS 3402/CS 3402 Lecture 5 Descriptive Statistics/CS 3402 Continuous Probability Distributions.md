#### **1. Discrete vs. Continuous (Slides 2–3)**
*   **Discrete Random Variable (Slide 2):**
  *   **Tool:** Probability Mass Function (**PMF**).
  *   **Logic:** You can list every possible outcome (e.g., Rolling a die: 1, 2, 3, 4, 5, 6).
  *   **Probability:** $P(X=1) = 1/6$. The sum of all probabilities is 1.
*   **Continuous Random Variable (Slide 3):**
  *   **Tool:** Probability Density Function (**PDF**).
  *   **Logic:** You cannot list all outcomes because there are infinite numbers between 0 and 1.
  *   **The Trap:** In continuous math, the probability of hitting *exactly* a specific number is effectively Zero ($P(X=c) = 0$). Instead, we measure the probability of falling within a **range**.
  *   **Math:** Area under the curve = 1 (Integral, not Sum).
- #### **2. The Cumulative Distribution Function (CDF) (Slide 4)**
  *   **Definition:** $F_X(x) = P(X \leq x)$.
  *   **Translation:** "What is the probability that the value is less than or equal to $x$?"
  *   **Relationship:** The CDF is the **Integral** (area under the curve) of the PDF. Conversely, the PDF is the **Derivative** (slope) of the CDF.
- #### **3. Important Distributions (Slides 5–6)**
  *   **Exponential Distribution (Slide 5):**
    *   **Shape:** Starts high and drops quickly (the "Hockey Stick" curve).
    *   **Use Case:** Modeling "Time until next event." (e.g., Time until the bus arrives, or time until a lightbulb burns out).
    *   **Solving the Slide Question:** *How to compute $P(a \leq X \leq b)$?*
        *   Formula: $\text{CDF}(b) - \text{CDF}(a)$.
  *   **Normal (Gaussian) Distribution (Slide 6):**
    *   **Shape:** The Bell Curve.
    *   **Parameters:** Defined entirely by Mean ($\mu$) and Standard Deviation ($\sigma$).
    *   **Why it matters:** The **Central Limit Theorem** states that if you take enough samples of *anything*, the averages will form a Normal Distribution. This is the foundation of almost all statistical testing.
- #### **4. Kernel Density Estimation (KDE) (Slide 7)**
  This is a critical Data Science technique.
  *   **The Problem:** In the real world, you don't know the equation for your data's distribution. You just have a list of numbers (dots on a line).
  *   **The Histogram Limitation:** Histograms are "blocky" and depend heavily on how wide you make the bins.
  *   **The Solution (KDE):**
    1.  Place a small "Bump" (Gaussian Kernel) on top of every single data point.
    2.  Sum all the bumps together.
    3.  **Result:** A smooth curve that estimates the underlying PDF of your data without needing a formula.
  
  ---
- ### **"Fill-in" Concepts for your Notes:**
  *   **Integration:** When the slide shows $\int$, remember that in Python/Numerical computing, we usually approximate this by summing small rectangles (Riemann sums).
  *   **Parameters:** Notice how distributions are defined by parameters ($\lambda$ for Exponential, $\mu, \sigma$ for Normal). "Learning" in statistics often just means "finding the best $\mu$ and $\sigma$ to fit our data."
  
  ---
- ### **Action Items for Section 1:**
  *   **Math Check:** Can you interpret the difference between PDF and CDF?
    *   *Test:* If the PDF represents "Speed of cars on a highway," the **PDF** height at 70mph tells you how *common* that speed is relative to others. The **CDF** at 70mph tells you "What percentage of cars are going 70mph *or slower*."
  *   **Visual Check:** Look at Slide 7. Notice how the Red line (Summed function) envelopes the Black dotted lines (Individual kernels). This is the visual representation of KDE.