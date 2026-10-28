## 1. Quick Review: Answers to Section 1 Checkpoints
  
  1. **Perceptron Arbitrariness vs. SVM Uniqueness:**
   * The Perceptron minimizes the loss function $\mathcal{L}_{\text{perceptron}} = \sum_{i \in \mathcal{M}} -y_i (w^T x_i + b)$ using stochastic subgradient descent. It terminates the moment all points are correctly classified ($\mathcal{M} = \emptyset$), stopping at whatever arbitrary hyperplane separates the data first. Its final solution depends entirely on initialization and sample presentation order.
   * The Hard-Margin SVM explicitly optimizes a convex quadratic objective subject to margin constraints:
     $$\min_{w, b} \frac{1}{2}\|w\|_2^2 \quad \text{subject to } y_i(w^T x_i + b) \ge 1$$
   * Because the objective is strictly convex and the constraints define a convex polytope, the solution is **unique**; it deterministically finds the single hyperplane that maximizes the geometric distance to the closest data points.
  2. **Derivation of the Margin Width $\gamma = \frac{2}{\|w\|_2}$:**
   * Let $x_+$ lie on the positive margin plane ($w^T x_+ + b = +1$), and let $x_-$ lie on the negative margin plane ($w^T x_- + b = -1$).
   * The margin width $\gamma$ is the orthogonal projection of the displacement vector $(x_+ - x_-)$ onto the unit normal vector $\hat{w} = \frac{w}{\|w\|_2}$:
     $$\gamma = (x_+ - x_-)^T \left(\frac{w}{\|w\|_2}\right) = \frac{w^T x_+ - w^T x_-}{\|w\|_2}$$
   * Substituting $w^T x_+ = 1 - b$ and $w^T x_- = -1 - b$:
     $$\gamma = \frac{(1 - b) - (-1 - b)}{\|w\|_2} = \frac{1 - b + 1 + b}{\|w\|_2} = \mathbf{\frac{2}{\|w\|_2}}$$
  3. **KKT Complementary Slackness and Moving Non-Support Vectors:**
   * The KKT condition states:
     $$\alpha_i^* \Big[ y_i(w^{*T} x_i + b^*) - 1 \Big] = 0$$
   * For any training point sitting strictly outside the margin, $y_i(w^{*T} x_i + b^*) > 1$. Therefore, its Lagrange multiplier must be zero: $\alpha_i^* = 0$.
   * Since the optimal weight vector is an expansion over active multipliers:
     $$w^* = \sum_{i=1}^n \alpha_i^* y_i x_i = \sum_{i \in \text{SVs}} \alpha_i^* y_i x_i$$
     points with $\alpha_i^* = 0$ contribute nothing to $w^*$ or $b^*$. Moving or deleting these non-support vectors does not alter the dual objective or the KKT stationary conditions, leaving the decision boundary completely unchanged.
  
  ---
## 2. Structural Robustness: SVM vs. Logistic Regression (Slide 9)
  
  Slide 9 introduces a comparative experiment highlighting the difference in how these two models treat training points far from the boundary:
  
  $$\mathbf{\text{"Add 15 easy points deep on the } +1 \text{ side, all outside the margin. Which boundary moves?"}}$$
  
  ```
                       The Boundary Shift Experiment (Slide 9)
         Original 40 Training Points                   +15 Easy Points Added Far from Boundary
  Feature x₂                                    Feature x₂
      ▲                                             ▲
      │           ■   ■ ■                           │           ■   ■ ■
      │        ■  ■ ■   ■   ■                       │        ■  ■ ■   ■   ■
      │   ─────────────────────── SVM               │   ─────────────────────── SVM (STAYS FIXED!)
      │   - - - - - - - - - - - - LogReg            │   ======================= LogReg (SHIFTS & ROTATES!)
      │         ●   ●   ●                           │         ●   ●   ●        ●●●●● (+15 Easy Points)
      │      ● ●  ●   ●                             │      ● ●  ●   ●          ●●●●●
      └────────────────────────▶ Feature x₁         └────────────────────────▶ Feature x₁
         ● = Class +1          ■ = Class -1
  ```
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Empirical Result:                                                  │
  │                                                                        │
  │ • The SVM Decision Boundary:       DOES NOT MOVE AT ALL.               │
  │ • The Logistic Regression Boundary: SHIFTS AND TILTS.                  │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### The Mathematical Explanation: Loss Function Gradients
  Why does Logistic Regression move when presented with obviously correct, easy points, while the SVM ignores them? 
  The answer lies in the **loss functions and their derivatives with respect to the margin score $z_i = y_i(w^T x_i + b)$**.
  
  ```
                   Hinge Loss vs. Logistic Loss Landscapes
    Loss ℒ(z)
      ▲
    3 │  \  Logistic Loss: ln(1 + e⁻ᶻ)
      │   \   (Never strictly zero; tails linger to infinity)
    2 │    \           \
      │     \           \
    1 │      \  Hinge    \
      │       \  Loss     \
      │        \           \
    0 ┼─────────●───────────- - - - - - - - - - - - - - - - - - - - - - - -▶ Margin Score z
     -2        0 1           2           3           4           5           z = yᵢ(wᵀxᵢ + b)
                 ▲
          Flat Zero Basin: ℒ_hinge = 0 and dℒ/dz = 0 for all z ≥ 1
  ```
#### 1. Logistic Regression (Negative Log-Likelihood / Soft Margin Pull):
  Under the $y_i \in \{-1, +1\}$ convention, logistic loss is:
  $$\mathcal{L}_{\text{log}}(z_i) = \ln\left(1 + e^{-y_i(w^T x_i + b)}\right)$$
  
  Evaluating the gradient with respect to $w$:
  $$\nabla_w \mathcal{L}_{\text{log}} = -\left(\frac{1}{1 + e^{y_i(w^T x_i + b)}}\right) y_i x_i = -P(Y \neq y_i \mid x_i) \, y_i x_i$$
  
  * For a point placed deep in the positive region ($z_i = +5.0$), the model predicts $P(Y = +1 \mid x_i) = 0.993$.
  * The error probability is small ($1 - 0.993 = 0.007$), but **it is strictly greater than zero**:
  $$\nabla_w \mathcal{L}_{\text{log}} = -0.007 \, y_i x_i \neq \mathbf{0}$$
  * Every single observation in the dataset exerts a non-zero pull on the parameter vector $w$. Adding 15 easy points multiplies this small gradient by 15, pulling the logistic regression boundary toward the new data cluster.
#### 2. Support Vector Machines (Hinge Loss / Hard Margin Cutoff):
  The SVM penalizes margin violations using the **Hinge Loss**:
  $$\mathcal{L}_{\text{hinge}}(z_i) = \max\Big(0, \; 1 - y_i(w^T x_i + b)\Big)$$
  
  Evaluating the subgradient with respect to $w$:
  $$\nabla_w \mathcal{L}_{\text{hinge}} = \begin{cases} -y_i x_i & \text{if } y_i(w^T x_i + b) < 1 \\ \mathbf{0} & \text{if } \mathbf{y_i(w^T x_i + b) > 1} \end{cases}$$
  
  * For any point sitting outside the margin ($z_i > 1$), the hinge loss evaluates to **identically zero**, and its gradient is **identically zero**.
  * The 15 easy points exert zero mathematical pull on the objective function. The SVM solver discards them as non-support vectors ($\alpha_i = 0$), leaving the boundary anchored strictly to the original support vectors.
  
  ---
## 3. The Fragility of the Hard-Margin SVM (Slide 10)
  
  Slide 10 reveals the fatal vulnerability of the hard-margin formulation:
  
  $$\mathbf{\text{"The hard margin insists that every point sits outside the margin. Add one point and watch."}}$$
  $$\mathbf{\text{"The margin shrinks from 2.28 to 0.28 and the line tilts by 45°."}}$$
  
  ```
                       The Outlier Breakdown (Slide 10)
         Clean Hard Margin (Width = 2.28)               Single Outlier Added (Width = 0.28)
  Feature x₂                                     Feature x₂
      ▲                                              ▲
      │           ■   ■ ■                            │           ■   ■ ■
      │        ■  ■ ■   ■   ■                        │        ■  ■ ■   ■   ■
      │   ─────────────────────── Margin = 2.28      │      /
      │                                              │    /  ─── Margin = 0.28 (Tilted by 45°!)
      │         ●   ●   ●                            │   /   ■ (ONE NOISY OUTLIER!)
      │      ● ●  ●   ●                              │  / ●   ●   ●
      └────────────────────────▶ Feature x₁          └────────────────────────▶ Feature x₁
  ```
### The Two Modes of Hard-Margin Breakdown:
  1. **Sensitivity to Outliers:**
   * Because the hard-margin constraint enforces $y_i(w^T x_i + b) \ge 1$ with zero tolerance, a single mislabeled sample or noisy measurement located near the opposing class forces the entire margin to collapse.
   * On Slide 10, adding one slightly misaligned point shrinks the margin by **$88\%$** ($2.28 \to 0.28$) and rotates the decision boundary by $45^\circ$, sacrificing generalization across the entire population to accommodate a single rogue sample.
  2. **Inability to Handle Non-Linearly Separable Data:**
   * If even two opposing points overlap in feature space ($x_1 \approx x_2$ with $y_1 = +1, y_2 = -1$), the constraint set:
     $$y_1(w^T x_1 + b) \ge 1 \quad \text{and} \quad y_2(w^T x_2 + b) \ge 1$$
     becomes mathematically impossible to satisfy simultaneously.
   * The feasible set of the Quadratic Program becomes **empty**; optimization fails, and no solution exists.
  
  ---
## 4. The Soft-Margin SVM: Slack Variables $\xi_i$ (Slide 11)
  
  Slide 11 introduces the classic solution formalized by Cortes and Vapnik (1995):
  
  $$\mathbf{\text{"Let each point violate the margin by an amount } \xi_i \ge 0\text{, and pay for it in the objective."}}$$
  
  ```
                      The Geometry of Slack Violations (Slide 11)
  Feature x₂
      ▲
      │           ■   ■ ■                  wᵀx + b = -1  (Negative Margin Boundary)
      │        ■  ■ ■   ■   ■
      │   - - - - - - - - - - - - - - - -  wᵀx + b = 0   (Decision Boundary)
      │          ● (ξ = 0.57: Inside margin, correct side)
      │   - - - - - - - - - - - - - - - -  wᵀx + b = +1  (Positive Margin Boundary)
      │        ■ (ξ = 2.10: Wrong side of decision boundary / Misclassified!)
      │      ● ●  ●   ●
      │         ● (ξ = 0: Outside margin, perfectly clean)
      └────────────────────────────────────────────────────────▶ Feature x₁
  ```
  
  ---
### A. The Soft-Margin Primal Formulation
  We relax the rigid inequality constraints by introducing non-negative **slack variables** $\xi_i \ge 0$:
  
  $$\mathbf{y_i(w^T x_i + b) \ge 1 - \xi_i, \quad \xi_i \ge 0 \quad \forall \; i = 1, \dots, n}$$
  
  To prevent the optimizer from setting $\xi_i \to \infty$ arbitrarily, we penalize the total sum of slack in the objective function, scaled by the regularization hyperparameter $C > 0$:
  
  ```
                       The Soft-Margin Optimization Problem (Slide 11)
  ┌────────────────────────────────────────────────────────────────────────┐
  │               minimize       ½ ||w||₂²  +  C ∑_{i=1}ⁿ ξᵢ               │
  │               w, b, ξ                                                  │
  │                                                                        │
  │               subject to     yᵢ(wᵀxᵢ + b) ≥ 1 - ξᵢ    ∀ i = 1, ..., n  │
  │                              ξᵢ ≥ 0                   ∀ i = 1, ..., n  │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
### B. Geometric Taxonomy of Slack Values (Slide 11)
  Slide 11 formalizes the three spatial regimes of $\xi_i$:
  
  ```
  ┌───────────────────────────┬───────────────────────────┬──────────────────────────────────┐
  │ Slack Value               │ Geometric Location        │ Classification Status            │
  ├───────────────────────────┼───────────────────────────┼──────────────────────────────────┤
  │ ξᵢ = 0                    │ On or outside the margin  │ Correctly classified; clean      │
  │                           │ boundaries: yᵢ zᵢ ≥ 1     │ observation.                     │
  ├───────────────────────────┼───────────────────────────┼──────────────────────────────────┤
  │ 0 < ξᵢ ≤ 1                │ Inside the margin, but on │ Correctly classified;            │
  │                           │ the correct side of the   │ violates margin street, but on   │
  │                           │ boundary: 0 ≤ yᵢ zᵢ < 1   │ the right side of the threshold. │
  ├───────────────────────────┼───────────────────────────┼──────────────────────────────────┤
  │ ξᵢ > 1                    │ Across the decision plane │ MISCLASSIFIED!                   │
  │                           │ onto the wrong side:      │ Observation is on the wrong side │
  │                           │ yᵢ zᵢ < 0                 │ of the boundary line.            │
  └───────────────────────────┴───────────────────────────┴──────────────────────────────────┘
  ```
  
  ---
### C. Theorem: $\sum \xi_i$ as an Upper Bound on Training Errors
  Slide 11 states:
  $$\mathbf{\sum \xi_i \text{ is an upper bound on the number of training errors.}}$$
#### Proof:
  An observation $x_i$ is misclassified if and only if its predicted label does not match its true label:
  $$y_i \neq \text{sign}(w^T x_i + b) \iff y_i(w^T x_i + b) < 0$$
  
  From the soft-margin constraint:
  $$y_i(w^T x_i + b) \ge 1 - \xi_i \implies \xi_i \ge 1 - y_i(w^T x_i + b)$$
  
  If an observation is misclassified ($y_i(w^T x_i + b) < 0$):
  $$\xi_i > 1 - 0 = 1 \implies \mathbf{\xi_i > 1}$$
  
  Let $N_{\text{err}} = \sum_{i=1}^n \mathbf{1}_{\{y_i(w^T x_i + b) < 0\}}$ denote the total number of training misclassifications:
  $$N_{\text{err}} = \sum_{i: \text{error}} 1 < \sum_{i: \text{error}} \xi_i \le \sum_{i: \text{error}} \xi_i + \sum_{i: \text{correct}} \xi_i = \sum_{i=1}^n \xi_i$$
  
  $$\mathbf{N_{\text{errors}} \le \sum_{i=1}^n \xi_i}$$
  
  By minimizing $\sum \xi_i$, the soft-margin SVM directly minimizes an analytical convex upper bound on the number of training classification errors.
  
  ---
### D. Equivalence to Regularized Hinge Loss
  Substituting the optimal slack value $\xi_i^* = \max\Big(0, \; 1 - y_i(w^T x_i + b)\Big)$ directly into the objective transforms the constrained optimization problem into an unconstrained loss minimization:
  
  $$\mathbf{\min_{w, b} \left[ \frac{1}{2}\|w\|_2^2 + C \sum_{i=1}^n \max\Big(0, \; 1 - y_i(w^T x_i + b)\Big) \right]}$$
  
  Dividing the entire objective by $C$:
  
  $$\min_{w, b} \left[ \sum_{i=1}^n \max\Big(0, \; 1 - y_i(w^T x_i + b)\Big) + \frac{1}{2C}\|w\|_2^2 \right]$$
  
  This formulation makes the architecture clear:
  * **Empirical Loss:** Hinge loss $\sum \mathcal{L}_{\text{hinge}}(y_i, f(x_i))$.
  * **Regularization Penalty:** $L_2$ weight decay $\frac{1}{2C}\|w\|_2^2$ with regularization parameter $\lambda = \frac{1}{C}$.
  
  ---
## 5. The $C$ Dial: Balancing Margins and Violations (Slide 12)
  
  Slide 12 illustrates how varying the hyperparameter $C$ shapes the decision space:
  
  $$\mathbf{\text{"Same 60 points, three values of } C\text{. Circled points are the support vectors."}}$$
  $$\mathbf{\text{"Small } C\text{: wide margin, many support vectors. Large } C\text{: a narrow margin that chases individual points."}}$$
  
  ```
                       The C Dial Comparison (Slide 12)
          C = 0.01 (Underfit / Soft)         C = 1.0 (Balanced)           C = 100 (Overfit / Hard)
       Margin = 6.05 | 27 Support Vectors  Margin = 1.85 | 9 Support Vectors Margin = 1.26 | 8 Support Vectors
  ┌───────────────────────────────────┐  ┌─────────────────────────────┐  ┌─────────────────────────────┐
  │         ■   ■ ■                   │  │         ■   ■ ■             │  │         ■   ■ ■             │
  │   - - - - - - - - - - - - - - - - │  │   - - - - - - - - - - - - - │  │   - - - - - - - - - - - - - │
  │   ○   ○   ○   ○   ○   ○   ○   ○   │  │              ○              │  │              ○              │
  │   ─────────────────────────────── │  │   ───────────────────────── │  │   ───────────────────────── │
  │   ○   ○   ○   ○   ○   ○   ○   ○   │  │        ○          ○         │  │        ○          ○         │
  │   - - - - - - - - - - - - - - - - │  │   - - - - - - - - - - - - - │  │   - - - - - - - - - - - - - │
  │         ●   ●   ●                 │  │         ●   ●   ●           │  │         ●   ●   ●           │
  └───────────────────────────────────┘  └─────────────────────────────┘  └─────────────────────────────┘
  Wide margin; allows many margin     Balanced trade-off between    Narrow margin; penalizes slack
  violations. High bias, low var.     margin width and errors.      heavily. Low bias, high var.
  ```
  
  ---
### The Operational Trade-Off Table:
  
  ```
  ┌───────────────────────────┬──────────────────────────────────────────┬──────────────────────────────────────────┐
  │ Property                  │ Small C (e.g., C = 0.01)                 │ Large C (e.g., C = 100)                  │
  ├───────────────────────────┼──────────────────────────────────────────┼──────────────────────────────────────────┤
  │ Slack Penalty Severity    │ Light: allows points to violate margin   │ Severe: heavily penalizes any margin     │
  │                           │ freely to maintain a wide street.        │ encroachment (ξᵢ > 0).                   │
  ├───────────────────────────┼──────────────────────────────────────────┼──────────────────────────────────────────┤
  │ Margin Width (2 / ||w||)  │ WIDE (e.g., width = 6.05).               │ NARROW (e.g., width = 1.26).             │
  ├───────────────────────────┼──────────────────────────────────────────┼──────────────────────────────────────────┤
  │ Weight Norm (||w||)       │ SMALL (strong L2 penalty).               │ LARGE (weak L2 penalty).                 │
  ├───────────────────────────┼──────────────────────────────────────────┼──────────────────────────────────────────┤
  │ Support Vector Count      │ MANY (e.g., 27 SVs out of 60 points).    │ FEW (e.g., 8 SVs out of 60 points).      │
  │                           │ Every point inside margin is an SV!      │ Only boundary edge cases are SVs.        │
  ├───────────────────────────┼──────────────────────────────────────────┼──────────────────────────────────────────┤
  │ Bias-Variance Regime      │ HIGH BIAS, LOW VARIANCE (Underfitting).  │ LOW BIAS, HIGH VARIANCE (Overfitting).   │
  │                           │ Resilient to noise; flat boundary.       │ Chases individual sample noise/outliers. │
  └───────────────────────────┴───────────────────────────┴──────────────────────────────────────────┘
  ```
### Why Does Small $C$ Increase the Number of Support Vectors?
  In a soft-margin SVM, the KKT conditions dictate that a point is a **Support Vector** ($\alpha_i^* > 0$) if it lies **on the margin boundary OR inside the margin**:
  
  $$i \in \text{Support Vectors} \iff y_i(w^T x_i + b) \le 1 \iff \xi_i \ge 0$$
  
  * When $C$ is small, the margin street is wide ($2/\|w\| \gg 0$).
  * A wide street physically covers a large portion of the feature space, engulfing dozens of training points inside the margin boundaries ($\xi_i > 0$).
  * Every single point lying inside the margin receives an active Lagrange multiplier ($\alpha_i^* = C$), dramatically **increasing the total count of support vectors** (Slide 12: 27 SVs at $C = 0.01$ vs. 8 SVs at $C = 100$).
  
  ---
## Summary Review Questions for Section 2
  
  1. *In Slide 9, why does adding 15 easy, correctly classified points deep in the positive region cause Logistic Regression to shift its boundary, while the Support Vector Machine boundary remains completely stationary?*
  2. *If an observation has a slack value of $\xi_i = 0.65$, which side of the decision boundary does it lie on? Has it been misclassified?*
  3. *Why does decreasing the regularization parameter $C$ in a soft-margin SVM cause the total number of support vectors to increase?*
  
  ---