### **Stat 3113: Lesson 19 - Determining Normality**
  
  Many powerful statistical methods (like t-tests and confidence intervals, which you'll learn about later) rely on the assumption that the data comes from a normally distributed population. However, not all data is normal. This lesson covers several methods for assessing whether a given dataset is *approximately* normal.
  
  ---
### **1. Why Assess Normality?**
  
  It is crucial to check for normality before applying statistical techniques that require this assumption.
  *   **Appropriate Analysis:** If the data is normal, you can confidently use methods designed for normal data.
  *   **Informed Decisions:** If the data is *not* normal (e.g., heavily skewed), you must choose alternative methods (like non-parametric tests) or transform the data to make it more symmetric.
  *   **Reliability of Findings:** Using a normality-based test on non-normal data can lead to incorrect conclusions and unreliable p-values.
  
  ---
### **2. Methods for Assessing Normality**
  
  There are several graphical and informal methods to check if a sample of data likely came from a normally distributed population.
#### **A. Histogram with a Superimposed Normal Curve**
  
  This is a direct, visual way to check for normality.
  *   **How it works:** A histogram of the sample data is created, and a perfect normal curve (using the sample's mean and standard deviation) is overlaid on top.
  *   **Interpretation:**
    *   **Good Fit (Approximately Normal):** If the shape of the histogram bars roughly follows the bell shape of the curve, the data is considered approximately normal. It doesn't need to be a perfect match.
    *   **Poor Fit (Not Normal):** If the histogram shows a clear deviation from the bell curve—such as strong skewness, multiple peaks (bimodality), or heavy tails—then the data is likely not normal.
  *   **Limitation:** This method can be subjective and unreliable for small sample sizes, as the shape of the histogram can vary significantly with small changes in the data.
#### **B. Checking against the Empirical (68-95-99.7) Rule**
  
  This is a quick numerical check.
  *   **How it works:**
    1.  Calculate the sample mean (\\(\bar{x}\\)) and sample standard deviation (\\(s\\)).
    2.  Calculate the intervals: \\(\bar{x} \pm s\\), \\(\bar{x} \pm 2s\\), and \\(\bar{x} \pm 3s\\).
    3.  Count the actual percentage of your data points that fall within each of these intervals.
  *   **Interpretation:**
    *   Compare your observed percentages to the theoretical 68%, 95%, and 99.7%. If your percentages are close to these values, it provides evidence that the data is normally distributed.
#### **C. Normal Probability Plot (or Quantile-Quantile Plot)**
  
  This is the most powerful and widely used graphical method for assessing normality.
  
  *   **How it works (Conceptually):** The plot compares the *observed* percentiles of your data against the *expected* percentiles from a perfect standard normal distribution.
    1.  Your data points are ordered from smallest to largest.
    2.  Each data point is treated as a percentile of your observed data.
    3.  The plot graphs your observed data values against the theoretical z-scores (quantiles) you would *expect* to see at those same percentiles in a perfect normal distribution.
  *   **Interpretation:**
    *   **Normally Distributed:** If the data is normal, the points on the plot will fall close to a **straight, diagonal line** (often a 45° line).
    *   **Not Normally Distributed:** Systematic deviations from the straight line indicate non-normality.
        *   An **S-shaped curve** suggests the data has "heavy" or "light" tails compared to a normal distribution.
        *   A **curved (bowed) pattern** indicates skewness.
  *   **Advantage:** This plot is better than a histogram at detecting deviations from normality, especially in the tails of the distribution.
  
  ---
  *(Note: A fourth method, formal hypothesis tests for normality like the Shapiro-Wilk test, provides a p-value to help make a decision but will be covered in later lessons.)*