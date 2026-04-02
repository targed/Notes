#### **1. Quiz Logistics (Slides 2–4)**
*   **Time & Weight:** 60 minutes, 100 points.
*   **Format:** A mix of YES/NO, Multiple Choice, Short Answer, and Calculations. *No AI allowed.*
*   **Scope:** Everything from Week 1 to Week 5 (Introduction, Python/IDE, Descriptive Stats, Derivatives, Supervised Learning).
*   **Difficulty Scaling:** 
  *   *Easy:* Concepts and Definitions.
  *   *Medium:* Descriptive Statistics, Derivatives, and Supervised Learning math.
- #### **2. Solving the Practice Questions: Part I (Foundations)**
  Here are the answers and "fill-in" explanations for the questions on **Slide 5**:
  
  *   **Q1: What is the relationship between Data Science/AI/Machine Learning/Deep Learning?**
    *   *Answer:* Think of concentric circles. **AI** is the broadest category. **Machine Learning** is a subset of AI (learning from data). **Deep Learning** is a subset of ML (using neural networks). **Data Science** is a separate but overlapping circle that uses all of the above, plus statistics and domain expertise, to solve data problems.
  *   **Q2: What is supervised learning?**
    *   *Answer:* An algorithm that learns from a training set of **labeled** examples (data paired with the correct answers) to make predictions on new, unseen data.
  *   **Q3: Difference between NumPy and PyTorch?**
    *   *Answer:* Both handle multidimensional arrays (tensors), but **PyTorch** has two superpowers: it can run on **GPUs** for massive parallel processing, and it has **Autograd** (automatic differentiation) to calculate gradients for neural networks. NumPy only runs on CPUs.
  *   **Q4: Supervised learning requires labeled data.**
    *   *Answer:* **Yes.**
  *   **Q5: An IDE helps organize, run, and debug code.**
    *   *Answer:* **Yes.**
- #### **3. Solving the Practice Questions: Part II (Python & Tools)**
  Here are the answers to the practical programming questions on **Slide 6**:
  
  *   **Which library is mainly used for numerical arrays?**
    *   *Answer:* **C. NumPy.** (Pandas is for tables/dataframes, Matplotlib is for plotting).
  *   **PyTorch tensors are similar to:**
    *   *Answer:* **B. NumPy arrays.** (They look and act almost identically, but PyTorch tensors can move to the GPU).
  *   **Which one is a common IDE?**
    *   *Answer:* **D. All of the above.** (Visual Studio Code, PyCharm, and Jupyter Notebooks are all tools used to develop code).
  *   **Python Code Interpretation:**
    ```python
    import numpy as np
    x = np.array([1, 2, 3, 4])
    print(np.mean(x))
    ```
    *   *What is the output?* **2.5**. (The math: $1+2+3+4 = 10$. $10 \div 4 = 2.5$).
    *   *What does `np.mean()` compute?* It computes the mathematical average of the elements in the array.
  
  ---
- ### **Action Items for Section 1:**
  *   **Self-Evaluation:** Did you know the answers to these immediately? If you struggled with the difference between ML and DL, review the Venn diagram from **Lecture 7**.
  *   **Study Tip:** For the "Short Answer" questions, keep your answers concise. The professor is looking for key buzzwords (e.g., "Labels" for Supervised Learning, "GPU/Autograd" for PyTorch).