### **Stat 3113: Lesson 32 - Scatterplots**
  
  This lesson begins the study of **bivariate analysis**, where we examine the relationship between two variables simultaneously. Specifically, we focus on two **quantitative** variables.
  
  ---
### **1. Roles of Variables**
  
  When analyzing the relationship between two variables, we often assign them specific roles based on the goal of the analysis (prediction or explanation).
  
  *   **Response Variable (Dependent Variable, `y`):**
    *   This is the variable we want to predict or explain.
    *   It is the outcome of interest.
  *   **Explanatory Variable (Independent/Predictor Variable, `x`):**
    *   This is the variable we use to explain the variation in the response.
    *   It is the variable we manipulate or observe to predict `y`.
  
  **Example:** If we want to predict the price of a diamond based on its size (carats):
  *   `x` = Size (Explanatory)
  *   `y` = Price (Response)
  
  *Note: Not all relationships are causal. Sometimes we just want to see if two variables are related without implying one causes the other.*
  
  ---
### **2. The Scatterplot**
  
  A scatterplot is the primary graphical tool for displaying the relationship between two quantitative variables.
  
  *   **Construction:**
    *   **X-axis (Horizontal):** Explanatory variable.
    *   **Y-axis (Vertical):** Response variable.
    *   Each point on the graph represents a single individual or object from the dataset, plotted at its `(x, y)` coordinates.
  
  ---
### **3. Interpreting Scatterplots (Four Aspects)**
  
  When describing a scatterplot, you should always address four key aspects:
  
  1.  **Outliers:** Are there any individual points that fall far away from the general pattern of the data? These can be influential points.
  2.  **Clusters:** Does the data split into distinct groups or clouds? This might suggest a categorical variable is influencing the relationship (e.g., different species).
  3.  **Association (Direction & Strength):**
    *   **Positive Association:** As `x` increases, `y` tends to increase (slope is positive).
    *   **Negative Association:** As `x` increases, `y` tends to decrease (slope is negative).
    *   **No Association:** There is no discernible pattern; the points are a random cloud.
  4.  **Functional Form (Shape):**
    *   **Linear:** The points cluster around a straight line.
    *   **Curved (Non-linear):** The points follow a curve (e.g., quadratic, exponential).
    *   **Cyclical:** The pattern repeats over intervals (common in time-series data).
    *   **Random:** No pattern.
  
  ---
### **4. Examples**
  
  *   **Example 1: Printer Cost vs. Speed**
    *   **Plot:** Points are scattered all over with no clear pattern.
    *   **Interpretation:** Functional Form is **Random**. There is **no association** between printer speed and cost in this dataset.
  
  *   **Example 2: Iris Data**
    *   **Plot:** The data clearly splits into two separate groups.
    *   **Interpretation:** There are distinct **clusters**. Within the clusters, there appears to be a positive linear association.
  
  *   **Example 3: Mutual Fund Returns**
    *   **Plot:** Points generally move upwards from left to right.
    *   **Interpretation:** There is a **positive linear association**. As 2001 returns increase, 5-year returns tend to increase. There are no obvious outliers or clusters.
  
  *   **Example 4: Melbourne Temperature**
    *   **Plot:** The data goes up and down in a repeating wave pattern.
    *   **Interpretation:** The functional form is **cyclical**. This is typical for seasonal temperature data.
  
  *   **Example 5: Crowdedness vs. Distance**
    *   **Plot:** The points fall along a downward-sloping curve.
    *   **Interpretation:** There is a **negative association** with a **curved (non-linear)** form. As distance increases, crowdedness decreases, but the rate of decrease slows down.