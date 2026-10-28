## 1. What is an Anomaly? (Slides 2–9)
  An anomaly (or outlier) is a data point that is considerably different from the remainder of the data. 
  
  **The Three Causes of Anomalies:**
  1.  **Data from different classes:** You are scanning oranges, and a grapefruit accidentally rolls down the conveyor belt. 
  2.  **Natural variation:** Someone who is 7 feet tall (they belong to the normal human class, but lie at the extreme edge of the distribution).
  3.  **Data errors:** A typo in a database stating a 2-year-old child weighs 200 pounds.
  
  **Crucial Distinction: Noise vs. Anomaly (Slide 9)**
  This is a highly testable concept.
  *   **Noise:** Random fluctuations, static, or minor measurement errors. It has *no underlying pattern or significance*. Our goal in data mining is usually to **ignore or smooth out** noise.
  *   **Anomaly:** A meaningful, systematic deviation. It indicates a rare event, a system fault, or a medical crisis. Our goal here is to **flag and investigate** it.
  *   *Analogy:* Static on a heart rate monitor is noise. A sudden, massive spike representing a heart attack is an anomaly.
## 2. The Three Types of Outliers (Slide 15)
  Anomalies can manifest in different structural ways:
  1.  **Point Anomalies:** A single data point is bizarre compared to the rest of the dataset. (e.g., A \$10,000 credit card transaction on a card that usually spends \$20).
  2.  **Contextual Anomalies:** The value itself is normal, but it is abnormal *for that specific context*. (e.g., A temperature of 35°F is normal in January, but an anomaly in July in Missouri).
  3.  **Collective Anomalies:** A single point isn't weird on its own, but a *group* of points acting together forms an anomaly. (e.g., Someone failing a password login once is normal. Failing it 500 times in 2 seconds is a collective anomaly/cyberattack).
## 3. The Three Learning Settings (Slides 11–13)
  How we build an anomaly detector depends entirely on what kind of labels we have.
### A. Supervised Learning
  *   **The Setup:** You have a dataset where every row is perfectly labeled as "Normal" or "Anomaly."
  *   **The Method:** You just train a standard classifier (like Random Forest or SVM).
  *   **The Flaw:** Anomalies are incredibly rare and hard to label. You suffer from severe class imbalance, and the model can only catch *known* types of anomalies it was trained on.
### B. Semi-Supervised Learning (Most Common)
  *   **The Setup:** You only have a dataset of **Normal Data** during training. 
  *   **The Method:** You train a model to perfectly understand "normality" (e.g., an Autoencoder or One-Class SVM). During testing, if a new point doesn't fit the "normal" profile, it gets flagged.
  *   **The Advantage:** You don't need expensive anomaly labels, and you can catch brand-new, never-before-seen anomalies.
### C. Unsupervised Learning
  *   **The Setup:** You just have a giant pile of raw, unlabeled data. 
  *   **The Assumption:** You assume that normal data makes up the vast majority (99%) and is tightly grouped, while anomalies are **rare** and **different** from the majority.
  *   **The Method:** You use clustering or distance-based algorithms (like K-Means or K-Nearest Neighbors) to find the isolated points.
  
  ***
# Problem Solving & Concept Check
  
  **1. Noise vs. Anomaly:**
  You are analyzing the stock price of a company. The price fluctuates by a few cents every second due to automated trading bots. Suddenly, the price drops 40% in one minute due to a scandal breaking on the news. 
  *   Which of these is the noise, and which is the anomaly?
  *   *Answer:* The cent-by-cent fluctuation is **Noise** (you want to smooth this out with a moving average). The 40% drop is the **Anomaly** (you want your algorithm to trigger an alert).
  
  **2. Identifying the Outlier Type:**
  An employee at a company usually downloads 5 to 10 PDF documents a day. One Tuesday, the employee downloads 3,000 confidential PDF documents over a span of two hours. 
  *   Is this a Point, Contextual, or Collective anomaly?
  *   *Answer:* **Collective Anomaly.** Downloading a PDF is a normal action (not a point anomaly). However, the *sequence/collection* of 3,000 downloads in rapid succession forms a major security anomaly.
  
  **3. Choosing the Learning Setting:**
  You are hired by a manufacturing plant to predict when their main generator is going to break down. You have 5 years of sensor data from the generator running perfectly, but it has never actually broken down yet, so you have zero examples of a "failure state." 
  *   Should you use Supervised, Semi-Supervised, or Unsupervised learning?
  *   *Answer:* **Semi-Supervised.** Because you only have "Normal" data, you will train a model to learn exactly what a healthy generator looks like. If the sensors start reporting data that deviates from this profile, the model will flag it as an anomaly. 
  
  ***