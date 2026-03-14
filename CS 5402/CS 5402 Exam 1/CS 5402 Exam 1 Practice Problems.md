### **Practice Questions: Lecture 2**

**Question 1: The "Algorithm-First" Fallacy**
You are analyzing a dataset of patients at a hospital. The dataset contains the following columns: `Patient_ID_Number`, `Age`, `Blood_Pressure`, and `Cholesterol_Level`. You decide to apply the K-Means clustering algorithm directly to all four columns to find groups of similar patients. 
What is the fundamental mathematical flaw in this approach?

**Question 2: Terminology**
In a database of Spotify songs, the rows represent individual songs, and the columns represent features like "Tempo", "Genre", "Duration", and "Release Year". In standard data mining terminology:
A) What are the rows commonly called? (List at least two synonyms).
B) What are the columns commonly called? (List at least two synonyms).

**Question 3: Levels of Measurement**
Categorize each of the following attributes as **Nominal**, **Ordinal**, **Interval**, or **Ratio**:
a) A marathon runner's finishing position (1st place, 2nd place, 3rd place).
b) The temperature of a server room in Celsius.
c) A student's major (Computer Science, Biology, English).
d) The number of clicks on a web advertisement.

**Question 4: Valid Mathematical Operations**
You are working with a dataset containing an **Interval** attribute (e.g., Calendar Years like 2024, 2025). Which set of mathematical operations are legally and logically meaningful for this data type?
A) Equality (=, !=) and Order (<, >) only.
B) Equality, Order, and Addition/Subtraction (+, -).
C) Equality, Order, Addition/Subtraction, and Multiplication/Division (*, /).
D) Multiplication and Division only.

**Question 5: Identifying Data Domains**
Match the dataset scenario to its correct structural domain (Record, Transaction, Time-Series, Graph, Spatial, or Text):
a) A dataset tracking the GPS coordinates of delivery trucks.
b) A dataset mapping users and their "Friend" connections on a social media app.
c) A matrix representing the specific items bought together in thousands of grocery store receipts.
d) A log of a server's CPU temperature recorded every second for 24 hours.

**Question 6: Structured vs. Unstructured Data**
You pull data from a Web API and receive a JSON file containing nested dictionaries, tags, and lists of user information. How is this data structurally classified?
A) Structured Data
B) Semi-Structured Data
C) Unstructured Data
D) Transactional Data

**Question 7: Handling Outliers**
In a credit card fraud detection pipeline, your algorithm flags a $15,000 purchase made in a foreign country on a card that typically spends $50 a week at a local grocery store. Mathematically, this is an extreme outlier. Should you automatically remove this outlier to "clean" the data before analysis? Why or why not?

**Question 8: Missing Values**
You are preprocessing a dataset and notice that 10% of the values in the "Income" column are missing (NaN). Which of the following is generally the **least** recommended approach for handling this?
A) Replacing the missing values with the median income of the dataset.
B) Replacing the missing values with random numbers between $20,000 and $100,000.
C) Using a predictive model to estimate the missing incomes based on the user's age and education.
D) Deleting the rows that contain the missing values.

**Question 9: The Accuracy Paradox**
You are building an AI model to detect a rare genetic mutation that occurs in 0.1% of the population. You build a "lazy" model that simply predicts "No Mutation" for every single patient. 
A) What is the accuracy of your model? 
B) Why is accuracy a terrible evaluation metric for this specific dataset?

**Question 10: Synthetic Data**
Because real medical data is heavily protected by privacy laws, a hospital uses software to generate "Synthetic" (fake) patient records that emulate the statistical properties of real patients. Provide one major advantage and one major disadvantage of using Generated Data.

***
***
- ### **Answer Key**
  
  **1. The "Algorithm-First" Fallacy**
  *   **Answer:** `Patient_ID_Number` is a **Nominal** attribute. It is merely a label used for identification, not a quantitative measure. K-Means computes mathematical averages (means) to find cluster centers. Calculating the "average" of an ID number is mathematically meaningless and will distort the distances, ruining the clustering results. You must drop the ID column before running the algorithm.
  
  **2. Terminology**
  *   **A (Rows):** Data Objects (also called records, points, cases, samples, entities, or instances).
  *   **B (Columns):** Attributes (also called variables, features, fields, or dimensions).
  
  **3. Levels of Measurement**
  *   **a) Ordinal:** There is a meaningful rank/order, but the time difference between 1st and 2nd isn't necessarily the same as 2nd and 3rd.
  *   **b) Interval:** Differences make sense (20C is 10 degrees hotter than 10C), but there is no true zero (0C doesn't mean "no heat"), so ratios don't make sense.
  *   **c) Nominal:** Distinct categories with no inherent mathematical order.
  *   **d) Ratio:** It has a true zero (0 clicks means the complete absence of clicks), and ratios make sense (100 clicks is exactly twice as many as 50 clicks).
  
  **4. Valid Mathematical Operations**
  *   **Answer: B.** Interval data supports Equality, Order, and Addition/Subtraction. It does *not* support Multiplication/Division because it lacks a true zero point.
  
  **5. Identifying Data Domains**
  *   **a)** Spatial Data
  *   **b)** Graph Data
  *   **c)** Transaction Data
  *   **d)** Time-Series Data
  
  **6. Structured vs. Unstructured Data**
  *   **Answer: B (Semi-Structured Data).** JSON and XML provide hierarchy and tags to separate elements, giving it more structure than raw text, but more flexibility than a rigid row/column SQL table.
  
  **7. Handling Outliers**
  *   **Answer:** **No, you should not automatically remove it.** As stated in the slides (Slide 47), outliers fall into two cases. Case 1 is "Noise" (e.g., a broken sensor) which should be removed. Case 2 is when the outlier is the **actual goal of the analysis**. In credit card fraud, the outlier *is* the fraud. Removing it would destroy the exact signal you are trying to detect.
  
  **8. Missing Values**
  *   **Answer: B.** Filling missing values with random numbers is explicitly warned against in the slides (Slide 54). It introduces artificial noise and destroys the underlying mathematical relationships the algorithm is trying to learn.
  
  **9. The Accuracy Paradox**
  *   **A)** 99.9%
  *   **B)** This is a highly **imbalanced dataset**. Accuracy is misleading here because a 99.9% accuracy score sounds fantastic, but the model has actually failed its only task: it identified 0% of the sick patients. You must use metrics like Precision, Recall, or F1-Score instead.
  
  **10. Synthetic Data**
  *   **Advantage:** Efficiency/Cost-effective to collect massive amounts of data; completely protects the privacy of real users/patients.
  *   **Disadvantage:** It is often very difficult to statistically validate if the fake data perfectly captures all the complex, hidden structures of the real world. (Also acceptable: Can be misused for deepfakes/misinformation).
  
  ***
- ---
- ---
- ### **Practice Questions: Lecture 3**
  
  **Question 1: The NaN Propagation Problem**
  You have a dataset representing the annual income of 500 users, but 10 users declined to answer, leaving `NaN` (Not a Number) in those rows. If you attempt to calculate the average income using `numpy.mean()` without handling the missing values first, what will the result be?
  A) The average of the 490 valid incomes.
  B) Zero.
  C) NaN.
  D) An error will crash the program.
  
  **Question 2: Outlier Math (The IQR Rule)**
  You are analyzing the age of customers in a store. You calculate the 25th percentile ($Q1$) to be 24 years old, and the 75th percentile ($Q3$) to be 40 years old. 
  A) Calculate the Interquartile Range (IQR).
  B) Calculate the exact Upper Bound and Lower Bound for outliers.
  C) A customer comes in who is 65 years old. According to the Boxplot Rule, is this customer considered a mathematical outlier?
  
  **Question 3: Feature Scaling Choice**
  You are predicting house prices. One feature is "Number of Bedrooms" (ranges from 1 to 5). Another feature is "Lot Size in Square Feet" (mostly ranges from 2,000 to 10,000, but one massive estate is 250,000 sq ft). 
  Which scaling method is **more appropriate** for this dataset, and why?
  A) Min-Max Scaling (Normalization)
  B) Z-Score Scaling (Standardization)
  
  **Question 4: Min-Max Calculation**
  You are applying Min-Max Scaling to a student's test scores. The minimum score in the class was a 40, and the maximum score was a 90. If a student scored a 75, what will their scaled score be?
  
  **Question 5: Feature Construction vs. Selection**
  Classify the following actions as either **Feature Construction** or **Feature Selection**:
  1. Removing the "Customer ID" column before training the model.
  2. Taking a "Height" column and a "Weight" column and combining them into a single "BMI" column.
  3. Dropping the "Sales Tax" column because you already have the "Total Price" column.
  4. Converting a "Timestamp" column into a boolean "Is_Weekend" column.
  
  **Question 6: Sampling Strategies**
  You are conducting a medical study on a dataset of 10,000 patients. 9,500 of them are healthy, and 500 have a specific disease. You want to draw a smaller sample of 1,000 patients to quickly train a baseline model, but you want to guarantee that exactly 50 of the sampled patients have the disease (maintaining the 5% ratio). 
  Which sampling technique MUST you use?
  A) Simple Random Sampling with replacement
  B) Simple Random Sampling without replacement
  C) Stratified Sampling
  D) Discretization
  
  **Question 7: Categorical Encoding**
  You are preprocessing data for a machine learning model. You have two categorical features: "T-Shirt Size" (Small, Medium, Large) and "Car Color" (Red, Blue, Green, Silver). 
  A) Which encoding method should you use for "T-Shirt Size"?
  B) Which encoding method should you use for "Car Color"?
  C) What is the specific "Warning / Danger" of using Label Encoding on the "Car Color" feature?
  
  **Question 8: The "Golden Rule" of Pipelines**
  A junior data scientist writes the following code for their machine learning pipeline:
  1. Load Data
  2. Apply Z-score Standardization to the whole dataset.
  3. Split the data into 80% Training and 20% Testing.
  4. Train the model.
  Identify the fatal flaw in this pipeline. What is this mistake called, and how do you fix it?
  
  **Question 9: The Impact of Unscaled Data**
  You train a K-Nearest Neighbors (KNN) classifier to predict loan defaults based on "Income" (which ranges in the tens of thousands) and "Credit History Length" (which ranges from 1 to 10 years). You forget to scale the features. How will the algorithm behave?
  A) It will crash.
  B) The "Income" feature will completely dominate the distance calculations, effectively making the model ignore "Credit History Length."
  C) The "Credit History Length" will dominate because the numbers are smaller.
  D) The algorithm will automatically adjust for the scale difference.
  
  **Question 10: Discretization**
  You have a continuous feature for "Customer Age" (e.g., 18, 24, 35, 42...). You decide to group these into three bins: "Young (18-29)", "Adult (30-49)", and "Senior (50+)". 
  What is the name of this preprocessing technique, and give one reason why you might do this.
  
  ***
  ***
- ### **Answer Key**
  
  **1. The NaN Propagation Problem**
  *   **Answer: C (NaN).** Mathematical operations in libraries like NumPy naturally propagate NaNs. If even one value is missing, the sum or mean of the whole column becomes NaN. This forces us to either remove or impute missing data before passing it to algorithms.
  
  **2. Outlier Math (The IQR Rule)**
  *   **A) IQR:** $40 - 24 = \mathbf{16}$
  *   **B) Lower Bound:** $Q1 - (1.5 \times IQR) = 24 - 24 = \mathbf{0}$. 
    **Upper Bound:** $Q3 + (1.5 \times IQR) = 40 + 24 = \mathbf{64}$.
  *   **C) Yes.** Because 65 is strictly greater than the Upper Bound of 64, it is mathematically flagged as an outlier.
  
  **3. Feature Scaling Choice**
  *   **Answer: B (Z-Score Standardization).** Because there is a massive, extreme outlier in the dataset (the 250,000 sq ft estate), Min-Max scaling would "squish" 99% of the normal houses into a tiny range between 0.0 and 0.05, destroying the variance. Z-score scaling is much less sensitive to extreme outliers.
  
  **4. Min-Max Calculation**
  *   **Calculation:** $x_{scaled} = \frac{x - x_{min}}{x_{max} - x_{min}}$
  *   $x_{scaled} = \frac{75 - 40}{90 - 40} = \frac{35}{50} = \mathbf{0.7}$.
  
  **5. Feature Construction vs. Selection**
  *   1. **Feature Selection** (Specifically, removing an irrelevant feature).
  *   2. **Feature Construction** (Creating a new representation).
  *   3. **Feature Selection** (Removing a redundant feature).
  *   4. **Feature Construction** (Encoding domain knowledge to make patterns easier to learn).
  
  **6. Sampling Strategies**
  *   **Answer: C (Stratified Sampling).** Simple random sampling relies on chance; you might accidentally pull 0 sick patients or 100 sick patients. Stratified sampling partitions the data first, ensuring the exact representative proportions are maintained.
  
  **7. Categorical Encoding**
  *   **A)** Label Encoding (Assigning 0, 1, 2). This is safe because "Small, Medium, Large" is an *Ordinal* variable with a meaningful rank.
  *   **B)** One-Hot Encoding (Creating a binary 0/1 column for every color).
  *   **C)** If you Label Encode nominal data (Red=0, Blue=1, Green=2), the algorithm will mathematically assume that "Green is greater than Red" and that "Blue is the exact average of Red and Green," which is total nonsense and will confuse the model.
  
  **8. The "Golden Rule" of Pipelines**
  *   **The Flaw:** Normalizing *before* the train/test split.
  *   **What it's called:** **Data Leakage**. By calculating the mean/std on the *whole* dataset, information about the test set "leaked" into the training phase, rendering the evaluation unfair/invalid.
  *   **The Fix:** You must split the data *first*. Then, fit the scaler ONLY on the Training data, and use those specific parameters to transform both the train and test sets.
  
  **9. The Impact of Unscaled Data**
  *   **Answer: B.** Because algorithms like KNN use Euclidean distance, differences in Income (e.g., $50,000 vs $60,000) will result in huge squared differences ($10,000^2$), while differences in Credit History (e.g., 2 vs 5 years) will result in tiny squared differences ($3^2$). The algorithm will practically ignore the small-scale feature.
  
  **10. Discretization**
  *   **Name:** Discretization (or Binning).
  *   **Why do it?:** 1) It simplifies the data and reduces noise/complexity. 2) It enables interpretability. 3) Certain algorithms (like basic Decision Trees or Association Rule Mining) often work better with or strictly require categorical/discrete inputs. 
  
  ***
- ---
- ---
- ### **Practice Questions: Lectures 4 & 5**
  
  **Question 1: Distance Metrics (Hand-Calculating)**
  You are comparing two user profiles based on two continuous features. User A is at coordinates $(0, 3)$ and User B is at coordinates $(4, 0)$. 
  A) Calculate the **L1 Distance (Manhattan)** between User A and User B.
  B) Calculate the **L2 Distance (Euclidean)** between User A and User B.
  
  **Question 2: Choosing the Right Metric (Short Answer)**
  "Similarity" is not universal; it depends heavily on how the data is represented. Which similarity/distance metric from the lectures should you use for the following data types?
  A) Text data (where you care about the direction/angle of word frequencies, not the magnitude).
  B) Binary asymmetrical data (e.g., Shopping baskets, where you only care about items that are present, not the thousands of items missing from both baskets).
  
  **Question 3: Model Validation (True/False)**
  **True or False:** If you want to find the best hyperparameters for your model (e.g., the optimal depth of a Decision Tree), you should repeatedly adjust the parameters until you achieve the highest possible accuracy on the Test Set.
  
  **Question 4: Cross-Validation Math (Hand-Calculating)**
  You are training a model on a dataset containing 1,000 samples. You decide to use **5-fold Cross-Validation**. 
  A) In any single iteration (fold) of the cross-validation process, how many samples are used for training, and how many are used for testing?
  B) How many times in total will a specific data point (e.g., sample #42) be used as a test sample across the entire 5-fold process?
  
  **Question 5: Overfitting vs. Underfitting (Open-Ended)**
  You train a classification model. When you evaluate it, you find that the Training Error is extremely low (1%), but the Validation Error is very high (45%). 
  A) What is the formal name for this phenomenon? 
  B) Geometrically or conceptually, what has the model done wrong? 
  C) What is one way to fix this?
  
  **Question 6: Metric Trade-offs (Multiple Choice)**
  You are building an AI model for an airport security scanner that flags bags for manual inspection if it suspects a weapon is present.
- A **False Positive** means a clean bag is manually searched (annoying, slows down the line).
- A **False Negative** means a weapon makes it onto the plane (catastrophic).
  Which metric MUST you optimize to ensure safety?
  A) Precision
  B) Recall
  C) Accuracy
  D) Specificity
  
  **Question 7: Confusion Matrix & Metrics (Hand-Calculating)**
  You test a binary classifier on 100 samples and obtain the following confusion matrix:
  *   True Positives (TP) = 40
  *   False Positives (FP) = 10
  *   False Negatives (FN) = 10
  *   True Negatives (TN) = 40
  Calculate the following metrics (write them as decimals or percentages):
  A) Accuracy
  B) Precision
  C) Recall
  D) F1-Score
  
  **Question 8: The Accuracy Paradox (Short Answer)**
  In a credit card transaction dataset, 99% of transactions are legitimate and 1% are fraudulent. You build a model that predicts "legitimate" for every single transaction without even looking at the features. 
  What is the accuracy of this model, and why is accuracy a poor choice of metric for this dataset?
  
  **Question 9: Interpreting the ROC Curve (Open-Ended)**
  Looking at an ROC Curve (which plots True Positive Rate vs. False Positive Rate):
  A) What does the bottom-left coordinate $(0,0)$ represent in terms of the model's threshold and predictions?
  B) What does the top-left coordinate $(0,1)$ represent?
  
  **Question 10: Area Under the Curve (Multiple Choice)**
  You train a binary classifier, plot the ROC curve, and calculate the AUC (Area Under the Curve) to be **0.25**. What does this indicate about your model?
  A) The model is excellent but slightly overfit.
  B) The model is equivalent to a random coin flip.
  C) The model is worse than random guessing (you should flip its predictions).
  D) The model is mathematically impossible.
  
  **Question 11: ROC and Imbalanced Data (True/False)**
  **True or False:** The ROC curve is highly sensitive to class imbalance. If you artificially increase the number of negative samples in your test set by 10x, the ROC curve will shift dramatically downward.
  
  ***
  ***
- ### **Answer Key**
  
  **1. Distance Metrics**
  *   **A) L1 (Manhattan):** $\sum |x_i - y_i| = |0 - 4| + |3 - 0| = 4 + 3 = \mathbf{7}$.
  *   **B) L2 (Euclidean):** $\sqrt{(0 - 4)^2 + (3 - 0)^2} = \sqrt{16 + 9} = \sqrt{25} = \mathbf{5}$.
  
  **2. Choosing the Right Metric**
  *   **A)** **Cosine Similarity / Distance.** (It measures the angle, ignoring the magnitude of the vectors).
  *   **B)** **Jaccard Similarity.** (It focuses strictly on the intersection of present items, ignoring mutual absences).
  
  **3. Model Validation**
  *   **False.** This is a severe violation of the fundamental rule of machine learning. You must use a **Validation Set** to tune hyperparameters. The Test Set must remain completely "unseen" until the very end to give an honest estimate of real-world performance. If you tune based on the Test Set, you leak data and overfit to the test set.
  
  **4. Cross-Validation Math**
  *   **A)** $1000 / 5 = 200$. In each iteration, **800** samples are used for training, and **200** samples are used for testing.
  *   **B)** Exactly **1 time**. In K-fold CV, every sample gets to be in the test set exactly once.
  
  **5. Overfitting vs. Underfitting**
  *   **A)** **Overfitting** (High Variance).
  *   **B)** The model is too complex. It has memorized the random noise, outliers, and coincidental regularities of the training data rather than capturing the general underlying pattern. 
  *   **C)** Provide more training data, reduce the model's complexity (e.g., prune the decision tree), or remove noisy features.
  
  **6. Metric Trade-offs**
  *   **Answer: B (Recall).** Recall (Completeness) answers the question: "Out of all the *actual* weapons, how many did we find?" You want Recall to be as close to 100% as possible, even if it means accepting a higher rate of False Positives (which lowers Precision).
  
  **7. Confusion Matrix & Metrics**
  *   **A) Accuracy:** $(40 + 40) / 100 = 80 / 100 = \mathbf{0.80}$ (80%)
  *   **B) Precision:** $TP / (TP + FP) = 40 / (40 + 10) = 40 / 50 = \mathbf{0.80}$ (80%)
  *   **C) Recall:** $TP / (TP + FN) = 40 / (40 + 10) = 40 / 50 = \mathbf{0.80}$ (80%)
  *   **D) F1-Score:** Since Precision and Recall are identical, the harmonic mean is also the same. $2 \times (0.8 \times 0.8) / (0.8 + 0.8) = \mathbf{0.80}$ (80%).
  
  **8. The Accuracy Paradox**
  *   **Accuracy:** **99%**.
  *   **Why it's poor:** For heavily imbalanced datasets, accuracy hides the model's complete failure on the minority class. The model correctly identified 0% of the actual fraud (Recall = 0%).
  
  **9. Interpreting the ROC Curve**
  *   **A) Point (0,0):** This is a threshold of 1.0 (100%). The model predicts "Negative" for everything. Therefore, it makes zero False Positive errors (FPR=0), but it also catches zero True Positives (TPR=0).
  *   **B) Point (0,1):** This is the "Perfect Classifier." It catches 100% of the positive cases (TPR=1.0) without making a single false alarm (FPR=0.0).
  
  **10. Area Under the Curve**
  *   **Answer: C.** An AUC of 0.5 is a random guess (the diagonal line). Anything *below* 0.5 means the model is actively learning the wrong thing. If you simply invert all of its predictions (change 0s to 1s and 1s to 0s), the AUC would jump to 0.75!
  
  **11. ROC and Imbalanced Data**
  *   **False.** The ROC curve is highly **robust** (insensitive) to class imbalance. Because TPR only calculates over actual positives, and FPR only calculates over actual negatives, changing the ratio of positives to negatives in the dataset does not mathematically alter the rates. 
  
  ***
- ---
- ---
- ### **Practice Questions: Lecture 6**
  
  **Question 1: The Law of Total Probability (Hand-Calculating)**
  You have two boxes of colored folders. 
  *   Box A contains 3 Red folders and 1 Blue folder.
  *   Box B contains 2 Red folders and 2 Blue folders.
  If you pick a box entirely at random (50% chance for each) and then pull out one folder at random, what is the probability that the folder is **Red**?
  
  **Question 2: Bayes' Theorem (Hand-Calculating)**
  A factory uses an AI scanner to detect defective microchips. 
  *   **Prior:** 2% of all microchips produced are defective ($P(Defect) = 0.02$).
  *   **Likelihood:** If a chip is defective, the scanner correctly flags it 90% of the time ($P(Flag | Defect) = 0.90$).
  *   **False Positive:** If a chip is perfectly fine, the scanner accidentally flags it 5% of the time ($P(Flag | Normal) = 0.05$).
  A chip passes through the scanner and is **Flagged**. Use Bayes' Theorem to calculate the exact probability that this chip is *actually* defective.
  
  **Question 3: Joint and Marginal Probabilities (Hand-Calculating)**
  Given the following joint probability distribution table for two discrete random variables, $X$ and $Y$:
  
  | | Y = 0 | Y = 1 |
  | :--- | :---: | :---: |
  | **X = 0** | 0.10 | 0.30 |
  | **X = 1** | 0.40 | 0.20 |
  
  A) Calculate the marginal probability $P(X = 0)$.
  B) Calculate the conditional probability $P(Y = 1 | X = 0)$.
  
  **Question 4: Independence (True/False)**
  **True or False:** If the Mutual Information between two variables is zero ($I(X; Y) = 0$), it mathematically guarantees that event X and event Y are completely independent.
  
  **Question 5: Entropy (Multiple Choice)**
  Entropy measures the uncertainty of a random variable. Which of the following probability distributions for a coin toss would yield the **highest** possible entropy?
  A) P(Heads) = 1.0, P(Tails) = 0.0
  B) P(Heads) = 0.9, P(Tails) = 0.1
  C) P(Heads) = 0.5, P(Tails) = 0.5
  D) P(Heads) = 0.1, P(Tails) = 0.9
  
  **Question 6: KL Divergence (True/False)**
  **True or False:** Relative Entropy (KL Divergence) is a symmetric distance metric. In other words, the divergence from distribution P to Q is exactly the same as the divergence from Q to P: $D_{KL}(P || Q) = D_{KL}(Q || P)$.
  
  **Question 7: The "Naïve" Assumption (Open-Ended)**
  The Naïve Bayes classifier makes a very bold assumption about the data in order to function: it assumes that all features are **conditionally independent** given the class label.
  A) Explain what this means in plain English using an example (e.g., predicting if an email is spam based on the words "Free" and "Money").
  B) Why is it absolutely necessary to make this assumption? (What mathematical problem does it solve?)
  
  **Question 8: Naïve Bayes Classification (Hand-Calculating)**
  You are building a Naïve Bayes classifier to predict if someone will Play Tennis ($Y = \text{Yes}$ or $\text{No}$) based on one feature: Outlook ($X = \text{Sunny}$ or $\text{Rainy}$).
  Your training data has 10 days total:
  *   Total "Yes" days = 6. Out of those 6 days, 2 were Sunny.
  *   Total "No" days = 4. Out of those 4 days, 3 were Sunny.
  
  Today is **Sunny**. Calculate the proportional posterior probabilities (Prior $\times$ Likelihood) for both "Yes" and "No", and state which class the model will predict.
  
  **Question 9: Handling Continuous Features (Short Answer)**
  Naïve Bayes easily handles categorical features (like "Sunny" or "Rainy") by counting frequencies. How does the algorithm handle a **continuous** feature, like "Temperature = 72.5 degrees"? 
  
  **Question 10: The Log-Probability Trick (Short Answer)**
  In a real-world Naïve Bayes implementation (like a spam filter looking at 1,000 words), we usually take the **logarithm** of the probabilities and add them together, rather than multiplying the raw probabilities directly. Why is this necessary for numerical stability?
  
  ***
  ***
- ### **Answer Key**
  
  **1. The Law of Total Probability**
  *   **Formula:** $P(Red) = P(Red | BoxA)P(BoxA) + P(Red | BoxB)P(BoxB)$
  *   **Calculation:** $(3/4 \times 0.5) + (2/4 \times 0.5) = (0.75 \times 0.5) + (0.50 \times 0.5) = 0.375 + 0.250 = \mathbf{0.625}$ (or 62.5%).
  
  **2. Bayes' Theorem**
  *   **Numerator (Likelihood $\times$ Prior):** $P(Flag|Defect) \times P(Defect) = 0.90 \times 0.02 = 0.018$
  *   **Denominator (Total Probability of a Flag):** $(0.90 \times 0.02) + (0.05 \times 0.98) = 0.018 + 0.049 = 0.067$
  *   **Posterior:** $0.018 / 0.067 = \mathbf{0.2686}$ (or roughly 26.9%).
  *   *Note:* Even though the scanner is 90% accurate, a flagged chip only has a 27% chance of actually being defective because normal chips are so overwhelmingly common.
  
  **3. Joint and Marginal Probabilities**
  *   **A) Marginal $P(X=0)$:** Sum the entire row where $X=0$. $0.10 + 0.30 = \mathbf{0.40}$.
  *   **B) Conditional $P(Y=1 | X=0)$:** $P(X=0 \cap Y=1) / P(X=0)$. The intersection cell is 0.30. The denominator is 0.40. $0.30 / 0.40 = \mathbf{0.75}$.
  
  **4. Independence**
  *   **True.** Mutual Information measures the amount of uncertainty in X that is removed by knowing Y. If it is exactly 0, knowing Y gives you zero information about X, meaning they are completely independent.
  
  **5. Entropy**
  *   **Answer: C.** A 50/50 split (uniform distribution) represents maximum uncertainty. You have no idea what the coin will land on, so the entropy peaks at 1.0 bits. The other options are highly predictable (low entropy).
  
  **6. KL Divergence**
  *   **False.** KL Divergence is strictly **asymmetric**. The amount of information lost when approximating distribution P with distribution Q is not the same as approximating Q with P. (This is why it is technically a "Divergence" and not a true distance "Metric").
  
  **7. The "Naïve" Assumption**
  *   **A)** It means that knowing one feature doesn't give you any hints about the other features, *assuming you already know the class*. For example, if you already know an email is Spam, Naive Bayes assumes that seeing the word "Free" doesn't change the probability of also seeing the word "Money." (In reality, they usually appear together, making this assumption "naive").
  *   **B)** It solves the problem of **parameter explosion**. If features aren't independent, you have to calculate the probability of every possible combination of features ($2^d$ parameters). By assuming independence, you just calculate them individually and multiply them together ($2d$ parameters), saving immense computational power and preventing overfitting.
  
  **8. Naïve Bayes Classification**
  *   **Priors:** $P(Yes) = 6/10 = 0.6$. $P(No) = 4/10 = 0.4$.
  *   **Likelihoods:** $P(Sunny | Yes) = 2/6 \approx 0.333$. $P(Sunny | No) = 3/4 = 0.75$.
  *   **Scores:**
    *   Score for Yes: Prior $\times$ Likelihood = $0.6 \times 0.333 = \mathbf{0.20}$
    *   Score for No: Prior $\times$ Likelihood = $0.4 \times 0.75 = \mathbf{0.30}$
  *   **Prediction:** Because $0.30 > 0.20$, the model predicts **No**.
  
  **9. Handling Continuous Features**
  *   **Answer:** It assumes the continuous values for each class follow a specific mathematical distribution (almost always a **Gaussian/Normal Distribution**). During training, it calculates the Mean ($\mu$) and Variance ($\sigma^2$) for that feature. During testing, it plugs the new value into the Gaussian Probability Density Function (PDF) equation to get the likelihood.
  
  **10. The Log-Probability Trick**
  *   **Answer:** Multiplying hundreds of tiny probability fractions (e.g., $0.01 \times 0.05 \times 0.02 \dots$) quickly results in a number so close to zero that computers physically cannot store it in memory. This is called **Arithmetic Underflow**, and the computer rounds it to exactly `0.0`. Taking the logarithm turns the multiplications into safe additions ($\log(A \times B) = \log(A) + \log(B)$), preserving the math.
  
  ***
- ---
- ---
- ### **Practice Questions: Lecture 7**
  
  **Question 1: Geometric Intuition (Multiple Choice)**
  A standard decision tree builds decision boundaries to separate classes. In a 2D feature space, what geometric shapes do these boundaries take?
  A) Diagonal lines based on feature correlation.
  B) Smooth, non-linear curves.
  C) Axis-aligned horizontal and vertical lines.
  D) Concentric circles around the cluster centroids.
  
  **Question 2: Tree Terminology (Short Answer)**
  In a decision tree, what do we call a node that has incoming edges but no outgoing edges, and what is its primary function?
  
  **Question 3: Entropy Calculation (Hand-Calculating)**
  You are evaluating a node in a decision tree. The node contains exactly 8 samples: 4 belong to Class A and 4 belong to Class B. 
  A) Calculate the Entropy of this node. (Hint: $\log_2(0.5) = -1$)
  B) What does this specific numerical value signify about the "purity" of the node?
  
  **Question 4: Information Gain (Hand-Calculating)**
  You have a parent node with **Entropy = 1.0** (it contains 10 samples total: 5 Yes, 5 No). You propose splitting this node using the feature "Has_Job". The split perfectly divides the data into two child nodes:
  *   **Child 1 (Has_Job = Yes):** Contains 6 samples (All 6 are "Yes").
  *   **Child 2 (Has_Job = No):** Contains 4 samples (All 4 are "No").
  Calculate the Information Gain for this split. Show your work.
  
  **Question 5: The Bias of Information Gain (Open-Ended)**
  If you use standard Information Gain to choose the root node of a decision tree on a dataset of hospital patients, why might the tree mathematically select the "Patient_SSN" (Social Security Number) column as the best feature? What mathematical modification (used in the C4.5 algorithm) was invented to fix this flaw?
  
  **Question 6: Handling Continuous Attributes (Short Answer)**
  Decision trees natively handle categorical data (like Color: Red, Blue, Green) easily. If a feature is continuous (e.g., Temperature: `60, 70, 80, 90`), explain the algorithmic step-by-step process the decision tree uses to convert this into a binary split.
  
  **Question 7: Overfitting and Pruning (True/False)**
  **True or False:** Post-pruning a decision tree involves growing the tree to its maximum possible depth on the training data, and then evaluating the subtrees using the **Test Set** to decide which branches to cut off.
  
  **Question 8: Pre-Pruning Disadvantages (Multiple Choice)**
  Which of the following is a major disadvantage of Pre-Pruning (e.g., stopping the tree when Information Gain is less than 0.01) compared to Post-Pruning?
  A) It takes significantly longer to compute than post-pruning.
  B) It is susceptible to the "Horizon Effect," where it stops before discovering a highly informative split further down the tree.
  C) It requires a much larger validation dataset to work.
  D) It only works if you are using the Gini Index instead of Entropy.
  
  **Question 9: Comparing Algorithms (Short Answer)**
  The **CART** (Classification and Regression Trees) algorithm differs from older algorithms like **ID3** in two major ways regarding the *shape* of its splits and the *metric* it uses to evaluate impurity. Name these two differences.
  
  **Question 10: Model Selection & Interpretability (Open-Ended)**
  You are working for a bank building a model to approve or deny mortgage applications. Your team builds a Deep Neural Network with 95% accuracy, and a Decision Tree with 88% accuracy. Why might the bank's compliance and legal department mandate that you deploy the Decision Tree instead of the Neural Network?
  
  ***
  ***
- ### **Answer Key**
  
  **1. Geometric Intuition**
  *   **Answer: C.** Decision trees create rules like "IF X > 5", which translates to drawing straight vertical or horizontal (axis-aligned) lines. They cannot draw diagonal lines (e.g., "IF X + Y > 5"). 
  
  **2. Tree Terminology**
  *   **Answer:** It is called a **Leaf Node** (or Terminal Node). Its primary function is to make the final prediction (assign a Class Label) to the data point that reaches it.
  
  **3. Entropy Calculation**
  *   **A) Calculation:** 
    $p(A) = 4/8 = 0.5$
    $p(B) = 4/8 = 0.5$
    $H = -[0.5 \log_2(0.5) + 0.5 \log_2(0.5)] = -[-0.5 - 0.5] = \mathbf{1.0}$.
  *   **B) Meaning:** An entropy of 1.0 is the maximum possible entropy for a binary classification. It means the node is perfectly mixed (50/50), representing maximum uncertainty and minimum purity.
  
  **4. Information Gain**
  *   **Step 1:** Parent Entropy is given as **1.0**.
  *   **Step 2:** Calculate Child Entropies. 
    *   Child 1 is 100% "Yes". Entropy = **0.0**.
    *   Child 2 is 100% "No". Entropy = **0.0**.
  *   **Step 3:** Calculate Weighted Average of Children. 
    *   $(6/10 \times 0.0) + (4/10 \times 0.0) =$ **0.0**.
  *   **Step 4:** $IG = Parent - Children$
    *   $IG = 1.0 - 0.0 =$ **1.0**. (This is a perfect split!).
  
  **5. The Bias of Information Gain**
  *   **Why it picks SSN:** Standard Information Gain favors features with many unique values. SSN is unique for every row. Splitting on it creates $N$ leaf nodes that each contain exactly 1 patient. Because a node with 1 patient is 100% pure (Entropy=0), the Information Gain is mathematically maximized, even though it's useless for predicting future patients.
  *   **The Fix:** **Information Gain Ratio (IGR)**. It divides the standard Gain by the "SplitInfo" (the entropy of the split itself), heavily penalizing attributes that fragment the data into too many tiny pieces.
  
  **6. Handling Continuous Attributes**
  *   **Answer:** 
    1. It sorts the data values from lowest to highest (`60, 70, 80, 90`).
    2. It calculates the midpoints between adjacent values (`65, 75, 85`).
    3. It treats every midpoint as a potential threshold (e.g., $Temp < 65$ vs $Temp \ge 65$).
    4. It calculates the Information Gain for *every single midpoint* and chooses the one that yields the highest gain.
  
  **7. Overfitting and Pruning**
  *   **Answer: False.** You evaluate the subtrees using a **Validation Set**. You must *never* use the Test Set to make decisions about model architecture or pruning, as that leads to data leakage.
  
  **8. Pre-Pruning Disadvantages**
  *   **Answer: B (The Horizon Effect).** A split might initially look useless (yielding almost 0 information gain), but that split might be necessary to unlock a massive, perfect split on the next level down. Pre-pruning stops blindly and misses this. Post-pruning grows the whole tree to map everything out, then cuts away the true garbage safely.
  
  **9. Comparing Algorithms**
  *   **Difference 1 (Shape):** CART restricts all splits to be strictly **Binary** (only two child nodes per split), whereas ID3 performs multi-way splits (one branch for every category).
  *   **Difference 2 (Metric):** CART uses the **Gini Index** for impurity, whereas ID3 uses **Entropy**.
  
  **10. Model Selection & Interpretability**
  *   **Answer:** Interpretability / The Right to Explanation. A Deep Neural Network is a "Black Box" model; it cannot explain *why* it denied someone's loan. A Decision Tree is a "White Box" model; you can trace the exact logical path (e.g., "Income was < 50k and Credit Score < 600") to explain the decision to a regulator or the customer. In regulated industries, explainability is often legally required, trumping raw accuracy.
  
  ***
- ---
- ---
- ### **Practice Questions: Lecture 8**
  
  **Question 1: Convergence (True/False)**
  **True or False:** If a dataset contains overlapping classes (i.e., it is *not* linearly separable), the standard Perceptron algorithm will eventually converge to a "best-fit" line that makes the minimum possible number of mistakes.
  
  **Question 2: Gradient Descent Dynamics (Multiple Choice)**
  You are training a machine learning model using Gradient Descent. You plot the loss over time and notice that it decreases for the first few steps, but then starts oscillating wildly and eventually explodes (diverges) towards infinity. What is the most likely cause?
  A) The learning rate ($\eta$) is too small.
  B) The learning rate ($\eta$) is too large.
  C) The loss function is perfectly convex.
  D) The algorithm is stuck in a local minimum.
  
  **Question 3: The Role of Bias (Short Answer)**
  In the Perceptron decision boundary equation $w^T x + b = 0$, what geometric role does the bias term $b$ play? What would happen if $b$ was forced to be 0 for a dataset where all points (both positive and negative) live in the far upper-right quadrant?
  
  **Question 4: Gradient Descent (Hand-Calculating)**
  You want to find the minimum of the function $f(x) = x^2 - 6x + 2$. 
  You use Gradient Descent with a learning rate of $\eta = 0.1$ and an initial guess of $x_0 = 10$. 
  Calculate the exact value of $x_1$ after **one** update iteration. Show your work.
  
  **Question 5: Perceptron Weight Update (Hand-Calculating)**
  You are training a Perceptron with a learning rate $\eta = 1$. The current weights are $w = [1, -1]^T$ and the bias is $b = 0.5$. 
  You observe a new training data point $x_i = [2, 4]^T$ with the true label $y_i = +1$.
  A) Calculate the raw prediction score and state whether the point is correctly classified or misclassified.
  B) Based on your answer to Part A, calculate the updated weights ($w_{new}$) and bias ($b_{new}$). If no update is needed, state "No update."
  
  **Question 6: Loss Functions (Multiple Choice)**
  Why doesn't the Perceptron algorithm use the intuitive "0-1 Loss" (simply counting the number of misclassified samples) to train its weights via Gradient Descent?
  A) Because 0-1 loss always leads to overfitting.
  B) Because 0-1 loss requires too much memory to compute.
  C) Because 0-1 loss is a step-function and is not differentiable, meaning the gradient is exactly zero almost everywhere.
  D) Because 0-1 loss penalizes outliers too heavily.
  
  **Question 7: The SGD Distinction (True/False)**
  **True or False:** The standard Perceptron Learning Algorithm utilizes "Batch Gradient Descent," meaning it calculates the combined gradient of all misclassified points in the entire dataset before making a single update to the weights.
  
  **Question 8: Convergence Theorem (Short Answer)**
  According to the Perceptron Convergence Theorem, the maximum number of mistakes the algorithm will make is bounded by $\frac{R^2}{\gamma^2}$. 
  Briefly explain conceptually what $R$ and $\gamma$ represent. If you have a dataset with a very "large margin," does the Perceptron converge faster or slower?
  
  **Question 9: Limitations (Open-Ended)**
  State three specific limitations of the Perceptron algorithm when compared to more advanced linear algorithms like Logistic Regression or Support Vector Machines (SVM).
  
  **Question 10: Convexity & Smoothness (Short Answer)**
  The Perceptron loss function is described mathematically as "convex but non-smooth." Explain what these two terms mean in regards to finding the minimum loss.
  
  ***
  ***
- ### **Answer Key**
  
  **1. Convergence (True/False)**
  *   **Answer: False.** The Perceptron will *never* converge if the data is not linearly separable. Because the stopping condition is reaching 0 errors, it will loop infinitely, thrashing back and forth between different hyperplanes. 
  
  **2. Gradient Descent Dynamics (Multiple Choice)**
  *   **Answer: B (The learning rate is too large).** If the steps are too massive, the algorithm overshoots the bottom of the valley, landing higher up on the opposite side, bouncing back and forth until the numbers explode to infinity (divergence).
  
  **3. The Role of Bias (Short Answer)**
  *   **Answer:** Geometrically, the bias $b$ allows the decision boundary (the line/plane) to shift away from the origin $(0,0)$. If $b$ was forced to be 0, the decision boundary would *have* to pass exactly through the origin. It would be impossible to separate classes that are grouped far away in the upper-right quadrant.
  
  **4. Gradient Descent (Hand-Calculating)**
  *   **Step 1 (Find the derivative/gradient):** $f'(x) = 2x - 6$.
  *   **Step 2 (Evaluate gradient at current position $x_0$):** $f'(10) = 2(10) - 6 = 20 - 6 = 14$.
  *   **Step 3 (Update rule):** $x_{new} = x_{old} - \eta \cdot f'(x_{old})$
  *   **Step 4 (Calculate):** $x_1 = 10 - 0.1(14) = 10 - 1.4 = \mathbf{8.6}$.
  
  **5. Perceptron Weight Update (Hand-Calculating)**
  *   **Part A (Prediction):** 
    *   Score = $w^T x_i + b = (1)(2) + (-1)(4) + 0.5 = 2 - 4 + 0.5 = \mathbf{-1.5}$.
    *   Because the score is $<0$, the model predicts Class -1. The true label is Class +1. Therefore, it is **Misclassified**.
  *   **Part B (Update):**
    *   Because it is misclassified, we update using: $w_{new} = w_{old} + \eta y_i x_i$ and $b_{new} = b_{old} + \eta y_i$.
    *   $w_{new} = [1, -1]^T + (1)(+1)[2, 4]^T = [1, -1]^T + [2, 4]^T = \mathbf{[3, 3]^T}$.
    *   $b_{new} = 0.5 + (1)(+1) = \mathbf{1.5}$.
  
  **6. Loss Functions (Multiple Choice)**
  *   **Answer: C.** Gradient Descent requires calculating a slope to know which direction is "downhill." A 0-1 step function is completely flat (slope = 0) everywhere except at the exact threshold, so the algorithm has no idea which way to adjust the weights to fix the error. 
  
  **7. The SGD Distinction (True/False)**
  *   **Answer: False.** The Perceptron uses **Stochastic Gradient Descent (SGD)**. It picks *one* misclassified sample, updates the weights based purely on that one sample, and then moves on to the next.
  
  **8. Convergence Theorem (Short Answer)**
  *   **Answer:** $R$ represents the "Radius" (the maximum distance of any data point from the origin). $\gamma$ represents the "Margin" (the distance from the decision boundary to the closest point). A dataset with a "large margin" means the classes are far apart ($\gamma$ is large). Because $\gamma$ is in the denominator, a larger margin means the bound $\frac{R^2}{\gamma^2}$ shrinks, meaning the algorithm takes **fewer steps and converges faster**.
  
  **9. Limitations (Open-Ended)**
  *   **Answer:** (Any three of the following):
    1.  It cannot solve non-linearly separable problems (like the XOR problem).
    2.  It outputs a "hard" prediction (+1 or -1) with no probability score/confidence level (unlike Logistic Regression).
    3.  It does not maximize the margin; it stops at the very first valid line it finds, which can lead to poor generalization (unlike SVM).
    4.  It is highly sensitive to feature scaling and initialization.
  
  **10. Convexity & Smoothness (Short Answer)**
  *   **Answer:** 
    *   **Convex:** The loss function is shaped like a bowl (or a "V"). This guarantees that there is only one "valley," meaning any local minimum is mathematically guaranteed to be the global minimum.
    *   **Non-smooth:** The loss function resembles a "V" (Hinge-like loss). At the exact bottom point of the V (where a point goes from misclassified to correctly classified), there is a sharp "kink" where the derivative is undefined.
  
  ***
- ---
- ---
- ### **Practice Questions: Lectures 9 & 10**
  
  **Question 1: The Sigmoid Function (Hand-Calculating)**
  You are using Logistic Regression to classify an email as Spam ($y=1$) or Not Spam ($y=0$). The model has learned the weights $w =[1.5, -0.5]^T$ and the bias $b = -1$. 
  You receive a new email represented by the feature vector $x = [2, 4]^T$.
  A) Calculate the raw linear score ($z$).
  B) Pass $z$ through the Sigmoid function and calculate the final predicted probability. (Hint: $e^0 = 1$). 
  C) Based on a standard 0.5 threshold, what is the final classification?
  
  **Question 2: Logistic Regression Update (Multiple Choice)**
  Which of the following best describes the fundamental difference between the weight update rule in Perceptron versus Logistic Regression?
  A) The Perceptron updates weights using the Sigmoid function, while Logistic Regression uses a step function.
  B) The Perceptron only updates weights when a sample is misclassified, while Logistic Regression updates weights based on the continuous error $(y - \hat{y})$ for all samples.
  C) Logistic Regression does not use Gradient Descent, whereas the Perceptron does.
  D) The Perceptron relies on calculating probabilities, while Logistic Regression relies on hard boundaries.
  
  **Question 3: SVM Geometry (True/False)**
  **True or False:** In a trained Hard-Margin SVM, if you delete a data point from the training set that is located far away from the decision boundary (i.e., it is *not* a Support Vector) and retrain the model, the resulting optimal hyperplane will shift slightly to account for the missing data.
  
  **Question 4: SVM Optimization Constraint (Short Answer)**
  When setting up the mathematical objective to maximize the SVM margin, the problem is initially scale-invariant (i.e., $w^Tx+b=0$ and $10w^Tx+10b=0$ represent the same line). To solve this, we fix a specific quantity. 
  What exactly do we set to equal $1$ for the Support Vectors, and why don't we simply fix $||w|| = 1$?
  
  **Question 5: The Dual Problem (Multiple Choice)**
  By using Lagrange Multipliers, we convert the "Primal" SVM problem into the "Dual" SVM problem. Which of the following is the primary mathematical advantage of the Dual formulation?
  A) It eliminates the bias term $b$ entirely from the model.
  B) It allows the model to predict multiple classes natively without needing One-vs-Rest.
  C) The optimization depends only on the dot product of the input vectors ($x_i^T x_j$), allowing us to use the Kernel Trick.
  D) It guarantees that the margin will be infinitely large.
  
  **Question 6: Soft-Margin and the $C$ Parameter (Open-Ended)**
  You are training a Soft-Margin SVM on a dataset with some mislabeled outliers. 
  A) Explain what the slack variable ($\xi_i$) represents mathematically or geometrically.
  B) Explain how changing the hyperparameter $C$ from $10,000$ to $0.01$ will change the model's priority, and what visual effect this will have on the decision boundary.
  
  **Question 7: The Kernel Trick (Short Answer)**
  You have a 2D dataset of concentric circles (a target shape) that is not linearly separable. You want to map this into a 3D space to separate it with a flat plane. 
  Explain what the "Kernel Trick" is and why we use a Kernel Function (like a Polynomial or RBF kernel) instead of manually calculating the new 3D coordinates for every single data point.
  
  **Question 8: Hard-Margin SVM (Hand-Calculating)**
  You have a simple 2D dataset with exactly two points:
  *   Positive class ($+1$): $x_1 = (4, 4)$
  *   Negative class ($-1$): $x_2 = (0, 0)$
  By geometric reasoning, find the optimal Maximum-Margin Hyperplane.
  A) What is the midpoint between these two classes?
  B) What is the equation of the decision boundary line?
  C) Determine the exact weight vector $w$ and bias $b$ that satisfy the canonical constraint $y_i(w^T x_i + b) = 1$ for the support vectors.
  
  **Question 9: Interpreting $\alpha$ (True/False)**
  **True or False:** After solving the SVM Dual problem, you look at the calculated Lagrange multipliers ($\alpha$). For 95% of your training dataset, $\alpha_i = 0$. This indicates that the algorithm has failed to learn from the majority of your data.
  
  **Question 10: Multi-Class SVM (Short Answer)**
  The standard SVM algorithm is strictly a binary classifier (+1 or -1). Briefly explain the "One-vs-Rest" (or One-vs-All) strategy used to adapt SVM for a problem with 5 different classes.
  
  ***
  ***
- ### **Answer Key**
  
  **1. The Sigmoid Function**
  *   **A) Raw Score ($z$):** $z = w^T x + b = (1.5)(2) + (-0.5)(4) - 1 = 3 - 2 - 1 = \mathbf{0}$.
  *   **B) Probability:** $\sigma(0) = \frac{1}{1 + e^0} = \frac{1}{1 + 1} = \mathbf{0.5}$.
  *   **C) Classification:** Because the probability is exactly $0.5$ (it sits perfectly on the decision boundary), most algorithms default to either picking the positive class or flagging it as "unsure." (By the strict math $\ge 0.5$, it predicts **Spam ($y=1$)**).
  
  **2. Logistic Regression Update**
  *   **Answer: B.** The Perceptron only triggers an update if a hard mistake is made. Logistic Regression updates on *every* sample based on how confident it was. If it predicted 0.99 for a positive class, the error is 0.01 (tiny update). If it predicted 0.10, the error is 0.90 (large update).
  
  **3. SVM Geometry**
  *   **Answer: False.** In SVMs, the optimal hyperplane is determined *exclusively* by the Support Vectors. Removing any data point that is not a support vector has absolutely zero mathematical effect on the final boundary. 
  
  **4. SVM Optimization Constraint**
  *   **Answer:** We set the **Functional Margin to 1** ($y_i(w^T x_i + b) = 1$) for the closest points (Support Vectors). We don't fix $||w|| = 1$ because the mathematical set representing $||w||=1$ is the surface of a sphere, which is **not a convex set**. Optimization over a non-convex space leads to getting stuck in local minima, whereas fixing the functional margin keeps the problem perfectly convex (Quadratic Programming).
  
  **5. The Dual Problem**
  *   **Answer: C.** The mathematical structure of the Dual objective function relies solely on the inner product (dot product) of the data points $x_i^T x_j$. This specific formulation is the mathematical gateway that allows the Kernel Trick to work.
  
  **6. Soft-Margin and the $C$ Parameter**
  *   **A) Slack ($\xi$):** It measures how much a specific point violates the margin. $\xi=0$ means safe, $0 < \xi < 1$ means inside the margin but correctly classified, and $\xi > 1$ means misclassified.
  *   **B) $C$ Tradeoff:** $C$ is the penalty for errors. Moving from 10,000 (huge penalty) to 0.01 (tiny penalty) shifts the model's priority away from "classifying everything perfectly" to "maximizing the margin." Visually, the boundary will transition from a highly jagged, overfitted line that dodges outliers, to a smooth, robust line with a wide margin that simply accepts the outliers as misclassifications (Underfitting / High Bias).
  
  **7. The Kernel Trick**
  *   **Answer:** The Kernel Trick is using a mathematical function (like a Gaussian/RBF or Polynomial formula) that calculates the dot product of two vectors *as if* they were in a higher dimension, without actually doing the physical transformation. It solves the **Computational Burden** problem; mapping points into infinite dimensions (like the RBF kernel does) would require infinite memory/time, but the Kernel function computes the similarity instantly in the original space.
  
  **8. Hard-Margin SVM**
  *   **A) Midpoint:** $((4+0)/2 , (4+0)/2) = \mathbf{(2, 2)}$.
  *   **B) Boundary Equation:** The line connecting the points is $y=x$. The perpendicular decision boundary passing through $(2,2)$ is $y = -x + 4$, which rearranges to **$x_1 + x_2 - 4 = 0$**.
  *   **C) Weights and Bias:** From the equation, an unscaled guess is $w=[1, 1], b=-4$. Let's test the constraint on $x_1(4,4)$: $1(1\cdot4 + 1\cdot4 - 4) = 4$. We need this score to be exactly 1 for our Support Vector. So, we divide our weights by 4. 
    *   **$w =[0.25, 0.25]^T$**
    *   **$b = -1$**
    *(Verification on $x_2(0,0)$ with $y=-1$: $-1(0.25(0) + 0.25(0) - 1) = 1$. It works!)*
  
  **9. Interpreting $\alpha$**
  *   **Answer: False.** This is exactly how SVM is supposed to work! The points where $\alpha_i > 0$ are the Support Vectors. The fact that 95% of $\alpha$ values are 0 is the definition of **"Sparseness."** It means the model successfully identified the 5% of data points that define the boundary and safely ignored the rest.
  
  **10. Multi-Class SVM**
  *   **Answer:** To classify 5 classes, you train 5 separate binary SVMs. SVM #1 learns "Class 1 vs. Not Class 1". SVM #2 learns "Class 2 vs. Not Class 2," and so on. During testing, you pass the new data point through all 5 models. Whichever model puts the prediction *furthest* into the positive region (highest positive margin distance) is chosen as the final predicted class.
  
  ***
- ---
- ---
- ### **Practice Questions: Lecture 11**
  
  **Question 1: Bagging vs. Boosting (Multiple Choice)**
  Which of the following best describes the fundamental difference in how Bagging and Boosting handle the Bias-Variance tradeoff?
  A) Bagging reduces Bias by training deep trees, while Boosting reduces Variance by averaging shallow stumps.
  B) Bagging reduces Variance by averaging independent models, while Boosting reduces Bias by sequentially correcting underfitted models.
  C) Bagging requires sequential training to reduce Variance, while Boosting requires parallel training to reduce Bias.
  D) Both methods reduce Bias and Variance equally using the exact same sampling mechanism.
  
  **Question 2: Random Forest Architecture (Short Answer)**
  A Random Forest is essentially a Bagging algorithm that uses Decision Trees as its base learners. However, Random Forests introduce one "extra trick" (a second source of randomness) when building the trees to make the ensemble even more powerful. 
  What is this trick, and why is it necessary?
  
  **Question 3: Base Learner Selection (Open-Ended)**
  You are tasked with choosing the base learners for two different ensemble algorithms. 
  A) For a **Bagging** ensemble, should you use deep, complex decision trees or shallow, simple "stumps"? Why?
  B) For a **Boosting** ensemble (like AdaBoost), should you use deep, complex decision trees or shallow, simple "stumps"? Why?
  
  **Question 4: AdaBoost Math: Calculating Error and Alpha (Hand-Calculating)**
  You are running the first iteration ($t=1$) of the AdaBoost algorithm on a dataset with **5 data points**. 
  *   **Step 1:** What is the initial weight $D_1(i)$ assigned to each of the 5 data points?
  *   **Step 2:** Your first weak learner ($h_1$) misclassifies exactly **1** out of the 5 points. Calculate the weighted training error ($\epsilon_1$) for this iteration.
  *   **Step 3:** Based on the error, calculate the "Amount of Say" or voting weight ($\alpha_1$) for this classifier. (Use the formula $\alpha_t = \frac{1}{2} \ln(\frac{1-\epsilon_t}{\epsilon_t})$. You may leave the answer in terms of $\ln$ or calculate the decimal).
  
  **Question 5: AdaBoost Math: Updating Weights (Hand-Calculating)**
  Continuing from Question 4, you must now update the weights for the next iteration ($D_2$) using the formula: $D_{new} = D_{old} \times \exp(-\alpha \cdot y \cdot h(x))$. 
  Assume $\alpha_1 \approx 0.693$, which means $e^{\alpha_1} = 2$ and $e^{-\alpha_1} = 0.5$.
  A) Calculate the new, unnormalized weight for the **1 misclassified point**.
  B) Calculate the new, unnormalized weight for one of the **4 correctly classified points**.
  C) *Concept check:* Did the math successfully force the algorithm to care more about the mistake?
  
  **Question 6: AdaBoost Math: The Final Vote (Hand-Calculating)**
  After 3 iterations, your AdaBoost algorithm stops. You have three weak classifiers with the following voting weights ($\alpha$):
  *   $\alpha_1 = 0.8$
  *   $\alpha_2 = 0.5$
  *   $\alpha_3 = 0.9$
  You pass a brand new testing data point ($x_{new}$) into the ensemble. 
  *   $h_1$ predicts $+1$
  *   $h_2$ predicts $-1$
  *   $h_3$ predicts $-1$
  Using the final classifier formula $H(x) = \text{sign}(\sum \alpha_t h_t(x))$, what is the final predicted class for this point? Show your calculation.
  
  **Question 7: Ensemble Scenario (Multiple Choice)**
  You are analyzing a healthcare dataset that is notoriously small and contains a high amount of noise (mislabeled data, sensor errors, and extreme outliers). Which algorithm is the most appropriate to use?
  A) AdaBoost
  B) Gradient Boosting (GBM)
  C) Random Forest
  D) A single Decision Tree with unlimited depth
  
  **Question 8: AdaBoost Failure Modes (True/False)**
  **True or False:** In AdaBoost, if a weak learner evaluates the training data and achieves an error rate of exactly 50% ($\epsilon_t = 0.5$), the algorithm will assign it an $\alpha$ (voting weight) of 0, effectively ignoring its predictions entirely.
  
  **Question 9: Bootstrapping Concept (Short Answer)**
  In Bagging, new datasets are created using a technique called "Bootstrapping" (sampling with replacement). Because of the math of replacement, some data points are picked multiple times, and some are never picked at all. 
  What do we call the data points that are left out, and how can they be useful?
  
  **Question 10: Overfitting in Boosting (Open-Ended)**
  Your colleague claims, "Because Boosting combines hundreds of models, it is perfectly immune to overfitting." 
  Is your colleague correct? Explain mathematically or conceptually how/why Boosting can overfit a dataset.
  
  ***
  ***
- ### **Answer Key**
  
  **1. Bagging vs. Boosting (Multiple Choice)**
  *   **Answer: B.** Bagging averages parallel models to smooth out Variance. Boosting trains sequential models to incrementally fix Bias. 
  
  **2. Random Forest Architecture**
  *   **The Trick:** At every single node split, the tree is only allowed to choose from a **random subset of features**, rather than all available features.
  *   **Why it's necessary:** It prevents all the trees in the forest from relying on the exact same "strongest" feature (which would make all the trees identical). Forcing the trees to look at different features ensures the models are **diverse and uncorrelated**, which is mathematically required for the "averaging" step to effectively reduce variance.
  
  **3. Base Learner Selection**
  *   **A) Bagging:** Use **deep, complex trees**. Bagging is designed to reduce Variance. Deep trees have low bias but high variance (they overfit easily). Bagging perfectly neutralizes this high variance.
  *   **B) Boosting:** Use **shallow, simple "stumps"**. Boosting is designed to reduce Bias. If you feed it a deep tree, the model will immediately overfit. You must start with a "weak learner" (high bias) so the boosting algorithm has room to sequentially correct the errors.
  
  **4. AdaBoost Math: Error and Alpha**
  *   **Step 1:** $D_1(i) = 1 / N = 1 / 5 = \mathbf{0.2}$.
  *   **Step 2:** The error is the sum of the weights of the mistakes. 1 mistake $\times 0.2$ weight = $\mathbf{0.2}$. (Or 20%).
  *   **Step 3:** $\alpha_1 = \frac{1}{2} \ln(\frac{1 - 0.2}{0.2}) = \frac{1}{2} \ln(\frac{0.8}{0.2}) = \frac{1}{2} \ln(4) \approx \mathbf{0.693}$.
  
  **5. AdaBoost Math: Updating Weights**
  *   *Rule Reminder:* If wrong, $y \cdot h(x) = -1$. If right, $y \cdot h(x) = 1$.
  *   **A) Misclassified Point:** $D_{new} = 0.2 \times \exp(-0.693 \cdot -1) = 0.2 \times \exp(0.693) = 0.2 \times 2 = \mathbf{0.4}$.
  *   **B) Correct Point:** $D_{new} = 0.2 \times \exp(-0.693 \cdot 1) = 0.2 \times \exp(-0.693) = 0.2 \times 0.5 = \mathbf{0.1}$.
  *   **C)** Yes! The misclassified point doubled in weight, and the correct points had their weights cut in half. The next model will be forced to pay attention to the heavy $0.4$ point. *(Note: To finish the algorithm, you would divide these by the sum $Z_t$ to normalize them back to 1).*
  
  **6. AdaBoost Math: The Final Vote**
  *   **Calculation:** Sum the products of the alphas and the predictions.
    *   $h_1: 0.8 \times (+1) = +0.8$
    *   $h_2: 0.5 \times (-1) = -0.5$
    *   $h_3: 0.9 \times (-1) = -0.9$
    *   Sum $= 0.8 - 0.5 - 0.9 = -0.6$
  *   **Final Prediction:** Because the sum is negative, $H(x) = \mathbf{-1}$. Even though $h_1$ was very confident, the combined "weight" of $h_2$ and $h_3$ overruled it.
  
  **7. Ensemble Scenario**
  *   **Answer: C (Random Forest).** AdaBoost and GBM are highly sensitive to noise because their entire mechanic revolves around increasing the weight of misclassified outliers. Bagging (Random Forest) is robust against noise because the random outliers get averaged out by the "wisdom of the crowd."
  
  **8. AdaBoost Failure Modes**
  *   **True.** If $\epsilon_t = 0.5$, then $\frac{1-0.5}{0.5} = 1$. The natural log of 1 is 0. Therefore $\alpha = 0$. A model with 50% error is essentially flipping a coin, so AdaBoost mathematically strips it of its voting power.
  
  **9. Bootstrapping Concept**
  *   **Answer:** They are called **Out-of-Bag (OOB)** samples. They are incredibly useful because they can act as a built-in Validation/Test set. You can test the performance of a tree using the exact data points that it didn't see during training, preventing the need for a strict Train/Test split beforehand.
  
  **10. Overfitting in Boosting**
  *   **Answer:** The colleague is incorrect. While Bagging is generally immune to overfitting (adding more trees just smooths the average), **Boosting is highly prone to overfitting if left unchecked.** Because Boosting sequentially forces the model to focus on the hardest, most misclassified points, it will eventually start treating random noise and outliers as critical patterns, warping the decision boundary to perfectly (and incorrectly) memorize the training data.
  
  ***
- ---
- ---
- ### **Practice Questions: Lectures 12 & 13 (Clustering)**
  
  **Question 1: Distance Metrics (Multiple Choice)**
  You are clustering text documents based on the frequencies of different words. Document A is a 500-word article about baseball. Document B is a 5,000-word article about baseball with an almost identical ratio of vocabulary. Which distance metric would be the best choice to ensure these two documents are clustered together?
  A) Euclidean Distance
  B) Manhattan Distance
  C) Cosine Distance
  D) Chebyshev Distance
  
  **Question 2: Distance Metric Properties (Short Answer)**
  Given two vectors $x =[2, 5]$ and $y = [5, 1]$, calculate the following:
  A) The Euclidean distance (L2 norm) between $x$ and $y$.
  B) The Manhattan distance (L1 norm) between $x$ and $y$.
  
  **Question 3: K-Means Algorithm Steps (Sequencing)**
  Put the following steps of the K-Means clustering algorithm in the correct order:
  1. Recompute the cluster centroids by calculating the mean of the assigned points.
  2. Initialize $K$ random points as the starting cluster centers.
  3. Stop when the cluster assignments no longer change.
  4. Assign each data point to the cluster with the closest centroid.
  
  **Question 4: K-Means Cost Function (True/False)**
  **True or False:** The K-Means cost function (the sum of squared Euclidean distances from points to their cluster centroids) is guaranteed to decrease or stay the same after every single iteration (E-step and M-step) of the algorithm. 
  
  **Question 5: Local Optima in K-Means (Open-Ended)**
  You run the K-means algorithm on a dataset and compute a final cost function value of $J = 150$. Your colleague runs the exact same algorithm with the exact same $K$ on the exact same dataset and gets a final cost function value of $J = 80$.
  Why did this happen, and what is the standard algorithmic practice to prevent this issue from ruining your analysis?
  
  **Question 6: Choosing K (Multiple Choice)**
  When using the "Elbow Method" to determine the optimal number of clusters ($K$) for a K-Means model, what are you plotting on the x and y axes?
  A) X-axis: Number of Clusters ($K$); Y-axis: Silhouette Score.
  B) X-axis: Minimized Cost Function ($J$); Y-axis: Number of Clusters ($K$).
  C) X-axis: Number of Iterations; Y-axis: Minimized Cost Function ($J$).
  D) X-axis: Number of Clusters ($K$); Y-axis: Minimized Cost Function ($J$).
  
  **Question 7: K-Means vs. GMM (Short Answer)**
  K-Means and Gaussian Mixture Models (GMM) approach clustering differently. 
  A) State whether K-Means performs "Hard" or "Soft" clustering and explain what that means.
  B) State whether GMM performs "Hard" or "Soft" clustering and explain what that means.
  
  **Question 8: Gaussian Mixture Model Parameters (Multiple Choice)**
  In a 2-dimensional feature space, a Gaussian Mixture Model must learn several parameters for each cluster during the M-step. Which of the following is **NOT** a parameter explicitly learned for a cluster in a standard GMM?
  A) The Mean vector ($\mu$)
  B) The Covariance matrix ($\Sigma$)
  C) The Prior probability / Mixing weight ($\pi$ or $P(C_i)$)
  D) The maximum Euclidean distance to the cluster edge.
  
  **Question 9: The EM Algorithm in GMM (Open-Ended)**
  The Expectation-Maximization (EM) algorithm is used to train Gaussian Mixture Models. 
  In the **E-step**, what specific value is calculated for every data point with respect to every cluster, and what mathematical theorem is used to calculate it?
  
  **Question 10: Geometric Assumptions (True/False)**
  **True or False:** Because K-Means only calculates the mean (centroid) and relies on Euclidean distance, it implicitly assumes that all clusters are spherical and have roughly equal variances. GMMs do not share this limitation because they calculate a covariance matrix, allowing them to model elliptical clusters.
  
  ***
  ***
- ### **Answer Key**
  
  **1. Distance Metrics**
  *   **Answer: C (Cosine Distance).** Cosine distance measures the angle between vectors, making it invariant to magnitude (document length). Since both articles have similar word ratios (similar direction in vector space), cosine similarity will be high (distance will be low). Euclidean distance would fail here because the massive difference in word counts would make them appear very far apart in space.
  
  **2. Distance Metric Properties**
  *   **A) Euclidean:** $\sqrt{(2-5)^2 + (5-1)^2} = \sqrt{(-3)^2 + (4)^2} = \sqrt{9 + 16} = \sqrt{25} = \mathbf{5}$.
  *   **B) Manhattan:** $|2-5| + |5-1| = |-3| + |4| = 3 + 4 = \mathbf{7}$.
  
  **3. K-Means Algorithm Steps**
  *   **Answer: 2, 4, 1, 3.** (Initialize $\to$ Assign (E-step) $\to$ Recompute Means (M-step) $\to$ Repeat until convergence).
  
  **4. K-Means Cost Function**
  *   **True.** K-Means is an alternating optimization algorithm (Coordinate Descent). Both assigning points to the closest centroid (E-step) and moving the centroid to the mathematical mean of those points (M-step) mathematically minimize the sum of squared distances. The cost $J$ will never increase.
  
  **5. Local Optima in K-Means**
  *   **Why it happened:** K-Means is highly sensitive to the initial, random placement of the centroids. If the initial centroids are placed poorly (e.g., two centroids stuck inside the same natural cluster), the greedy algorithm will converge to a suboptimal **local minimum**, resulting in a higher cost $J$. Your colleague got a "luckier" initial placement that allowed the algorithm to find a better minimum.
  *   **The Fix:** **Multiple Random Initializations.** You must run the entire K-Means algorithm multiple times (e.g., 50 or 100 times) using different random starting seeds, and then permanently select the single clustering result that produced the lowest overall cost $J$.
  
  **6. Choosing K**
  *   **Answer: D.** The Elbow Method plots the Number of Clusters ($K$) on the x-axis against the Minimized Cost Function / Sum of Squared Errors ($J$) on the y-axis. The optimal $K$ is at the "elbow" where adding more clusters yields diminishing returns in cost reduction.
  
  **7. K-Means vs. GMM**
  *   **A) K-Means (Hard Clustering):** Every data point is assigned to one and only one cluster. The probability of membership is either 1 or 0.
  *   **B) GMM (Soft Clustering):** Every data point is assigned a probability distribution over all clusters. For example, a point might have an 80% chance of belonging to Cluster A and a 20% chance of belonging to Cluster B.
  
  **8. Gaussian Mixture Model Parameters**
  *   **Answer: D.** GMMs do not have "edges" or maximum distances because Gaussian distributions extend infinitely (approaching zero probability). The algorithm learns the Mean (center), the Covariance (shape/spread), and the Prior (overall weight/size of the cluster).
  
  **9. The EM Algorithm in GMM**
  *   **Answer:** In the E-step, the algorithm calculates the **responsibility** (or probabilistic membership). It determines the probability that a specific data point $x_i$ was generated by a specific cluster $C_k$. It calculates this posterior probability $P(C_k | x_i)$ using **Bayes' Theorem** (combining the Gaussian likelihood of the point given the cluster with the prior probability of the cluster).
  
  **10. Geometric Assumptions**
  *   **True.** K-Means essentially acts as a restricted, special case of a GMM where the covariance matrices for all clusters are forced to be spherical (identity matrices) and equal. Because GMMs actually calculate the covariance matrix $\Sigma$ during the M-step, they can adapt to clusters that are stretched into ellipses or tilted at angles.
  
  ***
- ---
- ---
-