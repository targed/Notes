## 1. Missing Values
Missing data is a reality in almost every dataset. It occurs due to equipment failure, refusal to answer sensitive questions (like income), or because the attribute simply doesn't apply (e.g., "spouse name" for a single person).
- ### Strategies for Handling Missing Values:
  *   **Elimination:** Delete the row (object) or the column (attribute). 
    *   *Risk:* You might lose too much information if many rows have at least one missing value.
  *   **Imputation (Estimation):** Fill in the blanks with a reasonable guess.
    *   *Mean/Median Imputation:* Fill with the average of that column. (Common, but reduces variance).
    *   *Model-based Imputation:* Use other attributes to predict the missing one (e.g., use "Job Title" to estimate missing "Income").
  *   **Ignore:** Some algorithms (like Naïve Bayes or certain Decision Trees) can handle missing values internally without needing a fix.
  *   **The "Never" Rule (Slide 54):** Never fill missing values with **random numbers**. This introduces artificial noise and destroys the mathematical relationships the algorithm is trying to find.
- ## 2. Duplicate Data
  This is a major issue when merging data from different departments or companies (e.g., a customer exists in the "Sales" DB and the "Support" DB under slightly different names).
  *   **De-duplication:** The process of identifying and merging these records. 
  *   *Challenge:* "Almost duplicates" (e.g., "Jon Doe" vs "John Doe"). This requires "Fuzzy Matching" algorithms.
- ## 3. Labeled vs. Unlabeled Data
  *   **Labeled Data:** Data that has the "answer" attached (e.g., an image of a cat that is explicitly tagged "Cat"). This is required for **Supervised Learning**.
  *   **Unlabeled Data:** Data with no tags. This is what you use for **Unsupervised Learning** (Clustering).
  *   **The Reality:** Manually labeling data (Data Annotation) is slow and extremely expensive. Most of the world's data is unlabeled.
- ## 4. The Unbalanced Data Problem
  This is a high-probability exam topic.
  *   **The Scenario:** One class is very rare (e.g., 99% healthy patients, 1% with a rare disease).
  *   **The Accuracy Paradox:** If an algorithm predicts "Healthy" for every single person, it is **99% accurate** but **0% useful**.
  *   **The Fix:** 
    *   **Cost-Sensitive Learning:** Tell the algorithm that misclassifying a sick person (False Negative) is 100x more "expensive" than misclassifying a healthy person (False Positive).
    *   **Resampling:** Artificially duplicate the rare cases (Oversampling) or delete some common cases (Undersampling) to balance the scales.
- ## 5. Data Quality vs. Model Complexity
  This is the "Golden Rule" of Data Mining:
  > **High-Quality Data + Simple Model > Low-Quality Data + Complex Model**
  
  A complex model (like a Deep Neural Network) applied to low-quality data will simply "memorize the garbage" (Overfitting). A simple model (like Linear Regression) on high-quality data will often provide more robust, generalizable results.
  
  ***
- # Problem Solving & Concept Check
  
  **1. The Medical Outlier:**
  A patient’s heart rate is recorded as **250 bpm**. What should you do first?
  *   A. Remove it immediately.
  *   B. Replace it with the average.
  *   C. **Investigate whether it is an error or a rare event.**
  *   *Why:* In medicine, 250 bpm is physically possible but life-threatening (an anomaly). If it’s a recording error, delete it. If it’s a real event, it is the most important data point in your set!
  
  **2. Handling Missing Data:**
  You are analyzing a dataset of 1,000 house sales. The "Year Built" is missing for 500 of them.
  *   Should you use **Elimination** (deleting the 500 rows)?
  *   *Answer:* Probably not. Deleting 50% of your data is too much loss. You should use **Imputation** (perhaps using the average year built for that specific neighborhood).
  
  **3. The Unbalanced Metric:**
  You built a fraud detection model. It has 99.9% accuracy, but it missed every single actual fraud case. 
  *   Is the model successful?
  *   What should you change? (Answer: Switch from "Accuracy" to "Recall" as your primary metric).
  
  ***
- # Summary of Lecture 2
  We have moved from the "What" to the "How." You now know:
  1.  **Attributes** have types (Nominal, Ordinal, Interval, Ratio) that limit what math you can do.
  2.  **Datasets** have domains (Record, Transaction, Time-Series) that determine their structure.
  3.  **Data Quality** (GIGO) is the primary constraint on your success.
  4.  **Preparation** (handling missing values, duplicates, and imbalance) takes 80% of the work.