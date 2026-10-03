### **Stat 3113: Lesson 8 - Conditional Probability**
  
  This lesson introduces conditional probability, a fundamental concept that allows us to update the likelihood of an event occurring based on the knowledge that another event has already occurred.
  
  ---
### **1. Definition of Conditional Probability**
  
  The **conditional probability** of an event A occurring *given that* event B has already occurred is denoted as **P(A | B)**, which is read as "the probability of A given B."
  
  It is calculated using the formula:
  \( P(A | B) = \frac{P(A \cap B)}{P(B)} \), provided that P(B) > 0.
  
  **Interpretation:**
  Conditional probability redefines the sample space. Instead of considering all possible outcomes, we narrow our focus only to the outcomes where event B has happened. P(A | B) is then the probability that A also happens within this new, smaller sample space.
  
  **Intuitive Example:**
  *   Let A = a man is over 6 feet tall. (P(A) ≈ 0.15)
  *   Let B = a man played NCAA Division I basketball. (P(B) is very small, ≈ 0.002)
  
  Which is larger, P(A | B) or P(B | A)?
  
  *   **P(A | B):** "What is the probability a man is over 6 feet tall, *given that* we know he played NCAA D1 basketball?" This probability is very high (perhaps 85%) because NCAA players are overwhelmingly tall.
  *   **P(B | A):** "What is the probability a man played NCAA D1 basketball, *given that* we know he is over 6 feet tall?" This probability is extremely low (perhaps 1%). While being tall is a near-necessity for playing, most tall men have not played NCAA D1 basketball.
  
  ---
### **2. The General Multiplication Rule**
  
  By rearranging the conditional probability formula, we get the **Multiplication Rule**, which is used to find the probability of an intersection (an "AND" event).
  \( P(A \cap B) = P(A | B) \cdot P(B) = P(B | A) \cdot P(A) \)
  
  This rule is the key to calculating probabilities for sequential events, especially when using a tree diagram. To find the probability of a sequence of events, you multiply the probabilities along the corresponding branches of the tree.
  
  ---
### **3. The Law of Total Probability**
  
  This law is used to find the probability of an event (B) by considering all the different ways it can happen. If \(A_1, A_2, ..., A_k\) are mutually exclusive and exhaustive events (they don't overlap and they cover all possibilities), then the probability of event B is:
  
  \( P(B) = \sum P(B | A_i)P(A_i) = P(B | A_1)P(A_1) + P(B | A_2)P(A_2) + ... \)
  
  **In a tree diagram:** This means you find the probabilities of all the paths that end in event B and add them together.
  
  **Example: Marbles without replacement (40 total: 30 Red, 10 Green)**
  What is the probability that the second marble drawn is red, P(R₂)?
  There are two ways this can happen: (Red first, then Red second) OR (Green first, then Red second).
  *   Path 1: P(\(R_1 \cap R_2\)) = P(\(R_2 | R_1\))P(\(R_1\)) = (29/39) × (30/40)
  *   Path 2: P(\(G_1 \cap R_2\)) = P(\(R_2 | G_1\))P(\(G_1\)) = (30/39) × (10/40)
  
  P(\(R_2\)) = P(\(R_1 \cap R_2\)) + P(\(G_1 \cap R_2\)) ≈ 0.74 × 0.75 + 0.77 × 0.25 ≈ **0.7475**
  
  ---
### **4. Bayes' Theorem**
  
  Bayes' Theorem is a powerful formula that allows us to "reverse" the conditioning. It helps us find **P(A | B)** when we know **P(B | A)**.
  
  \( P(A_j | B) = \frac{P(A_j \cap B)}{P(B)} = \frac{P(B | A_j)P(A_j)}{\sum P(B | A_i)P(A_i)} \)
  
  **Interpretation:**
  The formula looks complex, but its application in a tree diagram is straightforward:
  \( P(\text{A | B}) = \frac{\text{Probability of the path that goes through A and ends at B}}{\text{Sum of probabilities of ALL paths that end at B}} \)
  
  **Example: Marbles without replacement**
  What is the probability that the first marble was red, given the second was green, P(\\(R_1 | G_2\\))?
  *   **Numerator (Path of interest):** The path is \\(R_1 \rightarrow G_2\\).
    P(\(R_1 \cap G_2\)) = P(\(G_2 | R_1\))P(\(R_1\)) = (10/39) × (30/40) ≈ 0.26 × 0.75 = 0.195
  *   **Denominator (All paths to \(G_2\)):**
    1.  Path \(R_1 \rightarrow G_2\): Probability is 0.195
    2.  Path \(G_1 \rightarrow G_2\): P(\(G_1 \cap G_2\)) = P(\(G_2 | G_1\))P(\(G_1\)) = (9/39) × (10/40) ≈ 0.23 × 0.25 = 0.0575
    P(\(G_2\)) = 0.195 + 0.0575 = 0.2525
  *   **Result:**
    P(\(R_1 | G_2\)) = \(\frac{0.195}{0.2525} \approx \textbf{0.7723}\)
  
  ---
### **Example 2: Tire Store Analysis**
  
  This example combines all the rules using a three-level tree diagram.
  
  *   **b) P(A ∩ B ∩ C):** Probability of (US tires AND balanced AND alignment).
    *   **Method:** Multiply along the top branch of the tree.
    *   P(A ∩ B ∩ C) = P(A) × P(B|A) × P(C|A∩B) = 0.75 × 0.9 × 0.8 = **0.54**
  
  *   **c) P(B ∩ C):** Probability of (balanced AND alignment).
    *   **Method:** Law of Total Probability. Find all paths that contain both B and C and add them.
    *   Path 1: A ∩ B ∩ C (Prob = 0.54)
    *   Path 2: A' ∩ B ∩ C = P(A') × P(B|A') × P(C|A'∩B) = 0.25 × 0.8 × 0.7 = 0.14
    *   P(B ∩ C) = 0.54 + 0.14 = **0.68**
  
  *   **e) P(A | B ∩ C):** Probability tires were from the US, given they were balanced and aligned.
    *   **Method:** Bayes' Theorem.
    *   P(A | B ∩ C) = \(\frac{P(A \cap B \cap C)}{P(B \cap C)} = \frac{0.54}{0.68} \approx \textbf{0.7941}\)