### **Stat 3113: Lesson 9 - Independence**
  
  This lesson defines the concept of independence, which is a crucial property when dealing with the probability of multiple events. It formalizes the intuitive idea that two events do not influence each other.
  
  ---
### **1. Definition of Independence**
  
  Two events, A and B, are **independent** if the occurrence of one event does not change the probability of the other event occurring.
  
  The formal definition is based on conditional probability:
  
  > Events A and B are **independent** if \( P(A | B) = P(A) \).
  > If the condition is not met, the events are **dependent**.
  
  **Interpretation:** This means the probability of A, *given that B has happened*, is exactly the same as the probability of A without any knowledge of B. Knowing B occurred provides no new information about the likelihood of A.
  
  **Example: Tossing a fair 6-sided die**
  *   Sample Space `S = {1, 2, 3, 4, 5, 6}`.
  *   Event A = "The outcome is even" = `{2, 4, 6}`. So, **P(A) = 3/6 = 1/2**.
  *   Event B = "The outcome is {1, 2, 3}". So, P(B) = 3/6 = 1/2.
  *   Event C = "The outcome is {1, 2, 3, 4}". So, P(C) = 4/6 = 2/3.
  
  *   **Are A and B independent?**
    We must check if \( P(A | B) = P(A) \).
    1.  Find the intersection: `A ∩ B = {2}`. So, P(A ∩ B) = 1/6.
    2.  Calculate the conditional probability: \\( P(A | B) = \frac{P(A \cap B)}{P(B)} = \frac{1/6}{3/6} = \frac{1}{3} \\).
    3.  Compare: Is \(P(A | B) = P(A)\)? Is \(1/3 = 1/2\)? No.
    4.  **Conclusion:** A and B are **dependent**. Knowing the outcome is in the set {1, 2, 3} lowers the probability that it is even.
  
  *   **Are A and C independent?**
    We must check if \( P(A | C) = P(A) \).
    1.  Find the intersection: `A ∩ C = {2, 4}`. So, P(A ∩ C) = 2/6.
    2.  Calculate the conditional probability: \( P(A | C) = \frac{P(A \cap C)}{P(C)} = \frac{2/6}{4/6} = \frac{2}{4} = \frac{1}{2} \).
    3.  Compare: Is \(P(A | C) = P(A)\)? Is \(1/2 = 1/2\)? Yes.
    4.  **Conclusion:** A and C are **independent**. Knowing the outcome is in the set {1, 2, 3, 4} does not change the probability that it is even.
  
  ---
### **2. Independence vs. Mutually Exclusive**
  
  These two concepts are **NOT** the same and are often confused.
  
  *   **Mutually Exclusive:** The events cannot happen at the same time (`A ∩ B = Ø`).
  *   **Independent:** The occurrence of one event does not affect the probability of the other.
  
  **Key Insight:** If two events A and B (with non-zero probabilities) are **mutually exclusive**, they are **always dependent**.
  *   *Reasoning:* If A and B are mutually exclusive, then knowing that B occurred means that A *cannot* have occurred. Therefore, P(A | B) = 0. Since P(A) was not zero, P(A | B) ≠ P(A), which is the definition of dependence.
  
  ---
### **3. Multiplication Rule for Independent Events**
  
  This is a simplified version of the General Multiplication Rule that applies *only* when events are independent.
  
  > If events A and B are independent, then the probability of their intersection is:
  > \( P(A \cap B) = P(A) \cdot P(B) \)
  
  This rule can be extended to any number of independent events:
  \( P(A \cap B \cap C) = P(A) \cdot P(B) \cdot P(C) \)
  
  **Useful Property:** If A and B are independent, then so are their complements (A' and B, A and B', A' and B').
  
  ---
### **4. Worked Example: Blood Phenotypes**
  
  **Scenario:** The proportions (probabilities) of blood types in a population are:
  | A | B | AB | O |
  | :--- | :--- | :--- | :--- |
  | 0.40 | 0.11 | 0.04 | 0.45 |
  
  Assume the blood types of two randomly selected individuals are **independent**.
  
  *   **a) What is the probability that both phenotypes are O?**
    *   Let \(O_1\) be the event that person 1 has type O blood.
    *   Let \(O_2\) be the event that person 2 has type O blood.
    *   We want to find \(P(O_1 \cap O_2)\).
    *   Because the events are independent, we use the multiplication rule:
        \( P(O_1 \cap O_2) = P(O_1) \cdot P(O_2) = 0.45 \times 0.45 = 0.2025 \)
  
  *   **b) What is the probability that the phenotypes of two individuals match?**
    *   A match can occur in four **mutually exclusive** ways: (Both A) OR (Both B) OR (Both AB) OR (Both O).
    *   We can calculate the probability of each and then add them (using the addition rule for mutually exclusive events).
    *   P(Match) = P(A ∩ A) + P(B ∩ B) + P(AB ∩ AB) + P(O ∩ O)
    *   Using the independence rule for each term:
    *   P(Match) = (0.40 × 0.40) + (0.11 × 0.11) + (0.04 × 0.04) + (0.45 × 0.45)
    *   P(Match) = 0.16 + 0.0121 + 0.0016 + 0.2025 = **0.3762**