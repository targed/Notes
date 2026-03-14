## 1. The Preprocessing Philosophy
**Slide Context:** *"Preprocessing often improves performance more than changing models."*
**Comprehensive Notes:**
In an algorithms class, it’s easy to get obsessed with complex architectures (like Deep Neural Networks). However, if your data is "garbage," a complex model will only find "complex garbage." 
*   **Real-world vs. Algorithm assumptions:** 
  *   *Real-world:* Data is noisy, incomplete (NaNs), and inconsistent.
  *   *Algorithms:* Most assume everything is a number and that all features are on a comparable scale (e.g., they don't know that "1" in a 'number of kids' column is huge while "1" in an 'annual salary' column is tiny).
- ## 2. Aggregation
  **Definition:** The process of combining two or more attributes (or objects) into a single summary.
  **Purpose & Benefits:**
  *   **Data Reduction:** Shrinking the dataset size so algorithms run faster.
  *   **Change of Scale:** Zooming out to see patterns. (e.g., Looking at "Monthly Sales" instead of "Single Transactions" to see seasonal trends).
  *   **Data Stability:** Aggregation acts as a natural "filter." Individual data points are volatile and noisy, but their *average* is usually more stable and reflective of the true underlying signal.
- ## 3. Handling Missing Data
  **Slide Context:** Slide 5 shows that simple NumPy operations like `np.mean()` will propagate `NaN`. If one value is missing, the entire result becomes `NaN`, which can crash a training pipeline.
- ### The Remediation Strategies:
  1.  **Removal (Listwise Deletion):** Deleting any row with a missing value. 
    *   *Best for:* When you have millions of rows and only a few are missing data.
  2.  **Imputation (Filling in the blanks):**
    *   **Numerical:** Use **Mean** (if data is symmetric) or **Median** (if there are outliers).
    *   **Categorical:** Use **Mode** (the most frequent category, like "Unknown" or the most common major).
    *   **Constant:** Filling with a specific value like `0` or `"Missing"`.
- ## 4. Spotting Outliers: The IQR Rule
  To handle outliers, you first have to define them mathematically. The most common method is the **Boxplot Rule (Interquartile Range - IQR).**
  
  **The Formula:**
  1.  Find **Q1** (25th percentile) and **Q3** (75th percentile).
  2.  Calculate **IQR** = $Q3 - Q1$.
  3.  Set the "Whiskers" (Thresholds):
    *   **Lower Bound** = $Q1 - (1.5 \times IQR)$
    *   **Upper Bound** = $Q3 + (1.5 \times IQR)$
  4.  Anything outside these bounds is a candidate for an outlier.
  
  **Example Walkthrough:**
  *   **Data:** [45, 48, 50, 52, 55, 57, 60, 62, 65, 70, 250]
  *   $Q1 \approx 50$, $Q3 \approx 65$.
  *   $IQR = 65 - 50 = 15$.
  *   **Upper Threshold** = $65 + (1.5 \times 15) = 87.5$.
  *   **Result:** 250 is much higher than 87.5, so it is mathematically an outlier.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Aggregation Logic:**
  You are tracking a patient's heart rate every second for 24 hours. This creates 86,400 data points per day. 
  *   If you aggregate this into **"Average Heart Rate per Hour,"** how many data points do you have now? (Answer: 24).
  *   What did you lose? (Answer: Brief spikes or anomalies that lasted only a few seconds).
  *   What did you gain? (Answer: A much smaller dataset and a clearer view of the patient's rest/activity cycles).
  
  **2. Imputation Choice:**
  You are analyzing a dataset of "Number of Pets." Most people have 0, 1, or 2 pets, but one person has 85 cats. 
  *   A new record is missing the 'Number of Pets' value. Should you use the **Mean** or the **Median** to fill it? 
  *   *Answer:* The **Median**. The outlier (85 cats) will pull the Mean upward, making your guess unrealistically high. The Median is "robust" to outliers.
  
  **3. IQR Calculation Practice:**
  A dataset has $Q1 = 100$ and $Q3 = 200$. 
  *   What is the $IQR$? (Answer: 100).
  *   What is the Upper Threshold for an outlier? (Answer: $200 + 1.5(100) = 350$).
  *   If a value is $349$, is it an outlier? (Answer: No, it's just a high value).