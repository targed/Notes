## 1. The Goal: Preserving Local Manifolds (Slides 26–32)
*   **The Visualization Gap:** We want to visualize 784-dimensional data (like the MNIST handwritten digits, where each pixel is a feature) on a 2D computer screen.
*   **Manifolds:** High-dimensional data often lies on a lower-dimensional "curved surface" called a manifold (like a crumpled piece of paper in 3D space). 
*   **PCA vs. t-SNE:** 
  *   PCA is rigid. It draws a straight line through the crumpled paper, preserving the *global* spread but mixing up points that are physically far apart on the paper.
  *   t-SNE is flexible. It preserves **local neighborhoods** (points that are close together on the crumpled paper stay close together when you flatten it out).
- ## 2. Step 1: High-Dimensional Probabilities ($P$) [Slides 33–35]
  Instead of looking at strict Euclidean distances, t-SNE thinks in terms of **probability** and **random walks**.
  
  *   **The Gaussian Neighborhood:** For every point $x_i$ in the high-dimensional space, we draw a Gaussian (Normal) distribution centered on it.
  *   **Similarity as Probability ($p_{j|i}$):** The similarity between point $x_i$ and point $x_j$ is defined as the probability that $x_i$ would pick $x_j$ as its neighbor. 
    *   If $x_j$ is very close to $x_i$, the probability is high.
    *   If $x_j$ is far away, the probability drops exponentially toward 0.
  *   **Dynamic Variance ($\sigma_i$):** In dense regions, the Gaussian is narrow. In sparse regions, the Gaussian is wide. This ensures every point has roughly the same number of "neighbors" regardless of density.
- ## 3. Step 2: The "Crowding Problem" & The 't' Distribution [Slides 36–39]
  Now, we want to map these points to a 2D space ($z$) and calculate their low-dimensional probabilities ($q_{j|i}$). 
  
  *   **The Crowding Problem:** In 1000D space, you have a massive amount of volume (room) to spread points out. When you force them into 2D, there isn't enough area to represent all those moderate distances. If we use a Gaussian distribution in 2D, the points will "crowd" and crush together in the center.
  *   **The Solution (The 't' in t-SNE):** Instead of using a Gaussian in the 2D space, we use a **Student's t-distribution**.
    *   *Why?* The t-distribution has a **"fatter tail"** than the Gaussian.
    *   *The Result:* To get the same probability score in 2D that they had in 100D, moderately far points are pushed *further away* from each other. This prevents the "crush" and creates distinct, separated clusters.
- ## 4. Step 3: Optimization via KL Divergence [Slides 40–42]
  We now have two matrices:
  1.  $P$: The target probabilities in high-dimensional space (Fixed).
  2.  $Q$: The current probabilities in 2D space (Adjustable).
  
  *   **The Cost Function:** We measure the difference between these two distributions using **KL Divergence** (from Lecture 6!).
    $$ Cost = \sum p_{ij} \log \left( \frac{p_{ij}}{q_{ij}} \right) $$
  *   **Gradient Descent:** We randomly drop the points into the 2D space, calculate the gradient of the KL Divergence, and move the 2D points ($z$) iteratively. 
    *   *Intuition:* The gradient acts like physical springs. It pulls points together if they are close in high-D, and violently repels them if they are far apart.
- ## 5. The Hyperparameter: Perplexity [Slides 43–44]
  This is the main "knob" you tune when running t-SNE.
  *   **Definition:** A smooth measure of the effective number of neighbors each point considers.
  *   **Rule of Thumb:** Usually set between 5 and 50.
  *   **Too Low (e.g., 2):** The algorithm only cares about your absolute closest neighbor. It over-segments the data into tiny, meaningless shards.
  *   **Too High (e.g., 100):** The algorithm tries to account for global structure, causing distinct clusters to merge back into a giant blob.
- ## 6. PCA vs. t-SNE: The Showdown (Slides 46–47)
  This comparison table is highly testable:
  
  | Feature | PCA | t-SNE |
  | :--- | :--- | :--- |
  | **Type** | Linear (Matrix Math) | Non-linear (Probabilities) |
  | **Goal** | Preserve Global Variance | Preserve Local Neighborhoods |
  | **Nature** | Deterministic (Always same result) | Stochastic (Different result every time) |
  | **Speed** | Very Fast | Very Slow |
  | **Use Case** | Preprocessing, Feature extraction | **Visualization** (Strictly 2D or 3D) |
  
  *   **The Pro-Tip (Slide 47):** Because t-SNE is slow, data scientists rarely run it on raw 10,000D data. Standard practice is to run **PCA first** to reduce the data to ~50 dimensions, and then run **t-SNE** to bring it down to 2D for visualization.
  
  ***
- # Problem Solving & Concept Check
  
  **1. The "t" Question:**
  Why doesn't t-SNE just use a Gaussian distribution for both the high-dimensional space and the low-dimensional space?
  *   *Answer:* The "Crowding Problem." If it used a Gaussian for the 2D map, points that were moderately far apart in high-D would be crushed into the exact same location in 2D due to the lack of volume. The t-distribution's "fat tails" push these points apart, allowing clusters to form.
  
  **2. Interpretation Trap:**
  You run t-SNE twice on the exact same dataset with the exact same hyperparameters. The two 2D plots look slightly different (clusters are rotated or swapped). Is there a bug in the code?
  *   *Answer:* No. t-SNE is **Stochastic** (the 'S' in t-SNE). It initializes the 2D points randomly and uses gradient descent on a non-convex cost function. It will converge to a different local optimum each time.
  
  **3. Pipeline Design:**
  You have a massive dataset of 1 million images, each with 3,000 pixels. You want to visualize them. What is the most computationally sound pipeline?
  *   *Answer:* Raw Data $\rightarrow$ PCA (Reduce to 50 dimensions) $\rightarrow$ t-SNE (Reduce to 2 dimensions) $\rightarrow$ Plot.
  
  ***
- # Part 4: Midterm Review Summary
  **(Covering Slides 48–55)**
  
  The final slides serve as a checklist for everything covered so far in the course. Here is your study roadmap:
  
  1.  **Data Preprocessing:** Know the difference between Nominal, Ordinal, Interval, and Ratio data. Know how to build a pipeline (Split $\to$ Impute $\to$ Encode $\to$ Scale) to avoid Data Leakage.
  2.  **Distance & Evaluation:** Know Manhattan ($L_1$) vs. Euclidean ($L_2$) vs. Cosine vs. Jaccard. Know when to use Precision vs. Recall, and how to read an ROC/AUC curve.
  3.  **Classifiers:** 
    *   *Decision Trees:* Entropy, Information Gain, Gain Ratio, Pruning.
    *   *Naïve Bayes:* Prior, Likelihood, Posterior, Conditional Independence assumption.
    *   *Perceptron & Logistic:* Hard vs. Soft boundaries, Gradient Descent, Sigmoid.
    *   *SVM:* Maximizing margins, Primal vs. Dual, Soft-margin $C$ parameter, Kernel Trick.
  4.  **Ensembles:** Bagging (Random Forest, parallel, reduces variance) vs. Boosting (AdaBoost, sequential, reduces bias).
  5.  **Clustering & Dim Reduction:** K-Means (Hard, Spherical) vs. GMM (Soft, Elliptical). PCA (Global, Linear) vs. t-SNE (Local, Non-linear).