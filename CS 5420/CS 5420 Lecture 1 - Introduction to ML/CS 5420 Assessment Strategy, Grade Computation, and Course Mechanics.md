## 1. Quick Review: Answers to Section 2 Checkpoints
  
  1. **MAP Estimation & Weight Penalties ($L_1$ vs. $L_2$):**
  * Maximizing the posterior corresponds to minimizing $-\log P(Y \mid X, w) - \log P(w)$.
  * Under a zero-mean **Gaussian prior** $P(w) \propto \exp\left(-\frac{\|w\|_2^2}{2\sigma^2}\right)$, the negative log prior is proportional to $\sum w_j^2$, yielding the smooth quadratic $L_2$ penalty (Ridge).
  * Under an independent **Laplace prior** $P(w) \propto \exp\left(-\frac{\|w\|_1}{b}\right)$, the negative log prior is proportional to $\sum |w_j|$, which has a non-differentiable sharp mode at zero. In optimization space, the diamond-shaped level sets of the $L_1$ ball intersect the loss contours at the coordinate axes, forcing coefficients strictly to zero (sparsity).
  2. **The Kernel Trick and RKHS:**
  * Evaluating high-dimensional feature mappings $\phi(x) \in \mathbb{R}^D$ requires computing $D$ explicit values per sample. If $D$ is very large or infinite (as in Gaussian RBF), calculating inner products $\langle \phi(x), \phi(x') \rangle$ directly is computationally intractable.
  * By Mercer’s Theorem, if a kernel function $K(x, x')$ is continuous, symmetric, and positive semi-definite, it computes the inner product in a Reproducing Kernel Hilbert Space (RKHS) directly in input space $\mathbb{R}^d$ in $\mathcal{O}(d)$ time without ever materializing $\phi(x)$.
  3. **SVD Equivalence to Covariance Eigendecomposition in PCA:**
  * For a zero-mean data matrix $X \in \mathbb{R}^{N \times d}$, the empirical covariance matrix is $C = \frac{1}{N-1}X^T X$.
  * Taking the Singular Value Decomposition $X = U \Sigma V^T$, we substitute:
   $$C = \frac{1}{N-1}(U \Sigma V^T)^T (U \Sigma V^T) = \frac{1}{N-1} V \Sigma U^T U \Sigma V^T$$
  * Because $U$ is orthonormal ($U^T U = I$):
   $$C = V \left(\frac{\Sigma^2}{N-1}\right) V^T$$
  * This is the exact spectral eigendecomposition $V \Lambda V^T$. Thus, the right-singular vectors $V$ of the raw data matrix are identically the principal eigenvectors of the covariance matrix, and the singular values relate to the eigenvalues by $\lambda_i = \frac{\sigma_i^2}{N-1}$.
  
  ---
## 2. Mathematical Breakdown of Course Grading
  
  The grading architecture consists of three distinct components:
  
  $$\text{Final Score} = 0.40 \cdot S_{\text{assign}} + 0.40 \cdot S_{\text{exams}} + 0.20 \cdot S_{\text{project}}$$
  
  Where each component score $S$ is computed as a percentage: $S = \left(\frac{\text{Points Earned}}{\text{Points Available}}\right) \times 100$.
  
  ```
                      Course Grade Distribution
  ┌────────────────────────────────────────────────────────────────────────┐
  │  Coding Assignments (7 × 100 pts)  ──▶  40% of Total Course Grade      │
  │  In-Semester Exams (3 × 100 pts)   ──▶  40% of Total Course Grade      │
  │  Group Project                     ──▶  20% of Total Course Grade      │
  └────────────────────────────────────────────────────────────────────────┘
  ```
### Component Sensitivity Analysis (The Risk Surface)
  
  A common mistake in graduate coursework is failing to understand the **marginal sensitivity** of each graded event:
  
  | Assessment Unit | Quantity | Points Each | Total Category Points | Final Course Grade Impact per Point Lost | Final Course Grade Impact per Assessment |
  | :--- | :---: | :---: | :---: | :---: | :---: |
  | **Individual Assignment** | 7 | 100 | 700 | $\frac{40\%}{700} \approx \mathbf{0.057\%}$ | $\frac{40\%}{7} \approx \mathbf{5.71\%}$ |
  | **Individual Exam** | 3 | 100 | 300 | $\frac{40\%}{300} \approx \mathbf{0.133\%}$ | $\frac{40\%}{3} \approx \mathbf{13.33\%}$ |
  | **Group Project** | 1 | 100 | 100 | $\frac{20\%}{100} = \mathbf{0.200\%}$ | $\mathbf{20.00\%}$ |
### Strategic Implications:
  1. **Exams Have High Leverage ($2.33\times$ Assignment Weight):**
   * Missing or failing a single exam is catastrophic. A zero on one exam drops your maximum possible course score to $86.67\%$, immediately eliminating an "A" regardless of assignment performance.
  2. **Assignments Are Resilient:**
   * A drop of 15 points on one assignment reduces your final grade by only $15 \times 0.057\% = 0.85\%$. Assignments serve as a protective baseline, while exams represent the primary variance in student outcomes.
  3. **No Final Exam:**
   * Since there is no comprehensive final exam, testing is strictly modular. Exam 3 covers Modules 10–13 during the regular semester, meaning post-Thanksgiving performance hinges heavily on the Group Project deliverables.
  
  ---
## 3. The Grading Scale, Non-Rounding, and Attendance Bonus Mechanics
  
  Slide 11 outlines a strict fixed scale with an asymmetry:
  
  $$\mathbf{A}: [90.0, \infty) \quad\Big|\quad \mathbf{B}: [80.0, 90.0) \quad\Big|\quad \mathbf{C}: [70.0, 80.0) \quad\Big|\quad \mathbf{D}: [60.0, 70.0)$$
### A. The "Scores Are Not Rounded" Constraint
  * A final calculated score of **$89.99\%$ is recorded as a B**.
  * Dr. Yu notes that adjustments can only be applied **upward** across the entire class if an assessment was empirically calibrated too difficult. Individual curves or rounding negotiations are explicitly disallowed.
### B. The Attendance Optimization Game
  * **Slide 13 Policy:** Attendance is formally optional, but unannounced sign-in sheets appear at random times on random days.
  * **The Mathematical Nature of the Points:** Sign-ins are awarded as **additive bonus points** on top of your final computed score.
  * **Why This Matters:**
  * Because grades are strictly not rounded, having even $+1.0$ or $+1.5$ percentage points of raw bonus eliminates borderline edge cases (e.g., converting an $88.8\%$ directly into an $89.8\%$ or $90.3\%$).
  * Regularly attending lecture functions as an insurance policy against exam variance and non-rounding policies.
  
  ---
## 4. Deadline Penalties and Submission Dynamics
  
  Slide 16 establishes an explicit decay function for late work:
  
  $$\text{Effective Score}(t) = \text{Raw Score} - 10 \cdot \left\lceil \frac{t}{24} \right\rceil \quad \text{for } 0 < t \le 72 \text{ hours}$$
  
  $$\text{Effective Score}(t) = 0 \quad \text{for } t > 72 \text{ hours}$$
  
  ```
                Late Submission Penalty Structure
  Points Deducted
      ▲
  30 ─┼──────────────────────────────┐ (48h < t ≤ 72h: -30 pts)
      │                              │
  20 ─┼───────────────┐              │ (24h < t ≤ 48h: -20 pts)
      │               │              │
  10 ─┼───────┐       │              │ (0 < t ≤ 24h:   -10 pts)
      │       │       │              │
   0 ─┴───────┴───────┴──────────────┴────────▶ Time Late (Hours)
     0h      24h     48h            72h     >72h (Zero Credit)
  ```
### The Late-Submission Decision Rule:
  * **Scenario A (Marginal Gain vs. Penalty):**
  If your code scores 75/100 at 11:58 PM, but you need 3 more hours to achieve 95/100:
  * Submitting on time: $75 \text{ pts}$.
  * Submitting 3 hours late: $95 - 10 = 85 \text{ pts}$.
  * **Decision:** Take the 10% penalty for a net gain of $+10$ points ($+0.57\%$ course grade).
  * **Scenario B (Diminishing Returns):**
  If pushing past the 24-hour mark yields only an incremental 5–8 points, taking the second 10% penalty results in a net loss. Never let an assignment cross the 24-hour boundary unless you are turning a near-zero submission into a near-perfect one.
  * **Hard Cutoff:** At $t = 72.01\text{ hours}$, credit drops instantaneously to zero.
  
  ---
## 5. Course Protocols: Regrades & Communication
### A. The Two-Stage Regrade Escalation Protocol (Slide 17)
  1. **Stage 1 (Teaching Assistant):**
   * Must be initiated within **7 calendar days** of scores being posted to Canvas.
   * Handled by the graduate TA, **Jiaqian Zhu** (`jzbmn@mst.edu`).
   * *Strict Statute of Limitations:* Requests submitted on day 8 or later are dismissed automatically.
  2. **Stage 2 (Instructor Escalation):**
   * If the resolution with the TA is unsatisfactory, you have a second **7-day window** to appeal directly to Dr. Yu.
   * *Warning:* Dr. Yu’s review is final and can result in the grade staying the same, increasing, or decreasing upon closer inspection.
### B. Communication Hygiene (Slide 18)
  * **Inbox Filtering Rule:** Subject line must include the exact tag: `"CS5420"`.
  * **Channel:** Institutional email (`@mst.edu`) exclusively. Non-university domains are ignored for identity verification and FERPA compliance.
  * **SLA (Service Level Agreement):**
  * Weekdays: Replies within 24 hours.
  * Weekends: Replies within 48 hours.
  
  ---
## 6. Academic Calendar Constraints & The Critical Convergence
  
  Slide 12 lists key university holidays:
  * **No Class:**
  * Monday, 09/07 (Labor Day)
  * Friday, 10/09 (Fall Break)
  * Wednesday, 11/11 (Veterans Day)
  * November 23–27 (Thanksgiving Recess)
  * **Final Class Day:** Friday, December 11
### The End-of-Semester Convergence Warning:
  Notice how the calendar compresses after Thanksgiving. From November 30 to December 11, three major items collide:
  1. **Exam 3** (Modules 10–13: Deep Learning, NLP, Transformers)
  2. **Final Project Report/Notebook Submission** (Due Monday, Dec 7)
  3. **Project Presentations** (Wednesday, Dec 9 and Friday, Dec 11)
  
  To manage this workload effectively, the group project code and experimental comparisons should be completed *before* departing for Thanksgiving break.
  
  ---
## Summary Review Questions for Section 3
  
  1. *If a student enters the final two weeks with a 92% average on assignments and an 81% average on exams, what minimum score do they need on the 100-point Group Project to guarantee an overall grade of "A" (90.0%)?*
  2. *Why does Dr. Yu's non-rounding policy make random attendance sign-ins significantly more valuable than standard extra credit?*
  3. *Under the late submission policy, if an assignment is currently running at a projected 60/100, is it mathematically advantageous to spend an extra 18 hours to fix the pipeline to a 95/100?*
  
  ---