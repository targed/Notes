## 1. The Three Hyperparameter Dilemmas of RANSAC (Slide 34)
  
  In **Part 1**, we saw that RANSAC fits models in the presence of severe outliers by pairing **random minimal sampling** with **consensus voting**.
  
  Slide 34 poses the three fundamental questions required to operationalize RANSAC in production:
  1. **The Inlier Threshold ($d$):** *How do we decide whether a point is close enough to be an inlier?*
  2. **The Minimal Sample Size ($s$):** *How many points should be drawn in each candidate round?*
  3. **The Iteration Count ($N$):** *How many sampling rounds are required to guarantee that we find the true inlier model with high statistical confidence?*
  
  ```
                       The Three Pillars of RANSAC Tuning
                                        │
       ┌────────────────────────────────┼────────────────────────────────┐
       ▼                                ▼                                ▼
  [ INLIER THRESHOLD d ]          [ SAMPLE SIZE s ]              [ ITERATIONS N ]
  Derived via Chi-Squared         Minimal points needed          Derived via Bernoulli
  distribution from Gaussian      to uniquely constrain          probability bounds to
  sensor noise σ (Slides 35-43)   the model (Slide 52)           guarantee success p (Slides 44-51)
  ```
  
  ---
## 2. Statistical Inlier Selection: The Chi-Squared ($\chi^2$) Test (Slides 35–43)
  
  Slide 35 introduces the foundational statistical assumption:
  > **The Noise Model Assumption:**
  > We assume that true physical correspondences (inliers) deviate from the ideal mathematical model due to **additive Gaussian measurement noise** with zero mean and standard deviation $\sigma$:
  > 
  > $$\epsilon \sim \mathcal{N}(0, \sigma^2)$$
  
  * $\sigma$ typically represents feature localization uncertainty introduced during corner or blob detection (typically $\sigma \in [0.5, \, 2.0]\text{ pixels}$).
  
  ---
### A. The Chi-Squared ($\chi^2$) Distribution
  Recall from probability theory that if $k$ independent random variables $Z_1, Z_2, \dots, Z_k$ follow a standard normal distribution ($Z_i \sim \mathcal{N}(0, 1)$), the sum of their squares follows a **Chi-Squared Distribution with $k$ degrees of freedom**:
  
  $$X = \sum_{i=1}^k Z_i^2 \sim \chi^2_k$$
  
  ```
                           The Chi-Squared (χ²) Inlier Boundary
                           
          Probability Density f(χ²)
              ^
              │    ╭───╮
              │  ╭─╯   ╰─╮
              │ ╭╯       ╰─╮
              │╭╯  95% Inliers  ╰─╮
              ││ (Accept Region)  ╰─╮
          0.0 ┴┴────────────────────┴───────┬────────────────────────► Test Statistic d² / σ²
              0                          d²_thresh
                                            │
                                            └─► 5% Inlier Rejection Tail (False Rejection)
  ```
  
  ---
### B. Case 1: 1D Residuals (Line Fitting — Slides 37–39)
  Consider fitting a 2D line $y = \beta_0 + \beta_1 x$. 
  The residual error $r_i = y_i - (\beta_0 + \beta_1 x_i)$ is a **1-dimensional scalar** with noise variance $\sigma^2$:
  
  $$\frac{r_i}{\sigma} \sim \mathcal{N}(0, 1) \implies \frac{r_i^2}{\sigma^2} \sim \chi^2_1$$
  
  Slide 37 highlights the cumulative distribution value for a 95% confidence interval:
  * For $k = 1$ degree of freedom, the cumulative distribution function satisfies:
  
  $$P\big(\chi^2_1 \le 3.841\big) = 0.95$$
  
  Multiplying through by $\sigma^2$:
  
  $$r_i^2 \le 3.84 \cdot \sigma^2 \iff |r_i| \le \sqrt{3.84} \cdot \sigma = \mathbf{1.96 \cdot \sigma}$$
  
  > **1D Inlier Rule (Slide 37):** 
  > To retain **95% of all valid inliers**, set the distance threshold to $d^2 \le 3.84 \sigma^2$ (or $|r| \le 1.96\sigma$, the standard 95% two-tailed normal confidence interval).
  
  ---
### C. Case 2: 2D Residuals (Homographies and Point Transfer — Slides 36–37)
  When aligning images via a Homography $\mathbf{H}$, the error is measured in the 2D image plane:
  
  $$\mathbf{x}'_i = (x'_i, y'_i) \quad \text{vs.} \quad \hat{\mathbf{x}}'_i = \left( \frac{h_{00}x_i + h_{01}y_i + h_{02}}{h_{20}x_i + h_{21}y_i + h_{22}}, \, \frac{h_{10}x_i + h_{11}y_i + h_{12}}{h_{20}x_i + h_{21}y_i + h_{22}} \right)$$
  
  The geometric transfer error vector is $\mathbf{e}_i = \begin{bmatrix} e_x & e_y \end{bmatrix}^T = \mathbf{x}'_i - \hat{\mathbf{x}}'_i$.
  Assuming horizontal and vertical localization errors are independent Gaussian variables ($e_x \sim \mathcal{N}(0, \sigma^2)$ and $e_y \sim \mathcal{N}(0, \sigma^2)$):
  
  $$\text{Squared Euclidean Distance: } d_i^2 = e_x^2 + e_y^2$$
  
  $$\frac{d_i^2}{\sigma^2} = \left(\frac{e_x}{\sigma}\right)^2 + \left(\frac{e_y}{\sigma}\right)^2 \sim \chi^2_2$$
  
  Slide 37 notes the 2-degree-of-freedom threshold:
  * For $k = 2$ degrees of freedom:
  
  $$P\big(\chi^2_2 \le 5.991\big) = 0.95$$
  
  $$d_i^2 \le 5.99 \cdot \sigma^2 \iff d_i \le \sqrt{5.99} \cdot \sigma = \mathbf{2.45 \cdot \sigma}$$
  
  > **2D Inlier Rule (Slide 37):** 
  > For 2D transformations (Homographies, Affine, Translation), setting $d^2 \le 5.99\sigma^2$ bounds a circular disk in the image plane that captures **95% of all true inliers**.
  
  ---
### D. The Impact of Threshold Mis-Specification (Slides 42–43)
  Slides 42 and 43 demonstrate the consequences of choosing $d$ incorrectly:
  * **Threshold Too Tight ($d \ll 2.45\sigma$):** Valid inliers corrupted by normal sensor noise fall outside the threshold and are rejected. The consensus count collapses (Slide 43: *Num inliers = 9*), leading to model instability.
  * **Threshold Too Loose ($d \gg 2.45\sigma$):** Outliers near the true model are accepted into the consensus set, corrupting the final least-squares re-fit.
  * **Correct $\chi^2$ Threshold:** Retains 95% of genuine matches while rejecting distant outliers (Slide 43: *Num inliers = 32*).
  
  ---
## 3. Derivation of the Theoretical Iteration Bound $N$ (Slides 44–51)
  
  Slide 44 poses the central optimization question:
  > **"How many times ('rounds' $N$) should we run RANSAC?"**
  > 
  > *Parameters:*
  > * $s$: Number of points drawn per round (minimal sample size).
  > * $e$: Outlier ratio in the dataset ($e \in [0, 1)$).
  > * $p$: Desired probability of success (typically $p = 0.99$, meaning a $99\%$ guarantee of finding the true model).
  
  Slide 44 asks: *"Where did the formula for $N$ come from?"*
  
  ```
       Probabilistic Derivation of the RANSAC Bound (Slides 45–48)
       
       [ Probability 1 point is an INLIER: 1 - e ]
                          │
                          ▼
       [ Probability all s points in a sample are INLIERS: (1 - e)^s ]
                          │
                          ▼
       [ Probability sample contains AT LEAST ONE OUTLIER: 1 - (1 - e)^s ]
                          │
                          ▼
       [ Probability ALL N rounds FAIL: ( 1 - (1 - e)^s )^N ]
                          │
                          ▼
       [ Probability of SUCCESS in N rounds: p = 1 - ( 1 - (1 - e)^s )^N ]
  ```
  
  ---
### Step-by-Step Proof (Slides 45–49):
  1. **Probability of a Single Inlier (Slide 45):**
   If the dataset contains an outlier fraction $e$, the probability of randomly drawing an inlier is:
  
  $$P(\text{inlier}) = 1 - e$$
  
  2. **Probability of a Clean Minimal Sample (Slide 46):**
   Assuming points are drawn with replacement (or $N_{\text{total}} \gg s$), the probability that **all $s$ randomly drawn points are inliers** is:
  
  $$P(\text{all } s \text{ are inliers}) = (1 - e)^s$$
  
  3. **Probability of a Contaminated Sample (Slide 47):**
   A model is invalid if it contains even one outlier. The probability that a sample contains **at least one outlier** is:
  
  $$P(\text{at least one outlier}) = 1 - (1 - e)^s$$
  
  4. **Probability of Total Failure Across $N$ Independent Rounds (Slide 48):**
   The probability that RANSAC fails to select a clean sample across all $N$ attempts is:
  
  $$P(\text{all } N \text{ samples fail}) = \Big( 1 - (1 - e)^s \Big)^N$$
  
  5. **Probability of Success $p$ (Slide 48):**
   The probability of finding **at least one clean sample of $s$ inliers** after $N$ attempts is:
  
  $$p = 1 - \Big( 1 - (1 - e)^s \Big)^N$$
  
  ---
### Solving for $N$ (Slide 49):
  Rearranging to isolate the iteration count $N$:
  
  $$1 - p = \Big( 1 - (1 - e)^s \Big)^N$$
  
  Taking the natural logarithm of both sides:
  
  $$\ln(1 - p) = \ln\left( \Big( 1 - (1 - e)^s \Big)^N \right)$$
  
  Applying the logarithmic power rule ($\ln(a^b) = b \ln(a)$):
  
  $$\ln(1 - p) = N \cdot \ln\big( 1 - (1 - e)^s \big)$$
  
  Dividing through by $\ln\big( 1 - (1 - e)^s \big)$:
  
  $$N \ge \frac{\ln(1 - p)}{\ln\big( 1 - (1 - e)^s \big)}$$
  
  > **The Fundamental RANSAC Theorem (Slide 49):**
  > To guarantee that RANSAC selects an all-inlier sample with probability $p$, the number of iterations must satisfy:
  > 
  > $$N = \left\lceil \frac{\log(1 - p)}{\log\big( 1 - (1 - e)^s \big)} \right\rceil$$
  
  ---
## 4. Analyzing the RANSAC Iteration Matrix (Slide 51)
  
  Slide 51 presents a table evaluating the required iteration count $N$ for a **$99\%$ confidence level ($p = 0.99 \implies \log(1 - p) = \log(0.01) \approx -4.605$)**:
  
  ```
  ┌───────┬────────────────────────────────────────────────────────────────────────┐
  │   s   │ Proportion of Outliers e                                               │
  │       │   5%      10%     20%     25%     30%     40%     50%     70%     80%  │
  ├───────┼────────────────────────────────────────────────────────────────────────┤
  │ **2** │    2       3       5       6       7       11      17      49     113  │ (Line)
  │ **3** │    3       4       7       9       11      19      35     169     573  │ (Affine)
  │ **4** │    3       5       9       13      17      34      72     588    2877  │ (Homography)
  │ **5** │    4       6       12      17      26      57     146    2053   14382  │
  │ **6** │    4       7       16      24      37      97     293    7171   71911  │
  │ **7** │    4       8       20      33      54      163    588   25055  359556  │
  │ **8** │    5       9       26      44      78      272   1177   87687 1797782  │ (Fundamental)
  └───────┴────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### Key Takeaways from the Data:
  1. **Exponential Sensitivity to Minimal Sample Size $s$:**
   * Look at the $50\%$ outlier column ($e = 0.5$):
     * For $s = 2$ (Line fitting): Requires only **$N = 17$ rounds**.
     * For $s = 4$ (Homography): Requires **$N = 72$ rounds**.
     * For $s = 8$ (8-point Fundamental Matrix): Requires **$N = 1,177$ rounds**.
   * **The Minimal Sample Principle:** 
     Always use the **smallest possible number of points** to fit candidate models in RANSAC. Never fit candidate models using non-minimal sets ($s + 1$ or $s + 2$), as adding even one extra point increases the required iterations exponentially.
  2. **The Curse of High Outlier Ratios:**
   * At an $80\%$ outlier ratio, estimating an 8-point Fundamental Matrix ($s = 8$) requires nearly **$1.8\text{ million iterations}$**, making standard RANSAC computationally prohibitive in severe clutter.
  
  ---
## 5. Choosing the Minimal Sample Size $s$ (Slide 52)
  
  Slide 52 connects $s$ to the geometric motion models from **Module: Image Transformations**:
  
  ```
                       Minimal Sample Size s by Geometric Model (Slide 52)
                       
    Geometric Transformation       Degrees of Freedom (DOF)      Minimal Sample Size s
    ──────────────────────────────────────────────────────────────────────────────────
    Translation                                2                 s = 1 Point Match
    Rigid / Euclidean (Rotation+Trans)         3                 s = 2 Point Matches
    Similarity (Rigid + Scale)                 4                 s = 2 Point Matches
    Affine Transformation                      6                 s = 3 Point Matches
    Projective Homography                      8                 s = 4 Point Matches
    Fundamental Matrix (Epipolar)              7 or 8            s = 7 or 8 Matches
  ```
  
  ---
### Degenerate Configurations
  When drawing random minimal samples of size $s$, algorithms must check for **geometric degeneracy** before solving:
  * **For Affine ($s = 3$):** The 3 points must not be collinear ($\text{Area}(\Delta) \approx 0$). Collinear points cannot constrain 2D shear or orthogonal scale.
  * **For Homography ($s = 4$):** No three of the 4 points can be collinear. If three points share a line, the projective denominator degenerates, yielding an unconstrained matrix.
  
  ---
## 6. Adaptive RANSAC: Handling Unknown Outlier Ratios (Slide 50)
  
  Slide 50 notes an operational challenge:
  > *"This means we need a pretty good idea of the inlier ratio, which means approximating the noise."*
  
  In practice, a vision system processing unknown camera feeds does not know the true outlier ratio $e$ in advance. Setting $N$ using a worst-case assumption (e.g., $e = 0.80$) wastes computation on clean scenes, while assuming $e = 0.20$ causes failure on cluttered scenes.
  
  ---
### The Adaptive Stopping Algorithm
  To avoid guessing $e$, modern computer vision uses **Adaptive (Dynamic) RANSAC**:
  
  ```
                Adaptive RANSAC Execution Loop
                
   Initialize: N = ∞,  Current_Iteration = 0,  p = 0.99
                │
                ▼
   Draw sample of size s ──► Fit Model ──► Count Inliers I_current
                                                  │
                                                  ▼
                 Does I_current exceed best inlier count so far?
                 ┌────────────────────────────────┴──────────────────────────────┐
             YES │                                                               │ NO
                 ▼                                                               ▼
   1. Record new best model.                                       Continue to next iteration.
   2. Update inlier ratio estimate:
         ŵ = I_current / Total_Points
         ê = 1 - ŵ
   3. Recompute required iterations:
         N_updated = log(1 - p) / log( 1 - (1 - ê)^s )
   4. N = min(N, N_updated)
                 │
                 ▼
   Check stopping condition: Is Current_Iteration ≥ N?
   If YES ──► TERMINATE EARLY!
  ```
  
  * **Advantage:** On clean image pairs ($e \approx 0.10$), the algorithm detects a large consensus set in early rounds, drops $N$ from thousands down to $5$, and terminates in milliseconds.
  
  ---
## 7. RANSAC Trade-Offs and Modern Variants (Slide 53)
  
  Slide 53 summarizes the practical strengths and weaknesses of RANSAC:
  
  ```
  ┌───────────────────────────────────────┬──────────────────────────────────────────────────────────┐
  │ Advantages ("The Good")               │ Limitations ("The Bad")                                  │
  ├───────────────────────────────────────┼──────────────────────────────────────────────────────────┤
  │ • General and mathematically simple   │ • Non-deterministic (different runs can yield different  │
  │ • Handles extreme outlier ratios      │   results on identical data)                             │
  │   (> 50%) where least squares fails   │ • Requires tuning the inlier threshold d                 │
  │ • Easily adapted to any model (lines, │ • Computationally expensive under extreme clutter (e>80%)│
  │   homographies, planes, fundamental F)│ • Discards residual information inside consensus set     │
  └───────────────────────────────────────┴──────────────────────────────────────────────────────────┘
  ```
  
  ---
### Modern Improvements Over Standard RANSAC
  Standard RANSAC uses a binary $0/1$ loss (a point is either inside the band or outside). Several variants refine this behavior:
  1. **MSAC (M-estimator SAmple Consensus):** 
   Instead of counting inliers, MSAC evaluates a bounded loss function: inliers are scored by their squared residual $r^2$, while outliers receive a constant maximum penalty $d^2$. This rewards models that pass through the center of inliers.
  2. **MLESAC (Maximum Likelihood Estimation SAmple Consensus):** 
   Models the data likelihood as a mixture model: Gaussian distributed inliers plus uniformly distributed outliers across the canvas.
  3. **PROSAC (Progressive Sample Consensus):** 
   Instead of sampling uniformly at random across all matches, PROSAC exploits descriptor similarity (e.g., SIFT ratio scores). Highly distinct matches are sampled first, reducing the required iterations by orders of magnitude.
  
  ---
## 8. Python Implementation: Adaptive RANSAC Homography Estimator (Slides 54–55)
  
  The following implementation combines the Normalized DLT solver from **Module M5.1** with the **$\chi^2$-based thresholding** and **adaptive stopping criterion** derived in this lecture:
  
  ```python
  import numpy as np
  
  def fit_homography_ransac_adaptive(matches: list, 
                                   sigma: float = 1.0, 
                                   p: float = 0.99, 
                                   max_iterations: int = 2000):
    """
    Fits an 8-DOF Homography using Adaptive RANSAC with Chi-Squared inlier bounds.
    
    Parameters:
        matches: List of [x_src, y_src, x_dst, y_dst].
        sigma: Standard deviation of feature localization noise (pixels).
        p: Desired success probability (typically 0.99).
        max_iterations: Hard upper limit on sampling rounds.
        
    Returns:
        H_best: Refined (3, 3) homography matrix.
        inliers_mask: Boolean array of size (N,) indicating inliers.
    """
    pts_src = np.array([[m[0], m[1]] for m in matches], dtype=np.float64)
    pts_dst = np.array([[m[2], m[3]] for m in matches], dtype=np.float64)
    N_total = pts_src.shape[0]
    
    s = 4  # Minimal sample size for 8-DOF Homography
    
    # 2D Chi-Squared threshold for 95% confidence: d^2 <= 5.99 * sigma^2
    chi2_thresh_sq = 5.991 * (sigma ** 2)
    
    best_inlier_count = 0
    best_inliers_mask = np.zeros(N_total, dtype=bool)
    best_H = None
    
    # Initialize adaptive iteration counter to infinity
    N_adaptive = float('inf')
    iteration = 0
    
    # Pre-allocate homogeneous arrays
    pts_src_h = np.hstack([pts_src, np.ones((N_total, 1))])
    
    while iteration < N_adaptive and iteration < max_iterations:
        iteration += 1
        
        # Step 1: Draw minimal random sample (s = 4)
        sample_indices = np.random.choice(N_total, size=s, replace=False)
        sample_matches = [matches[idx] for idx in sample_indices]
        
        # Fit candidate Homography via DLT
        try:
            # Import DLT solver from Module M5.1
            H_candidate = compute_minimal_homography_dlt(sample_matches)
        except np.linalg.LinAlgError:
            continue
            
        # Step 2: Compute symmetric/forward transfer error for all points
        projected = (H_candidate @ pts_src_h.T).T
        w = projected[:, 2:3]
        # Guard against points mapped near projective infinity
        valid_w = np.abs(w) > 1e-12
        w = np.where(valid_w, w, 1e-12)
        
        coords_proj = projected[:, :2] / w
        
        # Transfer error squared: ||x'_true - H(x)||^2
        transfer_error_sq = np.sum((pts_dst - coords_proj) ** 2, axis=1)
        
        # Step 3: Identify inliers using Chi-Squared bound
        current_inliers_mask = (transfer_error_sq <= chi2_thresh_sq) & valid_w.flatten()
        current_inlier_count = np.sum(current_inliers_mask)
        
        # Step 4: Record new best model & update adaptive iteration bound
        if current_inlier_count > best_inlier_count:
            best_inlier_count = current_inlier_count
            best_inliers_mask = current_inliers_mask
            best_H = H_candidate
            
            # Update estimated inlier fraction w_hat
            w_hat = best_inlier_count / N_total
            e_hat = 1.0 - w_hat
            
            # Avoid division by zero
            prob_clean_sample = (1.0 - e_hat) ** s
            if prob_clean_sample > 1e-7:
                # N = log(1 - p) / log(1 - (1 - e)^s)
                N_new = np.log(1.0 - p) / np.log(1.0 - prob_clean_sample)
                N_adaptive = min(N_adaptive, int(np.ceil(N_new)))
                
    # Step 5: Final Refinement via Normalized DLT over ALL Inliers
    if best_inlier_count >= s:
        inlier_matches = [matches[i] for i in range(N_total) if best_inliers_mask[i]]
        best_H = compute_minimal_homography_dlt(inlier_matches)
        
    return best_H, best_inliers_mask
  
  def compute_minimal_homography_dlt(matches: list) -> np.ndarray:
    """Helper DLT function for minimal / overdetermined correspondences."""
    pts_s = np.array([[m[0], m[1]] for m in matches])
    pts_d = np.array([[m[2], m[3]] for m in matches])
    N = pts_s.shape[0]
    
    A = np.zeros((2 * N, 9))
    for i in range(N):
        x, y = pts_s[i, 0], pts_s[i, 1]
        u, v = pts_d[i, 0], pts_d[i, 1]
        A[2*i]   = [x, y, 1, 0, 0, 0, -u*x, -u*y, -u]
        A[2*i+1] = [0, 0, 0, x, y, 1, -v*x, -v*y, -v]
        
    _, _, Vt = np.linalg.svd(A)
    H = Vt[-1].reshape((3, 3))
    return H / H[2, 2]
  ```
  
  ---
## Summary Matrix: The Complete RANSAC Formulation
  
  | Parameter / Metric | Symbol | Mathematical Derivation | Practical Rule of Thumb |
  | :--- | :--- | :--- | :--- |
  | **Inlier Noise Model** | $\sigma$ | Localization error: $\epsilon \sim \mathcal{N}(0, \sigma^2)$ | Set based on detector: $\sigma \approx 1\text{--}2\text{ px}$ |
  | **1D Inlier Bound** | $d_{\text{1D}}^2$ | 95% $\chi^2_1$ test: $r^2 \le 3.84 \sigma^2$ | $d_{\text{1D}} \le 1.96\sigma$ (Line fitting) |
  | **2D Inlier Bound** | $d_{\text{2D}}^2$ | 95% $\chi^2_2$ test: $d^2 \le 5.99 \sigma^2$ | $d_{\text{2D}} \le 2.45\sigma$ (Homographies) |
  | **Minimal Sample Size** | $s$ | Model parameters / constraints per match | $s=4$ for Homography; $s=3$ for Affine |
  | **Outlier Ratio** | $e$ | $e = 1 - (\text{Inliers} / N_{\text{total}})$ | Approximated dynamically in Adaptive RANSAC |
  | **Required Iterations** | $N$ | $N = \frac{\ln(1 - p)}{\ln(1 - (1 - e)^s)}$ | Table look-up or dynamic update |
  
  ---