### **Stat 3113: Lesson 10 - Random Variables**
  
  This lesson bridges the gap between the outcomes of an experiment and the numerical analysis we perform. Often, we are not interested in the experimental outcome itself, but rather in a numerical property associated with that outcome.
  
  ---
### **1. What is a Random Variable?**
  
  In simple terms, a random variable assigns a numerical value to every possible outcome of a random experiment.
  
  **Formal Definition:** A **random variable (rv)** is a function that maps each outcome in the sample space (S) to a real number.
  
  *   **Notation:**
    *   Random variables are denoted by **capital letters** (e.g., X, Y, Z).
    *   The specific values that a random variable can take are denoted by **lowercase letters** (e.g., x, y, z).
    *   The statement `X = x` means "The random variable X takes on the specific value x."
  
  *   **Support (or Domain):** The set of all possible values that a random variable can take is called its support.
  
  ---
### **2. Examples of Random Variables**
#### **Example A: Tossing a Coin Twice**
  *   **Sample Space (S):** `{HH, HT, TH, TT}`
  *   **Define the Random Variable (X):** Let X = the number of heads observed.
  *   **Mapping Outcomes to Values:**
    *   If the outcome is HH, then X = 2.
    *   If the outcome is HT, then X = 1.
    *   If the outcome is TH, then X = 1.
    *   If the outcome is TT, then X = 0.
  *   **Possible Values (Support) of X:** `{0, 1, 2}`
#### **Example B: Rolling a Pair of Dice**
  *   **Sample Space (S):** The set of all 36 ordered pairs `{(1,1), (1,2), ..., (6,6)}`.
  *   We can define several different random variables for this single experiment:
    1.  **Let X = the *maximum* value observed on the two dice.**
        *   **Possible Values of X:** `{1, 2, 3, 4, 5, 6}`.
    2.  **Let Y = the *sum* of the two numbers observed.**
        *   **Possible Values of Y:** `{2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12}`.
    3.  **Let Z = 1 if the numbers on the dice match (a pair), and 0 otherwise.**
        *   **Possible Values of Z:** `{0, 1}`.
        *   This is a special and very common type of random variable.
  
  *   **Bernoulli Random Variable:** A random variable whose only possible values are 0 and 1 is called a **Bernoulli random variable**. It is often used to model success/failure or yes/no outcomes.
  
  ---
### **3. Types of Random Variables: Discrete vs. Continuous**
  
  Just as we classified data variables, we also classify random variables.
#### **A. Discrete Random Variable**
  A random variable is **discrete** if its set of possible values is either finite or "countably infinite" (meaning you can list the values in a sequence, like 1, 2, 3, ...).
  
  *   **Finite Example:** The maximum value on a pair of dice, X, can only be `{1, 2, 3, 4, 5, 6}`. This is a finite set.
  *   **Countably Infinite Example:** Let Y = the number of attempts needed to start a car. The possible values are `{1, 2, 3, ...}`. This list is infinite but countable.
#### **B. Continuous Random Variable**
  A random variable is **continuous** if its possible values can be any number within a given interval or a collection of intervals.
  
  *   **Key Properties:**
    1.  It can take on an uncountably infinite number of values (a continuum).
    2.  The probability that a continuous random variable takes on any *single, specific value* is zero. That is, **P(X = c) = 0** for any constant c.
        *   *Why?* Because there are infinitely many possible values in any interval, the probability of hitting one exact value is essentially zero. We can only talk about the probability that X falls *within an interval*, e.g., `P(a ≤ X ≤ b)`.
  
  *   **Examples:**
    *   Let X = the lifetime of a lightbulb in hours. X can be any value greater than or equal to 0 (X ≥ 0).
    *   Let Y = the temperature of a chemical reaction.
  
  The next step in our study will be to assign probabilities to the values of these random variables, which leads to the concept of a **probability distribution**.