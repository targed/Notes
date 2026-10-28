### **1. Measures of Central Tendency (Slides 16 & 18)**
We want to find the "center" of the data. We have two main tools, and choosing the right one is a classic interview question.
- #### **The Mean (Average) (Slide 16)**
  *   **The Math:** $\mu = \frac{1}{n} \sum x_i$
  *   **Translation:** Add everything up and divide by the count.
  *   **The Strength:** It uses *every* data point.
  *   **The Weakness:** It is extremely sensitive to **Outliers**.
- #### **The Median (The Middle) (Slide 18)**
  *   **The Math:** Sort the list $x_1, x_2, ... x_n$. Pick the middle number.
  *   **The Strength:** It ignores outliers completely.
  *   **The Example (Slide 18):**
    *   List 1: `[34, 12, 1, 10000000, 46...]`
    *   List 2: `[34, 12, 1, 46...]` (The 10 million is removed).
    *   *Result:* The Median stays almost exactly the same. The Mean would have shifted massively.
    *   *Real World Context:* This is why **Home Prices** (like in your homework) and **Salaries** are always reported as "Median," not "Average." One billionaire moving to Rolla would ruin the "Average Income" statistic, but the Median would stay accurate for the normal person.
  
  ---
- ### **2. Measures of Spread: Variance & STD (Slide 17)**
  Knowing the center isn't enough; we need to know how "spread out" the data is.
  
  *   **Variance ($\sigma^2$):** The average squared distance from the mean.
    *   Formula: $\frac{1}{n} \sum (x_i - \mu)^2$
    *   *Why Square it?* Because distances can be negative (if a value is below the mean). Squaring makes everything positive so they don't cancel each other out.
  *   **Standard Deviation (STD):** The square root of Variance.
    *   *Why we need it:* Variance gives you units in "Squared Dollars" or "Squared Square Feet," which makes no sense. Taking the square root brings the number back to the original unit (e.g., "$10,000 spread").
  
  ---
- ### **3. The Shape of Data (Slides 20, 21, 23)**
  Data rarely looks like a perfect bell curve. We use visualizations to see its "Posture."
- #### **A. Histograms vs. CDFs**
  *   **Histogram (Slide 20):** Shows Frequency. Good for seeing "Where is the data clustered?"
  *   **CDF (Cumulative Distribution Function) (Slide 21):** Shows Probability ($X \le x$).
    *   *Visual:* It always starts at 0 (bottom left) and ends at 1 (top right).
    *   *Use Case:* "What percentage of people earn *less than or equal to* $50k?" You find 50k on the X-axis, go up to the line, and read the percentage on the Y-axis.
- #### **B. Skewness (Slide 23)**
  This measures Asymmetry.
  *   **Normal Distribution:** Skewness = 0. (Symmetrical).
  *   **Negative Skew (Left Skew):** The **Tail** points to the Left. The hump is on the Right.
    *   *Example:* Test scores on a very easy exam (most people got As, a few failed).
  *   **Positive Skew (Right Skew):** The **Tail** points to the Right. The hump is on the Left.
    *   *Example:* Income. Most people earn a little; Elon Musk is way out on the right tail dragging the mean up.
  
  ---
- ### **4. The Outlier Problem (Slide 22)**
  Outliers are data points that don't belong. Handling them is an art form.
- #### **Detection Methods:**
  1.  **Median Method:** Is the point far from the middle?
  2.  **Standard Deviation Method:** Is the value $> \text{Mean} + 3 \times \text{STD}$? (In a normal distribution, 99.7% of data fits within 3 STDs. Anything outside is statistically freakish).
- #### **Treatment Strategy:**
  *   **Option 1: Keep them.**
    *   *When:* If the data is **Real**. (e.g., In your homework, the \$1.7M house in Rolla is an outlier, but it's a real house. Deleting it biases your study of the "Total Market.")
  *   **Option 2: Remove them.**
    *   *When:* If it is an **Error**. (e.g., A house listed as 10 sq ft, or a person with Age = 200). These are likely typos ("Data Entry Errors").
  
  ---
- ### **Action Items for Section 3:**
  *   **Math Check:** The slide uses $\frac{1}{n}$ for Variance. In Statistics class, you often see $\frac{1}{n-1}$.
    *   *Note:* $\frac{1}{n}$ is for **Population** Variance. $\frac{1}{n-1}$ is for **Sample** Variance (it corrects for bias). For this class, stick to the slide's formula ($\frac{1}{n}$), but be aware of the difference.
  *   **Homework Connection:** You are required to calculate the **Median** price per sq ft. Expect the Mean and Median to be different because real estate data is **Positively Skewed** (High-price mansions pull the mean up).