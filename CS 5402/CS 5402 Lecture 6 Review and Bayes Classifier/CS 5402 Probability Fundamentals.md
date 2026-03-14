## 1. The Definitions
In Data Mining, we deal with uncertainty (noise, missing data, unknown future events). Probability is the calculus of uncertainty.

*   **Sample Space ($S$ or $\Omega$):** The set of *all* possible outcomes of an experiment.
  *   *Constraint:* The probabilities of all outcomes in the sample space must sum to 1.
*   **Event ($E$):** A subset of the sample space.
*   **The Axioms (Implied):**
  1.  $0 \le P(E) \le 1$.
  2.  $P(S) = 1$.
  3.  If events are mutually exclusive, the probability of their union is the sum of their individual probabilities.
- ## 2. Basic Rules
- ### The Addition Rule ("OR")
  *   **Mutually Exclusive:** Events that cannot happen at the same time (e.g., Rolling a 1 AND a 6 on a single die).
    *   Formula: $P(A \cup B) = P(A) + P(B)$
  *   **Non-Mutually Exclusive (The General Rule):** If events overlap (e.g., Rolling an Even number AND rolling a number > 3).
    *   *Fill-in:* You must subtract the overlap to avoid double-counting.
    *   Formula: $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
- ### The Multiplication Rule ("AND")
  *   **Independent Events:** The outcome of A does not affect B.
    *   Formula: $P(A \cap B) = P(A) \times P(B)$
  *   **Dependent Events (The General Rule):** The outcome of A changes the probability of B.
    *   Formula: $P(A \cap B) = P(A) \times P(B|A)$
- ## 3. Conditional Probability
  This is the most critical concept for Classification. In Data Mining, we usually ask: *"What is the probability of Class Y, **GIVEN** Feature X?"*
  
  *   **Definition:** $P(A|B)$ is the probability of A occurring given that B has *already* occurred.
  *   **The Intuition (Slide 6):** "Shrinking the Sample Space."
    *   When we condition on event $B$, we ignore everything in the universe that isn't $B$. $B$ becomes our new "Universe" (denominator).
    *   Formula: $$P(A|B) = \frac{P(A \cap B)}{P(B)}$$
- ## 4. Independence vs. Dependence
  *   **Independence:** $P(A|B) = P(A)$. Knowing B gives us **zero information** about A.
  *   **Why this matters for Data Mining:**
    *   Real-world features are rarely independent (e.g., "Income" and "Education Level" are dependent).
    *   However, the **Naïve Bayes** classifier (covered later) will *pretend* they are independent to make the math easier.
- ## 5. The Law of Total Probability
  **Slide Context:** A Venn diagram showing Sample Space $S$ divided into disjoint partitions $B_1, B_2, \dots B_n$.
  **The Concept:**
  Sometimes it is hard to calculate $P(A)$ directly. However, if we break the world into distinct scenarios ($B_1, B_2, \dots$), we can calculate $P(A)$ in each scenario and sum them up.
  
  *   **Formula:** $$P(A) = \sum_{i} P(A | B_i) P(B_i)$$
  *   *Application:* This is often used as the **denominator** (normalization constant) in Bayes' Theorem.
- ## 6. Solved Exercise
  **Problem:** Three bags with 100 marbles each.
  *   **Bag 1:** 75 Red, 25 Blue.
  *   **Bag 2:** 60 Red, 40 Blue.
  *   **Bag 3:** 45 Red, 55 Blue.
  *   **Task:** Pick a bag at random ($1/3$ chance), then pick a marble. What is $P(\text{Red})$?
  
  **Solution (Using Law of Total Probability):**
  We cannot calculate $P(\text{Red})$ directly because it depends on which bag we hold. We must sum the conditional probabilities.
  
  $$P(R) = P(R|B_1)P(B_1) + P(R|B_2)P(B_2) + P(R|B_3)P(B_3)$$
  
  1.  $P(B_1) = P(B_2) = P(B_3) = 1/3$ (Random selection).
  2.  $P(R|B_1) = 0.75$
  3.  $P(R|B_2) = 0.60$
  4.  $P(R|B_3) = 0.45$
  
  $$P(R) = (0.75 \times \frac{1}{3}) + (0.60 \times \frac{1}{3}) + (0.45 \times \frac{1}{3})$$
  $$P(R) = \frac{1}{3} (0.75 + 0.60 + 0.45) = \frac{1.8}{3} = \mathbf{0.6}$$
  
  ***
  
  **Self-Check Questions:**
  1.  If $P(A|B) = 0.5$ and $P(B) = 0.5$, what is the probability of A and B happening together?
  2.  If A and B are Mutually Exclusive, what is $P(A|B)$? (Answer: 0. If B happened, A *cannot* happen).