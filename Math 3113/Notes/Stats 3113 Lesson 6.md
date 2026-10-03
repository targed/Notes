### **Stat 3113: Lesson 6 - Introduction to Probability (Part 1)**
  
  This lesson transitions from descriptive statistics to the foundational principles of probability. Understanding probability is essential because it provides the mathematical framework for **quantifying uncertainty** and is the bedrock of **inferential statistics**. Concepts like confidence intervals and hypothesis testing, which are used to draw conclusions about populations from samples, are built directly upon probability theory.
  
  ---
### **1. Core Definitions**
#### **A. Sample Space and Events**
  
  *   **Experiment:** An action or process that generates a set of outcomes.
  *   **Sample Space (S):** The set of *all possible outcomes* of an experiment.
  *   **Event:** A specific subset of outcomes in the sample space. An event is a collection of one or more outcomes.
  
  **Examples of Sample Spaces:**
  
  1.  **Tossing a coin twice:** The sample space is `S = {HH, HT, TH, TT}`.
  2.  **Rolling two six-sided dice:** The sample space consists of 36 ordered pairs: `S = {(1,1), (1,2), ..., (6,6)}`.
  3.  **Measuring the lifetime of a lightbulb:** This is a continuous sample space, as the lifetime `t` can be any non-negative real number. In set-builder notation: `S = {t | t ≥ 0}`.
  
  **Examples of Events (using the "toss a coin twice" experiment):**
  
  *   **Event A:** Observing 2 heads. `A = {HH}`.
  *   **Event B:** Observing exactly 1 head. `B = {HT, TH}`.
  *   **Event C:** Observing at least 1 tail. `C = {HT, TH, TT}`.
  
  **Tree diagrams** are a useful tool for visualizing and identifying all possible outcomes in a multi-stage experiment.
  
  ---
### **2. Set Theory: Operations on Events**
  
  Since events are sets, we can use the language and operations of set theory to combine and manipulate them.
#### **A. Union (A ∪ B) - "OR"**
  
  The **union** of two events A and B contains all outcomes that are in event A, or in event B, **or in both**.
  
  *   **Example (coin toss):** The event "at least 1 head" is the union of event A (2 heads) and event B (exactly 1 head).
    `A ∪ B = {HH} ∪ {HT, TH} = {HH, HT, TH}`.
#### **B. Intersection (A ∩ B) - "AND"**
  
  The **intersection** of two events A and B contains all outcomes that are in **both** event A and event B simultaneously.
  
  *   **Example (cafeteria):** Let A = "person had ham" and B = "person had turkey". The intersection `A ∩ B` is the event "person had both ham AND turkey for lunch".
#### **C. Complement (Aᶜ) - "NOT"**
  
  The **complement** of an event A contains all outcomes in the sample space `S` that are **not** in event A.
  
  *   **Example (coin toss):** Let A = {HH} (observing 2 heads). The complement `Aᶜ` is the event of *not* observing 2 heads.
    `Aᶜ = {HT, TH, TT}`. Notice that this is the same as event C.
#### **D. Mutually Exclusive (Disjoint) Events**
  
  Two events A and B are **mutually exclusive** (or **disjoint**) if they have no outcomes in common. This means their intersection is the empty set (`A ∩ B = Ø`).
  
  *   **Key Idea:** Mutually exclusive events **cannot happen at the same time**.
  *   **Example 1 (Not Mutually Exclusive):** A person having ham (A) and a person having turkey (B) are *not* mutually exclusive because it's possible for `A ∩ B` to occur (a person could have both).
  *   **Example 2 (Mutually Exclusive):** A person being born in January (A) and being born in February (B) *are* mutually exclusive. `A ∩ B = Ø` because a person cannot be born in two different months.
  
  ---
### **3. Venn Diagrams**
  
  A Venn diagram is a visual representation of events within a sample space.
  
  *   The **rectangle** represents the entire sample space, `S`.
  *   **Circles** inside the rectangle represent events.
  *   The **overlap** of circles represents the intersection of events.
  *   The **total area** covered by circles represents the union of events.
  
  **Common Venn Diagrams:**
  | **Operation** | **Description** | **Venn Diagram** |
  | :--- | :--- | :--- |
  | **Union (A ∪ B)** | All outcomes in A or B or both. | ![Venn diagram showing the union of two sets.](https://storage.googleapis.com/generativeai-downloads/images/5053a479e390c377651030e844a49edc) |
  | **Intersection (A ∩ B)** | Outcomes in both A and B. | ![Venn diagram showing the intersection of two sets.](https://storage.googleapis.com/generativeai-downloads/images/e8c6521a074094a9737151a6cd150c18) |
  | **Complement (Aᶜ)** | All outcomes not in A. | ![Venn diagram showing the complement of a set.](https://storage.googleapis.com/generativeai-downloads/images/f39eddd748722b918b82d3ca65ed7056) |
  | **Mutually Exclusive** | A and B have no overlap. | ![Venn diagram showing two disjoint or mutually exclusive sets.](https://storage.googleapis.com/generativeai-downloads/images/e386ebf45aa5d288d447d21c2105156f) |