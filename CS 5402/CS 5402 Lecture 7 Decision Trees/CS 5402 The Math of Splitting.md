## 1. Defining "Impurity" (Slide 10-11)
To split data effectively, we need to measure how "mixed" or "dirty" a set of data is.
*   **Pure Node:** Contains only one class (e.g., 100% "Yes"). Impurity = 0.
*   **Impure Node:** Contains a mix of classes (e.g., 50% "Yes", 50% "No"). Impurity is High.

We use two main functions to measure this:
1.  **Entropy (Used in ID3, C4.5 algorithms):**
  $$H(S) = - \sum p_i \log_2 (p_i)$$
  *   *Interpretation:* Measures the amount of uncertainty. (High Entropy = High Uncertainty).
2.  **Gini Index (Used in CART algorithm):**
  $$Gini(S) = 1 - \sum p_i^2$$
  *   *Interpretation:* Measures the probability of misclassifying a randomly chosen element.
- ## 2. Information Gain (IG) (Slides 12–16)
  **Definition:** Information Gain measures the **reduction in entropy** (uncertainty) achieving by splitting the data on a specific attribute.
  
  **The Formula:**
  $$ Gain(D, A) = \underbrace{H(D)}_{\text{Parent Entropy}} - \underbrace{H(D|A)}_{\text{Weighted Avg Entropy of Children}} $$
  
  **The Calculation Process:**
  1.  **Calculate Parent Entropy:** How mixed is the data *before* the split?
  2.  **Split the Data:** Divide the data into subsets based on the values of Attribute $A$.
  3.  **Calculate Children Entropy:** Calculate the entropy of each subset.
  4.  **Weighted Average:** Multiply each child's entropy by the proportion of data in that child node ($N_{child} / N_{total}$) and sum them up.
  5.  **Subtract:** Parent Entropy - Weighted Children Entropy.
  
  **The Goal:** We want the attribute with the **Highest Information Gain** (meaning it reduced the uncertainty the most).
  
  ---
- ## 3. Walkthrough Example: "Play Tennis" (Slides 23–26)
  **The Scenario:** We want to predict if we should Play Tennis (Yes/No) based on 4 features: Outlook, Temp, Humidity, Wind.
  
  **Step 1: The Root Node (Slide 23)**
  The algorithm calculates the IG for all 4 features:
  *   Gain(Outlook) = **0.246**
  *   Gain(Humidity) = 0.151
  *   Gain(Wind) = 0.048
  *   Gain(Temp) = 0.029
  
  **Decision:** **Outlook** provides the most information. It becomes the **Root Node**.
  
  **Step 2: Creating Branches (Slide 24)**
  Outlook has 3 values: `Sunny`, `Overcast`, `Rain`. We create 3 branches.
  *   **Overcast Branch:** Looking at the data, *every single time* Outlook is Overcast, the class is "Yes."
    *   *Result:* Entropy is 0. We stop and make this a **Leaf Node (Yes)**.
  *   **Sunny Branch:** The data is still mixed (2 Yes, 3 No). We need to split again.
  
  **Step 3: Recursion (Slides 25–26)**
  We focus *only* on the rows where Outlook = `Sunny`.
  We calculate IG for the remaining features (Humidity, Temp, Wind) *using only this subset*.
  *   Gain(Humidity) is highest.
  *   **Decision:** Split the Sunny node by **Humidity**.
    *   If Sunny AND High Humidity $\rightarrow$ No.
    *   If Sunny AND Normal Humidity $\rightarrow$ Yes.
  
  ---
- ## 4. Visualizing the "Best" Split (Slides 17–19)
  **Slide Context:** Comparing Attribute A vs. Attribute B.
  *   **Attribute A (Slide 18):** Splits the data into two mixed bags (2 Yes/1 No) and (1 Yes/1 No).
    *   *Result:* Low Gain (0.02). The child nodes are still messy.
  *   **Attribute B (Slide 19):** Splits the data into two **Pure** bags (3 Yes/0 No) and (0 Yes/2 No).
    *   *Result:* High Gain (0.97). The child nodes are perfect.
  *   **Takeaway:** The algorithm always prefers splits that create **Pure** child nodes (Homogeneity).
  
  ***
- # Problem Solving & Concept Check
  
  **1. Entropy Calculation:**
  A node contains: [5 Yes, 5 No].
  *   $p(Yes) = 0.5$, $p(No) = 0.5$.
  *   $H = -(0.5 \log_2 0.5 + 0.5 \log_2 0.5) = -(-0.5 - 0.5) = \mathbf{1.0}$.
  *   *Concept:* 1.0 is the maximum entropy for binary classification. It represents maximum uncertainty (a coin flip).
  
  **2. A Pure Node:**
  A node contains: [10 Yes, 0 No].
  *   $p(Yes) = 1$, $p(No) = 0$.
  *   $H = -(1 \log_2 1 + 0 \text{...}) = -(0) = \mathbf{0.0}$.
  *   *Concept:* Zero entropy means zero uncertainty.
  
  **3. The Selection Logic:**
  *   Feature X yields Information Gain = 0.2.
  *   Feature Y yields Information Gain = 0.5.
  *   Which feature goes at the top of the tree?
    *   *Answer:* **Feature Y**. Higher gain means it does a better job of separating the Yes/No classes.
  
  ***