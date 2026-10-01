## 1. The Core Philosophy of First-Order Optimization (Slide 2)
  
  Slide 2 introduces the fundamental iterative algorithm of modern machine learning:
  
  $$\mathbf{\text{"Stand on the error surface, feel the slope, and step downhill."}}$$
  
  $$\mathbf{w \longleftarrow w - \eta \nabla J(w)}$$
  
  ```
                 The Gradient Descent Iterative Loop
  ┌────────────────────────────────────────────────────────────────────────┐
  │ 1. Current State:       Parameter vector w^(t) ∈ ℝ^(d+1)               │
  │ 2. Evaluate Slope:      Compute gradient vector ∇J(w^(t))              │
  │ 3. Compute Step:        Scale gradient by learning rate η              │
  │ 4. Update Position:     Step in opposite direction: -η ∇J(w^(t))       │
  │ 5. Termination:         Repeat until ||∇J(w)|| < ε (Ground is flat)    │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  Slide 2 highlights three core theoretical properties:
  1. **$\eta$ (Learning Rate):** The scalar hyperparameter controlling step size along the negative gradient vector.
  * Too large ($\eta \gg 0$): Overshoots the valley and diverges.
  * Too small ($\eta \approx 0$): Stalls in flat regions, consuming excessive compute.
  2. **Convexity Guarantee:** For convex loss functions (such as Ordinary Least Squares MSE), **any local stationary point is guaranteed to be the unique global minimum**.
  3. **Iterative Descent:** Updates continue until the magnitude of the gradient approaches numerical zero ($\|\nabla J(w)\|_2 \to 0$).
  
  ---
## 2. Mathematical Formalization: Why the Negative Gradient?
  
  To prove why stepping in the direction of $-\nabla J(w)$ minimizes loss, consider the first-order **Taylor Series Expansion** of the cost function $J(w)$ perturbed by a small displacement vector $\Delta w$:
  
  $$J(w + \Delta w) \approx J(w) + \nabla J(w)^T \Delta w$$
  
  We wish to choose a directional unit step $u = \frac{\Delta w}{\|\Delta w\|}$ of fixed length $\alpha = \|\Delta w\|$ that minimizes the change in loss $\Delta J = J(w + \Delta w) - J(w)$:
  
  $$\min_{u, \; \|u\| = 1} \Delta J \approx \min_{u, \; \|u\| = 1} \alpha \Big( \nabla J(w)^T u \Big)$$
  
  Using the definition of the vector dot product:
  
  $$\nabla J(w)^T u = \|\nabla J(w)\|_2 \|u\|_2 \cos(\theta) = \|\nabla J(w)\|_2 \cos(\theta)$$
  
  where $\theta$ is the angle between the gradient vector and the step direction $u$.
  * To minimize this quantity, we must minimize $\cos(\theta)$.
  * The minimum occurs at $\theta = \pi$ ($180^\circ$), where $\cos(\theta) = -1$.
  * Therefore, the direction $u$ must point **directly opposite** to the gradient vector:
  $$u^* = -\frac{\nabla J(w)}{\|\nabla J(w)\|_2}$$
  * Multiplying by the scalar step size $\eta$ yields the update rule:
  $$\Delta w = -\eta \nabla J(w) \implies \mathbf{w^{(t+1)} = w^{(t)} - \eta \nabla J(w^{(t)})}$$
  
  $$\mathbf{\text{The gradient points in the direction of steepest ascent; the negative gradient points in steepest descent.}}$$
  
  ---
## 3. Spot the Learning Rate: Loss Curve Diagnostics (Slides 3, 5)
  
  Slide 3 presents an everyday debugging challenge: diagnosing the learning rate purely by inspecting the training loss curve over time.
  
  ```
                      Loss Curve Topologies (Slide 3)
     Too Large (η high)              Too Small (η low)               Just Right (Optimal η)
  Loss                           Loss (Log Scale)               Loss
   ▲    /\    /\                  ▲                              ▲
   │   /  \  /  \  Divergence     │ 10⁰                          │ 35 ─●
   │  /    \/    \                │                              │ 25 ───╮
   │ /            \               │ 10⁻² ────────────── Plateau  │ 15 ────╰─╮
   │/              \              │      (Stalled / Crawling)    │  5 ───────╰─────────
   └──────────────────▶ Steps     └──────────────────▶ Epochs    └──────────────────▶ Epochs
     Wild Oscillations              0   100  200  300  400         0   5   10  15  20  25
  ```
  
  ---
### Step-by-Step Solutions to Dr. Yu’s Diagnostic Questions (Slide 5):
  
  Slide 5 compares two optimization paths on a 1D quadratic bowl:
  
  ```
                  Geometric Step Behavior in a Valley (Slide 5)
        Large Learning Rate (Left)                     Small Learning Rate (Right)
   -l(w)                                          -l(w)
     ▲                                              ▲
     │  \                                           │  \
     │   \      w⁰                                  │   \      w⁰
     │    \    /                                    │    \    /  ● w¹
     │     \  /  ● w¹ (Overshoots)                  │     \  /  ● w²
     │      \/                                      │      \/  ● w³ (Tiny crawl)
     └────────────────────────▶ w                   └────────────────────────▶ w
     • Bounces back and forth                       • Monotonic, slow crawl
     • Large residual oscillations                  • Small residual error
  ```
  
  1. **Which one converges first?**
   * **The moderate/optimal learning rate.** It takes assertive steps down the gradient without overshooting the valley floor, achieving asymptotic convergence in the fewest number of epochs.
  2. **Which one might never converge?**
   * **The large learning rate ($\eta$ high).** If the step size exceeds the curvature of the bowl, the update lands higher on the opposite canyon wall than where it started ($J(w^{(t+1)}) > J(w^{(t)})$). The loss oscillates with increasing amplitude and diverges to infinity ($\text{NaN}$).
  3. **Which one is safe but expensive?**
   * **The small learning rate ($\eta$ low).** It is mathematically guaranteed to decrease loss monotonically on convex surfaces, but it requires hundreds of thousands of matrix evaluations, wasting computational time and energy.
  4. **How would you detect each case from the loss curve alone?**
   * **$\eta$ Too High:** The loss curve spikes upward, oscillates erratically, or outputs `NaN` / `inf`.
   * **$\eta$ Too Low:** The loss curve forms an almost horizontal, flat line that barely decreases across hundreds of epochs, or decreases at a slow linear rate.
   * **$\eta$ Optimal:** The loss drops sharply in early iterations, forming a smooth, hyperbolic decay that flattens asymptotically as it nears the minimum.
  
  ---
## 4. The Maximum Stable Step Size: Hessian Curvature Bounds (The Graduate Fill-In)
  
  Slide 2 notes: *"Too big = diverges; too small = stalls."* 
  
  In graduate numerical optimization, the boundary between convergence and divergence is governed by the **Lipschitz Continuity of the Gradient** and the **Spectral Radius of the Hessian**.
### Theorem: The Convergence Upper Bound on Step Size
  Assume the loss function $J(w)$ is twice continuously differentiable, and its gradient is Lipschitz continuous with constant $L$:
  
  $$\|\nabla J(u) - \nabla J(v)\|_2 \le L \|u - v\|_2$$
  
  For Ordinary Least Squares with loss $J(w) = \frac{1}{n}\|y - Xw\|_2^2$, the Hessian is constant:
  
  $$H = \nabla^2 J(w) = \frac{2}{n} X^T X$$
  
  The Lipschitz constant $L$ equals the **maximum eigenvalue (spectral norm)** of the Hessian:
  
  $$L = \lambda_{\max}(H) = \frac{2}{n} \lambda_{\max}(X^T X)$$
  
  ```
  ┌────────────────────────────────────────────────────────────────────────┐
  │ The Fundamental Convergence Stability Bound:                          │
  │                                                                        │
  │ Gradient Descent converges to the global minimum IF AND ONLY IF:       │
  │                                                                        │
  │                      0 < η <  2 / λ_max(H)                             │
  │                                                                        │
  │ • If η ≥ 2 / λ_max: Updates bounce across the valley walls with        │
  │   non-diminishing or growing amplitude; the system DIVERGES.           │
  │                                                                        │
  │ • The Theoretically Optimal Step Size (Fastest Convergence Rate):      │
  │                                                                        │
  │                 η* = 2 / (λ_max(H) + λ_min(H))                         │
  └────────────────────────────────────────────────────────────────────────┘
  ```
  
  **Practical Takeaway:** If an unscaled dataset has a feature with massive variance, $\lambda_{\max}(X^T X)$ becomes enormous. This forces the maximum stable learning rate $\eta < \frac{2}{\lambda_{\max}}$ to be tiny, proving mathematically why **feature scaling (Lecture 8) is required for stable first-order optimization**.
  
  ---
## 5. Iterative Progression & Stopping Criteria (Slide 4)
  
  Slide 4 illustrates the discrete trajectory $w^{(0)} \to w^{(1)} \to w^{(2)} \to w^{(3)}$:
  
  $$\mathbf{\text{"Repeat until the ground is flat."}}$$
  
  ```
                      The Discrete Gradient Descent Path (Slide 4)
    Error(w)
      ▲
      │           ╭──────╮
      │         ╭─╯      ╰─╮
      │       ╭─╯          ╰─● w⁽⁰⁾ (Initialization)
      │      ╭╯              │
      │     ╭╯               ▼ -η∇J
      │    ╭╯                 ● w⁽¹⁾
      │    │                   │
      │    │                   ▼ -η∇J
      │    │                    ● w⁽²⁾
      │    │                     │
      │    │                     ▼ -η∇J
      │    ╰─╮                    ● w⁽³⁾ ──▶ ... ──▶ Ground is flat: ∇J ≈ 0
      └──────┴──────────────────────────────────────────────────────▶ w
  ```
### Production Stopping Criteria
  In numerical code, computers cannot run "until the ground is perfectly flat" because floating-point precision rarely reaches exact zero. Production implementations (such as scikit-learn's iterative solvers) monitor three convergence criteria:
  
  1. **Gradient Norm Tolerance (`tol`):**
   Terminate when the Euclidean norm of the gradient falls below a threshold:
   $$\|\nabla J(w^{(t)})\|_2 < \epsilon_{\text{grad}} \quad (\text{e.g., } 10^{-4})$$
  2. **Relative Objective Change:**
   Terminate when the marginal drop in loss between successive epochs becomes negligible:
   $$\frac{|J(w^{(t)}) - J(w^{(t-1)})|}{J(w^{(t-1)})} < \epsilon_{\text{rel}} \quad (\text{e.g., } 10^{-6})$$
  3. **Maximum Iteration Budget (`max_iter`):**
   A hard execution cap to prevent infinite loops if the algorithm fails to converge:
   $$t \ge \text{max\_iter} \quad (\text{e.g., } 1{,}000)$$
  
  ---
## Summary Review Questions for Section 1
  
  1. *Using the Taylor expansion of $J(w + \Delta w)$, prove why stepping in the direction of the negative gradient vector $-\nabla J(w)$ produces the steepest instantaneous drop in loss.*
  2. *If the Hessian of an OLS cost function has a maximum eigenvalue of $\lambda_{\max} = 500$, what is the theoretical maximum learning rate $\eta$ you can use before the optimization loop diverges?*
  3. *Why does an unscaled dataset containing features with widely disparate variances force you to choose an extremely small learning rate during gradient descent?*
  
  ---