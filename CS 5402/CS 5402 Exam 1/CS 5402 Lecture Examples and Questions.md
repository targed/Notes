-
- # Lecture 2: Data Types & Data Quality
- ### 1. The "Algorithm-First" Fallacy (ZIP Codes)
  *   **Slide:** 3
  *   **Category:** Attribute Types (Nominal vs. Numeric)
  *   **Problem:** You are clustering customers using ZIP Code, Age, and Income. You apply the *k-means* clustering algorithm directly to the dataset. Why does this fail?
  *   **Solution:** *k-means* computes mathematical averages (means). ZIP codes are **Nominal** identifiers, not quantitative numbers. Calculating the "average" ZIP code is mathematically meaningless and will ruin the clustering algorithm.
- ### 2. The "Algorithm-First" Fallacy (Time-Series)
  *   **Slide:** 4
  *   **Category:** Data Types by Domain (Temporal)
  *   **Problem:** You have hourly energy consumption data. You treat each row as an independent sample and apply standard classification/regression. Why does this fail?
  *   **Solution:** By treating rows as independent, you ignore the temporal (time) order. Crucial patterns like **trends** (energy use going up over years) and **seasonality** (energy spikes at 6 PM every day) are completely lost.
- ### 3. The "Algorithm-First" Fallacy (Fraud Detection)
  *   **Slide:** 5
  *   **Category:** Data Quality (Class Imbalance) / Evaluation Metrics
  *   **Problem:** You have a fraud detection dataset where 99.5% of transactions are non-fraud and 0.5% are fraud. You train a classifier and evaluate it using overall "Accuracy." Why does this fail?
  *   **Solution:** A "lazy" model could simply predict "non-fraud" for every single transaction. It would achieve **99.5% accuracy**, but the model is entirely useless because it caught 0% of the actual fraud. (This requires different metrics like Precision/Recall).
- ### 4. Warm-Up Question: Effort Allocation
  *   **Slide:** 8
  *   **Category:** The Data Mining Process
  *   **Problem:** Which task typically takes the **most time** in a real-world data mining project?
    *   A. Choosing the best algorithm
    *   B. Training the model
    *   C. Data understanding and preprocessing
    *   D. Visualizing results
  *   **Solution:** **C. Data understanding and preprocessing.** (It typically takes 70–80% of the total effort).
- ### 5. Quick Question: Object vs. Attribute
  *   **Slide:** 11
  *   **Category:** Data Terminology
  *   **Problem:** In a movie recommendation dataset, what is most commonly treated as the **object**?
    *   A. Movie genre
    *   B. User
    *   C. Rating value
    *   D. Timestamp
  *   **Solution:** **B. User.** (The User is the main entity/row being described, while genre, rating, and timestamp are the *attributes* or features describing that user's interactions).
- ### 6. Example: Interval Quantities Math
  *   **Slide:** 17
  *   **Category:** Levels of Measurement (Interval)
  *   **Problem:** You have an attribute "Temperature" expressed in Fahrenheit. Evaluate whether the following statements make mathematical sense:
    1.  "Today is 5 degrees hotter than yesterday."
    2.  "The combined temperature of yesterday and today is 188 F."
  *   **Solution:** 
    1.  **YES.** Interval data allows for meaningful differences (addition/subtraction).
    2.  **NO.** Interval data does not have a "true zero," so sums and ratios (multiplication/division) are mathematically invalid.
- ### 7. Quick Question: Attribute Types
  *   **Slide:** 22
  *   **Category:** Levels of Measurement
  *   **Problem:** Which attribute type best describes “customer satisfaction level: low, medium, high”?
    *   A. Nominal
    *   B. Ordinal
    *   C. Interval
    *   D. Ratio
  *   **Solution:** **B. Ordinal.** (There is a meaningful rank/order, but the exact mathematical distance between "low" and "medium" is unknown).
- ### 8. Quick Question: Attribute Mapping
  *   **Slide:** 23
  *   **Category:** Levels of Measurement
  *   **Problem:** Given a student performance dataset, identify the attribute types for the following features:
    1. Student ID
    2. GPA
    3. Major
    4. Year (Freshman–Senior)
    5. Number of absences
  *   **Solution:**
    1. **Student ID:** Nominal (Just a label, no math applies).
    2. **GPA:** Ratio / Continuous (Has a true zero, decimals exist).
    3. **Major:** Nominal (No intrinsic order).
    4. **Year:** Ordinal (Ordered, but "distance" between Freshman and Sophomore isn't numerical).
    5. **Number of absences:** Ratio / Discrete (Countable integers, has a true zero).
- ### 9. Quick Question: Identify Data Types
  *   **Slide:** 39
  *   **Category:** Types of Data by Domain
  *   **Problem:** For each example, identify the data type:
    1. Daily stock prices
    2. Shopping basket records
    3. GPS coordinates of Uber trips
    4. Email messages
  *   **Solution:**
    1. **Daily stock prices:** Time Series Data
    2. **Shopping basket records:** Transaction Data
    3. **GPS coordinates:** Spatial Data
    4. **Email messages:** Text Data (Unstructured)
- ### 10. Quick Question: Missing Data
  *   **Slide:** 54
  *   **Category:** Data Quality Remediation
  *   **Problem:** Which strategy is usually **NOT** recommended when handling missing data?
    *   A. Removing records with many missing values
    *   B. Filling missing values with the mean
    *   C. Filling missing values with random numbers
    *   D. Using model-based imputation
  *   **Solution:** **C. Filling missing values with random numbers.** (This introduces artificial noise and destroys the underlying patterns algorithms are trying to find).
- ### 11. Quick Question: Outlier
  *   **Slide:** 55
  *   **Category:** Data Quality Remediation
  *   **Problem:** Scenario: A patient’s heart rate is recorded as 250 bpm. What should you do first?
    *   A. Remove it immediately
    *   B. Keep it without question
    *   C. Investigate whether it is an error or rare event
    *   D. Replace it with the average
  *   **Solution:** **C. Investigate whether it is an error or rare event.** (In medical datasets, extreme outliers might be measurement errors, but they might also be the exact critical event you are trying to predict—e.g., a heart attack. You must investigate context before deleting).
  
  ***
- ---
- ---
- # Lecture 3: Data Preprocessing
- ### 1. Warm-Up Question: Improving Performance
  *   **Slide:** 2
  *   **Category:** Preprocessing Philosophy
  *   **Problem:** Which action is most likely to improve model performance on real-world data?
    *   A. Switching from logistic regression to a deep neural network
    *   B. Adding more hidden layers
    *   C. Improving data preprocessing and feature representation
    *   D. Increasing training epochs
  *   **Solution:** **C. Improving data preprocessing and feature representation.** (Preprocessing often improves performance more than changing models).
- ### 2. Example 1: The Danger of Missing Values
  *   **Slide:** 5
  *   **Category:** Missing Data
  *   **Problem:** You have a dataset of 1,000 student records. The GPA column has 100 missing values (NaN). What happens if you run standard statistical operations like `np.sum()`, `np.mean()`, or `np.std()` on this column?
  *   **Solution:** The result will be **NaN** for all of them. Operations propagate `NaN`. This illustrates why missing data must be explicitly handled (removed or imputed) before feeding it to an algorithm.
- ### 3. Example 2: Unscaled Distance Dominance
  *   **Slide:** 6
  *   **Category:** Feature Scaling
  *   **Problem:** You want to predict credit risk. Feature 1 is "Annual Income" (Range: \$20,000 – \$200,000). Feature 2 is "Number of Late Payments" (Range: 0 – 10). You calculate the distance between two customers:
    $Distance \approx (200000 - 50000)^2 + (2 - 1)^2$
  *   **Solution/Takeaway:** The distance is completely dominated by the Income feature ($150,000^2$ vs $1^2$). The "Late Payments" feature is mathematically ignored by the algorithm. This shows why we MUST scale/normalize features before calculating distance.
- ### 4. Mathematical Example: Spotting Outliers (IQR Boxplot Rule)
  *   **Slide:** 13
  *   **Category:** Outlier Detection
  *   **Problem:** Suppose we have a list of employee ages/salaries: `[45, 48, 50, 52, 55, 57, 60, 62, 65, 70, 250]`. Use the IQR (Boxplot Rule) to determine if 250 is mathematically an outlier.
  *   **Solution / Steps:**
    1.  Find $Q1$ (25th percentile) $\approx 50$
    2.  Find $Q3$ (75th percentile) $\approx 65$
    3.  Calculate $IQR = Q3 - Q1 = 65 - 50 = \mathbf{15}$
    4.  Calculate Lower Bound: $50 - (1.5 \times 15) = 50 - 22.5 = \mathbf{27.5}$
    5.  Calculate Upper Bound: $65 + (1.5 \times 15) = 65 + 22.5 = \mathbf{87.5}$
    *   **Conclusion:** Since $250 > 87.5$, 250 is an outlier.
- ### 5. Example: Feature Construction (House Prices)
  *   **Slide:** 17
  *   **Category:** Feature Engineering
  *   **Problem:** You are predicting house prices. Your raw features are `Total_Price` and `House_Size` (Square feet). Why is it beneficial to construct a new feature?
  *   **Solution:** You should construct `Price_per_SqFt = Total_Price / House_Size`. 
    *   *Why this helps:* It normalizes for house size, captures market value more directly, and makes it easier for the model to compare houses of vastly different sizes.
- ### 6. Quick Question 1: Feature Construction
  *   **Slide:** 20
  *   **Category:** Feature Engineering
  *   **Problem:** Which action is an example of feature construction?
    *   A. Removing highly correlated features
    *   B. Selecting top-k features using mutual information
    *   C. Creating BMI from height and weight
    *   D. Dropping low-variance features
  *   **Solution:** **C. Creating BMI from height and weight.** (You are using math to combine existing features into a brand new, more informative feature).
- ### 7. Quick Question 2: House Prices Pipeline
  *   **Slide:** 21
  *   **Category:** Feature Engineering / Selection
  *   **Problem:** In a dataset to predict house prices, you have features like: `Listing ID`, `#beds`, `#baths`, `#floors`, `sqft`, `Build year`, `Lot acres`. Identify which features should be constructed and which should be removed.
  *   **Solution:**
    *   **Constructed:** "Price per square foot" (from price and sqft) and "Age of the house" (from current year minus Build year).
    *   **Remove:** `Listing ID`. (It is an irrelevant feature/identifier that will only cause the model to overfit).
- ### 8. Example: Categorical Encoding Error
  *   **Slide:** 29
  *   **Category:** Categorical Encoding
  *   **Problem:** A machine learning model attempts to calculate $Loss = 0.85 - \text{'cat'}$. What is the issue?
  *   **Solution:** Machine learning models are math-based; they need numbers, not text. This operation will throw an error. Categorical variables must be converted to numerical formats via **Encoding**.
- ### 9. Example: The Danger of Label Encoding
  *   **Slide:** 30
  *   **Category:** Categorical Encoding
  *   **Problem:** You use Label Encoding on a "College Major" feature. CS $\rightarrow$ 0, Math $\rightarrow$ 1, Physics $\rightarrow$ 2. What is the danger here?
  *   **Solution:** This creates an **artificial order** ($CS < Math < Physics$). The algorithm will mathematically assume that Math is the "average" of CS and Physics. Label Encoding should ONLY be used for *Ordinal* variables (e.g., Small, Medium, Large). For Nominal variables, use **One-Hot Encoding**.
- ### 10. Quick Question 3: Encoding Choice
  *   **Slide:** 32
  *   **Category:** Categorical Encoding
  *   **Problem:** Which categorical feature should **NOT** be label-encoded?
    *   A. Education level (High School, Bachelor, Master)
    *   B. Product category (Laptop, Phone, Tablet)
    *   C. Satisfaction level (Low, Medium, High)
    *   D. Risk level (Low, Medium, High)
  *   **Solution:** **B. Product category (Laptop, Phone, Tablet).** (Because it is Nominal. There is no inherent rank or order between a laptop and a phone. The others are Ordinal and can be safely label-encoded).
- ### 11. Conceptual Problem: The Pipeline "Data Leakage" Mistake
  *   **Slide:** 35 & 36
  *   **Category:** Data Preprocessing Pipelines
  *   **Problem:** A student normalizes their entire dataset *before* splitting it into Training and Testing sets. What is the core issue with this?
  *   **Solution:** **Data Leakage**. By normalizing the whole dataset at once, the scaling math (e.g., the global min/max or mean) includes information from the Test set. The model has "peeked" into the future data. 
    *   *Golden Rule:* Split data *before* preprocessing. Fit the scaler ONLY on the training data, then apply that transformation to the test data.
  
  ***
- ---
- ---
- # Lecture 4 & 5: Similarity, Distance & Evaluation Basics
- ### 1. Intuition Example: The Need for Distance Metrics
  *   **Slide:** 3
  *   **Category:** Similarity / Distance
  *   **Problem:** You are doing Customer Segmentation. Customer A (Spend: 500, Visits: 2), Customer B (Spend: 520, Visits: 3), and Customer C (Spend: 5000, Visits: 25). Is Customer A closer to B or to C?
  *   **Solution:** Based on the raw numerical values, A is clearly closer to B. This illustrates why we need a formal mathematical distance metric: without one, an algorithm cannot define "near" vs. "far," making tasks like clustering and outlier detection impossible.
- ### 2. Example: The “99% Accuracy” Medical Paradox
  *   **Slide:** 17
  *   **Category:** Evaluation Metrics (The Flaw of Accuracy)
  *   **Problem:** Imagine a very rare, but serious, disease that affects 1 in 1,000 people. You build a "lazy" model that always predicts "No Disease." What is the accuracy of this model, and what is the punchline?
  *   **Solution:** 
    *   The model gets 999 out of 1,000 correct. 
    *   Accuracy = $999 / 1000 = \mathbf{99.9\%}$. 
    *   **The Punchline:** You have an overwhelmingly accurate model that completely fails at its one and only job: finding the sick person. This proves accuracy is dangerously misleading on imbalanced datasets.
- ### 3. Example: The Spam Filter (Precision vs. Recall)
  *   **Slide:** 18
  *   **Category:** Precision vs. Recall
  *   **Problem:** Your model identifies "Not Spam" (a "positive" result) and puts it in the inbox. What is the worse mistake: a False Negative (missed job offer sent to spam) or a False Positive (buy-one-get-one coupon sent to inbox)? Which metric should you optimize?
  *   **Solution:** A False Negative is disastrous, while a False Positive is just annoying. Because your filter must be perfect when it declares an email as "important," you must optimize for **High Precision**. You are okay letting some spam through (lower recall) to ensure you never lose a critical email.
- ### 4. Example: Doctor (Precision vs. Recall)
  *   **Slide:** 19
  *   **Category:** Precision vs. Recall
  *   **Problem:** Your model identifies patients who might have cancer (a "positive" result). What is the worse mistake: a False Negative (sick patient sent home without follow-up) or a False Positive (healthy patient sent for a biopsy)? Which metric should you optimize?
  *   **Solution:** A False Negative is catastrophic and potentially fatal. A False Positive is stressful/costly but not fatal. Because you *must* find every single person who is sick, you optimize for **High Recall**. You are willing to accept false alarms if it means you never miss a real case.
- ### 5. Walkthrough: Calculating the Core Metrics
  *   **Slides:** 27, 28, 29, 31
  *   **Category:** Confusion Matrix Calculations
  *   **Problem:** Given a confusion matrix with True Negatives (TN) = 85, False Positives (FP) = 5, False Negatives (FN) = 2, and True Positives (TP) = 8. Calculate Accuracy, Precision, Recall, and the F1-Score.
  *   **Solution:**
    *   **Accuracy:** $(TN + TP) / Total = (85 + 8) / 100 = \mathbf{93\%}$
    *   **Precision:** $TP / (TP + FP) = 8 / (8 + 5) = \mathbf{61.5\%}$
    *   **Recall:** $TP / (TP + FN) = 8 / (8 + 2) = \mathbf{80\%}$
    *   **F1-Score:** $2 \times \frac{Precision \times Recall}{Precision + Recall} = 2 \times \frac{0.615 \times 0.8}{0.615 + 0.8} = \frac{0.984}{1.415} = \mathbf{69.5\%}$
- ### 6. Quick Question: Which Metric Matters More?
  *   **Slide:** 30
  *   **Category:** Applied Evaluation
  *   **Problem:** For each scenario, would you optimize for Precision or Recall?
    1. **Airport Security Screening:** Model flags bags for weapons (Positive = Weapon).
    2. **Bank Fraud Detection:** Model flags transactions as fraudulent (Positive = Fraud).
    3. **Email Marketing:** Model predicts which customers will actually buy from a coupon (Positive = Will Buy).
  *   **Solution:**
    1. **Airport Security:** **Recall**. A False Negative (missing a weapon) is drastically worse than a False Positive (manually checking a clean bag).
    2. **Bank Fraud:** Usually **Recall**. Letting a fraudulent charge go through (FN) is typically much worse than temporarily blocking a real purchase (FP).
    3. **Email Marketing:** **Precision**. You want to minimize False Positives so you don't waste budget sending coupons to people who won't use them.
- ### 7. Question 1: F1-Score Calculation
  *   **Slide:** 32
  *   **Category:** Evaluation Metrics
  *   **Problem:** Given a confusion matrix where TN=170, FP=70, FN=40, and TP=120. What is the F1 score?
    *   A. 0.6315
    *   B. 0.75
    *   C. 0.6857
    *   D. 0.725
  *   **Solution/Steps:**
    1.  Calculate Precision: $TP / (TP + FP) = 120 / (120 + 70) = 120 / 190 \approx 0.6315$
    2.  Calculate Recall: $TP / (TP + FN) = 120 / (120 + 40) = 120 / 160 = 0.75$
    3.  Calculate F1: $2 \times \frac{0.6315 \times 0.75}{0.6315 + 0.75} = \frac{0.94725}{1.3815} \approx 0.6857$
    *   **Answer:** **C. 0.6857**
- ### 8. Example: Probabilities & Thresholds
  *   **Slide:** 36
  *   **Category:** ROC Curve Intuition
  *   **Problem:** A model gives three probability scores for spam: Email A (95%), Email B (60%), Email C (15%). How do the predictions change if you set the threshold at 50% versus 80%?
  *   **Solution:**
    *   **Threshold = 50%:** A and B are marked "Spam". C is "Good". (Lower precision, higher recall).
    *   **Threshold = 80%:** Only A is marked "Spam". B and C are "Good". (Higher precision, lower recall).
- ### 9. Walkthrough: How to Draw an ROC Curve
  *   **Slides:** 41–44
  *   **Category:** ROC Curve Calculation
  *   **Problem:** You have 5 samples with True Labels (Y) = `[1, 1, 0, 1, 1]` and Predicted Probabilities (Y') = `[0.95, 0.92, 0.80, 0.76, 0.71]`. Calculate the True Positive Rate (TPR) and False Positive Rate (FPR) as you sweep the threshold down through each predicted value.
  *   **Solution/Steps:**
    *(Total Actual Positives = 4. Total Actual Negatives = 1)*
    *   **t = 0.95:** Predictions `[1, 0, 0, 0, 0]`. TP=1, FP=0. $\rightarrow$ **TPR = 1/4, FPR = 0/1**
    *   **t = 0.92:** Predictions `[1, 1, 0, 0, 0]`. TP=2, FP=0. $\rightarrow$ **TPR = 2/4, FPR = 0/1**
    *   **t = 0.80:** Predictions `[1, 1, 1, 0, 0]`. TP=2, FP=1. $\rightarrow$ **TPR = 2/4, FPR = 1/1**
    *   **t = 0.76:** Predictions `[1, 1, 1, 1, 0]`. TP=3, FP=1. $\rightarrow$ **TPR = 3/4, FPR = 1/1**
    *   **t = 0.71:** Predictions `[1, 1, 1, 1, 1]`. TP=4, FP=1. $\rightarrow$ **TPR = 4/4, FPR = 1/1**
    *(These coordinates are then plotted on a graph to draw the ROC curve).*
- ### 10. Questions for Further Thought (1)
  *   **Slide:** 50
  *   **Category:** ROC Curve Properties
  *   **Problem:** Can ROC handle imbalanced datasets?
  *   **Solution:** **Yes.** TPR is calculated strictly out of the actual positive cases, and FPR is calculated strictly out of the actual negative cases. Because these calculations are separated, a massive imbalance (e.g., 99% negative) does not skew the rates or the shape of the curve, making ROC/AUC an excellent metric for imbalanced data.
- ### 11. Questions for Further Thought (2)
  *   **Slide:** 51
  *   **Category:** Interpreting ROC Curves
  *   **Problem:** Your team built two models to predict customer churn. Model A has an AUC of 0.91; Model B has an AUC of 0.72. Your boss says, "I don't care about false alarms. I want to find at least 80% of the customers who are actually going to leave (TPR = 0.8)." Which model is better for this specific task?
  *   **Solution:** **Model A.** First, Model A has a higher AUC overall (0.91 > 0.72). Second, looking at the graph where the Y-axis (TPR) equals 0.8, Model A achieves this at an FPR of 0.15, whereas Model B achieves it at an FPR of 0.40. Model A hits the target with significantly fewer false alarms.
  
  ***
- ---
- ---
- # Lecture 6: Review & Bayes Classifier
- ### 1. Basic Probability Rules (Examples)
  *   **Slide:** 4
  *   **Category:** Probability Fundamentals
  *   **Problem:** Calculate the following using a standard 6-sided die and a fair coin:
    1.  What is the probability of rolling an even number?
    2.  What is the probability of rolling a 1 OR a 6?
    3.  What is the probability of flipping Heads AND rolling a 6?
  *   **Solution:**
    1.  $P(\text{Even}) = |\{2,4,6\}| / 6 = 3/6 = \mathbf{0.5}$
    2.  **Addition Rule:** $P(1 \text{ or } 6) = P(1) + P(6) = 1/6 + 1/6 = \mathbf{2/6}$
    3.  **Multiplication Rule (Independent):** $P(\text{Heads and } 6) = P(\text{Heads}) \times P(6) = 1/2 \times 1/6 = \mathbf{1/12}$
- ### 2. Intuitive Example: Conditional Probability
  *   **Slide:** 6
  *   **Category:** Conditional Probability
  *   **Problem:** You roll a single die. What is the probability of rolling a 2, **given** that we know the roll was an even number?
  *   **Solution:** Knowing it is even shrinks our sample space from $\{1,2,3,4,5,6\}$ down to just $\{2,4,6\}$. Out of those 3 remaining possibilities, only one is a 2. Therefore, $P(\text{Roll is 2 } | \text{ Roll is Even}) = \mathbf{1/3}$.
- ### 3. Exercise-1: Marbles in Bags
  *   **Slide:** 9
  *   **Category:** Law of Total Probability
  *   **Problem:** There are three bags that each contain 100 marbles:
    *   Bag 1: 75 red, 25 blue
    *   Bag 2: 60 red, 40 blue
    *   Bag 3: 45 red, 55 blue
    You randomly select one of the bags (1/3 chance each) and then draw a marble at random. What is the probability of drawing a red marble?
  *   **Solution:** You must sum the conditional probabilities (Law of Total Probability).
    $P(\text{Red}) = P(\text{Red}|B_1)P(B_1) + P(\text{Red}|B_2)P(B_2) + P(\text{Red}|B_3)P(B_3)$
    $P(\text{Red}) = (0.75 \times \frac{1}{3}) + (0.60 \times \frac{1}{3}) + (0.45 \times \frac{1}{3}) = \frac{1.8}{3} = \mathbf{0.6}$
- ### 4. Bayes' Theorem Example: Spam Filter
  *   **Slide:** 12
  *   **Category:** Bayes' Theorem Definitions
  *   **Problem:** Map the components of a Spam Filter to the terms of Bayes' Theorem.
  *   **Solution:** 
    *   **Prior** $P(\text{Spam})$: 20% of all emails are generally spam.
    *   **Likelihood** $P(\text{"Great Deal"} | \text{Spam})$: The phrase "Great Deal" appears in 5% of spam emails (but only 0.01% of normal emails).
    *   **Posterior** $P(\text{Spam} | \text{"Great Deal"})$: The updated belief that an email is spam now that we see the phrase.
- ### 5. Exercise-2: The Medical Paradox
  *   **Slide:** 13
  *   **Category:** Bayes' Theorem Calculation
  *   **Problem:** Assume that roughly 0.1% of the population is infected with Disease-A ($P(X=DA) = 0.001$). The test reports positive for 100% of infections ($P(Y=+ | X=DA) = 1$). However, it also reports a false positive for 1% of healthy people ($P(Y=+ | X=\text{healthy}) = 0.01$). A patient tests positive. What is the probability they actually have the disease ($P(X=DA | Y=+)$)?
  *   **Solution:** 
    *   Numerator: $P(+|DA) \times P(DA) = 1 \times 0.001 = 0.001$
    *   Denominator (Total Positive): $(1 \times 0.001) + (0.01 \times 0.999) = 0.001 + 0.00999 = 0.01099$
    *   Posterior: $0.001 / 0.01099 \approx \mathbf{0.0909}$ (Only a ~9% chance they are actually sick, despite a "99% accurate" test!).
- ### 6. Exercise-3: Joint Probability Table (X)
  *   **Slide:** 17
  *   **Category:** Marginal and Conditional Probabilities
  *   **Problem:** Given a 3x3 table of Joint Probabilities $P(X,Y)$, where $X$ represents rows and $Y$ represents columns:
    1. What is the marginal probability $P(X=1)$?
    2. What is the conditional probability $P(X=2 | Y=1)$?
  *   **Solution:**
    1.  **Marginal:** Sum the entire row for $X=1$. 
        $0.32 + 0.03 + 0.01 = \mathbf{0.36}$.
    2.  **Conditional:** $P(X=2 \cap Y=1) / P(Y=1)$.
        The intersection cell is $0.06$. The marginal total for column $Y=1$ is $(0.32 + 0.06 + 0.02) = 0.40$. 
        $0.06 / 0.40 = \mathbf{0.15}$.
- ### 7. Exercise-4: Joint Probability Table (Y)
  *   **Slide:** 18
  *   **Category:** Marginal and Conditional Probabilities
  *   **Problem:** Using the same table from Exercise 3:
    1. What is the marginal probability $P(Y=3)$?
    2. What is the conditional probability $P(Y=2 | X=2)$?
  *   **Solution:**
    1.  **Marginal:** Sum the entire column for $Y=3$.
        $0.01 + 0.02 + 0.27 = \mathbf{0.30}$.
    2.  **Conditional:** $P(X=2 \cap Y=2) / P(X=2)$.
        The intersection cell is $0.24$. The marginal total for row $X=2$ is $(0.06 + 0.24 + 0.02) = 0.32$.
        $0.24 / 0.32 = \mathbf{0.75}$.
- ### 8. Entropy: Example-1
  *   **Slide:** 25
  *   **Category:** Information Theory
  *   **Problem:** Calculate the entropy $H(X)$ of a weather distribution where $P(\text{Sunny}) = 0.5$, $P(\text{Cloudy}) = 0.3$, $P(\text{Rainy}) = 0.2$.
  *   **Solution:**
    $H(X) = - \sum p(x) \log_2 p(x)$
    $H(X) = - (0.5 \log_2 0.5 + 0.3 \log_2 0.3 + 0.2 \log_2 0.2)$
    $H(X) \approx \mathbf{1.485}$
- ### 9. Entropy: Example-2 (Visual)
  *   **Slide:** 26
  *   **Category:** Information Theory
  *   **Problem:** Look at two bar charts. Chart A has one massive peak and two tiny bars. Chart B is perfectly flat/uniform. How do their Entropies compare?
  *   **Solution:** **$H(B) > H(A)$**. A flat, uniform distribution represents maximum uncertainty, so it has the highest possible entropy. A peaked distribution is highly predictable, so it has low entropy.
- ### 10. Bayes Classifier: Example (The Hand-Calculation)
  *   **Slides:** 43–46
  *   **Category:** Naïve Bayes Classification
  *   **Problem:** You are given a table of 15 samples with two features: $X_1 \in \{1,2,3\}$ and $X_2 \in \{S,M,L\}$. The target class is $Y \in \{1, -1\}$. What is the predicted class label $Y$ for a new sample $x = (2, S)$?
  *   **Solution / Steps:**
    *   **Step 1: Calculate Priors (Slide 45).**
        $P(Y=1) = 9/15$
        $P(Y=-1) = 6/15$
    *   **Step 2: Calculate Likelihoods for Class 1 (Slide 45).**
        Out of the 9 times $Y=1$ happens: $X_1=2$ happens 3 times ($3/9$). $X_2=S$ happens 1 time ($1/9$).
    *   **Step 3: Calculate Likelihoods for Class -1 (Slide 45).**
        Out of the 6 times $Y=-1$ happens: $X_1=2$ happens 2 times ($2/6$). $X_2=S$ happens 3 times ($3/6$).
    *   **Step 4: Combine via Naïve Bayes Assumption (Slide 46).**
        Multiply Prior $\times$ Likelihoods.
        Score for Class 1: $(9/15) \times (3/9) \times (1/9) = 1/45 \approx \mathbf{0.022}$
        Score for Class -1: $(6/15) \times (2/6) \times (3/6) = 1/15 \approx \mathbf{0.066}$
    *   **Final Decision:** Because $1/15 > 1/45$, the model classifies this new sample as **Class -1**.
  
  ***
- ---
- ---
- # Lecture 7: Decision Trees
- ### 1. Example: Rule-Based Classifier Geometry
  *   **Slides:** 4–5
  *   **Category:** Geometric Intuition
  *   **Problem:** Given a 2D feature space ($x_1, x_2$), how do the rules `IF x1 > 5 AND x2 > 6 THEN RED` translate visually and into a tree?
  *   **Solution:** The rules create **axis-aligned** (horizontal and vertical) cuts in the space. In tree format, $x_1 > 5$ becomes the root node. The "Yes" branch leads to a second node asking $x_2 > 6$. If "Yes", the leaf node is "RED".
- ### 2. Intuitive Example: Information Gain
  *   **Slide:** 12
  *   **Category:** Evaluating Splits
  *   **Problem:** You have a dataset of Green and Pink dots. Which test is more informative: Splitting over "Balance > 50K" or splitting over "Employed"?
  *   **Solution:** Splitting over **"Employed"** is more informative. The "Balance > 50K" split leaves both sides completely mixed (high entropy). The "Employed" split perfectly separates the Green and Pink dots (zero entropy in the child nodes).
- ### 3. Worked Example: Calculating Information Gain
  *   **Slides:** 15–16
  *   **Category:** Information Gain Math
  *   **Problem:** A parent node has 30 data points (16 Green, 14 Pink). Attribute A splits this into two child nodes: 
    *   Child $a_1$: 17 points (4 Green, 13 Pink)
    *   Child $a_2$: 13 points (12 Green, 1 Pink)
    Calculate the Information Gain.
  *   **Solution / Steps:**
    1.  **Parent Entropy:** $-(\frac{14}{30}\log_2\frac{14}{30}) - (\frac{16}{30}\log_2\frac{16}{30}) = 0.996$
    2.  **Child 1 ($a_1$) Entropy:** $-(\frac{13}{17}\log_2\frac{13}{17}) - (\frac{4}{17}\log_2\frac{4}{17}) = 0.787$
    3.  **Child 2 ($a_2$) Entropy:** $-(\frac{1}{13}\log_2\frac{1}{13}) - (\frac{12}{13}\log_2\frac{12}{13}) = 0.391$
    4.  **Weighted Average of Children:** $(\frac{17}{30} \times 0.787) + (\frac{13}{30} \times 0.391) = 0.615$
    5.  **Information Gain:** Parent $-$ Weighted Children $= 0.996 - 0.615 = \mathbf{0.38}$
- ### 4. Question 1: Which attribute has higher Information Gain?
  *   **Slides:** 17–19
  *   **Category:** Splitting Decision
  *   **Problem:** A dataset has 5 rows. Target is Yes/No (3 Yes, 2 No). Evaluate Attribute A vs. Attribute B.
    *   A splits into: $a_1$ (2 Yes, 1 No) and $a_2$ (1 Yes, 1 No).
    *   B splits into: $b_1$ (3 Yes, 0 No) and $b_2$ (0 Yes, 2 No).
  *   **Solution / Steps:**
    *   **Parent Entropy:** $H(Y) = 0.9710$
    *   **For A:** Child $a_1$ entropy is $0.9183$. Child $a_2$ entropy is $1.0$ (perfectly mixed).
        $Gain(D, A) = 0.9710 -[ (\frac{3}{5} \times 0.9183) + (\frac{2}{5} \times 1.0) ] = \mathbf{0.02}$
    *   **For B:** Child $b_1$ entropy is $0$ (pure). Child $b_2$ entropy is $0$ (pure). 
        $Gain(D, B) = 0.9710 -[ (\frac{3}{5} \times 0) + (\frac{2}{5} \times 0) ] = \mathbf{0.971}$
    *   **Conclusion:** Attribute B has much higher Information Gain and should be chosen as the split.
- ### 5. Example: Building the Tree (Play Tennis)
  *   **Slides:** 23–26
  *   **Category:** Recursive Splitting
  *   **Problem:** Given the "Play Tennis" dataset with 4 features (Outlook, Temperature, Humidity, Wind). Build the root node, and then build the next node for the "Sunny" branch.
  *   **Solution:**
    *   *Step 1 (Root):* Calculate IG for all 4. Outlook has the highest IG (0.246). **Outlook is the root.**
    *   *Step 2 (Recurse):* For the subset where Outlook="Sunny", calculate IG for the remaining 3 features. Humidity has the highest IG ($0.970$). **Humidity becomes the next node down the Sunny branch.**
- ### 6. The Problem with Information Gain (The Flaw)
  *   **Slides:** 27–28
  *   **Category:** Gain Ratio
  *   **Problem:** Calculate the Information Gain of the "Day" column (which contains unique IDs from 1 to 14). Why is this a problem?
  *   **Solution:** 
    *   The "Day" column splits the 14 rows into 14 separate leaf nodes, each containing exactly 1 item. 
    *   The entropy of each child is $0$. Therefore, $Gain(D, "Day") = 0.97 - 0 = \mathbf{0.97}$ (The maximum possible gain).
    *   *The Fix:* Use **Information Gain Ratio**, which divides the Gain by the "SplitInfo" (how heavily the data was fragmented). $IGR = 0.97 / 3.81 = \mathbf{0.255}$. This heavily penalizes the ID column.
- ### 7. Example: Splitting Continuous Attributes
  *   **Slides:** 30–34
  *   **Category:** Continuous Features
  *   **Problem:** How do you find the best value to split a continuous feature like $x_1$?
  *   **Solution / Steps:**
    1.  Sort the data points by $x_1$ from lowest to highest.
    2.  Calculate the midpoints between every adjacent pair of values: $x_{mid,1} = (x_i + x_{i+1})/2$.
    3.  Treat each midpoint as a binary split threshold (e.g., $x_1 < 2.2$ vs $x_1 \ge 2.2$).
    4.  Calculate the Information Gain for *every single midpoint*.
    5.  Pick the midpoint that yields the highest Information Gain.
- ### 8. After-Class Questions (Concepts)
  *   **Slide:** 53
  *   **Category:** Theory Review
  *   **Q1: What does "node impurity" mean?** It represents how mixed the classes are inside a single node. High impurity = mixed classes (high entropy). Low impurity = mostly one class.
  *   **Q2: Why do we need impurity measures?** To mathematically evaluate "how good" a proposed split is so the greedy algorithm can pick the best feature.
  *   **Q3: Example of a feature favored by IG but not useful?** An ID column (e.g., Social Security Number, Transaction ID).
  *   **Q4: Why are decision trees prone to overfitting?** Because they can recursively split until every leaf has exactly 1 sample, allowing them to perfectly memorize noise and outliers in the training data.
  *   **Q5: Two ways to control complexity?** 1. Pre-pruning (setting max depth, min samples per leaf). 2. Post-pruning (growing a full tree, then cutting back weak branches using a validation set).
  *   **Q6: Why are DTs considered interpretable?** Because they represent a series of human-readable "IF-THEN" rules (e.g., IF Age > 30 AND Income > 50k THEN Approve Loan).
  *   **Q7: Would you still choose a DT if a black-box model performs better?** Yes, if the domain requires explainability (e.g., healthcare, loan approvals due to legal/compliance reasons).
- ### 9. After-Class Questions (Further Algorithm Deep-Dive)
  *   **Slide:** 55
  *   **Category:** ID3 vs C4.5 vs CART
  *   **Q1: Why does ID3 not support continuous features?** ID3 creates a distinct branch for *every* unique value in a feature. For a continuous variable (like 75.43), almost every value is unique, causing the tree to instantly fragment into useless single-item leaves. It lacks the logic to find "midpoint thresholds".
  *   **Q2: How does C4.5 handle continuous features?** It sorts the continuous values, finds the midpoints between them, evaluates the Information Gain at every midpoint to find the best binary cut, and uses that cut as the rule.
  *   **Q3: Why does CART restrict splits to be binary?** To prevent data fragmentation. Splitting a node into many branches (e.g., 50 states) leaves very little data in each branch for subsequent splits. Binary splits keep the data pools larger and more statistically robust as you go deeper into the tree.
  
  ***
- ---
- ---
- # Lecture 8: Perceptron
- ### 1. Mathematical Example: Gradient Descent (1D)
  *   **Slide:** 28
  *   **Category:** Gradient Descent Basics
  *   **Problem:** Find the minimal value for the function $f(x) = x^2 - 4x + 1$. Assume an initial value $x_0 = 9$ and a learning rate $\alpha = 0.2$. Calculate the first update step.
  *   **Solution / Steps:**
    1.  **Find the Gradient (Derivative):** $f'(x) = 2x - 4$.
    2.  **Evaluate Gradient at $x_0$:** $f'(9) = 2(9) - 4 = 18 - 4 = 14$.
    3.  **Apply GD Formula:** $x_{new} = x_{old} - \alpha f'(x_{old})$
    4.  **Calculate:** $x_1 = 9 - 0.2(14) = 9 - 2.8 = \mathbf{6.2}$.
    *(The slide iterates this process down to $x_7 = 2.197$, slowly approaching the true minimum which is at $x=2$.)*
- ### 2. Walkthrough Example 1: Perceptron Update
  *   **Slides:** 38–40
  *   **Category:** Perceptron Learning Algorithm
  *   **Problem:** You have a dataset with three points:
    *   $x_1 =[3, 3]^T, y = +1$
    *   $x_2 = [4, 3]^T, y = +1$
    *   $x_3 = [1, 1]^T, y = -1$
    Given learning rate $\eta = 1$, initial weights $w_0 =[2, 1]^T$, and bias $b_0 = -0.5$. Evaluate the points and perform one weight update.
  *   **Solution / Steps:**
    1.  **Test $x_1$:** $w_0^T x_1 + b_0 = (2)(3) + (1)(3) - 0.5 = 8.5$. Since $y=+1$ and score $>0$, this is **Correct**.
    2.  **Test $x_2$:** $w_0^T x_2 + b_0 = (2)(4) + (1)(3) - 0.5 = 10.5$. **Correct**.
    3.  **Test $x_3$:** $w_0^T x_3 + b_0 = (2)(1) + (1)(1) - 0.5 = 2.5$. The score is positive, but the label is $y=-1$. **Misclassified!**
    4.  **Update Rule:** $w_{new} = w_{old} + \eta y_3 x_3$ and $b_{new} = b_{old} + \eta y_3$.
    5.  **Calculate $w_{new}$:** $[2, 1] + (1)(-1)[1, 1] = [2, 1] - [1, 1] = \mathbf{[1, 0]}$.
    6.  **Calculate $b_{new}$:** $-0.5 + (1)(-1) = \mathbf{-1.5}$.
- ### 3. Walkthrough Example 2: Perceptron Update (Different Init)
  *   **Slides:** 41–45
  *   **Category:** Perceptron Initialization Sensitivity
  *   **Problem:** Using the exact same data as Example 1, start with a different initialization: $w_0 = [0.5, -1]^T$ and $b_0 = 0$. What happens?
  *   **Solution:** 
    *   **Test $x_1$:** $(0.5)(3) + (-1)(3) + 0 = -1.5$. Label is $+1$. **Misclassified!**
    *   **Update $w_1$:** $[0.5, -1] + (1)(+1)[3, 3] = \mathbf{[3.5, 2]}$. Update $b_1 = 0 + 1 = \mathbf{1}$.
    *   *Takeaway:* Because the algorithm uses Stochastic Gradient Descent, changing the starting weights completely changes which points are misclassified first, leading to an entirely different final decision boundary line.
- ### 4. In-Class Participation Question (1)
  *   **Slide:** 47
  *   **Category:** Perceptron Limitations
  *   **Problem:** If the dataset is **not linearly separable**, what happens during Perceptron training?
    *   A. The algorithm always converges to a global minimum
    *   B. The algorithm may keep updating indefinitely
    *   C. The weights converge to zero
    *   D. The algorithm switches to logistic regression
  *   **Solution:** **B. The algorithm may keep updating indefinitely.** (Because there is no line that can yield 0 errors, the condition to stop updating is never met. It will thrash back and forth between different imperfect lines).
- ### 5. In-Class Participation Question (2)
  *   **Slide:** 48
  *   **Category:** Perceptron Convergence Theorem
  *   **Problem:** Assume two linearly separable datasets: Dataset A (large margin) and Dataset B (small margin). Which statement is TRUE?
    *   A. Perceptron converges in fewer updates on Dataset B
    *   B. Convergence speed is independent of margin
    *   C. Perceptron converges faster on Dataset A
    *   D. Perceptron always finds the maximum margin solution
  *   **Solution:** **C. Perceptron converges faster on Dataset A.** (According to the Convergence Theorem $k \le R^2/\gamma^2$, a larger margin $\gamma$ results in a smaller maximum number of steps $k$).
- ### 6. The XOR Problem (Limitation Example)
  *   **Slide:** 49
  *   **Category:** Linear Separability
  *   **Problem:** Try to classify the XOR logic gate: (0,0)->0; (0,1)->1; (1,0)->1; (1,1)->0.
  *   **Solution:** You physically cannot draw a single straight line that separates the 1s from the 0s. The Perceptron fails completely here. This requires a Multi-Layer Perceptron (Neural Network) or a Kernel transformation.
- ### 7. After-Class Questions (Concepts)
  *   **Slide:** 52
  *   **Category:** Theory Review
  *   **Q1: Why does the Perceptron update its weights only when a sample is misclassified?**
    *   *Answer:* The loss function for a correctly classified point is exactly 0. The derivative (gradient) of 0 is 0. If you multiply the learning rate by 0, the weight change is 0.
  *   **Q2: Explain why the Perceptron does not provide probabilistic outputs.**
    *   *Answer:* It applies a hard "Sign" function at the very end. If $w^Tx+b$ is 0.001, it outputs "+1". If it is 10,000, it outputs "+1". It has no mechanism (like a Sigmoid curve) to map that raw score to a confidence percentage.
  *   **Q3: What happens if the data is not linearly separable?**
    *   *Answer:* It will loop infinitely (or hit the `max_iter` cap) because it will never achieve 0 training errors.
  *   **Q4: Why does Perceptron not necessarily find the maximum margin solution?**
    *   *Answer:* Its stopping condition is simply `error == 0`. It stops the very millisecond it finds *any* line that separates the classes, even if that line is touching a data point (margin $\approx$ 0).
  *   **Q5: Explain why the Perceptron loss is convex but non-smooth.**
    *   *Answer:* It uses a variation of Hinge Loss (a "V" shape). The walls of the "V" are straight (convex), meaning it won't get stuck in local minima. However, at the exact point where a misclassification becomes a correct classification (the bottom point of the V), there is a sharp "kink". You cannot calculate a traditional derivative at a sharp corner, making it "non-smooth."
  
  ***
- ---
- ---
- # Lectures 9 & 10: Logistic Regression & SVM
- ### 1. Question: Geometric Margin Scaling
  *   **Slide:** 31
  *   **Category:** SVM Geometry
  *   **Problem:** The geometric margin is defined as $\frac{y_i(w^T x_i + b)}{||w||}$. If we scale the weights and bias $(w, b)$ by a constant factor $c$, what happens to the margin?
  *   **Solution:** The margin remains **unchanged**. Mathematically, the $c$ factored out of the numerator cancels exactly with the $|c|$ factored out of the denominator norm $||cw||$. Geometrically, $w^T x + b = 0$ and $10w^T x + 10b = 0$ define the exact same physical line.
- ### 2. Question: Fixing a Quantity (Constraint Selection)
  *   **Slide:** 32–33
  *   **Category:** SVM Optimization Setup
  *   **Problem:** Since the problem is scale-invariant, we have an infinite number of $w$ and $b$ combinations that represent the exact same margin. To solve the math, we must lock (fix) one quantity. Which is the better choice: 
    *   A) Fix $||w|| = 1$
    *   B) Fix $y_i(w^T x_i + b) = 1$
  *   **Solution:** **B. Fix $y_i(w^T x_i + b) = 1$.** If you try to fix $||w|| = 1$, the mathematical search space becomes the surface of a sphere, which is **not a convex set**. Optimization algorithms get stuck easily on non-convex surfaces. Fixing the functional margin to 1 keeps the problem convex (specifically, a Quadratic Programming problem) which guarantees we can find a global minimum.
- ### 3. Example: Lagrange Multipliers
  *   **Slide:** 43
  *   **Category:** Constrained Optimization Math
  *   **Problem:** Find the minimum of $f(x,y) = x + y$ subject to the constraint $x^2 + y^2 = 1$.
  *   **Solution / Steps:**
    1.  **Form the Lagrangian:** $\mathcal{L} = x + y + \beta(x^2 + y^2 - 1)$
    2.  **Take partial derivatives and set to 0:**
        *   $\frac{\partial \mathcal{L}}{\partial x} = 1 + 2\beta x = 0 \implies x = -\frac{1}{2\beta}$
        *   $\frac{\partial \mathcal{L}}{\partial y} = 1 + 2\beta y = 0 \implies y = -\frac{1}{2\beta}$
    3.  **Plug back into the constraint:**
        *   $(-\frac{1}{2\beta})^2 + (-\frac{1}{2\beta})^2 = 1 \implies \frac{2}{4\beta^2} = 1 \implies \beta = \pm \frac{1}{\sqrt{2}}$
    4.  **Find the minimum:** We want the smallest $x+y$, so we need negative values. We use $\beta = \frac{1}{\sqrt{2}}$, which gives **$x = -\frac{1}{\sqrt{2}}$ and $y = -\frac{1}{\sqrt{2}}$**.
- ### 4. Mathematical Walkthrough: Solving SVM by Hand
  *   **Slides:** 52–54
  *   **Category:** The Dual Problem
  *   **Problem:** Solve the SVM optimization for 3 points: $x_1=[3,3]^T (+1)$, $x_2=[4,3]^T (+1)$, $x_3=[1,1]^T (-1)$.
  *   **Solution / Steps:**
    1.  Set up the Dual constraint: $\sum \alpha_i y_i = 0 \implies \alpha_1(1) + \alpha_2(1) + \alpha_3(-1) = 0 \implies \alpha_3 = \alpha_1 + \alpha_2$.
    2.  Expand the Dual Lagrangian objective function using the dot products of the points.
    3.  Substitute $\alpha_3$ out of the equation so everything is in terms of $\alpha_1$ and $\alpha_2$.
    4.  Take the derivatives with respect to $\alpha_1$ and $\alpha_2$ and set to 0. 
    5.  Solve the system of equations: **$\alpha_1 = 1/4$, $\alpha_2 = 0$, $\alpha_3 = 1/4$**.
    6.  Calculate final weights: $w^* = \sum \alpha_i y_i x_i = \frac{1}{4}(1)[3,3] + 0 + \frac{1}{4}(-1)[1,1] = \mathbf{[0.5, 0.5]^T}$.
    7.  Calculate bias using support vector $x_1$: $b^* = y_1 - w^T x_1 = 1 - (1.5 + 1.5) = \mathbf{-2}$.
- ### 5. In-Class Participation Questions (Q1 & Q2)
  *   **Slides:** 56–57
  *   **Category:** Soft-Margin Intuition
  *   **Problem:** The data is not linearly separable (there are a few outliers). Which boundary is conceptually better: (A) A boundary with a wide margin that allows a couple of errors, or (B) A highly slanted boundary with a tiny margin that separates everything?
  *   **Solution:** **(A) is better.** This illustrates the need for a **Soft-Margin SVM**. Allowing a few misclassified points (slack) to maintain a wide, robust margin generally prevents overfitting and performs better on unseen data.
- ### 6. In-Class Participation Question (Q3)
  *   **Slide:** 65
  *   **Category:** Soft-Margin Hyperparameter ($C$)
  *   **Problem:** When the parameter $C$ is set to infinite, which of the following holds true?
    *   A) The optimal hyperplane if it exists, will be the one that completely separates the data.
    *   B) The soft-margin classifier will separate the data.
    *   C) None of the above.
  *   **Solution:** **A.** In the formula $\frac{1}{2}||w||^2 + C \sum \xi_i$, the $C$ parameter represents the penalty for making a mistake. If $C$ is infinity, even a microscopic error ($\xi$) results in an infinite penalty. Therefore, the algorithm acts exactly like a Hard-Margin SVM and refuses to allow any errors.
- ### 7. In-Class Participation Question (Q4)
  *   **Slides:** 72–73
  *   **Category:** Feature Mapping / The Kernel Trick
  *   **Problem:** You have 2D data that is not linearly separable (Red dots are on the X and Y axes, Green dots are in the quadrant space). Which 3D mapping function $Z$ will make it separable by a flat plane?
    *   (A) $Z = X + Y$
    *   (B) $Z = X \times Y$
    *   (C) $Z = X^2$
  *   **Solution:** **(B) $Z = X \times Y$.** The red dots lie exactly on the axes, meaning either their X is 0 or Y is 0. If you multiply them, their Z coordinate will be 0. The green dots are off the axes, so their $X \times Y$ will be $> 0$. You can now slice a flat plane between $Z=0$ and $Z>0$.
- ### 8. Example: Deriving the Kernel Mapping $\phi(x)$
  *   **Slide:** 76
  *   **Category:** Kernel Functions
  *   **Problem:** You are given 3D vectors $x =[x_1, x_2, x_3]$. You are using a quadratic Polynomial Kernel: $K(x,y) = (x^T y)^2$. Find the actual expanded feature mapping $\phi(x)$ that satisfies $K(x,y) = \phi(x)^T \phi(y)$.
  *   **Solution:** 
    *   Expand the square: $(x_1 y_1 + x_2 y_2 + x_3 y_3)^2$
    *   This gives 9 terms: $x_1^2 y_1^2 + x_1 x_2 y_1 y_2 + x_1 x_3 y_1 y_3 \dots$
    *   Separate the $x$ terms from the $y$ terms into a vector:
    *   $\mathbf{\phi(x) =[x_1^2, x_1 x_2, x_1 x_3, x_2 x_1, x_2^2, x_2 x_3, x_3 x_1, x_3 x_2, x_3^2]^T}$
    *   *Takeaway:* Computing the 9-dimensional dot product takes lots of math. The Kernel function achieves the exact same result just by squaring a single number in 3D space.
- ### 9. After-Class Questions (1): Hand-Calculating Toy Dataset
  *   **Slide:** 83
  *   **Category:** Hard-Margin SVM Geometry
  *   **Problem:** You have 3 points: $x_1=(2,2)$ label $+1$; $x_2=(4,4)$ label $+1$; $x_3=(2,0)$ label $-1$. Determine the maximum-margin hyperplane, $w$, and $b$ using geometric reasoning.
  *   **Solution:** 
    *   *Plot:* $x_1(2,2)$ and $x_3(2,0)$ face each other vertically. They are the closest opposing points. $x_2$ is further away.
    *   *Hyperplane:* The line must pass exactly halfway between $(2,2)$ and $(2,0)$. The midpoint is $(2,1)$. A line dividing them is horizontal: **$y = 1$** (or $x_2 - 1 = 0$).
    *   *Weights:* From $x_2 - 1 = 0$, we can deduce **$w = [0, 1]$** and **$b = -1$**. 
    *   *Support Vectors:* **$x_1$ and $x_3$**. We know this because plugging them into the constraint $y_i(w^T x_i + b)$ yields exactly $1$. $x_2$ yields $3$, meaning it is far away from the margin and not a support vector.
- ### 10. After-Class Questions (2): Soft-Margin & C
  *   **Slide:** 84
  *   **Category:** Hyperparameter Tuning
  *   **Problem:** Explain what happens to a Soft-Margin SVM when $C$ is very small, when $C$ is very large, and how it controls the tradeoff.
  *   **Solution:**
    *   **Very small $C$:** The penalty for misclassification is tiny. The model focuses entirely on making the margin as wide as possible, leading to a "soft" boundary that ignores outliers (High Bias / Underfitting).
    *   **Very large $C$:** The penalty is massive. The model contorts the margin to perfectly classify every single training point, leading to a very narrow, "hard" boundary (High Variance / Overfitting).
    *   **Tradeoff:** $C$ scales the Slack Variable penalty ($\sum \xi_i$). It acts as the mathematical scale balancing the desire for a wide margin vs. the desire for zero training errors.
  
  ***
- ---
- ---
- # Lecture 11: Ensemble Method
- ### 1. Real-World Examples: The Power of Ensembles
  *   **Slides:** 3–6
  *   **Category:** Motivation / Application
  *   **Problem:** How do top data scientists win massive Kaggle competitions (e.g., Otto Group Product Classification, CrowdFlower Search Results Relevance)?
  *   **Solution:** The winning solutions almost universally rely on **Ensemble Learning**. Instead of using a single Logistic Regression or SVM model, they combine hundreds of models (like Random Forests or XGBoost) to create a single, highly robust, and accurate predictor.
- ### 2. Toy Example: Intuition of Boosting 
  *   **Slides:** 20–24
  *   **Category:** Sequential Error Correction
  *   **Problem:** You have a 2D dataset with `+` and `-` points that cannot be separated by a single horizontal or vertical line (high bias/underfitting). How does Boosting fix this?
  *   **Solution:** 
    *   *Round 1:* Draw a vertical line ($h_1$). It gets most right, but misses three `+` points. We increase the "weight" (visual size) of those three points.
    *   *Round 2:* Draw a new vertical line ($h_2$) specifically targeting those three large `+` points. It gets them, but misses some `-` points. We increase the weight of the missed `-` points.
    *   *Round 3:* Draw a horizontal line ($h_3$) to capture the heavy `-` points.
    *   *Final:* Combine $h_1, h_2,$ and $h_3$ into a final classifier ($H_{final}$). The overlay of these three simple "stumps" perfectly bounds the classes in a non-linear way.
- ### 3. Walkthrough Example: AdaBoost Math (1) to (11)
  *   **Slides:** 30–40
  *   **Category:** AdaBoost Calculations
  *   **Problem:** You are given 10 data points on a 1D line ($x_i = 0$ to $9$). The labels are $y_i =[+1, +1, +1, -1, -1, -1, +1, +1, +1, -1]$. Initially, all weights are $D_1 = 0.1$. Train an AdaBoost ensemble using simple threshold classifiers (stumps).
  *   **Solution / Steps:**
    *   **Iteration 1 (Slides 32-35):**
        *   *Best Split:* $x < 2.5$ (Predicts $+1$ for $x < 2.5$, $-1$ for $x \ge 2.5$).
        *   *Mistakes:* Points at $x=6, 7, 8$ (they are $+1$, but fall in the $-1$ zone).
        *   *Error ($\epsilon_1$):* Sum of weights of mistakes = $0.1 + 0.1 + 0.1 = \mathbf{0.3}$.
        *   *Alpha ($\alpha_1$):* $\frac{1}{2} \ln(\frac{1-0.3}{0.3}) = \mathbf{0.4236}$.
        *   *Update Weights ($D_2$):* Mistaken points (6, 7, 8) become heavier ($\approx 0.166$). Correct points become lighter ($\approx 0.071$).
    *   **Iteration 2 (Slides 36-37):**
        *   *Best Split:* $x < 8.5$.
        *   *Mistakes:* Points at $x=0, 1, 2$ (they are $+1$, but the new split classifies them differently based on the new weights). 
        *   *Error ($\epsilon_2$):* Sum of *new* weights of mistakes = $0.071 + 0.071 + 0.071 = \mathbf{0.2143}$.
        *   *Alpha ($\alpha_2$):* $\frac{1}{2} \ln(\frac{1-0.2143}{0.2143}) = \mathbf{0.6496}$.
        *   *Update Weights ($D_3$):* Re-weight based on $\alpha_2$.
    *   **Iteration 3 (Slides 38-39):**
        *   *Best Split:* $x \ge 5.5$.
        *   *Mistakes:* Points at $x=3, 4, 5, 9$.
        *   *Error ($\epsilon_3$):* Sum of weights = $\mathbf{0.1820}$.
        *   *Alpha ($\alpha_3$):* $\mathbf{0.7514}$.
    *   **Final Classifier (Slide 40):** 
        $f(x) = 0.4236 \cdot h_1(x) + 0.6496 \cdot h_2(x) + 0.7514 \cdot h_3(x)$
- ### 4. Q1: AdaBoost True/False
  *   **Slide:** 41
  *   **Category:** AdaBoost Theory
  *   **Problem:** Which of the following statements about AdaBoost is TRUE?
    *   A. AdaBoost trains all weak learners independently and combines their predictions equally.
    *   B. AdaBoost assigns higher weights to misclassified samples, forcing the next weak learner to focus on them.
    *   C. AdaBoost is a bagging-based ensemble method that reduces variance.
    *   D. AdaBoost works only with decision trees and cannot use other weak learners.
  *   **Solution:** **B. AdaBoost assigns higher weights to misclassified samples...** (A is Bagging; C is false because AdaBoost reduces Bias, not Variance; D is false because Boosting can use any algorithm as a weak learner, though trees are most common).
- ### 5. Q2: Distinguishing AdaBoost from Random Forest
  *   **Slide:** 42
  *   **Category:** Bagging vs. Boosting
  *   **Problem:** Which of the following statements correctly distinguishes AdaBoost from Random Forest?
    *   A. AdaBoost is a boosting algorithm that trains weak learners sequentially, while Random Forest is a bagging algorithm that trains decision trees independently in parallel.
    *   B. Both use the same weighting mechanism.
    *   C. Random Forest tends to overfit more than AdaBoost.
    *   D. Both train models in parallel without dependencies.
  *   **Solution:** **A.** AdaBoost is sequential (each tree depends on the errors of the last). Random Forest is parallel (every tree is grown independently on a random subset of data).
- ### 6. After-Class Questions: Conceptual Understanding
  *   **Slide:** 45
  *   **Category:** Theory Review
  *   **Q1: Explain the main idea of an ensemble method.** 
    *   *Answer:* Combining multiple "weak" base models to create a single "strong" model that performs better, reduces overfitting, and generalizes better than any individual model inside it.
  *   **Q2: Compare Bagging and Boosting.**
    *   *Answer:* 
        *   **Sampling:** Bagging uses uniform random sampling with replacement (Bootstrapping). Boosting uses weighted sampling where difficult, misclassified points get higher probabilities of being picked.
        *   **Combining:** Bagging uses simple averaging or majority voting. Boosting uses a weighted sum (models with lower error rates get a higher voting weight $\alpha$).
        *   **Primary Goal:** Bagging reduces **Variance** (fixes overfitting). Boosting reduces **Bias** (fixes underfitting).
- ### 7. After-Class Questions: Bias–Variance Perspective
  *   **Slide:** 46
  *   **Category:** Bias-Variance Tradeoff
  *   **Problem:** Consider a decision tree with *unlimited depth* as the base learner.
    1. **Describe its bias/variance.** Low Bias (it perfectly fits the training data) and High Variance (it completely memorizes the noise/overfits).
    2. **How does Bagging (Random Forest) change this?** It drastically reduces the High Variance by averaging many deep, overfitted trees together, smoothing out the noise while maintaining the Low Bias.
    3. **How does Boosting change this?** Boosting is actually a *bad* idea for unlimited depth trees. Boosting reduces Bias by adding complexity. If you start with a tree that already has unlimited complexity, boosting will immediately cause severe overfitting.
    4. **When is Boosting more likely to overfit than Bagging?** When the dataset contains high amounts of random noise or mislabeled outliers. Boosting will obsess over those outliers, assigning them massive weights and distorting the decision boundary. Bagging just averages the outliers away.
- ### 8. After-Class Questions: Bagging vs. Boosting Scenarios
  *   **Slide:** 47
  *   **Category:** Applied Algorithm Selection
  *   **Problem:** State whether Bagging or Boosting is more appropriate:
    1. **The dataset is small and noisy.** $\rightarrow$ **Bagging.** It is robust to noise and won't overfit the small sample size as easily.
    2. **The base model is highly unstable.** $\rightarrow$ **Bagging.** Bagging is designed specifically to lower variance and stabilize unstable models.
    3. **The model consistently underfits the data.** $\rightarrow$ **Boosting.** Underfitting implies High Bias. Boosting sequentially adds complexity to fix high bias.
    4. **Interpretability is a key concern.** $\rightarrow$ **Neither.** Both are "black box" models. You should use a single, simple Decision Tree or Logistic Regression if you need to explain your exact reasoning to a human.
  
  ***
- ---
- ---
- # Lectures 12 & 13: Clustering
- ### 1. Visual Example: The Geometry of Distance Metrics
  *   **Slide:** 19
  *   **Category:** Distance Metrics (Euclidean vs. Cosine)
  *   **Problem:** You have three points plotted on a graph: A, B, and C. Points A and B are physically close to each other. Points A and C lie on the exact same straight line originating from the origin $(0,0)$, but C is further out. Evaluate their similarity using Euclidean distance vs. Cosine distance.
  *   **Solution:** 
    *   **Euclidean distance:** A is closer to B. ($d(A,B) < d(A,C)$).
    *   **Cosine distance:** A is closer to C. Because they lie on the same line from the origin, the angle between them is 0. ($d(A,B) > d(A,C)$).
    *   *Takeaway:* The metric you choose completely changes which points are grouped together in a cluster.
- ### 2. Example: Multiple Random Initializations
  *   **Slides:** 59–62
  *   **Category:** K-Means Optimization
  *   **Problem:** Because K-Means is prone to getting stuck in local optima, you run the algorithm 50 times with different random starting centroids. Trial 1 yields a minimized cost function $J = 23.5$. Trial 2 yields $J = 176.2$. Trial 50 yields $J = 39.1$. Which clustering result should you use?
  *   **Solution:** You pick the specific clustering assignment from the trial which gave the **lowest overall $J$ value** (in this case, Trial 1). The massive variance in $J$ between trials proves that K-Means is highly sensitive to where the initial centroids are placed.
- ### 3. In-Class Question: K-Means Cost Function Anomaly
  *   **Slide:** 69
  *   **Category:** K-Means Hyperparameter ($K$)
  *   **Problem:** Suppose you run k-means using $k = 3$ and $k = 5$. You find that the cost function $J$ (sum of squared distances) is much higher for $k = 5$ than for $k = 3$. What can you conclude?
    *   A) This is mathematically impossible. There must be a bug in the code.
    *   B) The correct number of clusters is $k = 3$.
    *   C) In the run with $k = 5$, k-means got stuck in a bad local minimum. You should try re-running k-means with multiple random initializations.
    *   D) In the run with $k = 3$, k-means got lucky.
  *   **Solution:** **C.** Mathematically, increasing $K$ should always *decrease* (or equal) the overall cost $J$, because having more centroids means points don't have to travel as far to reach one. If $K=5$ has a higher cost, it means the random initialization was extremely poor, and the algorithm converged to a bad **local minimum**.
- ### 4. Walkthrough Example: GMM in One Dimension (The EM Algorithm)
  *   **Slides:** 85–94
  *   **Category:** Gaussian Mixture Models (Soft Clustering)
  *   **Problem:** Assume 1-D data (points $x_1$ through $x_{10}$) comes from 2 overlapping Gaussian models (distributions). Which source does each data point come from? Walk through the EM algorithm to find the best-fit models to maximize the likelihood of the data.
  *   **Solution / Steps:**
    *   **Step 1: Initialization (Slide 88).** Start with two random Gaussians. Assign random means ($\mu_1, \mu_2$), random variances ($\sigma_1, \sigma_2$), and random prior probabilities ($p(C_1), p(C_2)$).
    *   **Step 2: E-Step / Expectation (Slide 89).** Instead of assigning a point to just one cluster, assign a *probabilistic membership* to all input samples using Bayes' Theorem. 
        *   For point $x_i$, the probability it belongs to Cluster 1 is:
            $$p_{1i} = \frac{P(x_i | C_1)P(C_1)}{P(x_i|C_1)P(C_1) + P(x_i|C_2)P(C_2)}$$
    *   **Step 3: M-Step / Maximization (Slide 90).** Re-estimate the Gaussian parameters using the probabilities calculated in the E-step as "weights".
        *   *New Mean ($\mu_1$):* The weighted average of all points (weighted by $p_{1i}$).
        *   *New Variance ($\sigma_1$):* The weighted variance of all points.
        *   *New Prior ($P(C_1)$):* The average of the probabilities for Cluster 1 across all points.
    *   **Step 4: Iterate (Slides 92-94).** Repeat the E and M steps until the parameters stop changing (convergence). The Gaussians will slowly slide over the dense areas of the points until they perfectly map the underlying distribution.
  
  ***
- ---
- ---
- # Lecture 14: Dimensionality Reduction & t-SNE
- ### 1. Mathematical Example: The Curse of Dimensionality (The Small Cube)
  *   **Slides:** 6–12
  *   **Category:** High-Dimensional Sparsity
  *   **Problem:** Let us consider a uniform distribution of $N=100$ data points over a large unit cube (length $= 1$, volume $= 1^d = 1$). We want to define a "small cube" neighborhood that captures exactly $K=3$ points (which is 3% of the data). How large must the side length ($l$) of this small cube be in 1, 3, 10, and 1000 dimensions?
  *   **Solution / Steps:**
    1.  **The Concept:** The probability of a point falling inside the small cube is simply the small cube's volume: $l^d$.
    2.  **The Empirical Ratio:** We want this volume to equal our target percentage of points: $K/N = 3/100 = 0.03$.
    3.  **The Formula:** $l^d \approx 0.03 \implies l \approx (0.03)^{\frac{1}{d}}$
    4.  **Calculations:**
        *   **$d = 1$:** $l = (0.03)^{\frac{1}{1}} = \mathbf{0.03}$ (Only covers 3% of the axis).
        *   **$d = 3$:** $l = (0.03)^{\frac{1}{3}} \approx \mathbf{0.311}$ (Covers 31% of the axis).
        *   **$d = 10$:** $l = (0.03)^{\frac{1}{10}} \approx \mathbf{0.704}$ (Covers 70% of the axis).
        *   **$d = 1000$:** $l = (0.03)^{\frac{1}{1000}} \approx \mathbf{0.9965}$ (Covers 99.65% of the axis).
    *   **The Punchline:** In high dimensions, to capture even a tiny fraction of your "closest" neighbors, you have to stretch your search across almost the entire dataset. "Local neighborhoods" cease to exist, and all points sit far away from each other near the edges of the space.
- ### 2. Walkthrough Example: PCA Mean-Centering & Projection
  *   **Slides:** 19–23
  *   **Category:** Principal Component Analysis (PCA)
  *   **Problem:** You are given 5 data points in a 2D space: $p_1(1,1), p_2(2,2), p_3(3,3), p_4(7,7), p_5(10,10)$. Explain the first steps of PCA geometrically.
  *   **Solution / Steps:**
    1.  **Calculate the Mean:**
        *   $\bar{x}_1 = (1+2+3+7+10) / 5 = \mathbf{4.6}$
        *   $\bar{x}_2 = (1+2+3+7+10) / 5 = \mathbf{4.6}$
    2.  **Mean-Center the Data:** Subtract the mean from every point to shift the cluster to the origin $(0,0)$.
        *   New $p_1 = (1-4.6, 1-4.6) = \mathbf{(-3.6, -3.6)}$
        *   New $p_5 = (10-4.6, 10-4.6) = \mathbf{(5.4, 5.4)}$
    3.  **Find the Projection (Slide 23):** PCA looks for the line passing through the origin that achieves two simultaneous goals:
        *   **Maximize:** The variance (spread) of the points along the line (maximize $\sum b_i^2$).
        *   **Minimize:** The projection error (the perpendicular distance the points "drop" to hit the line) (minimize $\sum d_i^2$).
        *   *Result:* Because the points lie perfectly on the $x_1 = x_2$ diagonal, PCA will draw PC1 perfectly through them, resulting in $0$ projection error.
- ### 3. Example: PCA Limitation (The Swiss Roll)
  *   **Slide:** 25
  *   **Category:** Linear vs. Non-Linear Manifolds
  *   **Problem:** A dataset forms a "Swiss Roll" (a 2D sheet rolled up into a 3D spiral). Why does PCA fail to reduce this to 2D properly?
  *   **Solution:** PCA is a **linear** method. It tries to preserve *global* variance by simply squashing/flattening the data along an axis. Doing this to a Swiss roll crushes layers together, falsely making points that were on opposite ends of the rolled-up sheet look like close neighbors. (This motivates the need for non-linear methods like t-SNE).
- ### 4. Conceptual Problem: The "Crowding Problem"
  *   **Slides:** 37–38
  *   **Category:** t-SNE Mechanics
  *   **Problem:** SNE uses a Gaussian (Normal) distribution to calculate probabilities in the high-dimensional space. Why does t-SNE switch to a **Student's t-distribution** for the low-dimensional space?
  *   **Solution:** Because of the **Crowding Problem**. When projecting from 1,000 dimensions down to 2 dimensions, there is simply not enough physical volume (area) to accommodate all the moderately-spaced points. If a Gaussian distribution is used in 2D, these points will "crowd" and crush into each other in the center. The Student's t-distribution has a **"fatter tail."** This mathematical property forces points that were moderately far in high-D to be pushed significantly further apart in 2D, preserving distinct clusters.
- ### 5. Conceptual Example: Perplexity Tuning
  *   **Slides:** 43–44
  *   **Category:** t-SNE Hyperparameters
  *   **Problem:** When tuning t-SNE, how does the `perplexity` hyperparameter (which dictates the assumed size of local neighborhoods) alter the output visualization?
  *   **Solution:** 
    *   **Too Small (e.g., Perplexity = 2):** The algorithm focuses strictly on immediate local structure. It ignores the big picture, resulting in **over-segmentation** (shattering a single natural cluster into dozens of tiny, meaningless shards).
    *   **Too Large (e.g., Perplexity = 100):** The algorithm focuses too much on global structure. It acts like PCA, bringing everything together and causing **over-merging** of distinct clusters into a single blob.
    *   *(Rule of thumb: Usually keep it between 5 and 50 depending on dataset size).*
  
  ***
- ---
- ---
-