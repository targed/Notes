## 1. Motivation: The Fragility of Least Squares to Outliers (Slides 2–5)
  
  In **Module M5.1**, we derived the classical least-squares solution for linear motion models:  
  
  $$\hat{\boldsymbol{\theta}} = \arg\min_{\boldsymbol{\theta}} \sum_{i=1}^n r_i(\boldsymbol{\theta})^2$$  
  Least-squares optimization operates under the fundamental probabilistic assumption that measurement noise follows an **independent identically distributed (i.i.d.) Gaussian distribution**:  
  
  $$r_i \sim \mathcal{N}(0, \sigma^2) \implies P(r_i) = \frac{1}{\sqrt{2\pi}\sigma} \exp\left( -\frac{r_i^2}{2\sigma^2} \right)$$  
  Under this assumption, minimizing the sum of squared residuals corresponds to the Maximum Likelihood Estimate (MLE).  
  
  ---
### A. The Reality of Real-World Feature Matching (Slides 2–5)
  Slides 2 and 3 show Vincent van Gogh’s *The Starry Night* warped using feature matches:  
  1. **The Inlier-Only Fit (Slide 2):** When correspondences are clean, the least-squares affine/homography fit aligns the two halves of the painting seamlessly.
  2. **The Single-Outlier Failure (Slide 3):** Introducing **just one bad match** causes the least-squares optimizer to produce a heavily distorted, skewed reconstruction.
   ```
  Image 1 (Template)                              Image 2 (Cluttered Scene)
  ┌───────────────────────────┐                   ┌───────────────────────────────────────┐
  │                           │                   │                   (Outlier Match)     │
  │         • Corner A        │                   │                        • Bottle Cap   │
  │         │                 │                   │                       /               │
  │         │ Inliers         │                   │         •────────────/                │
  │         ▼                 │ ────────────────► │        / \                            │
  │    ┌─────────┐            │                   │       /   \   True Inlier Region      │
  │    │  COMIC  │            │                   │      /_____\                          │
  │    │  BOOK   │            │                   │                                       │
  │    └─────────┘            │                   │                                       │
  └───────────────────────────┘                   └───────────────────────────────────────┘
   ```
  Slide 4 demonstrates this phenomenon on a real image pair: matching the "Making Comics" book against a cluttered desktop:
  * The green oval highlights dozens of valid inlier matches locking onto the book cover.
  * The red arrows highlight **outliers**: feature points matching corners on the book to random textures across the table (e.g., a plastic bottle cap or pens).
  ---
  ## 2. Geometric Breakdown: Why Least Squares Collapses (Slides 8–14)
    
    Slides 8–14 break down the mathematics of outlier corruption using a simple 2D line fitting problem:  
    
    $$y = \beta_0 + \beta_1 x$$  
    Suppose we collect $N = 12$ data points:  
    * $10$ points are **inliers** that follow the true line with minor Gaussian noise ( $r \approx 0.1$ ).
    
    * $2$ points are **outliers** resulting from sensor failure or spurious matching ( $r \approx 10.0$ ).  
    
    ```
       The Line We Want vs. The Line Least Squares Fits (Slides 9–11)
       
            ^ y
            │                                             • (Outlier)
            │                                           /
         10 │                                   • • •  /
            │                               • •       /
            │                           • •          /  ─── The True Line (Inliers Only)
          5 │                       • •             /       Residuals for the 2 outliers
            │                   • •                /        are ENORMOUS! (Slide 10)
            │               • •                   /
            │           • •                      /
          0 ┼─────────•─────────────────────────/─────────────────────────► x
            │       •                          /
            │                                 /
         -5 │   • (Outlier)                  /  ─── The Line Least Squares Actually Fits!
            │                               /       Tilted completely off the inliers! (Slide 11)
            └──────────────────────────────/──────────────────────────────
    ```
    
    ---
  ### The Mathematical Mechanism: Quadratic Penalty Explosion
    Recall the least-squares objective function:  
    
    $$J(\boldsymbol{\beta}) = \sum_{i=1}^{10} r_{i,\text{inlier}}^2 + r_{\text{outlier}_1}^2 + r_{\text{outlier}_2}^2$$  
    Compare the penalty contributions:  
    * Each inlier contributes:
    $$r_{\text{inlier}}^2 \approx (0.1)^2 = \mathbf{0.01}$$
    * Total inlier penalty for all 10 points:
    $$10 \times 0.01 = \mathbf{0.1}$$
    * A single outlier with a residual of $r = 10.0$ contributes:
    $$r_{\text{outlier}}^2 = (10.0)^2 = \mathbf{100.0}$$ $$\text{Outlier Penalty Contribution} = \frac{100.0}{0.1} = \mathbf{1000 \times \text{ the entire inlier dataset!}}$$ Because the cost function squares the error ( $r^2$ ), **a single gross outlier exerts an overwhelming influence on the gradient of the loss**:
    $$\nabla_{\boldsymbol{\beta}} J = -2 \sum_{i=1}^n r_i \, \mathbf{x}_i$$ The optimizer tilts and shifts the model away from the 10 valid inliers to reduce the residual of the outliers.
    ---
  ### The Statistical Concept: Breakdown Point
    In robust statistics, the **Breakdown Point** of an estimator is the smallest fraction of outlier contamination that can cause the estimator to produce an arbitrarily incorrect result.  
    * The breakdown point of Ordinary Least Squares (OLS) is:
    $$\epsilon_{\text{breakdown}}^{\text{OLS}} = \frac{1}{N} \to \mathbf{0\% \text{ as }} N \to \infty$$ A single contaminated data point can compromise the entire fit.
    ---
  ## 3. The Robust Estimation Philosophy (Slides 12–14)
    
    Slide 12 poses the central objective:  
    > *"If we split these points into inliers and outliers (somehow), we could fit the line we wanted to the inlying data and ignore the outliers."*  
    
    ```
       The Separation Goal:
       
       All Data Points ──► [ Classification Mechanism ] ──► Inlier Set:   Fit model using Least Squares!
                                                        └──► Outlier Set:  COMPLETELY IGNORE!
    ```
    
    This presents a "chicken-and-egg" dilemma:  
    1. If we knew the correct model parameters $\boldsymbol{\beta}$ , we could easily identify the inliers (any point with residual $|y_i - f(x_i)| < \tau$ ).
    2. If we knew which points were inliers, we could easily compute the correct model parameters via standard least squares.
    3. **The Catch:** We know neither in advance.
    ---
  ## 4. The RANSAC Algorithm: Fischler & Bolles (1981) (Slides 17–30)
    
    In 1981, **Martin A. Fischler and Robert C. Bolles** published their paper introducing **RANSAC (RANdom Sample Consensus)** [1].  
    
    ```
                     The Two Core Steps of RANSAC (Slide 17)
                     
    ┌────────────────────────────────────────────────────────────────────────┐
    │ 1. Hypothesis Generation:                                              │
    │    Randomly select a MINIMAL subset of points and fit a candidate model.│
    ├────────────────────────────────────────────────────────────────────────┤
    │ 2. Consensus Evaluation (Verification):                                │
    │    Test all remaining data points against the candidate model.         │
    │    Count the number of points that agree (the "Consensus Set").        │
    └────────────────────────────────────────────────────────────────────────┘
    ```
    
    RANSAC reverses the classical least-squares philosophy:  
    * **Least Squares:** Uses *all* data points simultaneously and tries to minimize global error.
    * **RANSAC:** Uses the *absolute minimum* number of points required to instantiate a model, hoping that at least once, a purely clean sample of inliers is selected by chance.
    ---
  ### A. Step-by-Step Geometric Walkthrough (Slides 19–29)
    Consider our 2D line fitting problem:
  #### Iteration 1 (Slides 20–21):
    1. **Random Minimal Sample ($s = 2$):** Pick two points randomly (the yellow dots in Slide 20).
    * Suppose we accidentally select one inlier and one outlier.
    2. **Fit Model:** Draw the unique line connecting these two yellow points (a nearly horizontal line).
    3. **Consensus Test:** Draw an inlier tolerance band (cyan stripe of width $2t$ ) around the line. Count points falling inside:
    * **Inliers Found: 4 points** (Slide 21).
    ```
    Iteration 1: Poor Sample (Inlier + Outlier)
    ^ y
    │        •        •
    │   ┌────•────────•────┐ Tolerance Band
    ───┼───│────○────────○────│─── Candidate Line (Inlier count = 4)
    │   └──────────────────┘
    │        •        •
    └─────────────────────────► x
    ```
    ---
  #### Iteration 2 (Slides 22–23):
    1. **Random Minimal Sample:** Pick another random pair of points (Slide 22).
    * Suppose we pick two points that cross the line diagonally.
    2. **Fit Model:** Draw the steep line connecting them.
    3. **Consensus Test:** Count points within the tolerance band:
    * **Inliers Found: 6 points** (Slide 23).
    * Better, but still misses the main trend.
    ---
  #### Iteration 3 (Slides 24–25):
    1. **Random Minimal Sample:** Pick another random pair (Slide 24).
    * **Both selected points happen to be true inliers!**
    2. **Fit Model:** Draw the line connecting them.
    3. **Consensus Test:**
    * **Inliers Found: 19 points** (Slide 25).
    * The consensus band captures nearly all the inlying data, while the two true outliers remain outside.
    ```
    Iteration 3: Clean Sample (Inlier + Inlier)
    ^ y                               • Outlier (REJECTED!)
    │                             ╭──/──╮
    │                           ╭─╯ •  ╭╯
    │                         ╭─╯ •  ╭─╯
    │                       ╭─╯ •  ╭─╯  Consensus Band
    │                     ╭─╯ •  ╭─╯    INLIER COUNT = 19!
    │                   ╭─╯ •  ╭─╯      (HIGHEST CONSENSUS FOUND)
    │         •       ╭─╯ •  ╭─╯
    │      Outlier  ╭─╯ •  ╭─╯
    └───────────────┴──/───┴────────────────────────► x
    ```
    ---
  ### B. Consensus Maximization and Re-Fitting (Slides 28–29)
    Slide 28 states the decision rule:  
    > **The Consensus Rule:**  
    We run this sampling process for $N$ iterations and choose the candidate model that **maximizes the total number of inliers** (largest consensus set).  
    
    Slide 29 highlights the critical final step:  
    > **Final Model Re-Fitting (Slide 29):**  
    Once the best consensus set of inliers has been identified:
    >	1. Discard all outliers.
    2. **Re-fit the model parameters using standard Least Squares across ALL inliers in the consensus set.**
    
    > *Why re-fit?* A model fit to only $s$ minimal points (e.g., $s=2$ ) is sensitive to the small localization noise of those specific points. Re-fitting over all $19$ inliers averages out Gaussian measurement noise, producing an optimal parameter estimate.  
    
    ---
  ## 5. The Formal RANSAC Algorithm Specification (Slide 30)
    
    Slide 30 presents the formal algorithmic specification from **Forsyth & Ponce** [2]:  
    
    ```
    Algorithm 15.4: RANSAC Model Fitting
    ─────────────────────────────────────────────────────────────────────────────
    Input Parameters:
    s - Minimal number of data points required to fit the model parameter vector
    N - Number of random iterations (sampling rounds) to execute
    d - Distance threshold defining whether a data point fits the model (inlier bound)
    T - Consensus threshold (minimum number of inliers required to accept model)
    
    Loop: Execute N iterations
    1. Draw a random sample of s points from the dataset uniformly at random
    2. Fit model parameters M_trial to this minimal sample of s points
    3. For each data point outside the sample:
           Compute geometric distance r_i to model M_trial
           If r_i < d:
               Add point to current Consensus Set S_trial
    4. If size(|S_trial|) > T:
           A valid consensus set is found.
           Re-fit model M_refined to all points in S_trial using Least Squares.
           Record M_refined if its consensus size exceeds previous best.
    End Loop
    
    Output: Return model with the largest consensus set and lowest inlier residual error.
    ─────────────────────────────────────────────────────────────────────────────
    ```
    
    ---
## 6. The Cost Function Dilemma: Inlier Counting vs. Residual Minimization (Slides 18, 31–33)
  
  Slide 18 and Slide 31 raise a natural question:  
  > **"Why can't we simply pick the candidate model that has the lowest Sum of Squared Residuals for its inlying points?"**  
  
  *Answer:* **Because that leads directly to the "Two-Point Zero-Residual Trap" (Slide 33).**  
  
  ```
       The Two-Point Zero-Residual Trap (Slide 33)
       
            ^ y                                       • (Inlier)
            │                                   •
            │                               •
            │                           •
            │                       •
            │                   •
            │               •
            │           •
          0 ┼─────────•───────────────────────────────────────────────────► x
            │       •                  Candidate Line connecting 2 arbitrary outliers!
            │                     ────○───────────────────────○───────────
            │   • (Outlier)                                     • (Outlier)
            └─────────────────────────────────────────────────────────────
            Residual for the 2 points on the line: r₁ = 0, r₂ = 0
            Total Sum of Squared Residuals: 0² + 0² = 0.0 (ABSOLUTE MINIMUM!)
  ```
### The Breakdown:
  1. Suppose we choose an arbitrary pair of points (even two random outliers).
  2. By definition, a straight line passing through those two points fits them **with zero error**:
  $$r_1 = 0, \quad r_2 = 0 \implies \sum_{i \in \text{inliers}} r_i^2 = 0.0$$
  3. If our selection criterion were minimizing the sum of squared residuals of inliers, the optimizer would prefer this two-point line over the true line (which has small, non-zero Gaussian residuals across $19$ points: $\sum r_i^2 \approx 0.19$ ).
  4. **The Principle:**  
  Inlier count takes absolute priority over residual magnitude. **Maximizing the size of the consensus set ensures model generalization.** We only compare residual errors to break ties between models that share the exact same inlier count.
  ---
## 7. Python Implementation: Fundamental RANSAC Line Fitter
  
  The following self-contained NumPy program implements the core RANSAC loop for 2D line fitting, including minimal sampling, consensus thresholding, and final inlier re-fitting:  
  
  ```python
  import numpy as np
  
  def fit_line_ransac(x: np.ndarray, 
                     y: np.ndarray, 
                     num_iterations: int = 100, 
                     inlier_threshold: float = 0.5, 
                     min_inliers_ratio: float = 0.5):
    """
    Fits a 2D line (y = beta_0 + beta_1 * x) using RANSAC.
    
    Parameters:
        x, y: 1D coordinate arrays of noisy data points with outliers.
        num_iterations: Number of random minimal sampling rounds (N).
        inlier_threshold: Maximum residual distance to be classified as an inlier (d).
        min_inliers_ratio: Minimum fraction of total points to consider a model valid.
        
    Returns:
        best_beta: Array [beta_0, beta_1] representing the optimal line.
        best_inliers_mask: Boolean array indicating inlier points.
    """
    num_points = len(x)
    s = 2  # Minimal sample size for a 2D line
    
    best_inlier_count = -1
    best_inliers_mask = np.zeros(num_points, dtype=bool)
    best_beta = None
    
    for _ in range(num_iterations):
        # ---------------------------------------------------------
        # Step 1: Uniform Random Minimal Sampling (s = 2)
        # ---------------------------------------------------------
        sample_indices = np.random.choice(num_points, size=s, replace=False)
        x_sample = x[sample_indices]
        y_sample = y[sample_indices]
        
        # Avoid vertical degenerate lines
        if np.abs(x_sample[1] - x_sample[0]) < 1e-7:
            continue
            
        # ---------------------------------------------------------
        # Step 2: Fit Model to Minimal Sample
        # Analytical 2-point line: slope = dy/dx, intercept = y - slope*x
        # ---------------------------------------------------------
        beta_1 = (y_sample[1] - y_sample[0]) / (x_sample[1] - x_sample[0])
        beta_0 = y_sample[0] - beta_1 * x_sample[0]
        
        # ---------------------------------------------------------
        # Step 3: Consensus Set Evaluation (Thresholding Residuals)
        # Vertical residual: r_i = |y_i - (beta_0 + beta_1 * x_i)|
        # ---------------------------------------------------------
        predictions = beta_0 + beta_1 * x
        residuals = np.abs(y - predictions)
        
        current_inliers_mask = residuals < inlier_threshold
        current_inlier_count = np.sum(current_inliers_mask)
        
        # ---------------------------------------------------------
        # Step 4: Record Model with Largest Consensus Set
        # ---------------------------------------------------------
        if current_inlier_count > best_inlier_count:
            best_inlier_count = current_inlier_count
            best_inliers_mask = current_inliers_mask
            best_beta = np.array([beta_0, beta_1])
            
    # -------------------------------------------------------------
    # Step 5: Final Least-Squares Re-fitting over ALL Consensus Inliers
    # -------------------------------------------------------------
    if best_inlier_count >= (num_points * min_inliers_ratio):
        x_inliers = x[best_inliers_mask]
        y_inliers = y[best_inliers_mask]
        
        # Inhomogeneous Least Squares via Design Matrix
        X_design = np.column_stack([np.ones_like(x_inliers), x_inliers])
        refined_beta, _, _, _ = np.linalg.lstsq(X_design, y_inliers, rcond=None)
        return refined_beta, best_inliers_mask
        
    return best_beta, best_inliers_mask
  ```
  
  ---
## Summary Matrix: Least Squares vs. RANSAC
  
  | Aspect | Classical Least Squares (OLS) | RANSAC (Fischler & Bolles, 1981) |
  |---|---|---|
  | **Noise Assumption** | Strictly Gaussian ( $\mathcal{N}(0, \sigma^2)$ ) | Multi-modal: Gaussian inliers + Uniform outliers |
  | **Outlier Handling** | Catastrophic failure ( $\epsilon_{\text{breakdown}} = 0\%$ ) | **Robust up to $>50\text{--}80%$ outliers** |
  | **Data Scope** | Uses all $N$ data points simultaneously | Hypothesizes from **minimal subset $s$**, verifies on rest |
  | **Cost Objective** | Minimizes Sum of Squared Errors ( $\sum r_i^2$ ) | **Maximizes consensus set cardinality** ($ | S | $) |
  | **Computational Class** | Deterministic closed-form ( $\mathcal{O}(N)$ ) | Non-deterministic randomized iterations ( $\mathcal{O}(N \cdot s)$ ) |
  | **Refinement Phase** | N/A | Re-fits final model via Least Squares on inliers |
  
  ---