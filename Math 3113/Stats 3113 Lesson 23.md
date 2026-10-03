### **Stat 3113: Lesson 23 - Introduction to Hypothesis Testing**
  
  Hypothesis testing is a formal procedure for using sample data to evaluate a claim about a population. It is one of the most important methods in statistical inference, providing a structured way to make decisions in the face of uncertainty.
  
  ---
### **1. The Core Idea**
  
  The fundamental question of hypothesis testing is: **"Is the effect I'm seeing in my sample real, or could it have just happened by random chance?"**
  
  We start by assuming there is "no effect" or "no change" (the status quo) and then look for evidence in our sample data that is strong enough to make us reject that initial assumption.
  
  ---
### **2. The Two Hypotheses**
  
  Every hypothesis test involves two competing, mutually exclusive statements about a population parameter (like \(\mu\) or `p`).
#### **A. The Null Hypothesis (H₀)**
  
  *   **What it is:** The **null hypothesis (H₀)** is the default assumption, the "status quo," or the claim of "no effect." It always contains a statement of equality (`=`, `≤`, or `≥`). We assume H₀ is true to begin the test.
  *   **Analogy:** In a legal system, this is the presumption of "innocent until proven guilty." H₀ is the "innocent" plea.
#### **B. The Alternative Hypothesis (Hₐ or H₁)**
  
  *   **What it is:** The **alternative hypothesis (Hₐ)** is the research hypothesis—the new claim you are trying to find evidence for. It is what you will conclude if you have enough evidence to reject the null hypothesis. It never contains a statement of equality (`≠`, `>`, or `<`).
  *   **Analogy:** This is the prosecutor's claim of "guilty." The burden of proof is on the data to support Hₐ.
  
  **Types of Alternative Hypotheses:**
  *   **Two-Tailed Test (≠):** Used when you are testing for any difference from the null value (e.g., `Hₐ: μ ≠ 20`).
  *   **Right-Tailed Test (>):** Used when you are testing for an increase (e.g., `Hₐ: μ > 20`).
  *   **Left-Tailed Test (<):** Used when you are testing for a decrease (e.g., `Hₐ: μ < 20`).
  
  ---
### **3. The Four Steps of a Hypothesis Test**
  
  All hypothesis tests follow a common logical structure.
#### **Step 1: State the Hypotheses**
  Translate the research question into a null (H₀) and an alternative (Hₐ) hypothesis about a specific population parameter.
#### **Step 2: Calculate the Test Statistic**
  The **test statistic** is a number calculated from the sample data that measures how far your sample result is from the null hypothesis value. It's a standardized score (like a Z-score or a t-score).
  > **Test Statistic = (Sample Statistic - Null Value) / (Standard Error)**
  
  A large test statistic (far from zero) suggests the sample result is far from the null hypothesis, providing evidence against H₀.
#### **Step 3: Calculate the P-value**
  The **p-value** is the most important piece of evidence.
  *   **Definition:** The p-value is the probability of observing a test statistic as extreme as, or more extreme than, the one calculated from your sample, **assuming the null hypothesis (H₀) is true**.
  *   **Interpretation:**
    *   **Small p-value:** Your observed sample result is very surprising (unlikely) if the null hypothesis is true. This provides **strong evidence against H₀**.
    *   **Large p-value:** Your observed sample result is not surprising; it's consistent with what you'd expect to see from random chance if the null hypothesis is true. This provides **weak evidence against H₀**.
#### **Step 4: Make a Conclusion**
  To make a final decision, we compare the p-value to a pre-determined threshold called the **significance level (α)**.
  *   **Significance Level (α):** This is the probability of making a Type I error (see below). It represents our "willingness to be wrong." Common values are `α = 0.05`, `0.01`, or `0.10`.
  
  **The Decision Rule:**
  *   If **p-value ≤ α**: **Reject the null hypothesis (H₀)**. There is statistically significant evidence to support the alternative hypothesis (Hₐ).
  *   If **p-value > α**: **Fail to reject the null hypothesis (H₀)**. There is not enough evidence to support the alternative hypothesis. (Note: This does *not* mean we've proven H₀ is true, only that we couldn't disprove it).
  
  ---
### **4. Errors in Hypothesis Testing**
  
  Since our decision is based on a sample, not the entire population, we can make one of two mistakes.
  
  | | **H₀ is Actually True** | **H₀ is Actually False** |
  | :--- | :--- | :--- |
  | **Decision: Reject H₀** | **Type I Error** (False Positive) | **Correct Decision** (Power) |
  | **Decision: Fail to Reject H₀** | **Correct Decision** | **Type II Error** (False Negative)|
  
  *   **Type I Error (α):** Rejecting a true null hypothesis. (A "false alarm").
    *   The probability of a Type I error is equal to the significance level, **`P(Type I Error) = α`**. We control this by setting α at the start.
  *   **Type II Error (β):** Failing to reject a false null hypothesis. (Failing to detect a real effect).
    *   The probability of a Type II error is **`P(Type II Error) = β`**.
  *   **Power of a Test (1 - β):** The probability of correctly rejecting a false null hypothesis. This is the test's ability to detect a real effect when one exists.