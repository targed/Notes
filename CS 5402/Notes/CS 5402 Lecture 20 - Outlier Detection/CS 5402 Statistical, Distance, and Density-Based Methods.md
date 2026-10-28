## 1. Statistical Methods (Slides 17–20)
  **Core Idea:** Assume the data follows a known mathematical distribution (usually the Normal/Gaussian bell curve). If a point has a very low probability of occurring under that curve, it is an anomaly.
### A. The Z-Score (Slide 18)
  *   **The Math:** $Z = \frac{x - \mu}{\sigma}$
  *   **The Logic:** It measures how many standard deviations ($\sigma$) a point $x$ is away from the mean ($\mu$).
  *   **The Rule:** A standard rule of thumb is that any point with a Z-score $< -3$ or $> 3$ is an outlier. (In a perfect bell curve, 99.7% of data falls within 3 standard deviations).
### B. The IQR (Interquartile Range) Method (Slide 19)
  We covered this extensively in Lecture 3!
  *   **The Math:** $IQR = Q_3 - Q_1$.
  *   **The Rule:** Outliers are points below $Q_1 - 1.5(IQR)$ or above $Q_3 + 1.5(IQR)$.
  *   *Advantage over Z-score:* The Z-score requires calculating the mean and standard deviation, which are heavily distorted by massive outliers. The IQR uses percentiles (medians), making it much more robust.
  
  **Strengths & Weaknesses of Statistical Methods:**
  *   *Strengths:* Computationally extremely fast. Great if the data actually follows a bell curve.
  *   *Weaknesses:* Real-world data is rarely a perfect bell curve. They also struggle heavily with high-dimensional data (calculating joint distributions across 50 variables is nearly impossible).
  
  ---
## 2. Distance-Based Approaches (Slides 22–25)
  **Core Idea:** An outlier is a point that is physically far away from its neighbors in the feature space.
  
  **The Algorithms:**
  1.  **K-Nearest Neighbors (KNN) Distance:** Calculate the distance from a point to its $K^{th}$ nearest neighbor. If that distance is huge, the point is sitting out in the middle of nowhere (it's an outlier).
  2.  **Radius Count:** Draw a circle of radius $R$ around a point. Count how many other points fall inside that circle. If the count is very low (e.g., $< 3$), the point is an outlier.
  
  ---
## 3. Density-Based Approaches: LOF (Slide 26)
  This is a guaranteed exam topic. Distance-based approaches fail when clusters have different densities. 
  *   *The Problem:* Imagine a dense cluster where points are 1 inch apart, and a sparse cluster where points are 10 inches apart. If you use a strict distance rule of "anything further than 5 inches is an outlier," you will accidentally classify every normal point in the sparse cluster as an outlier!
  
  **The Solution: Local Outlier Factor (LOF)**
  LOF solves this by comparing a point's local density to the local density of its neighbors.
  
  **The 4 Steps of LOF:**
  1.  **Find Neighbors:** Find the $K$ nearest neighbors for point $X$.
  2.  **Compute LRD (Local Reachability Density):** How dense is the area directly around point $X$? Mathematically, this is the inverse of the average distance to its neighbors. (If neighbors are close, LRD is high).
  3.  **Compare Densities:** Compare $X$'s LRD to the LRD of its $K$ neighbors.
  4.  **Calculate LOF Score:**
    $$ \text{LOF}(X) = \frac{\text{Average LRD of Neighbors}}{\text{LRD of point } X} $$
  
  **Interpreting the LOF Score (Crucial for Exams):**
  *   **$\text{LOF} \approx 1$:** The point's density is similar to its neighbors. It is a **Normal Point** deep inside a cluster.
  *   **$\text{LOF} \gg 1$ (Much greater than 1):** The point's density is much *lower* than its neighbors. The neighbors are tightly packed together, but this point is sitting far away. It is an **Outlier**.
  *   **$\text{LOF} \ll 1$ (Much less than 1):** The point is in a super dense region, surrounded by a sparse region. (This is rare, but implies complex internal cluster structures).
  
  ***
# Problem Solving & Concept Check
  
  **1. The Z-Score Trap:**
  You have a dataset tracking employee salaries. 99 employees make exactly \$50,000. One employee (the CEO) makes \$50,000,000. 
  Why might the Z-score fail to flag the CEO as an outlier?
  *   *Answer:* The Z-score relies on the Mean ($\mu$) and Standard Deviation ($\sigma$). The \$50M salary will drastically inflate both the mean and the standard deviation of the dataset. Because the standard deviation becomes so massive, the CEO's Z-score might actually fall below the $>3$ threshold, meaning the outlier "hides" itself by breaking the math. (This is why IQR is safer!).
  
  **2. LOF Intuition:**
  You are running the LOF algorithm on a data point $P$. 
  *   $P$ has an LRD (density) of 0.1 (very sparse). 
  *   $P$'s neighbors have an average LRD of 10.0 (very dense).
  Calculate the LOF score for $P$. Is $P$ an outlier?
  *   *Calculation:* $\text{LOF} = 10.0 / 0.1 = \mathbf{100}$.
  *   *Conclusion:* Because $100 \gg 1$, point $P$ is a massive **Outlier**.
  
  **3. Distance vs. Density:**
  Look at the image on **Slide 25**. There is a large, sparse blue cluster on the bottom left, and a tiny, incredibly dense red cluster on the top right. 
  If you use a simple "Distance-Based" approach (flagging any point whose nearest neighbor is more than 5 units away), what error will you likely make?
  *   *Answer:* You will likely flag all the normal blue points as outliers (because they are naturally spread out), while completely missing true outliers that might be sitting just 2 units away from the super-dense red cluster. Density-based methods (LOF) are required to fix this.
  
  ***