## 1. The Two Main Approaches
Data mining tasks generally fall into one of two buckets based on the goal and the data available.
- ### A. Predictive Tasks (Supervised Learning)
  *   **The Goal:** Predict the value of a specific attribute (target/label) based on the values of other attributes (features).
  *   **The Data:** Requires **Labeled Data**. You need a historical dataset where you already know the answer (the "Ground Truth") to train the algorithm.
  *   **Key Tasks:** Classification, Regression.
- ### B. Descriptive Tasks (Unsupervised Learning)
  *   **The Goal:** Derive patterns (correlations, trends, clusters) that summarize the underlying relationships in the data. There is no single "correct" answer to predict.
  *   **The Data:** Uses **Unlabeled Data**. You just have the data, and you want the algorithm to tell you what the structure looks like.
  *   **Key Tasks:** Clustering, Association Analysis, Anomaly Detection.
  
  ---
- ## 2. Deep Dive: Predictive Tasks
- ### Classification
  *   **Definition:** Assigning data points into discrete, predefined categories (classes).
  *   **The Math/Intuition:** The algorithm learns a **Decision Boundary** (a line, curve, or hyperplane) that separates the classes in the feature space.
  *   **Types:**
    *   *Binary Classification:* Only two classes (e.g., Spam vs. Not Spam, Tumor Benign vs. Malignant).
    *   *Multi-class Classification:* More than two classes (e.g., Classifying news into Sports, Politics, Finance).
  *   **Real-World Nuance:** A common challenge here is **Class Imbalance** (e.g., in fraud detection, 99.9% of transactions are legit, and only 0.1% are fraud. A model that guesses "legit" every time is 99.9% accurate but useless).
- ### Regression
  *   **Definition:** Predicting a continuous numerical value rather than a category.
  *   **The Math/Intuition:** Instead of a separator line, we try to fit a **Trend Line** (or curve) that passes as close as possible to all data points.
  *   **Examples:** Predicting stock prices, temperature, or the number of seconds a user will watch a video.
  *   **Distinction:** If you are predicting "Will the house sell? (Yes/No)" → **Classification**. If you are predicting "What price will the house sell for? ($500k)" → **Regression**.
  
  ---
- ## 3. Deep Dive: Descriptive Tasks
- ### Clustering
  *   **Definition:** Grouping data points so that points in the same group are similar, and points in different groups are dissimilar.
  *   **Key Metrics (Implied in Slide 26):**
    *   **Intra-cluster distance:** We want this **minimized** (points inside the cluster should be tight).
    *   **Inter-cluster distance:** We want this **maximized** (clusters should be far apart).
  *   **Hard vs. Soft Clustering:**
    *   *Hard:* A point belongs to exactly one cluster.
    *   *Soft (Fuzzy):* A point has a probability of belonging to multiple clusters (e.g., a movie might be 70% Action, 30% Comedy).
- ### Association Analysis
  *   **Definition:** Discovering rules that describe items that occur together frequently.
  *   **The Classic Example:** Market Basket Analysis. "People who buy Diapers also buy Beer." (A famous urban legend in Data Mining).
  *   **Key Concepts (Fill-in):**
    *   *Support:* How often does the rule happen? (e.g., 5% of all transactions have both Bread and Milk).
    *   *Confidence:* If I buy Bread, how likely am I to buy Milk? (Conditional Probability).
- ### Anomaly/Outlier Detection
  *   **Definition:** Finding points that deviate significantly from the "Normal."
  *   **The Challenge:** Defining "Normal." In some cases, anomalies are errors (bad sensor data) that should be removed. In other cases, anomalies are the gold mine (Credit Card Fraud, Cyber Intrusion).
  
  ---
- ## 4. Traditional vs. Modern Methods
  The course emphasizes that while Deep Learning (Modern) is powerful, Traditional methods are not obsolete.
  
  | Feature | Traditional (Decision Trees, KNN, SVM) | Modern (Deep Learning, LLMs) |
  | :--- | :--- | :--- |
  | **Data Req.** | Works well with small/medium data | Needs massive datasets (Big Data) |
  | **Interpretability** | **High** (White Box). You can explain *why* a loan was rejected. | **Low** (Black Box). Hard to explain the internal weights. |
  | **Resources** | Low computational cost (can run on a laptop) | High cost (requires GPUs) |
  | **Use Case** | Tabular data, simple baselines | Images, Text, Complex patterns |
  
  **Takeaway:** Always start with a simple traditional model (a "Baseline"). Only move to complex Deep Learning if the simple model isn't good enough.
  
  ---
- # Review Questions (Self-Check)
  
  1.  **Identify the Task:** A bank wants to decide if they should give a loan to a customer (Yes/No). Is this Classification, Regression, or Clustering? Is it Supervised or Unsupervised?
  2.  **Identify the Task:** A marketing team wants to find three distinct "personas" or customer groups in their sales data to target with different ads, but they don't know what the groups are yet. What task is this?
  3.  **Critical Thinking:** Why is "Association Analysis" considered Unsupervised?
  
  ***