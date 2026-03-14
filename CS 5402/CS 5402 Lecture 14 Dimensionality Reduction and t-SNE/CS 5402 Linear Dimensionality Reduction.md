## 1. Feature Selection vs. Feature Extraction (Slide 16)
If you have 1,000 features and want to reduce them to 10, you have two fundamentally different approaches:
*   **Feature Selection:** You pick the 10 best original features and throw away the 990 bad ones. (e.g., Using Mutual Information or Information Gain). 
  *   *Pro:* Retains original meaning (e.g., "Age", "Income").
*   **Feature Extraction:** You mathematically combine the 1,000 original features to create 10 brand new, "super" features. (e.g., PCA, t-SNE).
  *   *Pro:* Captures information from *all* original features.
  *   *Con:* The new features are abstract mathematical combinations (e.g., "0.3*Age + 0.8*Income - 0.2*Height"), losing direct interpretability.
- ## 2. What is PCA? (Slides 17–18)
  **Principal Component Analysis (PCA)** is an unsupervised feature extraction technique. 
  *   **The Goal:** Project data onto a lower-dimensional linear subspace (a flat line, plane, or hyperplane) while retaining as much of the original data's **variability** (spread) as possible.
  *   **The Result:** It transforms a set of highly *correlated* variables into a smaller set of completely *uncorrelated* variables called **Principal Components (PCs)**.
- ## 3. The Geometric Intuition of PCA (Slides 19–24)
  How does PCA actually find the "best" line to project data onto?
  
  **Step 1: Mean-Centering (Slides 19–20)**
  Before running PCA, you must calculate the mean of every feature and subtract it from the data points. This shifts the entire dataset so that it is perfectly centered on the origin $(0,0)$.
  
  **Step 2: Finding the Principal Components (Slides 21–23)**
  Imagine a cloud of data points in 2D. You want to squash them down onto a 1D line. Which line do you draw? PCA relies on a beautiful mathematical duality. The "best" line simultaneously achieves two things:
  1.  **Maximizes Variance:** When you project (drop) the points onto the line, they should be spread out as widely as possible along that line. (You want to preserve the "signal").
  2.  **Minimizes Projection Error:** The orthogonal (perpendicular) distance from the original data points to the line should be as short as possible. (You want to minimize the "information lost" by squashing them).
  
  *   **Principal Component 1 (PC1):** The line that captures the *most* variance.
  *   **Principal Component 2 (PC2):** The line that captures the *second most* variance, with the strict rule that it must be **orthogonal (perpendicular)** to PC1.
  
  *(Note: Under the hood, this is solved using Linear Algebra tools like Eigenvalue Decomposition or Singular Value Decomposition (SVD) on the data's covariance matrix).*
- ## 4. The Fatal Limitation of PCA (Slide 25)
  PCA is a **Linear** method. It only looks at the **Global Structure** of the data (the overall covariance). 
  *   **The Swiss Roll Problem:** Imagine a dataset shaped like a rolled-up piece of carpet (a 3D spiral). 
  *   If you use PCA to reduce this to 2D, it just flattens the roll by stepping on it. Points that were originally far away from each other on the unrolled carpet (but happened to be rolled up next to each other) get crushed together.
  *   PCA destroys the **Local Neighborhoods** of non-linear data. 
  
  ***
- # Problem Solving & Concept Check
  
  **1. Maximizing Variance vs. Minimizing Error:**
  Imagine data points shaped like a long, thin cigar pointing at a 45-degree angle. 
  *   If you project the data onto the X-axis, the "shadow" of the cigar is relatively short (low variance), and the points had to travel a long distance to hit the axis (high error).
  *   *Where does PC1 go?* **Directly through the longest center axis of the cigar.** The shadow is maximized, and the distance the points fall to hit the line is minimized.
  
  **2. Correlation Check:**
  After successfully performing PCA and transforming your data into Principal Components, you calculate the correlation between PC1 and PC2. What will the correlation be?
  *   *Answer:* **Exactly zero.** Principal components are mathematically constrained to be orthogonal (perpendicular) to each other, meaning they are completely uncorrelated. They capture entirely different patterns in the data.
  
  **3. The Scaling Trap:**
  Before running PCA, your features are "Income" (ranges from 20k to 200k) and "Age" (ranges from 18 to 80). If you do not standardize (Z-score scale) this data, what will happen?
  *   *Answer:* Because PCA seeks to maximize variance, it will look at "Income" (which varies by 180,000) and completely ignore "Age" (which varies by 62). PC1 will essentially just be the Income axis. **Always standardize data before PCA.**
  
  ***