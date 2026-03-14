## 1. The GIGO Principle
*   **Definition:** **G**arbage **I**n, **G**arbage **O**ut.
*   **Algorithmic Impact:** If your training data contains errors, biases, or noise, the algorithm will "learn" those errors as if they were valid patterns. The resulting model might have high "accuracy" on that bad data but will fail catastrophically in the real world.
- ## 2. The 6 Dimensions of Data Quality
  To "trust" data, it must meet these criteria:
  1.  **Accuracy:** Is it true to fact? (e.g., A person’s age cannot be 150).
  2.  **Completeness:** Are there missing values? (e.g., A survey where 40% of people skipped the "Income" question).
  3.  **Consistency:** Does the data match across systems? (e.g., "M/D/Y" format in the sales DB vs. "Y/M/D" in the shipping DB).
  4.  **Timeliness:** Is the data recent? (e.g., Using 2010 real estate prices to predict 2026 home values).
  5.  **Noise Level:** How much "random error" is in the measurement?
  6.  **Bias:** Does the sample represent the entire population? (e.g., Training a self-driving car only in sunny California and expecting it to drive in a Missouri snowstorm).
  
  ---
- ## 3. Noise vs. Outliers (The Critical Distinction)
- ### A. Noise
  *   **Definition:** The random component of a measurement error. 
  *   **The Formula:** **Data = Signal + Noise**.
  *   **Signal-to-Noise Ratio (SNR):** The ratio of meaningful information to background interference. 
    *   *Low SNR:* The pattern is buried (like a grainy photo or a static-filled radio station).
  *   **Algorithmic Handling:** Algorithms often use **"Smoothing"** techniques (like moving averages) to filter out the high-frequency "jitter" of noise to reveal the underlying trend.
- ### B. Outliers
  *   **Definition:** Data objects with characteristics considerably different from the rest of the dataset.
  *   **The Two Cases:**
    *   **Case 1: Outliers as "Bad" (Noise/Error):** A sensor glitched and recorded a temperature of 500°F. These should be removed because they distort the mean and the model.
    *   **Case 2: Outliers as "Good" (The Goal):** In **Anomaly Detection**, the outlier is exactly what we want to find.
        *   *Example:* A \$10,000 purchase on a credit card usually used for \$20 groceries. This is a "Rare Event" outlier.
  
  ---
- ## 4. Inconsistent Data
  Inconsistency often happens during **Data Integration** (merging different sources).
  *   **Unit Mismatches:** One system uses centimeters, another uses feet. (Note: The \$125 million Mars Climate Orbiter was lost because one team used Metric and the other used Imperial units).
  *   **Semantic Mismatches:** "ID" might mean "Social Security Number" in one table and "Internal Employee Number" in another.
  
  ---
- ## 5. Generated (Synthetic) Data
  Because real data is expensive and sensitive, we often use software to create "Fake" data that looks real.
  *   **The "Pros":**
    *   **Privacy:** You can train a medical AI on synthetic patients without leaking real private records.
    *   **Rare Case Generation:** If you only have 5 examples of a rare disease, you can generate 500 synthetic ones to help the model learn the pattern.
  *   **The "Cons":**
    *   **Validation:** It is hard to prove the "fake" data accurately reflects all the hidden complexities of the real world.
    *   **Misuse:** Deepfakes (fake video/audio) are a form of synthetic data that can be used for misinformation.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Noise or Outlier?**
  A dataset of human heights contains an entry of **"24 inches"**. 
  *   Is this likely **Noise** or an **Outlier**? 
  *   *Answer:* It is an **Outlier**. Noise is usually a small, random variation (e.g., 70.1 inches instead of 70). 24 inches is a valid height (for a baby) but is "considerably different" from an adult population. You must investigate the context before deleting it.
  
  **2. SNR Logic:**
  If you are trying to predict the stock market and the "Signal" (the actual economic trend) is very weak compared to the "Noise" (daily random trading fluctuations), is a **complex** model or a **simple** model better?
  *   *Answer:* Usually a **simple** model. Complex models are prone to **Overfitting**, meaning they will mistake the noise for a real pattern and make terrible predictions.
  
  **3. Consistency Check:**
  You merge two spreadsheets. One lists "State" as **"Missouri"** and the other as **"MO"**. 
  *   What dimension of data quality is being violated? (Answer: Consistency). 
  *   What is the fix? (Answer: Data Normalization/Standardization).