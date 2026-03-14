## 1. Feature Selection (Reducing Columns)
**The Goal:** Identify and remove attributes that do not contribute to the "signal" of the model.

*   **The Danger of Irrelevant Features:**
  *   If you add a "Random Value" column to a dataset, algorithms like Decision Trees may accidentally treat that random noise as a real pattern. This causes the model to **overfit**, leading to poor performance on new data.
*   **Target 1: Redundant Features:**
  *   These are features that are highly correlated. 
  *   *Example:* Including both "Purchase Price" and "Sales Tax Paid." Since tax is a percentage of price, they provide the exact same information. You only need one.
*   **Target 2: Irrelevant Features:**
  *   Features that have no logical connection to the target.
  *   *Example:* Using "Student ID" to predict "GPA."
- ## 2. Sampling (Reducing Rows)
  **The Goal:** Selecting a representative subset of the data to make analysis faster and cheaper.
  
  *   **The Representative Principle:** 
    *   A sample is only useful if it has the same statistical properties as the original population. 
    *   *Example:* If your population is 50% Men and 50% Women, but your sample is 90% Men, your inferences will be **biased**.
  *   **Types of Sampling:**
    1.  **Simple Random Sampling (SRS):** Every item has an equal chance of being picked.
        *   *With Replacement:* An object can be picked more than once.
        *   *Without Replacement:* Once picked, an object is removed from the "pool."
    2.  **Stratified Sampling:** 
        *   You split the data into groups (strata) first, then take a random sample from each.
        *   *Use Case:* If you have a rare class (e.g., 1% of transactions are fraud), simple random sampling might miss them entirely. Stratified sampling ensures you get a representative amount of both "Fraud" and "Legit" cases.
- ## 3. Discretization (Simplifying Values)
  **The Goal:** Converting a continuous numeric attribute (like Age) into discrete categories (like "Young," "Adult," "Senior").
  
  *   **Why Discretize?**
    *   **Algorithm Requirement:** Some algorithms (like basic Association Rule Mining) *only* work with categorical data.
    *   **Interpretability:** It is easier for humans to understand "High Temperature" than "102.4°F."
  *   **Typical Methods:**
    *   **Equal-Width Binning:** Dividing the range into $N$ intervals of the same size (e.g., 0–10, 10–20, 20–30). 
        *   *Cons:* Very sensitive to outliers.
    *   **Equal-Frequency Binning:** Ensuring each "bin" has the same number of data points.
    *   **Clustering-based:** Using an algorithm like K-means to find natural "breaks" in the data.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Identifying Redundancy:**
  A healthcare dataset contains: `Date of Birth`, `Current Age`, and `Patient Weight`. 
  *   Which feature is **redundant**? 
  *   *Answer:* `Date of Birth` or `Current Age`. Since one can be calculated from the other, having both adds no new information and increases the model's complexity unnecessarily.
  
  **2. Choosing the Sampling Method:**
  You are analyzing student satisfaction at a university. 90% of students are Undergraduates and 10% are PhD students. You want to make sure the PhD perspective is included in your 100-person study.
  *   Should you use **Simple Random Sampling** or **Stratified Sampling**?
  *   *Answer:* **Stratified Sampling**. In a random sample of 100, you might accidentally get 0 PhD students. By stratifying, you guarantee that exactly 10 PhD students are included.
  
  **3. Discretization Logic:**
  You have the following Income data: [\$10k, \$15k, \$20k, \$25k, \$200k]. 
  *   If you use **Equal-Width** binning with 2 bins, what are the ranges?
    *   *Range:* \$200k - \$10k = \$190k. Each bin is \$95k wide.
    *   *Bins:* [\$10k–\$105k] and [\$105k–\$200k].
  *   Why is this bad here?
    *   *Answer:* Four people end up in the first bin, and the outlier is alone in the second. **Equal-Frequency** would be better here to create more balanced groups.
  
  ***