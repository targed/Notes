## 1. Geometric Lifting: From Non-Separable Inputs to Linear Hyperplanes (Slides 2–5)
  
  Slide 2 opens with an intuitive 1-dimensional diagnostic:
  
  $$\mathbf{\text{"One feature } x\text{. Where would a threshold go? Teal sits in the middle, orange at both ends."}}$$
  $$\mathbf{\text{"No single threshold on } x \text{ works. You may add ONE new feature, computed from } x\text{. Which one?"}}$$
  
  ```
                       The 1D Non-Separable Dilemma (Slide 2)
  ───────■──■─■─■──■──────────●─●──●─●─●──●──────────■──■──■─■──■───────▶ Feature x
        -3      -2           -1    0     1           2      3
        ■ = Class -1 (Orange)       ● = Class +1 (Teal)
  ```
  
  ---
### A. The 1D-to-2D Parabolic Lift: $\phi(x) = (x, x^2)$ (Slide 3)
  Slide 3 reveals the solution:
  
  $$\mathbf{\text{"Plot each point at } (x, x^2) \text{ and fit the same linear SVM."}}$$
  $$\mathbf{\text{"A line in } (x, x^2) \text{ is two thresholds on } x\text{. Linear in } \phi(x)\text{, not linear in } x\text{."}}$$
  
  ```
                        The Parabolic Lift in ℝ² (Slide 3)
    x²
    ▲
  10 ┼  ■ (-3, 9)                                               ■ (3, 9)
    │   ■                                                     ■
    │    ■                                                   ■
    │     ■ (-2, 4)                                         ■ (2, 4)
  2 ┼──────\───────────────────────────────────────────────/────── Separating Hyperplane
    │       \                                             /        w₁x + w₂x² + b = 0
    │        \       ● (-1, 1)                  ● (1, 1) /
    │         \──●───────●───────●───────●───────●──────/
  0 ┴────────────┴───────┴───────┴───────┴───────┴───────────────▶ x
    -3          -2      -1       0       1       2       3
  ```
#### Mathematical Mechanics:
  * In the 1D input space $\mathcal{X} = \mathbb{R}$, no single threshold $w_1 x + b = 0$ can isolate the middle cluster (Class $+1$) from the outer clusters (Class $-1$).
  * Define the non-linear lifting map:
  $$\phi(x) = \begin{bmatrix} x \\ x^2 \end{bmatrix} \in \mathbb{R}^2$$
  * In the lifted 2D space $\mathbb{R}^2$, we fit an Ordinary Linear SVM:
  $$w_1 x + w_2 x^2 + b = 0$$
  * If the solver sets $w_1 = 0$, $w_2 = -1$, and $b = 2$, the linear decision line is a simple horizontal slice:
  $$-x^2 + 2 = 0 \implies x^2 = 2 \implies x = \pm \sqrt{2}$$
  * **The Boundary Projection:** Projecting this 2D straight line back into the original 1D input space produces **two simultaneous symmetric decision thresholds**:
  $$\text{Predict Class } +1 \iff -\sqrt{2} \le x \le +\sqrt{2}$$
  
  ---
### B. The 2D-to-3D Elliptic Lift: $\phi(x) = (x_1^2, \; \sqrt{2}x_1 x_2, \; x_2^2)$ (Slides 4–5)
  Slides 4 and 5 expand this concept to 2-dimensional concentric rings:
  
  $$\mathbf{\text{"Two features are not enough: Teal inside, orange on a ring around it."}}$$
  $$\mathbf{\text{"Map each point to } \phi(x) = (x_1^2, \; \sqrt{2}x_1 x_2, \; x_2^2) \text{ and fit the same linear SVM. A flat plane in 3-D is a circle in 2-D."}}$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Coordinate Transformation:                                         │
  │                                                                        │
  │   Input Space:      x = [x₁, x₂]ᵀ ∈ ℝ²                                 │
  │   Lifted Space:     φ(x) = [z₁, z₂, z₃]ᵀ = [x₁²,  √2 x₁x₂,  x₂²]ᵀ ∈ ℝ³ │
  │                                                                        │
  │ In ℝ³, the linear decision boundary is an affine flat plane:           │
  │                    w₁ z₁ + w₂ z₂ + w₃ z₃ + b = 0                       │
  │                                                                        │
  │ Substituting the lifted coordinates back into original space:          │
  │                    w₁ x₁² + √2 w₂ x₁x₂ + w₃ x₂² + b = 0                │
  │                                                                        │
  │ When w₁ = 1, w₂ = 0, w₃ = 1, and b = -R²:                             │
  │                         x₁² + x₂² = R²                                 │
  │ A flat 2D plane in 3D projects down to a closed circle in 2D!          │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 2. Solving a Linear SVM by Hand (Slide 6)
  
  Slide 6 presents an exact geometric derivation of a linear Support Vector Machine on an 8-sample toy dataset:
  
  $$\mathbf{\text{"8 samples. The 3 circled support vectors lie on the margin: } y_i(w^T x_i + b) = 1 \text{ for each."}}$$
  
  ```
                       The 8-Sample Geometry (Slide 6)
    x₂
    ▲
  2 ┼
    │                                         ● (3, 1)  [Class +1, Support Vector]
  1 ┼        ■ (-1, 1)                        │
    │                                         │
  0 ┼───■───────■ (1, 0) [SV] ────────────────┼───────────────────────● (6, 0)
    │  (-1,0)   │                             │
    │           │                             ● (3, -1) [Class +1, Support Vector]
  -1 ┼        ■ (-1, -1)                       │
    │           │                             │
    └───────────┼─────────────┼───────────────┼───────────────────────▶ x₁
     -1         1             2               3           6
            x₁ = 1          x₁ = 2          x₁ = 3
         (Margin -1)      (Boundary)      (Margin +1)
  ```
  
  The three active Support Vectors that define the boundary are:
  1. $x_1 = (1, 0)^T, \quad y_1 = -1$
  2. $x_2 = (3, 1)^T, \quad y_2 = +1$
  3. $x_3 = (3, -1)^T, \quad y_3 = +1$
  
  ---
### Step 1: Write the Margin Equations for the Support Vectors
  Because support vectors lie directly on the canonical margin boundaries:
  $$y_i(w^T x_i + b) = 1 \iff w^T x_i + b = y_i$$
  
  Evaluating for each support vector:
  1. For $x_1 = (1, 0)^T, \; y_1 = -1$:
   $$w_1(1) + w_2(0) + b = -1 \implies \mathbf{w_1 + b = -1}$$
  2. For $x_2 = (3, 1)^T, \; y_2 = +1$:
   $$w_1(3) + w_2(1) + b = +1 \implies \mathbf{3w_1 + w_2 + b = +1}$$
  3. For $x_3 = (3, -1)^T, \; y_3 = +1$:
   $$w_1(3) + w_2(-1) + b = +1 \implies \mathbf{3w_1 - w_2 + b = +1}$$
  
  ---
### Step 2: Solve the Linear System (3 Equations, 3 Unknowns)
  Subtract Equation 3 from Equation 2:
  $$(3w_1 + w_2 + b) - (3w_1 - w_2 + b) = 1 - 1$$
  $$2w_2 = 0 \implies \mathbf{w_2 = 0}$$
  
  Substitute $w_2 = 0$ back into Equation 2:
  $$3w_1 + b = 1$$
  
  Now solve the $2 \times 2$ system with Equation 1:
  $$\begin{cases} w_1 + b = -1 \\ 3w_1 + b = 1 \end{cases}$$
  
  Subtract the first from the second:
  $$(3w_1 + b) - (w_1 + b) = 1 - (-1) \implies 2w_1 = 2 \implies \mathbf{w_1 = 1}$$
  
  Substitute $w_1 = 1$ into Equation 1:
  $$1 + b = -1 \implies \mathbf{b = -2}$$
#### The Geometric Parameters:
  * **Optimal Weight Vector:** $\mathbf{w^* = (1, \; 0)^T}$
  * **Optimal Bias:** $\mathbf{b^* = -2}$
  * **Decision Boundary:** $w_1 x_1 + w_2 x_2 + b = 0 \implies 1(x_1) + 0(x_2) - 2 = 0 \implies \mathbf{x_1 = 2}$
  * **Margin Width:** $\gamma = \frac{2}{\|w\|_2} = \frac{2}{\sqrt{1^2 + 0^2}} = \mathbf{2.000}$
  
  ---
## 3. Where Do the $\alpha$'s Come From? The Lagrangian Dual (Slide 7)
  
  Slide 7 connects the geometric weights to the underlying optimization theory:
  
  $$\mathbf{\text{"Why } \alpha\text{? With a kernel, } w \text{ can have } \infty \text{ coordinates, but there is only one } \alpha_i \text{ per training point."}}$$
  $$\mathbf{\text{"We don't pick the SVs. The solver does: Support Vectors = points with } \alpha_i > 0\text{."}}$$
  
  To find the optimal hyperplane, we construct the **Primal Lagrangian**:
  
  $$L(w, b, \alpha) = \frac{1}{2}\|w\|_2^2 - \sum_{i=1}^n \alpha_i \Big[ y_i(w^T x_i + b) - 1 \Big], \quad \text{where } \alpha_i \ge 0$$
  
  ```
                         The Three Governing KKT Rules (Slide 7)
  ┌────────────────────────────────────────────────────────────────────────┐
  │ Rule 1: w from the Data (Stationarity w.r.t. w)                        │
  │   ∂L / ∂w = w - ∑ αᵢ yᵢ xᵢ = 0  ──▶  w = ∑_{i=1}ⁿ αᵢ yᵢ xᵢ             │
  │   • The weight vector w is a linear combination of training points!    │
  │   • αᵢ quantifies the "importance weight" of training point i.         │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Rule 2: Balance (Stationarity w.r.t. b)                                │
  │   ∂L / ∂b = -∑ αᵢ yᵢ = 0  ──▶  ∑_{i=1}ⁿ αᵢ yᵢ = 0                      │
  │   • Total α on the +1 side MUST equal total α on the -1 side!          │
  ├────────────────────────────────────────────────────────────────────────┤
  │ Rule 3: Only SVs Count (KKT Complementary Slackness)                   │
  │   αᵢ · [ yᵢ(wᵀxᵢ + b) - 1 ] = 0                                        │
  │   • Off the margin: yᵢ(wᵀxᵢ + b) > 1  ──▶  αᵢ = 0.0                    │
  │   • On the margin:  yᵢ(wᵀxᵢ + b) = 1  ──▶  αᵢ ≥ 0.0 (Support Vectors!) │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  ---
## 4. Solving for the Exact Dual Multipliers ($\alpha_i$) by Hand (Slide 8)
  
  Slide 8 demonstrates how to calculate the exact numerical values of the Lagrange multipliers $\alpha_i$ for our 8-sample dataset:
  
  $$\mathbf{\text{Known: } w = (1, 0)^T. \quad \text{Unknowns: } \alpha_1, \alpha_2, \alpha_3 \text{ for the 3 SVs. Rule 3: other 5 } \alpha\text{'s are } 0.}$$
  
  ---
### Step 1: Write Out Rule 1 Vectorially
  Expand $w = \sum_{i=1}^3 \alpha_i y_i x_i$:
  
  $$w = \alpha_1 (-1) \begin{bmatrix} 1 \\ 0 \end{bmatrix} + \alpha_2 (+1) \begin{bmatrix} 3 \\ 1 \end{bmatrix} + \alpha_3 (+1) \begin{bmatrix} 3 \\ -1 \end{bmatrix}$$
  
  Distribute into coordinate components:
  
  $$w = \begin{bmatrix} -\alpha_1 \\ 0 \end{bmatrix} + \begin{bmatrix} 3\alpha_2 \\ \alpha_2 \end{bmatrix} + \begin{bmatrix} 3\alpha_3 \\ -\alpha_3 \end{bmatrix} = \begin{bmatrix} -\alpha_1 + 3\alpha_2 + 3\alpha_3 \\ \alpha_2 - \alpha_3 \end{bmatrix}$$
  
  Because $w = (1, 0)^T$, equate the vectors:
  
  $$\mathbf{\begin{bmatrix} -\alpha_1 + 3\alpha_2 + 3\alpha_3 \\ \alpha_2 - \alpha_3 \end{bmatrix} = \begin{bmatrix} 1 \\ 0 \end{bmatrix}}$$
  
  ---
### Step 2: System of Equations & Solution (Slide 8 Table)
  
  ```
  ┌───────────────────────────┬──────────────────────────────────────────┬───────────────────────┐
  │ Source of Equation        │ Algebraic Expression                     │ Deductions / Solves   │
  ├───────────────────────────┼──────────────────────────┼───────────────────────┤
  │ Rule 1, 2nd Coordinate    │ α₂ - α₃ = 0                              │ α₂ = α₃               │
  │                           │                                          │                       │
  │ Rule 2 (Balance)          │ -α₁ + α₂ + α₃ = 0                        │ α₁ = 2α₂              │
  │                           │                                          │                       │
  │ Rule 1, 1st Coordinate    │ -α₁ + 3α₂ + 3α₃ = 1                      │ -2α₂ + 6α₂ = 1        │
  │                           │ Substitute α₁ = 2α₂ and α₃ = α₂:         │ 4α₂ = 1 ──▶ α₂ = ¼    │
  └───────────────────────────┴──────────────────────────────────────────┴───────────────────────┘
  ```
#### Final Numerical Dual Solutions:
  * $\alpha_2 = \mathbf{\frac{1}{4} = 0.250}$
  * $\alpha_3 = \alpha_2 = \mathbf{\frac{1}{4} = 0.250}$
  * $\alpha_1 = 2\alpha_2 = 2\left(\frac{1}{4}\right) = \mathbf{\frac{1}{2} = 0.500}$
  * For the remaining 5 non-support vectors: $\alpha_4 = \alpha_5 = \alpha_6 = \alpha_7 = \alpha_8 = \mathbf{0.000}$
  
  ---
### Verification:
  1. **Balance Check (Rule 2):**
   $$\sum_{i=1}^3 \alpha_i y_i = \left(\frac{1}{2}\right)(-1) + \left(\frac{1}{4}\right)(+1) + \left(\frac{1}{4}\right)(+1) = -\frac{1}{2} + \frac{1}{2} = \mathbf{0 \quad (\text{Balanced!})}$$
  2. **Weight Reconstruction (Rule 1):**
   $$w = \frac{1}{2}(-1)\begin{bmatrix} 1 \\ 0 \end{bmatrix} + \frac{1}{4}(+1)\begin{bmatrix} 3 \\ 1 \end{bmatrix} + \frac{1}{4}(+1)\begin{bmatrix} 3 \\ -1 \end{bmatrix} = \begin{bmatrix} -0.5 + 0.75 + 0.75 \\ 0 + 0.25 - 0.25 \end{bmatrix} = \mathbf{\begin{bmatrix} 1 \\ 0 \end{bmatrix} \equiv w^*}$$
  
  ---
## Summary Review Questions for Section 1
  
  1. *In the 1D-to-2D lift $\phi(x) = (x, x^2)$, prove how a linear decision boundary $w_1 x + w_2 x^2 + b = 0$ is able to construct two distinct decision thresholds in the original 1D input space.*
  2. *State the three KKT conditions governing the relationship between the optimal weight vector $w^*$, the bias $b^*$, and the dual Lagrange multipliers $\alpha_i^*$.*
  3. *Why is the dual formulation ($\alpha_i$) computationally advantageous over the primal formulation ($w$) when working with non-linear feature maps $\phi(x)$ that have very high or infinite dimensions?*
  
  ---