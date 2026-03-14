## 1. The "Algorithm-First" Fallacy
**Slide Context:** three examples of why applying an algorithm immediately leads to failure.
**Comprehensive Notes:**
A common mistake for beginners is to think the "Data Mining" happens when you run the code (the algorithm). However, algorithms have mathematical assumptions. If your data violates those assumptions, the output is "garbage."

*   **Example 1: The ZIP Code Problem (Semantic Error)**
  *   *Mistake:* Using k-means on ZIP codes.
  *   *Why it fails:* k-means relies on calculating a "mean" (average). While a ZIP code looks like a number (65401), it is mathematically **Nominal**. Adding two ZIP codes or averaging them produces a number that does not exist in reality. 
  *   *The "Fill-in" Knowledge:* This highlights the difference between **Value** and **Meaning**. An algorithm sees a float/integer; a data scientist must see the *entity* it represents.

*   **Example 2: Ignoring Time (Assumption Error)**
  *   *Mistake:* Treating time-series rows as independent.
  *   *Why it fails:* Most standard algorithms assume data is **I.I.D. (Independent and Identically Distributed)**. In time-series, today's energy consumption is highly dependent on yesterday's. If you shuffle the rows, you lose the "Seasonality" (e.g., energy spikes every evening) and the "Trend" (e.g., energy use grows every year).

*   **Example 3: Class Imbalance (Evaluation Error)**
  *   *Mistake:* Evaluating a fraud model using only "Accuracy."
  *   *Why it fails:* In a dataset with 0.5% fraud, a "dumb" model that always says "No Fraud" is 99.5% accurate but finds 0% of the actual fraud. 
  *   *The "Fill-in" Knowledge:* This introduces the need for different metrics like **Precision, Recall, and F1-Score**, which we will likely cover later in the course.
- ## 2. Core Philosophy: Data-Driven vs. Algorithm-Driven
  **Slide Context:** 70–80% of effort is in data understanding.
  **Comprehensive Notes:**
  *   **The GIGO Principle:** "Garbage In, Garbage Out." No amount of "Algorithmic Magic" (even Deep Learning) can fix fundamentally broken or misinterpreted data.
  *   **Data understanding determines:**
    1.  **Operations:** Can I add these? Can I rank these?
    2.  **Validity:** Is k-means appropriate for this data type?
    3.  **Trust:** If the sensor was broken 20% of the time, can I trust the final prediction?
- ## 3. The "Building Blocks" of a Dataset
  To speak the language of data mining, you must know the synonyms for the two primary components of a dataset.
- ### A. The Data Object (The "Rows")
  A dataset is a collection of **Data Objects**. These represent the individual entities you are studying.
  *   **Standard Synonyms:** Record, Point, Vector, Case, Sample, Entity, Instance.
  *   **Concept:** If you are looking at a spreadsheet, one row = one object.
- ### B. The Attribute (The "Columns")
  An **Attribute** is a property or characteristic of a data object.
  *   **Standard Synonyms:** Variable, Field, Feature, Dimension.
  *   **Key Insight:** Attributes are usually **partial descriptions**. We can never capture everything about a "Customer" (object); we only capture specific attributes (Age, Income, ZIP) that we are able to observe.
  
  ***
- # Problem Solving & Concept Check
  
  **1. Semantic Mapping:** 
  You have a dataset of "Hospital Patients." 
  *   What is the **Object**?
  *   What are three likely **Attributes**?
  *   If you have an attribute "Room Number," why would it be a mistake to calculate the "Average Room Number" for the hospital?
  
  **2. Identifying the Fallacy:** 
  A researcher uses a standard regression model to predict the price of Bitcoin. They treat every day's price as an independent data point and ignore the date/time. Based on Slide 4, what specific information are they losing?
  
  **3. The "Algorithm-First" Trap:** 
  Why does the slide suggest that "Data understanding" must happen *before* choosing an algorithm? Give an example of a decision that depends on data understanding.