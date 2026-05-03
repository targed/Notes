### 1. The Rule: Decision Problems Only (Slide 19)
Before we define P and NP, we have to change how we ask questions. Complexity classes like P and NP strictly deal with **Decision Problems**—questions that have a simple **YES or NO** answer.

*   **Optimization Problem:** "What is the shortest path from A to B?" (Asks for a specific value/structure).
*   **Decision Problem:** "Is there a path from A to B that is 14 hops or less?" (Asks for a Yes/No).

If a problem is an optimization problem (like finding the exact shortest Traveling Salesperson route), we must convert it into a decision problem (e.g., "Is there a tour with a total distance of $\le 100$?") before we can classify it into P or NP.
- ### 2. The Class "P" (The "Doers")
  *   **Definition (Slide 6):** The class **P** consists of all Decision Problems that can be **solved** in polynomial time.
  *   **What it means:** These are the tractable, "Easy" problems we covered in Part 1. If a problem is in P, it means humanity has discovered an algorithm that can reliably calculate the Yes/No answer in $O(n^d)$ time.
  *   *Examples:* Checking if a graph is connected, finding if a word exists in a sorted dictionary (Binary Search), determining if a number is even.
- ### 3. The Class "NP" (The "Graders")
  This is the most misunderstood term in computer science. 
  *   **The Trap:** NP does **NOT** stand for "Not Polynomial." 
  *   **The Real Name (Slide 9):** It stands for **Nondeterministic Polynomial time**.
  *   **The Working Definition (Slides 20-22):** The class **NP** consists of all Decision Problems where, if someone hands you a proposed solution, you can **verify** if that solution is correct in **polynomial time**.
  
  **The Analogy: Sudoku**
  Imagine a massive $100 \times 100$ Sudoku puzzle. 
  *   *Solving it from scratch* is incredibly difficult and might take you years (Exponential time).
  *   *Checking a completed puzzle* is incredibly easy. If I hand you a filled-out grid, you just scan the rows and columns to make sure there are no duplicate numbers. You can verify it in seconds (Polynomial time).
  *   Because checking the answer is fast, Sudoku belongs to the class **NP**.
  
  **The Professor's Example (Slide 21):**
  Integer Prime Factorization. 
  *   *Solving:* "What are the prime factors of 4,028,039?" (Very hard).
  *   *Verifying:* "Are 2003 and 2011 the prime factors of 4,028,039?" (Very easy! Just multiply $2003 \times 2011$ in $O(1)$ time to see if it equals $c$). 
  *   Because we can verify the proposed solution instantly, it is in **NP**.
- ### 4. The Relationship: Is P inside NP?
  Look at the Venn Diagram on Slide 9. Notice that the bubble for **P** is entirely inside the bubble for **NP**.
  *   *Why?* If you can *solve* a problem from scratch in polynomial time (P), then you can obviously *verify* a given solution in polynomial time. You just solve it yourself and see if your answer matches theirs!
  *   Therefore, **all problems in P are also in NP.**
- ### 5. The Million-Dollar Question: P vs. NP (Slides 23–24)
  This is the greatest unsolved problem in Theoretical Computer Science. The Clay Mathematics Institute will literally hand you **$1,000,000** if you can prove whether $P = NP$ or $P \neq NP$.
  
  *   **What if $P = NP$?**
    *   This would mean that if a problem can be *verified* quickly, it can also be *solved* quickly from scratch. 
    *   It would mean human creativity, mathematical proofs, and puzzle-solving are fundamentally easy to automate. It would also instantly break all internet encryption (as we discussed in Part 1).
  *   **What if $P \neq NP$? (Slide 24)**
    *   This is what 99% of computer scientists believe.
    *   It means there is a fundamental law of the universe that **checking a solution is inherently easier than creating a solution from scratch**. 
    *   As your slide says: It is easier to *check* a proof than to *create* a proof. It is easier to *check* a program than to *write* a program.
- ### 6. Quiz Check: Can NP problems be solved? (Slide 18)
  Your professor put a pop quiz on Slide 18 that tests a major misconception:
  *   *Statement 1:* "Problems in class NP cannot be solved." $\rightarrow$ **FALSE.**
  *   *Statement 2:* "Problems in class NP can be solved by exhaustive search." $\rightarrow$ **TRUE.**
  *   **The Deep Dive:** Every single problem in NP *can* be solved by a computer. The catch is that without a clever polynomial-time algorithm, we are forced to use "brute force" exhaustive search (like trying every single possible Sudoku combination until one works). This takes **Exponential Time**, but it *does* solve it.
  
  ---
- ### Part 2 Practice Questions (Theory & Definitions)
  
  **Q1: The Acronym**
  What does the "NP" in the complexity class NP stand for?
  A) Not Polynomial
  B) Non-computable Polynomial
  C) Nondeterministic Polynomial
  D) Non-Parametric
  
  **Q2: Categorizing Problems**
  You are given a problem: *"Given a graph of cities, find the absolute shortest possible route that visits every city and returns to the start."*
  Why can this specific phrasing of the problem **NOT** be classified as belonging to the class NP?
  A) Because it is impossible to solve.
  B) Because it is an Optimization problem, and NP only contains Decision (Yes/No) problems.
  C) Because verifying the shortest route takes polynomial time.
  D) Because it is a P problem.
  
  **Q3: The Definition of NP**
  Which of the following is the most accurate working definition of the class NP?
  A) The set of problems that take exponential time to solve.
  B) The set of decision problems where a "Yes" answer can be verified by a polynomial-time algorithm given the correct certificate/proof.
  C) The set of problems that cannot be solved by a Turing machine.
  D) The set of problems that are mathematically proven to be harder than P.
  
  **Q4: The $P \neq NP$ Implication**
  If you believe that $P \neq NP$, which of the following statements must you also believe?
  A) There are some problems where verifying an answer is fundamentally much faster/easier than finding the answer from scratch.
  B) All problems in NP can be solved in polynomial time.
  C) Exhaustive search (brute force) cannot solve NP problems.
  D) Encryption is completely useless.
  
  ---
-