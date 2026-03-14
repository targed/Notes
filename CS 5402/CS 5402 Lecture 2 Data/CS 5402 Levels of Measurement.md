## 1. The Broad Split: Qualitative vs. Quantitative
Before looking at specific types, we split all attributes into two "families":
*   **Qualitative (Categorical):** Represents qualities or characteristics. Values are labels or names. 
  *   *Example:* Eye color, Brand of car.
*   **Quantitative (Numeric):** Represents measurable quantities. Values are numbers.
  *   *Example:* Weight, Temperature, Price.

---
- ## 2. Categorical Attributes (Nominal & Ordinal)
- ### A. Nominal
  *   **Definition:** Values are distinct symbols or names with **no intrinsic order**.
  *   **Valid Operation:** Only **Equality (=, ≠)**.
  *   **Mathematical Limits:** You cannot say one is "greater than" another. You cannot add them.
  *   **Common Stats:** Mode (the most frequent), Entropy.
  *   **Examples:** ZIP Codes, ID numbers, Gender {Male, Female}, Outlook {Sunny, Overcast, Rainy}.
    *   *Critical Insight:* Even if a Nominal attribute looks like a number (ID: 1024), it is still nominal because the number is just a label.
- ### B. Ordinal
  *   **Definition:** Values have a **meaningful order or rank**, but the "distance" between ranks is not defined or equal.
  *   **Valid Operations:** Equality (=, ≠) AND **Comparison (<, >)**.
  *   **Mathematical Limits:** Addition and Subtraction make no sense. (e.g., If you have satisfaction levels 1-5, is "Great (5)" minus "Good (4)" the same as "Neutral (3)" minus "Bad (2)"? We don't know).
  *   **Common Stats:** Median, Percentiles, Rank Correlation.
  *   **Examples:** T-shirt size {S, M, L, XL}, Grades {A, B, C}, Army ranks.
  
  ---
- ## 3. Numeric Attributes (Interval & Ratio)
- ### A. Interval
  *   **Definition:** Differences between values are meaningful (measured in fixed, equal units), but there is **no true zero point**.
  *   **Valid Operations:** Equality, Comparison, AND **Addition/Subtraction (+, -)**.
  *   **The "Zero" Problem:** Zero in an interval scale is just an arbitrary position; it does not mean "nothing." 
    *   *Example:* 0° Celsius does not mean "no heat." 
  *   **Mathematical Limits:** You cannot use Ratios (Multiplication/Division). 
    *   *Example:* 100°F is NOT "twice as hot" as 50°F. (If you convert to Celsius, the ratio disappears).
  *   **Examples:** Temperature (F/C), Calendar Dates.
- ### B. Ratio
  *   **Definition:** All the properties of Interval, plus a **True Zero Point** (representing the absence of the quantity).
  *   **Valid Operations:** **All mathematical operations** are allowed (+, -, *, /).
  *   **Ratios are Meaningful:** You can say "X is twice as much as Y."
  *   **Examples:** Height, Weight, Age, Income, Temperature in **Kelvin** (0K actually means no molecular motion).
  
  ---
- ## 4. The Mathematical Property View
  This table is high-yield for exams. It shows the cumulative nature of these types:
  
  | Attribute Type | Distinctness (=) | Order (<) | Addition (+) | Multiplication (*) |
  | :--- | :---: | :---: | :---: | :---: |
  | **Nominal** | Yes | No | No | No |
  | **Ordinal** | Yes | Yes | No | No |
  | **Interval** | Yes | Yes | Yes | No |
  | **Ratio** | Yes | Yes | Yes | Yes |
  
  ---
- ## 5. Discrete vs. Continuous
  Independent of the hierarchy above, numeric attributes can be:
  *   **Discrete:** Values are countable. Often integers. There are "gaps" between values.
    *   *Example:* Number of children, Zip Codes (Nominal-Discrete), Number of absences.
  *   **Continuous:** Values are real numbers. There are an infinite number of values between any two points. 
    *   *Example:* Height, Weight, GPA.
    *   *Note:* In practice, computers represent continuous values as discrete (limited by floating-point precision), but we treat them as continuous in theory.
  
  ---
- ## 6. Why it matters for Algorithms
  Algorithms usually calculate **Distance** (similarity) between points.
  *   If you have **Ratio** data, you can use **Euclidean Distance** ($\sqrt{(x_1-x_2)^2...}$).
  *   If you have **Nominal** data, Euclidean distance is impossible. You might use **Simple Matching Coefficient** (count how many attributes are the same).
  *   *Conclusion:* You must identify your attribute types before you can pick your distance metric.
  
  ***
- # Problem Solving & Concept Check
  
  **1. The Temperature Trap:**
  A dataset records temperature in Celsius.
  *   Is it Nominal, Ordinal, Interval, or Ratio? (Answer: Interval).
  *   If you convert it to Kelvin, does the type change? (Answer: Yes, to Ratio).
  
  **2. Identifying the Type (Slide 23):**
  Classify these four attributes:
  *   A. **Student ID** (e.g., 12345): __________
  *   B. **GPA** (e.g., 3.82): __________
  *   C. **Year in School** (Freshman, Sophomore, etc.): __________
  *   D. **Number of Absences**: __________
  
  **3. Operation Check:**
  You are working with an **Ordinal** attribute (Customer Review: 1 star to 5 stars). 
  *   Is it mathematically valid to calculate the **Mean** (Average) star rating? (Answer: Technically no, though industry often does it improperly. The **Median** is the correct measure of center for Ordinal data).