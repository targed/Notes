### **Stat 3113: Lesson 7 - Introduction to Probability (Part 2)**
  
  This lesson builds upon the foundational concepts of sample spaces and events by introducing the formal rules and axioms that govern the calculation of probabilities.
  
  ---
### **1. The Three Axioms of Probability**
  
  The entire field of probability is built upon three fundamental rules, called axioms. For any event *A* in a sample space *S*:
  
  *   **Axiom 1:** The probability of any event is non-negative.
    \( P(A) \ge 0 \)
    *(In simple terms: A probability can be 0% or higher, but it can never be negative.)*
  
  *   **Axiom 2:** The probability of the entire sample space is 1.
    \( P(S) = 1 \)
    *(In simple terms: The probability that *something* in the set of all possible outcomes will happen is 100%.)*
  
  *   **Axiom 3 (Addition Rule for Mutually Exclusive Events):** If \(A_1, A_2, ..., A_n\) are a collection of mutually exclusive (disjoint) events, the probability that one of them occurs is the sum of their individual probabilities.
    \( P(A_1 \cup A_2 \cup ... \cup A_n) = P(A_1) + P(A_2) + ... + P(A_n) \)
    *(In simple terms: If events cannot happen at the same time, you can find the probability of "A or B" by simply adding their probabilities together.)*
  
  ---
### **2. Probability Rules Derived from the Axioms**
  
  From the three axioms, we can derive several useful rules for manipulating probabilities.
  
  *   **Rule 1: General Addition Rule (Inclusion-Exclusion Formula)**
    For any two events A and B (not necessarily disjoint):
    \( P(A \cup B) = P(A) + P(B) - P(A \cap B) \)
    *Why do we subtract the intersection?* To avoid double-counting. When we add P(A) and P(B), we include the outcomes in their overlap (intersection) twice. Subtracting \(P(A \cap B)\) once corrects for this.
  
  *   **Rule 2: The Complement Rule**
    The probability that an event A does not occur is 1 minus the probability that it does occur.
    \( P(A^c) = 1 - P(A) \)
  
  *   **Rule 3: Probability of the Empty Set**
    The probability of an impossible event (the empty set, Ø) is 0.
    \( P(\emptyset) = 0 \)
  
  ---
### **3. Calculating Probabilities: Examples**
#### **Example 1: Disjoint Events**
  Given that events A and B are **disjoint**, with P(A) = 0.4 and P(B) = 0.5:
  *   a)  **P(A ∪ B):** Since they are disjoint, we use Axiom 3.
    \( P(A \cup B) = P(A) + P(B) = 0.4 + 0.5 = 0.9 \)
  *   b)  **P(Aᶜ):** Using the Complement Rule.
    \( P(A^c) = 1 - P(A) = 1 - 0.4 = 0.6 \)
#### **Example 2: Non-Disjoint Events**
  Let \(A_1\) be the event "older car is American" and \(A_2\) be "newer car is American".
  Given: P(\(A_1\)) = 0.7, P(\(A_2\)) = 0.5, and P(\(A_1 \cap A_2\)) = 0.4.
  *   a)  **Probability at least one car is American (P(\(A_1 \cup A_2\))):**
    These events are *not* disjoint (a family can own two American cars), so we must use the General Addition Rule.
    \( P(A_1 \cup A_2) = P(A_1) + P(A_2) - P(A_1 \cap A_2) = 0.7 + 0.5 - 0.4 = 0.8 \)
  *   b)  **Probability the older car is not American (P(\\(A_1^c\\))):**
    \( P(A_1^c) = 1 - P(A_1) = 1 - 0.7 = 0.3 \)
#### **Example 3: Mutual Fund Selections**
  The different fund types are **mutually exclusive** because a customer owns shares in *just one* fund.
  *   a)  **P(Balanced Fund):** We can read this directly from the table.
    P(Balanced) = 7% or 0.07.
  *   b)  **P(Bond Fund):** This is the event "Short Bond OR Intermediate Bond OR Long Bond". Since they are mutually exclusive, we add their probabilities.
    P(Bond) = P(Short) + P(Intermediate) + P(Long) = 15% + 10% + 5% = 30% or 0.30.
  *   c)  **P(Not a Stock Fund):** We can use the complement rule. First find the probability of a stock fund.
    P(Stock) = P(High-Risk) + P(Moderate-Risk) = 18% + 25% = 43%.
    P(Not Stock) = 1 - P(Stock) = 100% - 43% = 57% or 0.57.
  
  ---
### **4. Special Case: Equally Likely Outcomes**
  
  When an experiment has *N* total possible outcomes, and each outcome is **equally likely**, the probability of an event A is the ratio of the number of outcomes favorable to A to the total number of outcomes.
  
  \( P(A) = \frac{N(A)}{N} \)
  where:
  *   **N(A)** is the number of outcomes in event A.
  *   **N** is the total number of outcomes in the sample space.
#### **Example: Engineering Students**
  Total students (N) = 25 (Industrial) + 10 (Mechanical) + 10 (Electrical) + 8 (Civil) = **53**.
  Each student is equally likely to be chosen.
  
  *   a) **P(Industrial Major):**
    \( P(\text{Industrial}) = \frac{\text{Number of Industrial Majors}}{\text{Total Students}} = \frac{25}{53} \approx 0.47 \) (or 47%)
  *   b) **P(Civil OR Electrical Major):**
    These are mutually exclusive events.
    \( P(\text{Civil} \cup \text{Electrical}) = P(\text{Civil}) + P(\text{Electrical}) = \frac{8}{53} + \frac{10}{53} = \frac{18}{53} \approx 0.34 \) (or 34%)